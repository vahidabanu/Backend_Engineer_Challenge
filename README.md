# Backend_Engineer_Challenge
# Workforce Management API

A Spring Boot application for managing workforce tasks, built as part of a Backend Engineer challenge.

## Project Overview

This application provides a comprehensive API for workforce management, including:
- Task creation and assignment
- Task status tracking
- Priority management
- Activity history and comments
- Smart date-based task filtering

## Features Implemented

### Bug Fixes
1. **Bug #1 Fixed**: Task re-assignment now properly cancels old tasks instead of creating duplicates
2. **Bug #2 Fixed**: Date-based task fetching now excludes cancelled tasks

### New Features
1. **Smart Daily Task View**: Enhanced date filtering that shows active tasks within range plus ongoing tasks from before the range
2. **Task Priority Management**: 
   - Set and update task priorities (HIGH, MEDIUM, LOW)
   - Fetch tasks by priority level
3. **Activity History & Comments**:
   - Automatic logging of all task activities
   - User comments on tasks
   - Complete activity history in task details

## Technology Stack

- **Java 17**
- **Spring Boot 3.0.4**
- **Gradle** (Build tool)
- **Lombok** (Reduces boilerplate)
- **MapStruct** (Object mapping)
- **In-memory storage** (No database required)

## Project Structure

```
src/main/java/com/railse/hiring/workforcemgmt/
├── Application.java
├── common/
│   ├── exception/
│   │   ├── CustomExceptionHandler.java
│   │   ├── ResourceNotFoundException.java
│   │   └── StatusCode.java
│   └── model/response/
│       ├── Pagination.java
│       ├── Response.java
│       └── ResponseStatus.java
├── controller/
│   └── TaskManagementController.java
├── dto/
│   ├── AddCommentRequest.java
│   ├── AssignByReferenceRequest.java
│   ├── TaskCreateRequest.java
│   ├── TaskFetchByDateRequest.java
│   ├── TaskManagementDto.java
│   ├── UpdatePriorityRequest.java
│   └── UpdateTaskRequest.java
├── mapper/
│   └── ITaskManagementMapper.java
├── model/
│   ├── TaskActivity.java
│   ├── TaskManagement.java
│   └── enums/
│       ├── Priority.java
│       ├── Task.java
│       └── TaskStatus.java
├── repository/
│   ├── InMemoryTaskActivityRepository.java
│   ├── InMemoryTaskRepository.java
│   ├── TaskActivityRepository.java
│   └── TaskRepository.java
└── service/
    ├── TaskManagementService.java
    └── impl/
        └── TaskManagementServiceImpl.java
```

## How to Run

### Prerequisites
- Java 17 or higher
- Gradle 7.0 or higher

### Setup Instructions

1. **Clone or download the project**
2. **Navigate to the project directory**
3. **Run the application**:
   ```bash
   ./gradlew bootRun
   ```
   Or on Windows:
   ```bash
   gradlew.bat bootRun
   ```

4. **Access the application**:
   - The API will be available at `http://localhost:8080`
   - Swagger UI (if added) would be at `http://localhost:8080/swagger-ui.html`

## API Endpoints

### Core Endpoints

#### Get Task by ID
```bash
GET /task-mgmt/{id}
```

#### Create Tasks
```bash
POST /task-mgmt/create
Content-Type: application/json

{
   "requests": [
       {
           "reference_id": 105,
           "reference_type": "ORDER",
           "task": "CREATE_INVOICE",
           "assignee_id": 1,
           "priority": "HIGH",
           "task_deadline_time": 1728192000000
       }
   ]
}
```

#### Update Tasks
```bash
POST /task-mgmt/update
Content-Type: application/json

{
   "requests": [
       {
           "task_id": 1,
           "task_status": "STARTED",
           "description": "Work has been started on this invoice."
       }
   ]
}
```

#### Assign Tasks by Reference (Bug #1 Fixed)
```bash
POST /task-mgmt/assign-by-ref
Content-Type: application/json

{
   "reference_id": 201,
   "reference_type": "ENTITY",
   "assignee_id": 5
}
```

#### Fetch Tasks by Date (Bug #2 Fixed + Smart View)
```bash
POST /task-mgmt/fetch-by-date/v2
Content-Type: application/json

{
   "start_date": 1672531200000,
   "end_date": 1735689599000,
   "assignee_ids": [1, 2]
}
```

### New Feature Endpoints

#### Update Task Priority
```bash
PUT /task-mgmt/priority
Content-Type: application/json

{
   "task_id": 1,
   "priority": "HIGH"
}
```

#### Get Tasks by Priority
```bash
GET /task-mgmt/priority/HIGH
```

#### Add Comment to Task
```bash
POST /task-mgmt/comment
Content-Type: application/json

{
   "task_id": 1,
   "comment": "This is an important task that needs attention",
   "performed_by": "user123"
}
```

## Testing the Application

### Test Bug Fixes

1. **Test Bug #1 Fix**:
   ```bash
   # First, assign tasks to reference 201
   curl -X POST http://localhost:8080/task-mgmt/assign-by-ref \
   -H "Content-Type: application/json" \
   -d '{"reference_id": 201, "reference_type": "ENTITY", "assignee_id": 3}'
   
   # Now reassign to a different assignee
   curl -X POST http://localhost:8080/task-mgmt/assign-by-ref \
   -H "Content-Type: application/json" \
   -d '{"reference_id": 201, "reference_type": "ENTITY", "assignee_id": 5}'
   
   # Check that old tasks are cancelled and new ones are created
   curl http://localhost:8080/task-mgmt/1
   ```

2. **Test Bug #2 Fix**:
   ```bash
   # Fetch tasks by date - should exclude cancelled tasks
   curl -X POST http://localhost:8080/task-mgmt/fetch-by-date/v2 \
   -H "Content-Type: application/json" \
   -d '{"start_date": 1672531200000, "end_date": 1735689599000, "assignee_ids": [1, 2]}'
   ```

### Test New Features

1. **Test Priority Management**:
   ```bash
   # Update priority
   curl -X PUT http://localhost:8080/task-mgmt/priority \
   -H "Content-Type: application/json" \
   -d '{"task_id": 1, "priority": "HIGH"}'
   
   # Get tasks by priority
   curl http://localhost:8080/task-mgmt/priority/HIGH
   ```

2. **Test Comments**:
   ```bash
   # Add a comment
   curl -X POST http://localhost:8080/task-mgmt/comment \
   -H "Content-Type: application/json" \
   -d '{"task_id": 1, "comment": "This task is urgent!", "performed_by": "manager1"}'
   
   # Get task with activities
   curl http://localhost:8080/task-mgmt/1
   ```

## Key Improvements Made

### Bug Fixes
1. **Task Re-assignment**: Now properly cancels existing tasks before creating new ones
2. **Date Filtering**: Excludes cancelled tasks from date-based queries

### New Features
1. **Smart Daily View**: Returns active tasks within date range plus ongoing tasks from before the range
2. **Priority System**: Full priority management with update and query capabilities
3. **Activity Tracking**: Complete audit trail of all task changes and user comments
4. **Enhanced Data Model**: Added timestamps and activity history to all tasks

## Response Format

All API responses follow this format:
```json
{
   "data": {...},
   "pagination": null,
   "status": {
       "code": 200,
       "message": "Success"
   }
}
```

## Error Handling

The application includes comprehensive error handling:
- Resource not found (404)
- Bad request (400)
- Internal server error (500)

All errors return consistent response format with appropriate HTTP status codes.

## Development Notes

- Uses in-memory storage for simplicity (no database required)
- Includes seed data for testing
- All activities are automatically logged
- Thread-safe implementation using ConcurrentHashMap
- Follows Spring Boot best practices 
