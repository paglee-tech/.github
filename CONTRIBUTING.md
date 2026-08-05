# Contributing

Engineering conventions for every repository under `paglee-tech`.

## Branching model

Release branching — Gitflow without the merge-back complexity that squash
merges break.

```
main                    production
develop                 integration
feat/123-slug           branches from develop, squash merges into it
fix/123-slug            idem
chore/123-slug          idem
release/1.4.0           branches from develop, merges into main + tag
hotfix/1.4.1            branches from main, merges into main + develop
```

| Prefix | Purpose | Suffix |
|---|---|---|
| `feat/` | New functionality | issue number + slug |
| `fix/` | Bug fix | issue number + slug |
| `chore/` | Maintenance, infra, tooling | issue number + slug |
| `release/` | Release stabilization | semantic version |
| `hotfix/` | Emergency production fix | semantic version |

Examples:

```
feat/142-pix-webhook
fix/187-checkout-timeout
chore/93-upgrade-node-24
release/1.4.0
hotfix/1.4.1
```

`release/` and `hotfix/` carry a version, not an issue number — they cover a
batch of work rather than a single item.

## Release flow

1. Work items reach `Ready to Deploy` on the board.
2. Cut `release/x.y.z` from `develop` with everything in that column.
3. Validate, then merge into `main` and tag `vx.y.z`.
4. Move the included issues to `Done`.

Hotfixes skip this: branch from `main`, fix, merge into `main` **and**
`develop` so the fix is not lost on the next release.

### Fixing a bug found during release validation

Never commit directly on `release/*` — a release branch does not merge back
into `develop`, so the commit would be lost on the next release. Fix it on
`develop` first, then cherry-pick:

```
git switch develop && git switch -c fix/456-slug
# fix, open the pull request, squash merge into develop
git switch release/x.y.z
git cherry-pick <sha-from-develop>
```

## Pull requests

- Target `develop` — never `main` directly, except `release/*` and
  `hotfix/*`.
- Title follows [Conventional Commits](https://www.conventionalcommits.org):
  `type(scope): description`. The squash merge uses it as the commit message,
  so the history in `develop` stays readable without demanding
  commit-level discipline inside the branch.
- Link the issue with `Closes #123`.
- **Squash merge into `develop`.** Each pull request becomes a single
  commit, titled after the pull request.
- **Merge commit into `main`.** `release/*` and `hotfix/*` merge with their
  full history — the already-squashed commits are preserved.

Rebase merge is disabled. The choice between squash and merge commit is a
convention: branch protection requires a paid plan, so nothing enforces it
mechanically today.

**Never write `Closes #` on a `release/*` pull request.** Merging one moves
every linked item back to `Test`, undoing the `Ready to Deploy` status the
release was cut from. Release pull requests reference issues without closing
keywords.

After merging a `hotfix/*`, move its item to `Done` by hand — the automation
sets it to `Test`, which is wrong for a fix that is already in production.

Allowed types: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `perf`,
`build`, `ci`, `revert`.

## Issues

Issue Type (`Epic`, `Feature`, `Chore`, `Task`, `Bug`) and `Priority` are set
in the issue sidebar — issue forms cannot set them automatically.

Hierarchy uses native sub-issues:

```
Epic
 └── Feature / Chore / Bug
      └── Task
```

**Feature vs. Chore:** ships something new and visible to the user or the
system → Feature. Maintenance or infrastructure that does not change observed
behaviour → Chore.

Labels are intentionally unused. Type, priority and status live in Issue
Types, Issue Fields and the project board.
