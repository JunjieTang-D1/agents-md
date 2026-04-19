# Coding Rules

## Global Rules
- ALL code MUST use write_project_file tool
- Read existing code before writing (read-before-write mandate)
- No hardcoded secrets or connection strings — use environment variables
- Type hints on all function signatures
- Docstrings on all public functions

## src/api/
- All endpoints must validate input with Pydantic models
- Error responses must use consistent JSON format
- Pagination must use cursor-based or offset/limit pattern

## src/auth/
- Passwords must be hashed (never stored in plaintext)
- JWT tokens must have configurable expiration
- Failed login attempts must be rate-limited or logged

## src/models/
- All models must have created_at and updated_at timestamps
- Soft delete (is_deleted flag) instead of hard delete
- Foreign keys must have appropriate ON DELETE behavior

## tests/
- Test names: test_<what>_<condition>_<expected>
- Use fixtures from conftest.py
- Integration tests in tests/integration/
- Include pytest timeout: timeout = 30, timeout_method = signal
