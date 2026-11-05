# renovate-config

Shared Renovate preset for all repositories. The policy and the app credentials are described in
[README.md](README.md).

- `default.json` is the preset and every repository extends it, so a change affects all of them. Validate
  with `npx --yes --package renovate -- renovate-config-validator --strict --no-global default.json renovate.json`.
- Later `packageRules` override earlier ones: general rules first, then Docker, GitHub Actions, npm and
  NuGet, and the special groups (Playwright, Velopack) last.
- Renovate runs as the Mend app, so there is no workflow to run it; this repository only has a lint workflow.
