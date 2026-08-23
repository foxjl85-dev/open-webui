# Fork notes

This repository is a patch-staging fork of [`open-webui/open-webui`](https://github.com/open-webui/open-webui), not a product rewrite. The fork's default `main` was inspected at `01f4282f1` and is identical to the upstream `main` commit at that point.

## Local delta

- `dev` carries the upstream-authored July 27 `refac` stack that was present in this fork.
- `fix/27698-a11y-dev` adds three fork-authored accessibility commits on top of that `dev` stack:
  - sidebar controls remain independently interactive;
  - folder and dropdown controls use explicit labels and stop row-level event propagation;
  - skip-link, rich-text, model-information, list, and contrast semantics are preserved.
- `fix/b77292-a11y` carries the same three accessibility commits on top of the fork's `main` line rather than the `dev` stack.

The A9 hardening branch is based on `fix/27698-a11y-dev` so its validation stays attached to the latest local patch stack. `src/lib/components/layout/sidebar-a11y.test.ts` covers the patch's markup and compile-time invariants, including nested folder controls and section-action event isolation.

## CI hardening

The frontend workflow previously invoked the write-mode `npm run format` script as a verification step, and the formatter configuration still carried the obsolete `--plugin-search-dir`/`pluginSearchDirs` settings. The formatter scripts now use Prettier's explicit plugin configuration, a non-mutating `format:check` script backs the workflow, and the build job uses `npm ci --force` so the checked-in lockfile is the dependency source of truth.

## Sync policy

Keep this fork patch-oriented. Future upstream movement should be an explicit, reviewable rebase or cherry-pick of the intended patch stack. Do not merge an upstream history dump into the fork, and do not place secrets or environment-specific configuration in the fork.
