# Ideas

Changes that wait for a later port. Each one names the skill it belongs to and what it waits for.

## A bug-hunt gate in `ship`

`review` runs `bug-hunt` only on risky changes, so `ship` can say **Ready** on an ordinary change with only the Correctness check behind it. A gate could require a bug hunt for every ship, at the cost of the long hunt each time.

Waits for the `ship` port. It is likely not worth it, since the risky changes are already covered.
