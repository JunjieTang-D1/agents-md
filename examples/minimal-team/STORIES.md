# User Stories

## US-001: Database schema and models
- **Points:** 3
- **Module:** models
- **Acceptance Criteria:**
  - SQLAlchemy models for User, Task, Team
  - Migration scripts or table creation
  - Indexes on frequently queried fields

## US-002: JWT authentication
- **Points:** 5
- **Module:** auth
- **Acceptance Criteria:**
  - Register new user with email/password
  - Login returns JWT access + refresh tokens
  - Protected endpoints reject invalid/expired tokens
  - Password hashing with bcrypt

## US-003: Task CRUD endpoints
- **Points:** 3
- **Module:** api
- **Acceptance Criteria:**
  - POST /tasks creates task (title, description, priority)
  - GET /tasks lists with pagination and filtering
  - PUT /tasks/:id updates task fields
  - DELETE /tasks/:id soft-deletes
  - Input validation on all endpoints
