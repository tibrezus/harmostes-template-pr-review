# harmostes-template-pr-review

The pr-review **template repo** — the hottest pipeline (three-tier phase
3b, ADR-0011 amendment). One repo owns the adversarial review pipeline:
the WorkflowTemplate CR, the agent's task text, and vendored copies of the
plugins its nodes run.

```
template.yaml      the WorkflowTemplate CR (spec.source → THIS repo; #787)
tasks/pr-review.txt  the agent's environment briefing (verbatim from the chart)
plugins/           vendored copies of the image built-ins this template runs
```

## The decision fields (three-tier end state)

- **model / models windows**: NOT here — the environment's **AgentProfile**
  decides (profile windows → template fallback, phase 1a). Dev and prod run
  different engines; the repo cannot pin one.
- **thinking / timeout / toolBudget / maxFixes**: the template's own BASE,
  carried here (this repo is owner-private versioned content — the chart-era
  "deployment decides via values" contract governed a PUBLIC chart default
  and retires with the chart's template rendering at phase 4). Per-env
  divergence rides the field-wise INSTANCE merge (instance wins, the
  day-one rule) until AgentProfile grows per-template overlays.
- **sessions.pvc**: the base names the lineage claim; deployments override
  per instance if theirs differs.

## Drift guards

`plugins/` are vendored copies of the worker image's built-ins: CI diffs
them against the kernel repo at the pinned tag — an image-side edit without
the matching sync fails CI, and vice versa the kernel's own suite still
guards the originals. The chart keeps rendering its copy of the template
until the per-env flip; both sides' CI keeps the transition honest.
