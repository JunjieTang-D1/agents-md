# AGENTS.md

> **IMPORTANT NOTICE:** This project is NOT production-ready. It is provided strictly for testing, experimentation, and educational purposes only. This code has NOT undergone any security review and is NOT intended for deployment in production environments or for use with real end-users. Use at your own risk. No guarantees are made regarding security, reliability, availability, or fitness for any particular purpose.

Five rules for multi-agent LLM coding teams. Derived from 27 controlled experiments across 13 configurations, 5 team topologies, 2 domains, and 2 model generations.

The evidence and reasoning behind each rule is in the [companion blog post](https://junjietang.dev/blog/2026/five-rules-multi-agent-coding-teams/).

**Tradeoff:** These rules bias toward structure over autonomy. For simple tasks, a single agent is fine.

---

## 1. Fewer Agents, Not More

**Default to 3–5 agents. Add more only when measurement says you need to.**

In 18 baseline runs, 5-agent teams scored 14% higher than 10-agent teams on LLM-judged artifact quality (7.60 vs 6.64 on a 10-point scale). The gap widened with project complexity.

Why: a single agent holds the full project in its context window. Split that context across N agents and you're compressing understanding. Every delegation drops signal. Coordination overhead grows faster than output.

This is Amdahl's Law applied to agents — when more than half the work is serial (architecture, integration, review), adding agents adds overhead without proportional gain.

Ask yourself: *"Can one agent hold the full context?"* If yes, don't split it.

## 2. Shared Directory, Scoped Writes

**One repo. One `src/` tree. Each agent writes only to its own paths.**

Every configuration tested that used per-day folders or per-agent output directories produced zero deployable code. Every configuration that used a shared project directory with path-scoped isolation produced 18K–35K lines of working software.

Rules:
- All agents write to one shared `project/` directory.
- Each agent owns specific paths: `src/billing/` for the billing agent, `src/auth/` for auth.
- Cross-path writes are blocked at the tool layer. No agent modifies another agent's files.
- Shared contracts (event schemas, API types) live in `src/common/`, owned by the planner.

The test: `git log --name-only` should show each file touched by exactly one agent.

## 3. Test Every Night, Fix Every Morning

**Run automated tests after every sprint day. Inject failures into the next day's prompt.**

Without nightly tests, per-day quality in multi-day sprints swung from 4.0 to 9.0 — agents compound mistakes silently. With nightly tests, per-day quality held 7.0 to 10.0 across all runs.

Minimum nightly suite:
- Syntax check: every `.py` file parses.
- Import check: cross-module imports resolve.
- `pytest` run: all tests in `tests/` pass.
- Dockerfile check: valid structure, entry point exists.
- Hardcoded-values check: no account IDs, no pinned regions.

Day N+1's prompt must begin with: *"These tests failed yesterday. Fix them FIRST before writing new code."*

**Verify the harness.** In 2 of 3 of my runs, a path bug made pytest silently collect 0 items for the entire 10-day sprint. Agents received no real test feedback and still produced 92–95% pass rates — but the silent failure masked the problem for weeks. If the feedback loop can fail quietly, assume it will. Add a liveness assertion.

## 4. One Agent Owns Deployment

**Assign a dedicated DevOps agent. It owns Dockerfile, Makefile, CI, and the entry point.**

Without a DevOps agent: zero deployment artifacts across 30+ experiments. Domain agents write domain code; none spontaneously produce a Dockerfile.

With a DevOps agent: Dockerfile, Makefile, `main.py`, `pyproject.toml`, and CI workflow appeared in every single run. 100% hit rate.

DevOps agent's scope:
- `Dockerfile`, `docker-compose.yml`
- `Makefile` with `test`, `lint`, `build`, `run` targets
- `src/main.py` — entry point that imports and wires all modules
- `pyproject.toml` — dependencies, pytest config
- `.github/workflows/ci.yml`

No other agent touches these. DevOps reads the project structure each day and builds the glue.

## 5. Run It Twice

**One run is noise. Two runs is a data point. Report variance.**

Same configuration, same model — two runs of a 10-agent team produced: Run 1, 55 test errors; Run 2, zero errors. If only Run 2 had been published, the result would have looked flawless. Both runs were real.

Rules:
- Run every configuration at least twice (N≥2); N=3 is better.
- Pre-register evaluation criteria before seeing results.
- Report mean and variance. Don't cherry-pick the good run.
- If runs disagree materially, that *is* the finding.

---

**These rules are working if:** every run produces a repo you can `git clone && make build && docker run`, test counts are stable across runs, and you're not debugging coordination failures between agents.

**These rules fail if:** your task fits in a single agent's context window. Then just use one agent and skip the overhead.

---

## 6. Define Your Team in Markdown, Not Code

**One directory per domain. Five markdown files. Zero code changes to add a new team.**

The first five rules tell you *what* works. This section tells you *how* to configure it. A **domain pack** is a directory with five files that fully define a multi-agent team and its project:

```
domains/your-project/
  DOMAIN.md    # Mission, context, sprint config, success criteria
  TEAM.md      # Agents: roles, models, write scopes, responsibilities
  STORIES.md   # User stories with acceptance criteria
  RULES.md     # Coding rules per module path
  PLAN.md      # Daily plan — phases and per-day assignments
```

Creating a new domain = copy the template directory + edit the markdown. No Python, no YAML escaping, no config parser changes.

**Why markdown, not YAML/JSON:**
- Humans read it naturally. LLMs parse it natively.
- Diffs are reviewable. PRs make sense.
- No escaping hell for multi-line strings.
- Extensible — add sections without breaking parsers.

**TEAM.md** is where the five rules become concrete:

```markdown
# Team Configuration

## Defaults
- **Planner model:** opus
- **Worker model:** sonnet
- **Judge model:** opus

## chief_planner
- **Role:** Platform Architect
- **Type:** planner
- **Write Scope:** docs/, planning/
- **Reports To:** none
- **Responsibilities:**
  - Decompose project into phases and assign work
  - Track cross-agent dependencies
  - Review judge feedback and adjust plan

## backend_dev
- **Role:** Backend Engineer
- **Type:** worker
- **Write Scope:** src/backend/, tests/test_backend.py
- **Reports To:** chief_planner
- **Responsibilities:**
  - Implement API endpoints and business logic
  - Write unit and integration tests

## devops_engineer
- **Role:** DevOps & Deployment Engineer
- **Type:** worker
- **Write Scope:** Dockerfile, Makefile, src/main.py, pyproject.toml, .github/
- **Reports To:** chief_planner
- **Responsibilities:**
  - Own all deployment artifacts (Rule 4)
  - Wire modules into entry point

## judge
- **Role:** Quality Assurance Judge
- **Type:** judge
- **Write Scope:** docs/GATE_REVIEW.md, docs/FINAL_REVIEW.md
- **Responsibilities:**
  - Evaluate at mid-sprint and end-of-sprint
  - Score each agent on 5 pillars
```

Notice how Rules 1–5 are encoded directly:
- 4 agents (Rule 1)
- Write scopes enforce isolation (Rule 2)
- PLAN.md injects nightly test failures (Rule 3)
- DevOps agent owns deployment (Rule 4)
- Run the domain pack N≥2 times (Rule 5)

---

*Built from AgentCorp: 27 experiments, 13 configurations, 5 team topologies, 2 domains, 2 model generations. The full framework and raw run artifacts will be released open-source soon.*
