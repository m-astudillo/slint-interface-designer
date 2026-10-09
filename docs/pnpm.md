# Package Management (pnpm)

The package manager for this project is **pnpm**. Always use `pnpm` for every dependency operation — never `npm` or `yarn`.

## Commands

| Operation | Command |
| --- | --- |
| Install dependencies | `pnpm install` |
| Add a dependency | `pnpm add <pkg>` |
| Add a dev dependency | `pnpm add -D <pkg>` |
| Remove a dependency | `pnpm remove <pkg>` |
| Run a script | `pnpm run <script>` (e.g. `pnpm dev`) |
| Run a one-off package binary | `pnpm exec <bin>` |
| Update dependencies | `pnpm update` |
| Check outdated | `pnpm outdated` |

## Rules

- `pnpm-lock.yaml` is the single source of truth for versions; commit it.
- `pnpm-workspace.yaml` controls workspace policy (build allowlist, release-age exclusions) — do not edit casually.
- Never install a dependency without prior user confirmation (see AGENTS.md).
- Any add/update/remove must be reflected in `docs/stack.md`.
