# plugin-example-stepkind

The `plugin-example-stepkind` plugin candy of the [opencharly/charly](https://github.com/opencharly/charly)
candy library, as a standalone repo (the candy de-submodule cutover, plugin
kind). The Go module lives at `candy/plugin-example-stepkind/` with module path
`github.com/opencharly/plugin-example-stepkind/candy/plugin-example-stepkind`; the charly resolver fetches this repo at the pinned tag and
the compiled-in wiring imports the module at that path.
