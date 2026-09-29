# pi-agent

An ephemeral native Pi agent runtime as a charly layer — the Pi coding agent,
its TUI, and a thin runner built on Charly's agent control plane.

The `pi-agent` candy pins `@earendil-works/pi-coding-agent` and `pi-tui` at
`0.80.10` and installs a runner built on `createAgentSessionRuntime`,
`SessionManager`, and Pi's own `runRpcMode` JSONL transport. It has **no
service, listener, supervisor, or boot-time process**: one runner exists only for
an active Charly run. When the separately installed official Pi orchestrator CLI
is present, the runner delegates orchestrated streams to that CLI without copying
or translating its protocol. It also installs a `pi-tui` controller that renders
durable Charly sessions/events and invokes only the typed `charly agent` API.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `pi-agent` |
| Requires | `layer-nodejs` |
| Pinned packages | `@earendil-works/pi-coding-agent` 0.80.10, `@earendil-works/pi-tui` 0.80.10 |
| Runners | `charly-pi-agent-runner`, `charly-agent-tui` |
| Providers | `pi` (structured), `tmux` (terminal, profile `pi`) |
| Volume | `sessions` → `~/.pi/agent/sessions` |
| Service / port | none — no boot-time process |

## How to use it

Compose the layer in a box that drives the agent control plane:

```yaml
check-agent-box:
  candy:
    base: arch
    candy:
      - '@github.com/opencharly/layer-pi-agent:v2026.239.1638'
```

The runner is then reachable through `charly agent` (structured) and the `pi`
terminal profile (terminal).

## Layout

- `charly.yml` — the `pi-agent:` candy entity: the `require:` dep, the `volume:`
  declaration, the `agent_provide:`/`terminal_profile:` blocks, and the `plan:`
  `run:`/`check:` steps.
- `pi-agent-runner.mjs` — the native runner entrypoint.
- `charly-agent-tui.mjs` — the Pi-TUI controller.
- `package.json` — the pinned npm package spec.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Closest family skill: `/charly-automation:agent` — the nearest owning procedure; this
  repo carries no `skill:` entity of its own.
- `/charly-automation:agent` — the Charly agent control plane, Pi native/
  orchestrator/TUI modes.
- `/charly-automation:tmux` — the terminal-profile transport.
- `/charly-image:layer` — candy authoring reference.
- [`opencharly/opencharly](https://github.com/opencharly/opencharly) — the umbrella.
