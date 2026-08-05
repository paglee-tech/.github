# .github

Organization-wide defaults for `paglee-tech`.

Every repository in the organization inherits the issue templates and the
pull request template from here, unless it defines its own.

**This repository is public.** On the Free plan a private `.github` shares
nothing — the template chooser falls through to a blank issue. Keep it to
templates and conventions: anything committed here is world-readable.

```
.github/
  ISSUE_TEMPLATE/
    config.yml      template chooser settings
    work.yml        Feature, Chore or Task
    bug.yml         Bug
    epic.yml        Epic
  PULL_REQUEST_TEMPLATE.md
CONTRIBUTING.md     branching model, PR and issue conventions
```

Engineering conventions live in [CONTRIBUTING.md](CONTRIBUTING.md).
