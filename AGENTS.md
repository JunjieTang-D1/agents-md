# AGENTS.md

> **IMPORTANT NOTICE:** This project is NOT production-ready. It is provided strictly for testing, experimentation, and educational purposes only. This code has NOT undergone any security review and is NOT intended for deployment in production environments or for use with real end-users. Use at your own risk. No guarantees are made regarding security, reliability, availability, or fitness for any particular purpose.

Five rules for multi-agent LLM coding teams. Derived from 40 controlled experiments across 13 configurations, 5 team topologies, 2 domains, and 2 model generations.

The evidence and reasoning behind each rule is in the companion blog post (link when published).

**Tradeoff:** These rules bias toward structure over autonomy. For simple tasks, a single agent is fine.

---

## 1. Fewer Agents, Not More

**Default to 3–5 agents. Add more only when measurement says you need to.**

In 36 baseline runs, 5-agent teams scored 14% higher than 10-agent teams on LLM-judged artifact quality (7.60 vs 6.64 on a 10-point scale). The gap widened with project complexity.

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

*Built from AgentCorp: 40 experiments, 13 configurations, 5 team topologies, 2 domains, 2 model generations. The full framework and raw run artifacts will be released open-source soon.*
