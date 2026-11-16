# Contributing

Ideas, bug reports and pull requests are welcome. For a larger change, open an issue first, so we can agree on the approach before you write the code.

## Getting started

The repository's README says what it needs and lists its commands. Most of these repositories are TypeScript on pnpm: you need Node.js and pnpm, and if you don't have pnpm yet, `corepack enable` sets it up.

```sh
pnpm install
```

## Before you open a pull request

- Run `pnpm lint`, `pnpm type-check` and `pnpm test`.
- Add a changeset with `pnpm changeset` if you changed a published package.
- Sign your commits. The default branch only takes signed commits.

The pull request template asks what changed and why, and how you checked it.
