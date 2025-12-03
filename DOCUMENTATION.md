# Documentation

## Overview

This is a full-stack CRUD (Create, Read, Update, Delete) web application that demonstrates a modern three-tier architecture. The application allows users to manage tutorials through a web interface, with all data persisted in a MySQL database.

## What It Does

The application provides a complete tutorial management system where users can:
- **Create** new tutorials with title, description, and published status
- **View** all tutorials or search by title
- **Update** existing tutorial information
- **Delete** individual tutorials or all tutorials at once
- **Filter** tutorials by published status

## Architecture & Components

The application consists of three main layers:

### 1. **Frontend (Angular 17)**
- **Location**: `angular-17-client/`
- **Technology**: Angular 17 with TypeScript, Bootstrap 4
- **Components**:
  - `tutorials-list`: Displays all tutorials with search functionality
  - `tutorial-details`: Shows and edits individual tutorial details
  - `add-tutorial`: Form for creating new tutorials
- **Service**: `TutorialService` handles HTTP communication with the backend
- **Port**: 4200 (development) or 80 (Docker production)

### 2. **Backend (Spring Boot)**
- **Location**: `spring-boot-server/`
- **Technology**: Spring Boot 3.1.5, Spring Data JPA, Java 17
- **Components**:
  - `TutorialController`: REST API endpoints (`/api/tutorials`)
  - `TutorialRepository`: Data access layer using Spring Data JPA
  - `Tutorial`: Entity model with id, title, description, published fields
- **Port**: 8080 (default) or 8081 (as configured)

### 3. **Database (MySQL)**
- **Version**: MySQL 8.0
- **Database Name**: `testdb`
- **Port**: 3306
- **Auto-schema**: Tables are automatically created/updated via Hibernate

## How to Run

### Prerequisites
- **Java 17** or higher
- **Maven 3.6+**
- **Node.js 18+** and npm
- **MySQL 8.0** (or use Docker)
- **Angular CLI** (`npm install -g @angular/cli`)

### Option 1: Manual Setup

#### Step 1: Start MySQL Database
```bash
# Using Docker (recommended)
docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=testdb -p 3306:3306 -d mysql:8.0

# Or use your existing MySQL instance
# Create database: CREATE DATABASE testdb;
```

#### Step 2: Start Spring Boot Backend
```bash
cd spring-boot-server
mvn spring-boot:run
```
The API will be available at `http://localhost:8080/api/tutorials`

**Note**: Update `application.properties` if your MySQL credentials differ from:
- Username: `root`
- Password: `123456`
- Database: `testdb`

#### Step 3: Start Angular Frontend
```bash
cd angular-17-client
npm install
ng serve
```
The application will be available at `http://localhost:4200`

### Option 2: Docker Compose (Recommended)

Run the entire stack with a single command:

```bash
docker-compose up --build
```

This will start:
- MySQL database on port `3306`
- Spring Boot API on port `8080`
- Angular frontend on port `4200`

To stop all services:
```bash
docker-compose down
```

## API Endpoints

The backend exposes the following REST endpoints:

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/tutorials` | Get all tutorials (optional `?title=keyword` for search) |
| GET | `/api/tutorials/{id}` | Get tutorial by ID |
| GET | `/api/tutorials/published` | Get all published tutorials |
| POST | `/api/tutorials` | Create a new tutorial |
| PUT | `/api/tutorials/{id}` | Update a tutorial |
| DELETE | `/api/tutorials/{id}` | Delete a tutorial |
| DELETE | `/api/tutorials` | Delete all tutorials |

## Project Structure

```
angular-17-spring-boot-mysql-example/
├── angular-17-client/          # Angular frontend application
│   ├── src/app/
│   │   ├── components/         # UI components
│   │   ├── models/             # TypeScript models
│   │   └── services/           # HTTP services
│   └── Dockerfile              # Frontend Docker image
├── spring-boot-server/         # Spring Boot backend application
│   ├── src/main/java/
│   │   └── com/bezkoder/spring/datajpa/
│   │       ├── controller/     # REST controllers
│   │       ├── model/          # Entity models
│   │       └── repository/     # Data repositories
│   └── Dockerfile              # Backend Docker image
└── docker-compose.yml          # Multi-container orchestration
```

## Configuration

### Backend Configuration
Edit `spring-boot-server/src/main/resources/application.properties`:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/testdb?useSSL=false
spring.datasource.username=root
spring.datasource.password=123456
```

### Frontend Configuration
The Angular app connects to the backend API. Update the service URL in `tutorial.service.ts` if your backend runs on a different port.

## Troubleshooting

- **Connection refused**: Ensure MySQL is running and credentials match
- **Port conflicts**: Change ports in `application.properties` (backend) or `docker-compose.yml`
- **CORS errors**: Backend has `@CrossOrigin(origins = "*")` enabled
- **Database not found**: Create the `testdb` database manually or let Docker create it

## Technology Stack Summary

- **Frontend**: Angular 17, TypeScript, Bootstrap 4, RxJS
- **Backend**: Spring Boot 3.1.5, Spring Data JPA, Java 17
- **Database**: MySQL 8.0
- **Build Tools**: Maven, npm, Angular CLI
- **Containerization**: Docker, Docker Compose

