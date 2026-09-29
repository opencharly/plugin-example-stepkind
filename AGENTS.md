# AGENTS.md — plugin-example-stepkind

Standalone plugin repo for the `examplestepkind` capability
(`step:examplestepkind`) — the reference external install-step-kind plugin (F3).
The plugin is a Go module at `candy/plugin-example-stepkind/` (module path
`github.com/opencharly/plugin-example-stepkind/candy/plugin-example-stepkind`);
the root `charly.yml` only declares `discover: candy` so the repo is a project and
its candy is scanned.

Canonical files:

- `candy/plugin-example-stepkind/charly.yml` — the `plugin-example-stepkind:`
  candy entity (`plugin:` block, `plan:` check).
- `candy/plugin-example-stepkind/plugin.go` — the provider (`NewProvider()` +
  `NewMeta()` with the declared `StepContract`) and the `OpEmit` / `OpExecute`
  dispatch.
- `candy/plugin-example-stepkind/schema/examplestepkind.cue` — the self-contained
  `#ExamplestepkindInput`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model (incl. the `step` class and the declared
  `StepContract`), the per-plugin CUE-schema contract, placement.
- `/charly-internals:install-plan` — the InstallPlan IR that carries the opaque
  external-step payload, and the `OpEmit` build-emit seam.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-example-stepkind/` — compile the plugin module.
- `go test ./...` in `candy/plugin-example-stepkind/` — the plugin's Go tests.
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.

## Modify this repo

- Edit the `plugin-example-stepkind:` candy entity, the Go source, and
  `schema/examplestepkind.cue` **together**.
- The declared `StepContract` (scope/venue/gate/`Emits`) is the contract the host
  carries opaquely; keep it in step with the step's actual behavior. Both legs —
  deploy `OpExecute` (venue marker + teardown reverse op) and build `OpEmit`
  (Containerfile fragment) — must keep working.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
