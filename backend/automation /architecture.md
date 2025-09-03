      # 1) Create a unique version label for THIS run
      - name: Version label
        id: ver1
        run: |
          echo "label=gh-${GITHUB_SHA::7}-${GITHUB_RUN_ID}-${GITHUB_RUN_ATTEMPT}" >> $GITHUB_OUTPUT

      # 2) Create the EB application version from the ZIP we uploaded
      - name: Create EB application version
        run: |
          aws elasticbeanstalk create-application-version \
            --application-name "${{ env.EB_APP_NAME }}" \
            --version-label "${{ steps.ver1.outputs.label }}" \
            --source-bundle S3Bucket="${{ env.EB_BUCKET }}",S3Key="${{ env.EB_APP_NAME }}/${{ steps.ver1.outputs.label }}.zip"

      # 3) Deploy that exact label to the correct environment (dev -> dev-epmo, test -> Epmo-env)
      - name: Deploy to EB + wait
        run: |
          ENV="${{ steps.select-env.outputs.env_name }}"
          VER="${{ steps.ver1.outputs.label }}"
          aws elasticbeanstalk update-environment --environment-name "$ENV" --version-label "$VER"

          # wait until Ready/Green and on the target label
          for i in $(seq 1 40); do
            STATUS=$(aws elasticbeanstalk describe-environments --environment-names "$ENV" --query "Environments[0].Status" --output text)
            HEALTH=$(aws elasticbeanstalk describe-environments --environment-names "$ENV" --query "Environments[0].Health" --output text)
            CURVER=$(aws elasticbeanstalk describe-environments --environment-names "$ENV" --query "Environments[0].VersionLabel" --output text)
            echo "[$i] Status=$STATUS Health=$HEALTH Version=$CURVER"
            if [ "$STATUS" = "Ready" ] && [ "$HEALTH" = "Green" ] && [ "$CURVER" = "$VER" ]; then
              echo "✅ Deployed $VER to $ENV"
              exit 0
            fi
            sleep 15
          done

          echo "❌ Timed out waiting for $ENV to reach $VER. Recent events:"
          aws elasticbeanstalk describe-events --environment-name "$ENV" --max-items 50 --query "Events[].[EventDate,Severity,Message]" --output table
          exit 1

---

## Architecture — detailed sketches

### 1) CI/CD deployment pipeline (GitHub → EB)
```mermaid
flowchart LR
  dev["Developer pushes to dev/test branch"] -->|Triggers| gha["GitHub Actions (deploy-eb.yml)"]
  gha -->|OIDC: AssumeRoleWithWebIdentity| iam[("IAM Role: GitHubActions-EB-Deploy")]
  gha -->|Upload bundle.zip| s3[("S3: elasticbeanstalk-af-south-1-<ACCOUNT_ID>")]
  gha -->|CreateApplicationVersion| appver["EB Application Version"]
  gha -->|UpdateEnvironment| env["EB Environment (dev-epmo / Epmo-env)"]
  env --> ec2["EC2 instance(s) running AL2023 + Corretto 21"]
  env --> srole[("Service-linked role: AWSServiceRoleForElasticBeanstalk")]
  ec2 --> cw["CloudWatch Logs & Metrics"]

  classDef infra fill:#e7f3ff,stroke:#2b6cb0,stroke-width:1px,color:#1a365d;
  class iam,s3,appver,env,ec2,srole,cw infra;
```

> **Notes**
> - OIDC removes the need for long‑lived AWS keys in GitHub.  
> - The workflow labels each build (e.g., `gh-<sha>-<run>-<attempt>-<ts>`), uploads to S3, creates an **EB Application Version**, and deploys it to the selected **Environment**.

---

### 2) Runtime topology (DNS, VPC, ALB / Single instance, target group)
```mermaid
flowchart TB
  subgraph r53["Route 53 (Hosted Zone)"]
    dns["A/ALIAS: www.myapp.com"]
  end

  %% Two routing options: Single-instance vs Load-balanced
  dns -->|Alias/CNAME| choice{Deploy mode}
  choice -->|Single instance| ebCNAME["EB Environment CNAME"]
  choice -->|Load balanced| alb["Application Load Balancer (HTTPS)"]

  subgraph vpc["VPC"]
    subgraph pub[Public subnets]
      alb ---|Targets| tg["Target Group (HTTP/HTTPS)"]
      ebCNAME --> ec2si["EC2 (single instance)"]
    end
    subgraph priv[Private subnets]
      ec2a["EC2 instance A"]
      ec2b["EC2 instance B"]
    end
  end

  tg --> ec2a
  tg --> ec2b

  %% Shared components
  s3b[("S3: EB app versions bucket")]:::ext -->|Read artifact| envrt["EB Deployment Engine"]
  envrt --> ec2a
  envrt --> ec2b
  envrt --> ec2si

  iamr[("IAM Role: GitHubActions-EB-Deploy")]:::ext
  srole2[("EB service role: AWSServiceRoleForElasticBeanstalk")]:::ext
  iprof[("EC2 instance profile (app permissions)")]:::ext

  classDef opt fill:#fff3cd,stroke:#f0ad4e,color:#5c3a00,stroke-width:1px;
  classDef ext fill:#eef7f1,stroke:#2f855a,color:#1c4532,stroke-width:1px;
  class alb,tg,ec2a,ec2b opt;
```

**Legend & notes**
- **Route 53** resolves `www.myapp.com` to either the **EB CNAME** (single instance) or an **ALB** (load‑balanced).  
- **VPC** contains your EB instances. In load‑balanced mode, the **ALB** forwards to a **Target Group** of EC2 instances; in single‑instance mode, Route 53 can alias directly to the **EB CNAME** which points to the instance.
- **S3 EB bucket** stores the uploaded `bundle.zip` and EB’s staged copies.
- **IAM Roles**:
  - *GitHubActions-EB-Deploy* (assumed via OIDC) to create app versions & kick off deploys.
  - *AWSServiceRoleForElasticBeanstalk* used by EB to perform internal operations (incl. ASG calls even on single‑instance).
  - *EC2 instance profile* for app‑level AWS access (e.g., S3, SSM) if required.
- **Security groups**: ALB, EC2, and DB/other services should each have tight SGs allowing only necessary traffic.
- **TLS**: Use ACM certs on the ALB (or on EB if terminating at instance).
- **Secrets**: Prefer EB **environment variables** or **SSM Parameter Store**; avoid committing secrets.

---

### 3) Request path (sequence)
```mermaid
sequenceDiagram
  participant User
  participant R53 as Route 53
  participant ALB as ALB (optional)
  participant EB as EB Env CNAME / Target Group
  participant EC2 as EC2 (Spring Boot)
  participant S3 as S3 (artifacts)

  User->>R53: Lookup www.myapp.com
  R53-->>User: Alias to ALB or EB CNAME
  alt Load balanced
    User->>ALB: HTTPS
    ALB->>EB: Forward to Target Group
  else Single instance
    User->>EB: HTTPS to EB CNAME
  end
  EB->>EC2: HTTP/HTTPS request
  EC2-->>User: Response
  note right of EC2: App reads config/secrets from env/SSM. EB pulled artifact from S3 during deploy.
```

---

### Resource map (quick reference)

| Layer | Service | What it does |
|---|---|---|
| DNS | Route 53 | Public DNS; ALIAS/CNAME for `www.myapp.com` |
| Edge/LB | **ALB** *(optional)* | TLS termination & routing to target group |
| Compute | Elastic Beanstalk | Manages EC2s, deployments, health |
| Compute | EC2 (AL2023 + Corretto 21) | Runs Spring Boot JAR |
| Storage | S3 (EB bucket) | Stores `bundle.zip` and EB staged copies |
| IAM | GitHubActions-EB-Deploy | Assumed via OIDC by GitHub Actions |
| IAM | AWSServiceRoleForElasticBeanstalk | EB’s service‑linked role for internal ops |
| IAM | EC2 instance profile | App’s AWS permissions (optional) |
| Observability | CloudWatch | Logs/metrics/alarms |
| Networking | VPC, subnets, SGs | Isolation and traffic control |

> If you want, we can export these Mermaid diagrams to PNG/SVG or tailor a **load‑balanced** vs **single‑instance** only view for the README.
