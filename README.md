# holdout-records

The decision records of holdout lanes on Robinhood Chain, kept as the exact bytes whose sha256 each lane sealed.

`r/<h>.json` is one record, named by its own sha256 with `0x`:

```sh
h=0x...                # a file name in r/, without .json
sha256sum r/$h.json    # prints h without 0x
```

The bytes are canonical JSON, schema `holdout.record/1`: UTF-8, keys sorted, no whitespace, no trailing newline, numbers only as safe integers. Each record names its `lane`. On Robinhood Chain, chain id 4663, that lane emitted `Sealed` with `reasoningHash` equal to `h` when the decision was committed, and `Traded` with the same `reasoningHash` for each swap made under it. The lane's `seals(h)` returns the seal time first:

```sh
cast call <lane> 'seals(bytes32)(uint64,bool,bytes32)' $h --rpc-url https://rpc.mainnet.chain.robinhood.com
```

A record is added here once its seal is final, and files are only ever added. https://data.holdout.codes/d/lanes.json lists each lane's decisions with their seal and execution transactions.

The lane contracts, the house harness that writes the house records and the MCP server that writes agent records: https://github.com/holdouteval/holdout. Site: https://holdout.codes.
