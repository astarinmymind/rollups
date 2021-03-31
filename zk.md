## features
- use *validity proofs* 
1. each batch includes a cryptographic proof (zk-snark) that proves post-state root is correct 
2. proof can be quickly verified on chain
- ~100-200x boost in scalability because: 
    - snark verification is cheaper than individual transaction verification
    - off-chain state storage is cheaper than storage in evm
- guarantees: 
    - validators can not corrupt state or steal funds
    - data availability: user can retrieve funds without validator cooperation
    - no monitoring needed to prevent fraud
## architecture
1. user signs and submits transaction to validator
2. validator rolls up thousands of transactions and submits: 
    - root hash: cryptographic commitment of new state of smart contract
    - snark: cryptographic proof that new state is correct 
3. to allow anyone to reconstruct state at any time, the state (small amount of data for every transaction) is published as cheap calldata 
4. proof and state verified by smart contract (transaction validity + data availabiliity guaranteed)
## challenges
- currently not capable of supporting general-purpose evm computation