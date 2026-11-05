# Ideas

Changes that wait for a later port. Each one names the skill it belongs to and what it waits for.

## A bug-hunt gate in `ship`

`review` runs `bug-hunt` only on risky changes, so `ship` can say **Ready** on an ordinary change with only the Correctness check behind it. A gate could require a bug hunt for every ship, at the cost of the long hunt each time.

Waits for the `ship` port. It is likely not worth it, since the risky changes are already covered.

## Rust and Go references for `simplify`

A language reference earns its place with opinions the model wouldn't follow by default, not with the language's own idioms or what `rustfmt`, `clippy`, `gofmt` and `go vet` already enforce. Note each correction made to an agent twice while writing Rust or Go, and start the reference once a few have collected. Rust likely gets there first, since it leaves more choices of style open.

Waits for those corrections.
