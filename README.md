# agents-md

> **IMPORTANT NOTICE:** This project is NOT production-ready. It is provided strictly for testing, experimentation, and educational purposes only. This code has NOT undergone any security review and is NOT intended for deployment in production environments or for use with real end-users. Use at your own risk. No guarantees are made regarding security, reliability, availability, or fitness for any particular purpose.

**Five rules for multi-agent LLM coding teams.** Derived from 40 controlled experiments.

📄 **The rules:** [AGENTS.md](./AGENTS.md)
📝 **The evidence:** companion blog post (link when published)

---

## The Rules

1. **Default to 3–5 agents.** Adding more hurts more than it helps.
2. **One shared project directory with per-agent write scopes.** Per-agent folders produce zero deployable code.
3. **Nightly integration tests with failure injection.** Silent feedback loops mask everything.
4. **One dedicated DevOps agent owns the deployment artifacts.** Nobody else writes the Dockerfile.
5. **N≥2 runs per configuration, report variance.** One run is a story, not a result.

Full rules with reasoning, data, and failure modes: [AGENTS.md](./AGENTS.md).

## Why this exists

Most multi-agent coding demos are indistinguishable from each other. A team of LLMs builds something. Screenshots get posted. Nobody tries to deploy it.

The field doesn't lack demos. It lacks *operating rules* — the load-bearing decisions that separate teams that produce deployable software from teams that produce interesting-looking artifacts. `AGENTS.md` is an attempt at those rules, grounded in controlled experiments rather than anecdote.

## How to use

### In your own project

1. Adopt the five rules as-is, or fork them.
2. Copy [AGENTS.md](./AGENTS.md) to the root of your multi-agent project.
3. Enforce the rules at the tool layer (write scopes, nightly tests, DevOps ownership), not just by prompt convention.

### As a contribution

Propose Rule 6. Add evidence that contradicts a rule. Report variance from your own experiments. See [CONTRIBUTING.md](./CONTRIBUTING.md).

## Citation

If `AGENTS.md` informs your work, please cite it. See [CITATION.cff](./CITATION.cff) for the format, or use:

```
Tang, J. (2026). AGENTS.md: Operating Rules for Multi-Agent LLM Coding Teams.
https://github.com/JunjieTang-D1/agents-md
```

## License

[MIT licensed](./LICENSE). Fork, adapt, and republish freely. Attribution appreciated but not legally required.

## Related

- **Companion blog post:** Five Rules for Multi-Agent Coding Teams (link when published)
- **Experimental framework (AgentCorp):** to be released open-source soon
