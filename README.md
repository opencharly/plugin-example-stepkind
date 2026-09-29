# plugin-example-stepkind

The reference **external install-step kind** plugin (`step:examplestepkind`) — a
plugin that contributes a first-class install-step KIND whose contract it declares
itself.

The plugin declares its step's `Scope` / `Venue` / `Gate` and `Emits=true` in its
`Describe` `StepContract`; the host carries the step **opaquely**
(`external:examplestepkind` + `Payload`) through the InstallPlan IR. It proves both
legs:

- **DEPLOY leg (`OpExecute`)** — the host's open default arm dispatches the step's
  `OpExecute` over the E3b reverse channel, where the plugin writes a venue marker
  and returns a teardown `ReverseOp` the host records and replays
  (record-and-replay).
- **BUILD leg (`OpEmit`)** — the plugin declares `Emits=true` and answers `OpEmit`
  with a Containerfile `RUN` fragment; composed into a pod overlay (`add_candy`),
  the open external-step arm splices it, baking
  `/etc/examplestepkind-build-baked` into the overlay image.

It is the step-class companion of the verb-class `candy/plugin-example-external`
and the deploy-class `candy/plugin-example-deploy`.

## What it provides

| Capability | Surface |
|---|---|
| `step:examplestepkind` | the `examplestepkind` install-step kind — declared `StepContract` (user scope, host-native venue, no gate, `Emits=true`) |

## How to use it

Compose the plugin candy in a box or check bed's `candy:` list, then author the
step:

```yaml
- '@github.com/opencharly/plugin-example-stepkind/candy/plugin-example-stepkind:<tag>'
```

The plugin's own ADE plan is a build-context check that the out-of-tree module is
present and buildable; the deploy and build-emit legs are exercised by the pod
overlay bed that composes this step via `add_candy`.

## Layout

- `candy/plugin-example-stepkind/` — the plugin module: `plugin.go` (the provider
  + `NewProvider()`/`NewMeta()` + the `StepContract` and the
  `OpEmit`/`OpExecute` dispatch), `schema/examplestepkind.cue` (the self-contained
  `#ExamplestepkindInput`), `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-internals:plugin` — the plugin/provider model, including
  the `step` class and the external step-kind contract. This candy carries no
  `skill:` entity of its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-internals:install-plan` — the InstallPlan IR that carries the opaque
  step payload.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
