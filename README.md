# FUACS - Face-based University Attendance Check System

A comprehensive attendance management system for universities that leverages facial recognition technology to automate and streamline attendance tracking for both regular classes and exams.

## Overview

FUACS is a full-stack application consisting of three main components:
- **Backend API** - Spring Boot REST API for core business logic
- **Frontend Web** - Next.js web application with role-based interfaces
- **Recognition Service** - Python FastAPI service for face detection and recognition using InsightFace

## Features

### For Students
- View attendance records and statistics
- Check class schedules
- Update profile information
- Track attendance across semesters

### For Lecturers
- Monitor class attendance in real-time
- Generate attendance reports
- Manage class rosters
- View teaching schedule and calendar
- Access slot-based attendance tracking

### For Supervisors
- Oversee exam attendance
- Generate comprehensive reports
- Monitor multiple exam slots
- Track attendance across rooms

### For Administrators
- Complete system management
- User and role management
- Configure cameras and rooms
- Manage academic data (semesters, subjects, majors, classes)
- Import/export data
- System-wide analytics dashboard

### Core Capabilities
- Real-time face recognition via RTSP camera streams
- Automated attendance recording with photo evidence
- Multi-role access control with granular permissions
- Bulk student photo upload and management
- Excel import/export for academic data
- Email notifications and password reset
- Real-time updates via Server-Sent Events (SSE)
- Scheduled attendance finalization
- Comprehensive audit trails

## Tech Stack

### Backend
- **Framework**: Spring Boot 3.5.6
- **Language**: Java 21
- **Database**: PostgreSQL with Flyway migrations
- **Security**: Spring Security with OAuth2
- **API Documentation**: OpenAPI/Swagger
- **Testing**: JUnit, Testcontainers, JaCoCo (80% coverage)
- **Build Tool**: Maven

### Frontend
- **Framework**: Next.js 15.5.4 (App Router)
- **Language**: TypeScript
- **UI Library**: React 19
- **Styling**: Tailwind CSS 4
- **Components**: Radix UI, shadcn/ui
- **State Management**: Zustand
- **Data Fetching**: TanStack Query
- **Forms**: React Hook Form with Zod validation
- **Internationalization**: next-intl
- **Authentication**: Google OAuth

### Recognition Service
- **Framework**: FastAPI
- **Language**: Python 3.10+
- **Face Recognition**: InsightFace with ONNX Runtime GPU
- **Computer Vision**: OpenCV
- **Package Manager**: Poetry
- **Testing**: pytest, pytest-asyncio

## Architecture

```
┌─────────────────┐
│  Frontend Web   │
│   (Next.js)     │
└────────┬────────┘
         │
         │ REST API
         │
┌────────▼────────┐      ┌──────────────────┐
│  Backend API    │◄────►│  Recognition     │
│  (Spring Boot)  │      │  Service         │
└────────┬────────┘      │  (FastAPI)       │
         │               └────────┬─────────┘
         │                        │
         │                        │ RTSP
┌────────▼────────┐      ┌────────▼─────────┐
│   PostgreSQL    │      │  IP Cameras      │
│   Database      │      │  (RTSP Streams)  │
└─────────────────┘      └──────────────────┘
```

## Prerequisites

- **Java 21** or higher
- **Node.js 20** or higher
- **Python 3.10** or higher
- **PostgreSQL 14** or higher
- **Poetry** (Python package manager)
- **Maven** (or use included wrapper)
- **Docker** (optional, for containerized deployment)

## Installation & Setup

### 1. Backend Setup

```bash
cd backend

# Copy environment template
cp .env.example .env

# Configure your .env file with:
# - Database credentials
# - Google OAuth credentials
# - JWT token settings
# - Email configuration
# - Frontend URL

# Run with Maven
./mvnw spring-boot:run

# Or build and run
./mvnw clean package
java -jar target/backend-0.0.1-SNAPSHOT.jar
```

The backend API will be available at `http://localhost:8080`

### 2. Frontend Setup

```bash
cd frontend-web

# Install dependencies
npm install

# Copy environment template
cp .env.example .env.local

# Configure your .env.local with:
# - Backend API URL
# - Google OAuth Client ID

# Run development server
npm run dev

# Build for production
npm run build
npm start
```

The frontend will be available at `http://localhost:3000`

### 3. Recognition Service Setup

```bash
cd recognition-service

# Install dependencies with Poetry
poetry install

# Copy environment template
cp .env.example .env

# Configure your .env file

# Run the service
poetry run python -m recognition_service.main

# Or activate virtual environment
poetry shell
python -m recognition_service.main
```

The recognition service will be available at `http://localhost:8000`

## Environment Variables

### Backend (.env)
```env
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
TOKEN_SIGN_KEY=your_jwt_secret_key
SPRING_DATASOURCE_PASSWORD=your_database_password
TOKEN_EXPIRE=120
MAIL=your-email@gmail.com
MAIL_APP_PASSWORD=your_app_password
FRONTEND_URL=http://localhost:3000
```

### Frontend (.env.local)
```env
NEXT_PUBLIC_API_URL=http://localhost:8080/api/v1
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_google_client_id
```

### Recognition Service (.env)
Configure according to your camera setup and hardware capabilities.

## Database Setup

The backend uses Flyway for database migrations. On first run, it will automatically:
1. Create the database schema
2. Set up initial tables
3. Seed default roles and permissions

Ensure PostgreSQL is running and the database specified in your configuration exists.

## API Documentation

Once the backend is running, access the interactive API documentation at:
- Swagger UI: `http://localhost:8080/swagger-ui.html`
- OpenAPI JSON: `http://localhost:8080/v3/api-docs`

## Testing

### Backend Tests
```bash
cd backend

# Run unit tests
./mvnw test

# Run integration tests
./mvnw verify

# Generate coverage report
./mvnw jacoco:report
```

### Frontend Tests
```bash
cd frontend-web
npm test
```

### Recognition Service Tests
```bash
cd recognition-service
poetry run pytest
```

## Deployment

### Docker Deployment

Each service includes a Dockerfile for containerized deployment. The project uses GitLab CI/CD for automated builds and deployments to staging and production environments.

### CI/CD Pipeline

The project includes GitLab CI configuration with:
- Automated testing on commits
- Docker image building and pushing
- Automated deployment to staging/production
- Environment-specific configurations

## Project Structure

```
.
├── backend/                 # Spring Boot API
│   ├── src/
│   │   ├── main/java/com/fuacs/backend/
│   │   │   ├── config/     # Security, OpenAPI, etc.
│   │   │   ├── controller/ # REST endpoints
│   │   │   ├── entity/     # JPA entities
│   │   │   ├── repository/ # Data access
│   │   │   ├── service/    # Business logic
│   │   │   └── dto/        # Data transfer objects
│   │   └── resources/
│   │       └── db/migration/ # Flyway migrations
│   └── pom.xml
│
├── frontend-web/            # Next.js application
│   ├── app/                # App router pages
│   │   ├── admin/         # Admin interface
│   │   ├── lecturer/      # Lecturer interface
│   │   ├── student/       # Student interface
│   │   └── supervisor/    # Supervisor interface
│   ├── components/        # Reusable components
│   ├── hooks/            # Custom React hooks
│   ├── lib/              # Utilities and API clients
│   └── messages/         # i18n translations
│
└── recognition-service/    # Python face recognition
    ├── src/recognition_service/
    │   ├── api/          # FastAPI routes
    │   ├── core/         # Configuration
    │   ├── models/       # Data models
    │   ├── services/     # Recognition logic
    │   └── utils/        # Helper functions
    └── tests/
```

## Security

- JWT-based authentication
- Role-based access control (RBAC)
- Google OAuth integration
- Password reset via email
- Secure token management
- HTTPS recommended for production

## Performance

- Real-time face recognition with GPU acceleration
- Efficient embedding storage and comparison
- Connection pooling for database
- Caching strategies for frequently accessed data
- Optimized RTSP stream handling

## Contributing

1. Follow the existing code style and conventions
2. Write tests for new features
3. Ensure all tests pass before submitting
4. Update documentation as needed
5. Use meaningful commit messages

## License

FPT University

## Support

For issues, questions, or contributions, please contact the FUACS development team.
