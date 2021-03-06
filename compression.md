## rollups can compress transactions
### comparison for an eth transfer on main chain vs rollup
| parameter  |  ethereum | rollup  | explanation |
|---|---|---|------|
| nonce  | ~3  |  0 | rollups recover nonce from pre-state |
|  gasprice | ~8  |  0-0.5 | can allow fixed range, fixed fee, or move off rollup
| gas  | 3  |  0-0.5 | can restrict to consecutive powers of two or implement batch gas limit 
| to  | 21  |  4 | replace 20-byte address with index n determined by nth address added to the tree (maintain subtree to state which maps indices to addresses)
|  value | ~9  |  ~3 | store value in scientific notation with 1-3 sig figs
| signature | ~68 (2 + 33 + 33)  |  ~0.5 | use bls aggregate signatures where signatures are aggregated into 32-96 bytes and checked against a batch all at once to produce one signature per ~100 transactions
|  from |  0 (recovered from sig) | 4  | same as to
|  total | ~112  |  ~12 |
&nbsp;
### zk rollups offer further compression
- some parts of a transaction is only used for verification:
    - this must be on-chain for optimistic rollup for it to be checked in a fraud proof
    - this can be off-chain for zkrollup as the snark proves all data needed for verification was provided 

- privacy-preserving rollups:
    - optimistic rollup needs to include ~500 byte zk-snark on-chain for privacy in each transaction
    - zkrollup has zk-snark that can cover entire batch
