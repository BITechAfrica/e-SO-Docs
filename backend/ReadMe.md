# Backend DOCS
<img width="795" height="408" alt="image" src="https://github.com/user-attachments/assets/6f08471a-dd79-4b27-bf1d-b546c6748f15" />

# e-SO - Electronic Strategic Management Office Backend

[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.3.2-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://openjdk.org/)
[![MariaDB](https://img.shields.io/badge/Database-MariaDB-blue.svg)](https://mariadb.org/)
[![AWS](https://img.shields.io/badge/Cloud-AWS-yellow.svg)](https://aws.amazon.com/)

## 🏢 Overview

The **e-SO (Electronic Strategic Management Office)** is a comprehensive project management platform designed to streamline organizational project governance, strategic alignment, and operational efficiency. This Spring Boot backend application provides robust APIs for managing the complete project lifecycle from strategic planning to execution monitoring.

### 🎯 Core Purpose

- **Strategic Alignment**: Link Initiatives to organizational strategic objectives
- **Project Governance**: Comprehensive project oversight and quality management
- **Risk Management**: Proactive risk identification, assessment, and mitigation
- **Resource Management**: Team allocation and capacity planning
- **Financial Oversight**: Budget management and billing integration
- **Performance Monitoring**: Real-time dashboards and progress tracking

## 🏗️ Architecture Overview

### Technology Stack

| Component | Technology | Version |
|-----------|------------|---------|
| **Framework** | Spring Boot | 3.3.2 |
| **Language** | Java | 21 |
| **Database** | MariaDB | Latest |
| **Cloud Platform** | AWS | - |
| **Documentation** | SpringDoc OpenAPI | 2.6.0 |
| **Build Tool** | Gradle | Kotlin DSL |
| **Email Service** | Spring Mail | - |
| **File Storage** | AWS S3 | - |
| **Payment Gateway** | PayStack | - |

### 🔧 Key Dependencies

- **Spring Data JPA** - Database operations and ORM
- **Spring Web** - REST API development
- **Thymeleaf** - Email templating engine
- **MapStruct** - Object mapping
- **Lombok** - Boilerplate code reduction
- **Spring Shell** - Interactive testing framework
- **AWS SDK** - Cloud service integration
- **iText PDF** - PDF generation
- **Spring Validation** - Input validation

## 📊 Core Business Domains

### 1. **Project Management** 📋
- **Schedule Management**: Project timelines, milestones, and deliverables
- **Business Cases**: Investment justification and strategic alignment
- **Project Readiness**: Pre-execution assessment and preparation
- **Quality Management**: Quality metrics, standards, and compliance

### 2. **Strategic Planning** 🎯
- **Strategic Objectives**: Organizational goals and KPIs
- **Program Management**: Portfolio-level project coordination
- **Performance Dashboards**: Executive reporting and analytics

### 3. **Risk & Compliance** ⚠️
- **Risk Management**: Risk identification, assessment, and mitigation
- **Risk Monitoring**: Continuous risk tracking and alerting
- **Compliance Tracking**: Regulatory and internal policy compliance

### 4. **Resource Management** 👥
- **Team Management**: Team composition and role assignments
- **User Management**: Authentication, authorization, and profiles
- **Department Management**: Organizational structure and hierarchy

### 5. **Financial Management** 💰
- **Billing & Invoicing**: Automated billing and payment processing
- **Subscription Management**: Service tier management
- **Payment Integration**: PayStack payment gateway integration

### 6. **Communication & Alerts** 📧
- **Email Notifications**: Automated project alerts and updates
- **Marquee Alarms**: Critical project status notifications
- **Meeting Management**: Project meeting coordination

## 🏛️ System Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   API Gateway   │    │   Load Balancer │
│   (Vue.js)      │◄──►│   (Spring Web)  │◄──►│   (AWS ALB)     │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                       ┌────────┴────────┐
                       │                 │
              ┌─────────────────┐  ┌─────────────────┐
              │  Business Logic │  │   Data Access   │
              │   (Services)    │  │   (JPA/Hibernate)│
              └─────────────────┘  └─────────────────┘
                       │                 │
              ┌─────────────────┐  ┌─────────────────┐
              │  External APIs  │  │    Database     │
              │ (PayStack, AWS) │  │    (MariaDB)    │
              └─────────────────┘  └─────────────────┘
```

## 📁 Project Structure

```
src/main/java/com/bitech/e-SO/
├── 🚀 e-SOApplication.java              # Application entry point
├── 📁 config/                          # Configuration classes
│   ├── JacksonConfig.java               # JSON serialization config
│   ├── PayStackConfig.java              # Payment gateway config
│   └── WebMvcConfig.java                # Web configuration
├── 📁 controller/                       # REST API controllers
│   ├── DashboardController.java         # Project dashboards
│   ├── ProjectController.java           # Project management
│   ├── RiskManagementController.java    # Risk management
│   └── PayStackController.java          # Payment processing
├── 📁 service/                          # Business logic layer
│   ├── DashboardService.java            # Dashboard operations
│   ├── EmailService.java                # Email notifications
│   ├── RiskManagementService.java       # Risk operations
│   └── PayStackService.java             # Payment operations
├── 📁 Entities/                         # JPA entities
│   ├── Schedule.java                    # Project schedules
│   ├── BusinessCase.java                # Business cases
│   ├── RiskManagement.java              # Risk records
│   └── User.java                        # User accounts
├── 📁 repository/                       # Data access layer
├── 📁 DataTransferObjects/              # API DTOs
├── 📁 util/                            # Utility classes
└── 📁 shell/                           # Spring Shell commands
```

## 🌟 Key Features

### 📈 **Dashboard & Analytics**
- Real-time project performance metrics
- Executive dashboards with KPI tracking
- Custom report generation
- Data visualization and insights

### 🔔 **Intelligent Alerting**
- Quality score monitoring with threshold alerts
- Slip rate notifications for schedule delays
- High-risk project identification
- Critical path change notifications

### 💼 **Business Case Management**
- Strategic impact assessment
- Financial justification tracking
- ROI calculation and monitoring
- Decision support documentation

### 🛡️ **Risk Management**
- Comprehensive risk register
- Risk assessment matrices
- Mitigation strategy tracking
- Automated risk escalation

### 👥 **Team Collaboration**
- Multi-role user management
- Team assignment and tracking
- Meeting coordination
- Document management

### 🔄 **Integration Capabilities**
- AWS S3 for file storage
- PayStack for payment processing
- Email service integration
- RESTful API design

## 🚀 Getting Started

### Prerequisites

- **Java 21** or higher
- **MariaDB** database
- **AWS Account** (for S3 storage)
- **PayStack Account** (for payments)

### Environment Setup

1. **Clone the repository**
```bash
git clone <repository-url>
cd e-SO
```

2. **Configure environment variables**
Create a `.env` file in the project root:
```env
ENVIRONMENT=dev
DB_URL=jdbc:mariadb://your-db-host:3306/e-SO
DB_USERNAME=your-username
DB_PASSWORD=your-password
AWS_ACCESS_KEY=your-aws-key
AWS_SECRET_KEY=your-aws-secret
PAYSTACK_SECRET_KEY=your-paystack-key
MAIL_HOST=your-mail-host
MAIL_USERNAME=your-mail-username
MAIL_PASSWORD=your-mail-password
```

3. **Build and run the application**
```bash
./gradlew bootRun
```

4. **Access the application**
- **API Documentation**: http://localhost:8080/swagger-ui
- **Health Check**: http://localhost:8080/api/health
- **Spring Shell**: Available for interactive testing

## 📚 API Documentation

The application provides comprehensive REST APIs documented with OpenAPI 3.0:

### Core API Endpoints

| Endpoint | Description |
|----------|-------------|
| `/api/dashboard/*` | Project dashboards and analytics |
| `/api/projects/*` | Project management operations |
| `/api/risks/*` | Risk management APIs |
| `/api/teams/*` | Team and user management |
| `/api/business-cases/*` | Business case operations |
| `/api/payments/*` | Payment and billing APIs |

**Full API Documentation**: Available at `/swagger-ui` when running

## 🔧 Configuration

### Database Configuration
The application uses MariaDB with connection pooling:
- **Pool Size**: 10 connections
- **Idle Timeout**: 30 seconds
- **DDL Auto**: Update mode for development

### Email Configuration
Supports SMTP with SSL:
- **Default Host**: mail.e-pmo.africa
- **Port**: 465 (SSL)
- **Templates**: Thymeleaf-based HTML templates

### AWS Integration
- **S3 Bucket**: Document and file storage
- **Region**: af-south-1 (Africa - Cape Town)

## 🧪 Testing

### Email Testing Infrastructure
The application includes comprehensive email testing capabilities:

```bash
# Test quality score alerts
curl "http://localhost:8080/api/test/quality-alert?email=test@example.com"

# Test risk notifications
curl "http://localhost:8080/api/test/risk-alert?email=test@example.com"
```

### Spring Shell Testing
Interactive testing available through Spring Shell:
```bash
shell:> test-email-notification --type quality --recipient test@example.com
```

## 🔒 Security & Compliance

- **Authentication**: JWT-based authentication
- **Authorization**: Role-based access control
- **Data Validation**: Comprehensive input validation
- **Error Handling**: Global exception handling
- **Audit Logging**: Complete change tracking

## 🌍 Deployment

### Environment Profiles
- **Development**: Local development with H2/MariaDB
- **Testing**: Staging environment with test data
- **Production**: AWS-hosted production environment

### Docker Support
```dockerfile
# Dockerfile included for containerized deployment
FROM openjdk:21-jdk-slim
COPY build/libs/e-SO-*.jar app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

## 📊 Monitoring & Observability

- **Health Checks**: Spring Boot Actuator endpoints
- **Logging**: Structured logging with configurable levels
- **Metrics**: Application performance monitoring
- **Error Tracking**: Comprehensive error handling and reporting

## 📞 Support

For technical support and questions:
- **Documentation**: Available in `[/docs](https://dev.epmo-backend.com/swagger-ui/index.html)` directory
- **API Testing**: Use Swagger UI for interactive testing 

## 📜 License

This project is proprietary software developed by BiTech for enterprise strategy management solutions.

---

**Built with ❤️ by the BiTech Development Team**
