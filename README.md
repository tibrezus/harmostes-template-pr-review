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

## The decision-field split (the Configuration Contract)

The DECISION FIELDS — model, models windows, thinking, timeout, toolBudget,
maxFixes — are **not in this repo and never were in the chart base**: the
deployment decides them. Once this repo DELIVERS the CR (the per-env Flux
wiring), the config tier (`tibrezus/harmostes-config` overlays) patches the
decision fields onto it per environment — kustomize strategic merge, the
same values that rode the HelmRelease before. Model WINDOWS additionally
ride the AgentProfile (the decision surface, phase 1a).

## Drift guards

`plugins/` are vendored copies of the worker image's built-ins: CI diffs
them against the kernel repo at the pinned tag — an image-side edit without
the matching sync fails CI, and vice versa the kernel's own suite still
guards the originals. The chart keeps rendering its copy of the template
until the per-env flip; both sides' CI keeps the transition honest.
