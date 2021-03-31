## snapshot
> Rollups move computation (and state storage) off-chain, but keep some data per transaction on-chain.
- designed to be **general purpose** (eg can run an EVM inside it, allowing all existing layer 1 applications to *migrate*without the need to write new code)
- transactions are verified in batches rather than one by one ("rolled up")

## meat
1. on-chain rollup smart contract keeps track of state via a **state root** (a hash of account balances, contract code, etc)
![state root](https://vitalik.ca/images/rollup-files/diag1.png)
2. the state root updates when anyone publishes a batch
![batch](https://vitalik.ca/images/rollup-files/diag2.png)
note: depositing and withdrawing is done by transferring and withdrawing assets from the rollup contract
3. ensure post-state root is correct (2 flavors: optimistic and zk rollup)
## roadmap
> the ethereum ecosystem is likely to be all-in on rollups (plus some plasma and channels) as a scaling strategy for the near and mid-term future. eth2’s long-term future is a single high-security execution shard that everyone processes, plus a scalable data availability layer.
### short term: 
- protocol: eth core dev focusing on increasing how much data blocks can hold
- infrastructure: 
    - accounts, balances, assets move to L2
    - direct wallet integration
    - cross L2 asset transfers 
    - standardize intermediate compiling language (eg Yul) 
### long term: 
- ethereum: 15 TPS
- rollups: 3000 TPS
- rollups on eth2 sharded chains: 100000 TPS

## unsolved challenges 
- user and ecosystem onboarding 
- rollup to rollup transactions
- plasma and rollup hybrid design space
- absent sequencers 
- efficient ZK-VM: generating ZK-SNARK proof that general-purpose EVM code has been executed correctly and has a given result

- for ORs
    - security of pre-confirmations, in the case that a sequencer immediately provides a promise that a transaction will be in the next batch
    - auditing incentives to guarantee at least one honest node will verify and generate fraud proof if necessary

