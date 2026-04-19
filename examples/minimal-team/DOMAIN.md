# Task Management API

## Mission
Build a task management REST API with team collaboration, authentication, and a deployment-ready Docker image.

## Sprint
- **Duration:** 5 days
- **Complexity:** M
- **Topology:** hierarchical (planner → workers → judge)

## Success Criteria
- docker build succeeds, python -m src.main starts without error
- pytest tests/ passes with 0 failures
- All code in project/src/<module>/, tests in project/tests/
- JWT authentication works end-to-end
- Task CRUD operations with proper validation
