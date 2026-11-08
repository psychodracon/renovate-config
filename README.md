# Renovate configuration

Shared Renovate policy for the psychodracon repositories. Renovate itself runs as the
[Mend Renovate GitHub app](https://developer.mend.io/), installed on all repositories; this repository
only holds the policy.

- `default.json` - shared preset; repositories extend `local>psychodracon/renovate-config`
- `renovate.json` - this repository's own config

## Policy

Routine updates are opened on Sundays between 07:00 and 10:00 (Europe/Warsaw) after a 3-day maturity
wait. Vulnerability fixes ignore both and are opened at any time. Commits are conventional
(`chore(deps): ...`) and automerges use merge commits once CI passes.

| Update | Handling |
| --- | --- |
| Docker digest and pin | grouped as `Docker digests`, automerged |
| GitHub Actions minor, patch, pin and digest | grouped as `GitHub Actions`, automerged |
| npm patch and pin | grouped, automerged |
| npm dev dependency minor | grouped, automerged |
| NuGet patch and pin | grouped, automerged |
| Docker minor and patch | grouped as `Docker images`, manual merge |
| npm and NuGet minor | grouped per ecosystem, manual merge |
| Major updates | one PR each, manual merge |
| `Velopack` and `vpk` | always one group, manual merge |
| Playwright image and npm package | always one group, manual merge |

PRs that need a manual merge are assigned to `psychodracon`, so they show up in notifications;
automerged ones are not.

A repository that uses `@psychodracon/*` npm packages commits an `.npmrc` that maps the scope to
`https://npm.pkg.github.com`; Renovate reads it for lookups.

## App credentials

The app needs a host rule in the Mend portal (Settings -> Credentials) with a classic personal access
token that has `read:packages`, for both `npm.pkg.github.com` and `ghcr.io`. Without it, lookups of the
private `@psychodracon/*` packages and images fail and show up on the Dependency Dashboard.

## License

Copyright (C) Kamil Ganczarek. Licensed under the GNU General Public License, version 3 or (at your option) any later version. See [LICENSE](LICENSE).
