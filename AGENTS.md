# renovate-config

Shared Renovate preset that every repository in the workspace extends (`local>psychodracon/renovate-config`),
run by the hosted Mend Renovate app; no workflow runs Renovate. The policy and the app credentials are
described in [README.md](README.md).

## Commands

```sh
npx --yes --package renovate -- renovate-config-validator --strict --no-global default.json renovate.json
```

CI runs the same validation.

## Conventions

- There are no releases: `default.json` on `main` applies to every repository on its next Renovate run.
- Later `packageRules` override earlier ones. Order: general rules, then Docker, GitHub Actions, npm,
  NuGet, and the version-sync groups (Playwright, Velopack) last.
- Every rule has a `description` that states the policy in one sentence.

## Boundaries

- **Ask first:** loosening auto-merge rules or the 3-day release age; anything that changes behaviour for
  all repositories at once.
- **Never:** split the Playwright or Velopack groups; their versions must update together.
