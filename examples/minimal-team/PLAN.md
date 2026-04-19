# Sprint Plan

## Phase Overview
| Days | Phase | Focus |
|------|-------|-------|
| 1-2  | FOUNDATION | Models, auth, basic endpoints |
| 3    | GATE REVIEW | Judge evaluates, DevOps creates deployment stack |
| 4    | INTEGRATION | Fix failures, wire modules, harden |
| 5    | FINAL | All tests pass, docker builds, judge final review |

## Day 1: FOUNDATION
- **backend_dev:** Database models (User, Task, Team) + JWT auth module
- **devops_engineer:** Project scaffolding — pyproject.toml, src/main.py skeleton, Makefile
- NO frontend. NO admin panels. Backend API only.

## Day 2: FOUNDATION (continued)
- **backend_dev:** Task CRUD endpoints + input validation + tests
- **devops_engineer:** Dockerfile + docker-compose.yml + CI workflow
- Judge: spot-check code structure and test coverage

## Day 3: GATE REVIEW
- **judge:** Evaluate all modules — code quality, test coverage, API design
- **devops_engineer:** Wire all modules into main.py, verify docker build
- Fix any issues flagged by nightly tests

## Day 4: INTEGRATION
- **backend_dev:** Fix all test failures from nightly run, add missing tests
- **devops_engineer:** Health check endpoint, graceful shutdown, environment config
- Goal: pytest passes with 0 failures

## Day 5: FINAL
- Verify: make test passes (0 failures)
- Verify: docker build succeeds
- Verify: python -m src.main starts without error
- **judge:** Final 5-pillar evaluation
- README.md with setup instructions and API docs
