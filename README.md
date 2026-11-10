# renovate-config

The shared [Renovate](https://docs.renovatebot.com/) preset for the psychodracon repositories: when
dependency update pull requests are opened, which ones merge themselves once CI passes, and which wait
for a person. Renovate runs as the hosted [Mend Renovate app](https://developer.mend.io/); this
repository only holds the policy.

## Requirements

- The Mend Renovate app installed on the repositories, from
  [GitHub Marketplace](https://github.com/apps/renovate).
- To look up the private `@psychodracon` npm packages and `ghcr.io/psychodracon` images, a classic
  GitHub personal access token with `read:packages`.

## Installation

1. In the [Mend portal](https://developer.mend.io/), under **Settings → Credentials**, add a host rule
   with the token for both `npm.pkg.github.com` and `ghcr.io`. Without it, lookups of the private
   packages and images fail and show up on each repository's Dependency Dashboard.
2. In each repository, add `renovate.json`:

   ```json
   {
     "$schema": "https://docs.renovatebot.com/renovate-schema.json",
     "extends": ["local>psychodracon/renovate-config"]
   }
   ```

3. A repository that uses `@psychodracon` npm packages also commits an `.npmrc` with
   `@psychodracon:registry=https://npm.pkg.github.com`, which Renovate reads for lookups.

Changes to `default.json` apply to every repository on its next Renovate run; there are no releases.

## Usage

Routine updates are opened on Sundays between 07:00 and 10:00 (Europe/Warsaw), once a release is
three days old. Vulnerability fixes skip both and are opened at any time. Commit messages are
conventional (`chore(deps): ...`), and pull requests merge themselves with a merge commit once CI
passes.

| Update | Handling |
| --- | --- |
| Docker digest and pin | grouped as `Docker digests`, automerged |
| GitHub Actions minor, patch, pin and digest | grouped as `GitHub Actions`, automerged |
| npm patch and pin | grouped, automerged |
| npm dev dependency minor | grouped, automerged |
| NuGet patch and pin | grouped, automerged |
| Docker minor and patch | grouped as `Docker images`, manual merge |
| npm and NuGet minor | grouped per ecosystem, manual merge |
| Major updates | one pull request each, manual merge |
| `Velopack` and `vpk` | always one group, manual merge |
| Playwright image and npm package | always one group, manual merge |

Pull requests that need a manual merge are assigned to `psychodracon`, so they show up in
notifications; automerged ones are not.

Third-party GitHub Actions are pinned to a commit SHA with the version as a comment
(`uses: owner/action@<sha> # v1.2.3`), because a tag can be moved to other code without a new
release. The psychodracon actions and workflows stay on their moving `@v1` tag.

## Development

`default.json` is the preset; `renovate.json` is this repository's own configuration. Validate both
before pushing, as CI does:

```sh
npx --yes --package renovate -- renovate-config-validator --strict --no-global default.json renovate.json
```

## License

Copyright (C) Kamil Ganczarek. Licensed under the GNU General Public License, version 3 or (at your
option) any later version. See [LICENSE](LICENSE).
