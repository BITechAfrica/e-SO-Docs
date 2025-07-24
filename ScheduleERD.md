# Schedule Entity Relationship Diagram

This document shows the relationships between the Schedule entity and all related entities in the ePMO system.

## Entity Relationship Diagram

```mermaid
erDiagram
    Schedule {
        int schedule_id PK
        string project_name
        string project_sponsor
        int program_id FK
        int project_manager_id FK
        int project_sponsor_id FK
        date start_date
        date end_date
        double budget
        string status
    }
    
    Program {
        int program_id PK
        string program_name
        string program_sponsor
        string status
    }
    
    User {
        int user_id PK
        string user_first_name
        string user_last_name
        string user_email
        string user_phone
        string job_title
        string department
    }
    
    BusinessCase {
        int business_case_id PK
        int schedule_id FK
        double business_problem_definition_score
        string business_problem_definition_comment
        double overall_business_case_score
    }
    
    UserRequirement {
        int requirement_id PK
        int schedule_id FK
        double project_definition
        double requirement_gathering
        double high_level_scope
    }
    
    ProjectReadiness {
        int id PK
        int schedule_id FK
        double scope
    }
    
    StrategicObjective {
        int id PK
        int schedule_id FK
        string objective_name
        double actual_measure
    }
    
    SubObjective {
        int id PK
        int strategic_objective_id FK
        string name
        decimal amount
    }
    
    Meeting {
        int id PK
        int schedule_id FK
        string project_status
        date forecasted_end_date
    }
    
    RiskManagement {
        int risk_id PK
        int schedule_id FK
        string risk_name
        date due_date
        string mitigation_strategy
        string responsible_person
        string status
    }
    
    QualityManagement {
        int quality_id PK
        int schedule_id FK
        string quality_desc
        double performance_metrics
        double quality_points
    }
    
    Team {
        int team_id PK
        int schedule_id FK
        string team_name
        string team_desc
        string team_status
    }
    
    TeamUser {
        int team_user_id PK
        int team_id FK
        int user_id FK
        string role
        string job_title
        string invitation_status
    }
    
    Documents {
        int document_id PK
        int schedule_id FK
        string document_type
        string document_name
    }
    
    ScheduleLog {
        int log_id PK
        int schedule_id FK
        string change_description
        date change_date
        string changed_by
    }
    
    MarqueeAlarm {
        int alarm_id PK
        int project_id FK
        string assessment_type
        double assessment_score
    }

    %% Direct Relationships - Schedule as Parent
    Schedule ||--o{ BusinessCase : "has"
    Schedule ||--o{ UserRequirement : "has"
    Schedule ||--o{ ProjectReadiness : "has"
    Schedule ||--o{ StrategicObjective : "has"
    Schedule ||--o{ Meeting : "has"
    Schedule ||--o{ RiskManagement : "has"
    Schedule ||--o{ QualityManagement : "has"
    Schedule ||--o{ Team : "has"
    Schedule ||--o{ Documents : "has"
    Schedule ||--o{ ScheduleLog : "tracks_changes"
    
    %% Indirect Relationships - Schedule as Child
    Program ||--o{ Schedule : "contains"
    User ||--o{ Schedule : "manages_as_pm"
    User ||--o{ Schedule : "sponsors"
    
    %% Sub-entity Relationships
    StrategicObjective ||--o{ SubObjective : "has"
    Team ||--o{ TeamUser : "has_members"
    User ||--o{ TeamUser : "belongs_to_team"
    
    %% Indirect Relationship via project_id
    Schedule ||--o{ MarqueeAlarm : "triggers_alarms"
```

## Relationship Descriptions

### Direct Relationships (Foreign Key to Schedule)

1. **BusinessCase** → Schedule (Many-to-One)
   - Each business case belongs to one schedule/project
   - Contains financial assessments, strategic impact, and problem definitions

2. **UserRequirement** → Schedule (Many-to-One)
   - Stores project requirements and their scores
   - Links requirement gathering to specific projects

3. **ProjectReadiness** → Schedule (Many-to-One)
   - Tracks readiness metrics for various project aspects
   - Monitors project preparation status

4. **StrategicObjective** → Schedule (Many-to-One)
   - Links strategic objectives to specific projects
   - Tracks objective achievement and measures

5. **Meeting** → Schedule (Many-to-One)
   - Records project meetings and status updates
   - Tracks project progress over time

6. **RiskManagement** → Schedule (Many-to-One)
   - Manages risks associated with specific projects
   - Tracks mitigation strategies and responsible parties

7. **QualityManagement** → Schedule (Many-to-One)
   - Monitors quality metrics for projects
   - Tracks quality performance and corrective actions

8. **Team** → Schedule (Many-to-One)
   - Associates teams with specific projects
   - Manages team composition and status

9. **Documents** → Schedule (Many-to-One)
   - Links project documents to schedules
   - Organizes project-related documentation

10. **ScheduleLog** → Schedule (Many-to-One)
    - Tracks all changes made to schedules
    - Provides audit trail for project modifications

### Indirect Relationships

1. **Program → Schedule** (One-to-Many)
   - Programs contain multiple projects/schedules
   - Provides program-level organization

2. **User → Schedule** (One-to-Many)
   - Users can be project managers or sponsors
   - Links human resources to projects

3. **MarqueeAlarm ← Schedule** (Many-to-One via project_id)
   - Alarms are triggered based on project assessments
   - Provides early warning system for project issues

4. **SubObjective ← StrategicObjective** (Many-to-One)
   - Indirectly related to Schedule through StrategicObjective
   - Breaks down strategic objectives into measurable components

## Key Insights

- **Schedule** is the central entity in the project management system
- Most project-related data flows through the Schedule entity
- The system supports hierarchical organization (Program → Schedule → Sub-entities)
- Comprehensive tracking of project aspects (risks, quality, requirements, etc.)
- Strong audit capabilities through ScheduleLog
- Team management integrated with project scheduling
