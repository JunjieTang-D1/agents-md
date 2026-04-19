# agents-md

> **IMPORTANT NOTICE:** This project is NOT production-ready. It is provided strictly for testing, experimentation, and educational purposes only. This code has NOT undergone any security review and is NOT intended for deployment in production environments or for use with real end-users. Use at your own risk. No guarantees are made regarding security, reliability, availability, or fitness for any particular purpose.

**Six rules for multi-agent LLM coding teams.** Derived from 27 controlled experiments across 13 configurations, 5 team topologies, 3 domains, and 2 model generations.

📄 **The rules:** [AGENTS.md](./AGENTS.md)
📝 **The evidence:** [Five Rules for Multi-Agent Coding Teams — Derived From 27 Controlled Experiments](https://junjietang.dev/blog/2026/five-rules-multi-agent-coding-teams/)
🏢 **The framework:** Coming soon

---

## The Rules

1. **Default to 3–5 agents.** Adding more hurts more than it helps.
2. **One shared project directory with per-agent write scopes.** Per-agent folders produce zero deployable code.
3. **Nightly integration tests with failure injection.** Silent feedback loops mask everything.
4. **One dedicated DevOps agent owns the deployment artifacts.** Nobody else writes the Dockerfile.
5. **N≥2 runs per configuration, report variance.** One run is a story, not a result.
6. **Define your team in Markdown, not code.** Five files per domain. Zero code changes to add a new team.

Full rules with reasoning, data, and failure modes: [AGENTS.md](./AGENTS.md).

## Why this exists

Most multi-agent coding demos are indistinguishable from each other. A team of LLMs builds something. Screenshots get posted. Nobody tries to deploy it.

The field doesn't lack demos. It lacks *operating rules* — the load-bearing decisions that separate teams that produce deployable software from teams that produce interesting-looking artifacts. `AGENTS.md` is an attempt at those rules, grounded in controlled experiments rather than anecdote.

## How to use

### Define your team

Copy [`examples/minimal-team/`](./examples/minimal-team/) and edit the five markdown files to match your domain. No code changes needed.

```
your-domain/
  DOMAIN.md    # What you're building
  TEAM.md      # Who's on the team (roles, write scopes, models)
  STORIES.md   # What they're building (user stories)
  RULES.md     # How they should write code (per-module rules)
  PLAN.md      # When they do it (daily phases)
```

See [Rule 6 in AGENTS.md](./AGENTS.md#6-define-your-team-in-markdown-not-code) for the format specification.

### In your own project

1. Adopt the six rules as-is, or fork them.
2. Copy [AGENTS.md](./AGENTS.md) to the root of your multi-agent project.
3. Create a domain pack for your project using the template above.
4. Enforce the rules at the tool layer (write scopes, nightly tests, DevOps ownership), not just by prompt convention.

### Tested domains

These domain packs have been tested in controlled experiments:

| Domain | Agents | Output | Runs |
|--------|:------:|--------|:----:|
| Physical AI Factory | 5 | 80 files, 31K LOC mean, 92–99% pytest pass | N=3 |

### As a contribution

Propose Rule 7. Add evidence that contradicts a rule. Report variance from your own experiments. See [CONTRIBUTING.md](./CONTRIBUTING.md).

## FAQ

**Q: Do I need AgentCorp to use these rules?**
No. The rules are framework-agnostic. Apply them to any multi-agent coding system — CrewAI, AutoGen, LangGraph, or your own.

**Q: Why markdown domain packs instead of YAML?**
Humans read markdown naturally. LLMs parse it natively. Diffs are reviewable. No escaping hell. See Rule 6 for the full rationale.

**Q: What if my task fits in one agent's context?**
Use one agent. These rules are for tasks that genuinely require specialization across multiple agents. Rule 1 exists specifically to push back on unnecessary splitting.

## Citation

If `AGENTS.md` informs your work, please cite it. See [CITATION.cff](./CITATION.cff) for the format, or use:

```
Tang, J. (2026). AGENTS.md: Operating Rules for Multi-Agent LLM Coding Teams.
https://github.com/JunjieTang-D1/agents-md
```

## License

[MIT licensed](./LICENSE). Fork, adapt, and republish freely. Attribution appreciated but not legally required.

## Related

- **Companion blog post:** [Five Rules for Multi-Agent Coding Teams](https://junjietang.dev/blog/2026/five-rules-multi-agent-coding-teams/)
- **Built with:** [AWS Strands Agents SDK](https://github.com/strands-agents/sdk-python) + [Amazon Bedrock](https://aws.amazon.com/bedrock/)
