# AGENTS.md — layer-pi-agent

Standalone candy repo for the `pi-agent` layer — an ephemeral native Pi agent
runtime: the pinned Pi coding agent + TUI, a runner built on `charly agent`'s
typed API, and a `pi-tui` controller. The candy lives in `charly.yml` at the repo
root: the `require:` on `layer-nodejs`, the `volume:`, `agent_provide:` and
`terminal_profile:` declarations, and the `plan:` `run:`/`check:` steps.

This repo has **no `skill:` entity** in `charly.yml`, so there is no dedicated
owning skill projected into the marketplace corpus. The gap is recorded against
`opencharly/opencharly#291` (the batch that authors missing `skill:` entities).

Canonical files:

- `charly.yml` — the `pi-agent:` candy entity.
- `pi-agent-runner.mjs` — the native runner entrypoint.
- `charly-agent-tui.mjs` — the Pi-TUI controller.
- `package.json` — the pinned npm package spec.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-automation:agent` — the closest owning procedure: the Charly agent
  control plane, its Pi native/orchestrator/TUI modes, sessions, runs and
  terminal channels. Load before editing or troubleshooting.
- `/charly-automation:tmux` — the terminal-profile transport the `tmux` provider
  and the `pi` terminal profile use.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:` and `run:`, the `agent_provide:` /
  `terminal_profile:` / `volume:` blocks, and service declarations). Load before
  editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps assert the pinned package versions, both
  installed scripts (mode `0755`), and both PATH symlinks resolve to their
  targets.

## Modify this repo

- There is no `skill:` entity to keep in sync; if one is added (per #291), it
  must be edited together with the candy entity in the same change.
- The candy is **ephemeral by design**: no service, listener, supervisor, or
  boot-time process. Do not add one — a runner exists only for an active Charly
  run.
- Keep the pinned versions, the installed script modes, and the PATH symlinks in
  step: the checks assert exact versions (`0.80.10`) and symlink targets.
- New behaviour claims belong in the `plan:` as an observable `check:` step.

## Landing

- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo — read it
  before landing.
- Release history lives in `CHANGELOG/`.
