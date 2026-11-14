# Hello World — Test Generation & Maintenance Route (`R63-T032`)

## Summary

- **Exercise:** `exercises/practice/hello-world`
- **Track Architecture:** In the Exercism Go track, generator-driven exercises use `.meta/gen.go` executed by `bin/run-generators`. `hello-world` does **not** have a `.meta/gen.go` template/generator script; its test file `hello_world_test.go` is maintained by hand.
- **Canonical Synchronization:** `configlet sync` manages canonical case metadata in `.meta/tests.toml` from `problem-specifications`.

## Exact Generation & Synchronization Commands

To synchronize canonical test metadata from `problem-specifications`:

```bash
# Sync canonical tests into .meta/tests.toml (using cached problem-specifications)
./bin/configlet sync --tests include --exercise hello-world --update --yes --offline

# Full online sync (fetches latest problem-specifications if needed)
./bin/configlet sync --tests include --exercise hello-world --update --yes
```

To run the exercise test suite against the reference implementation:

```bash
# Inside a scratch copy with .meta/example.go copied to hello_world.go:
go test -v -count=1 ./...
```

## Input Files

1. `problem-specifications/exercises/hello-world/canonical-data.json`
   - Upstream canonical test definition containing UUID `af9ffe10-dc13-42d8-a742-e7bdafac449d`.
2. `exercises/practice/hello-world/.meta/tests.toml`
   - Track-level test configuration tracking inclusion status and rationale comments.
3. `exercises/practice/hello-world/.meta/config.json`
   - Exercise file mappings (`files.solution`, `files.test`, `files.example`).
4. `exercises/practice/hello-world/hello_world_test.go`
   - Maintained Go test file containing `TestHelloWorld` and `BenchmarkHelloWorld`.

## Case-to-Output Mapping

| UUID | Canonical Description | Property | Output Test Function | Source / Route | Status |
| --- | --- | --- | --- | --- | --- |
| `af9ffe10-dc13-42d8-a742-e7bdafac449d` | `Say Hi!` | `hello` | `TestHelloWorld` in `hello_world_test.go` | Canonical case from `canonical-data.json`; synced to `tests.toml`; hand-maintained test verifying `HelloWorld() == "Hello, World!"`. | Included |

### Additional Test Symbols

- `BenchmarkHelloWorld` in `hello_world_test.go`: Track-specific benchmark function measuring execution performance (`b.Loop()` / `HelloWorld()`), maintained alongside the canonical unit test.

## Regeneration & Reproducibility

- Two fresh temporary runs of the synchronization workflow yield identical `hello_world_test.go` files (0 diff).
- No canonical cases are dropped.
- Manual comments added via `comment = "..."` keys in `.meta/tests.toml` are preserved across `configlet sync` runs.
