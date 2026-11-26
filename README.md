<img src="./.github/assets/readme-header.svg" alt="whatsaaaa" width="100%" />

# .github

The default community health files and the shared Renovate preset of the [whatsaaaa](https://github.com/whatsaaaa) repositories.

## What is in it

| File | What it is |
| --- | --- |
| [`SECURITY.md`](./SECURITY.md) | How to report a vulnerability privately, and which versions get fixes |
| [`CONTRIBUTING.md`](./CONTRIBUTING.md) | How to set up, check and open a pull request |
| [`SUPPORT.md`](./SUPPORT.md) | Where questions go |
| [`.github/ISSUE_TEMPLATE/`](./.github/ISSUE_TEMPLATE) | The bug report and feature request forms. Blank issues are off, and a link points to the security policy |
| [`.github/pull_request_template.md`](./.github/pull_request_template.md) | What changed and why, how it was checked, and the changeset |
| [`default.json`](./default.json) | The Renovate preset |

GitHub shows these files in every repository of the account that has none of its own, private ones included. A repository's own file of a kind replaces the default, and a repository with its own `.github/ISSUE_TEMPLATE/` folder uses none of the default issue forms. GitHub cannot default a license, so every repository keeps its own `LICENSE`.

## The Renovate preset

A repository uses it with a `renovate.json` of one line:

```json
{ "extends": ["github>whatsaaaa/.github"] }
```

What it does, on top of Renovate's `config:recommended`:

- **Updates come once a week.** Every minor, patch, pin and digest update of a repository comes in one "all non-major dependencies" pull request, raised early on Monday (before 6 AM, Europe/Belgrade). Major updates come on the same schedule, each in a pull request of its own. Outside that window Renovate leaves its branches alone, so each pull request runs CI once a week at most.
- **Branches are rebased only when they conflict.** A merge to `main` does not rebase every open Renovate pull request and run its CI again. The rebase checkbox in a pull request still rebases it on demand.
- **Updates wait a day.** Renovate raises a new version once it is a day old, as pnpm only installs packages that are a day old.
- **The `@whatsaaaa/*` packages do not wait.** They come in one group, at any time, as soon as they are published, which matches the `minimumReleaseAgeExclude: ['@whatsaaaa/*']` of each repository's `pnpm-workspace.yaml`.
- **Template updates come from Copier.** The `copier` manager reads `.copier-answers.yml` and opens a `copier update` pull request for each new tag of [`whatsaaaa/template`](https://github.com/whatsaaaa/template), outside the weekly group and at any time. They do not wait either: the template is ours, and its git tags carry no release time.
- **Security fixes do not wait for Monday.** Renovate raises a fix for a vulnerable version right away, in a pull request of its own.
- **The template's workflows move with the template.** Renovate leaves `whatsaaaa/template` refs in workflows (`@v1`) alone: a new major comes with `copier update`, together with the callers that use it, and fixes reach every repository through the floating tag.
- **Third-party actions are pinned by commit SHA,** with the version in a comment, such as `actions/checkout@<sha> # v7.0.0`. Renovate keeps both up to date.
- **Container images outside Docker Hub do not wait.** Only Docker Hub reports when an image was pushed, so an image like the Playwright one is raised right away instead of never.
- **Nothing merges on its own.** Every update is a pull request to review.

Renovate needs its GitHub app installed on the account for any of this to run.

## Changing things

Every repository reads these files live from `main`, so a change reaches all of them as soon as it merges. Check the preset before you push:

```sh
npx --yes --package renovate -- renovate-config-validator --strict --no-global default.json
```

## License

[MIT](./LICENSE)
