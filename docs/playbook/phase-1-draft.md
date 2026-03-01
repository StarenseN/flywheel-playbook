---
icon: lucide/file-text
---

# Phase 1 -- Draft the Plan

Produce a single, detailed markdown plan that is agent-operable -- not just human-readable.

---

Use a web app with strong reasoning enabled (ChatGPT Pro with extended thinking, or Claude with extended thinking). The web interface is ideal because extended reasoning is cheap relative to code generation tokens.

!!! important
    A human-readable plan says "implement authentication." An agent-operable plan says "implement JWT-based authentication using RS256, with refresh token rotation, 15-minute access token TTL, stored in httpOnly cookies, with a `/auth/refresh` endpoint that validates the refresh token and issues a new pair."

## Plan Drafting Prompt

```
You are a senior architect and staff engineer. We are starting a greenfield project:

Project: <PROJECT_NAME>
Users: <TARGET_USERS>
Problem: <PROBLEM_STATEMENT>
Constraints: <CONSTRAINTS>
Environment: <LANGUAGE/STACK/PLATFORM>

Write a single, extremely detailed markdown plan we can implement with coding agents.

Requirements:
- Start with Goals / Non-goals.
- Include a clear architecture section: components, boundaries, invariants, data flow.
- Include data model / schemas (as needed).
- Include security & privacy model (threats, mitigations, secrets handling).
- Include performance targets (with concrete numbers) and an instrumentation plan.
- Include error handling, retries, and "no silent fallback" philosophy.
- Include a test plan: unit + integration + e2e (with detailed logging expectations).
- Include rollout plan: feature flags, migrations, backwards compatibility (if relevant).
- Include a task breakdown section that could be converted into a dependency graph.

Be explicit and operational. Avoid vague advice.
Assume this plan will be executed by multiple parallel agents.
```

Save the result as your canonical plan file (e.g., `PLAN.md` in repo root). The first output is always bad. That's not a failure, that's a starting point.

!!! important
    **Stop condition:** You have a comprehensive first draft.
