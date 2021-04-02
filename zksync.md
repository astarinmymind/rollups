## overview
- zk rollup --> inherits mainnet security
- in relation to layer 1: 
    - eth and erc20 token transfers with instant confirmations and 10 minute finality
    - low transaction fees (1/100 for erc20, 1/30 for eth transfer)
    - payments to existing ethereum addresses (accounts + contracts)
    - fees payable in token transferred
    - withdrawal to mainnet in under 15 minutes
- **maximum throughput**: ~2000 tps due to support of recursion (limited by need to publish state for each transaction via calldata)
- **transaction finality**: 10 minutes for snark proof generation (will get shorter once blocks fill faster, aiming for 1 minute)
- **instant confirmations**: transactions immediately displayed + transferred assets can be immediately used (can even be in same block)
    - confirmations are a _promise_ by zkSync validators to include the transaction in the next block --> if don't trust, wait for finality
    - economic finality guarantee via security bond 
        - validators post security bond
        - confirmation of transaction signed by more than 2/3 of consensus participants by stake
        - if new zkSync block does not contain promised transaction, security bond of all malicious validators slashed 
        - slashed funds: portion goes to tx recipient and rest burned

## comparison
![zksync compared](https://zksync.io/chart4.png)

## security
> the protocol's claim is that, given correct implementation and validity of cryptographic assumptions, funds placed into zkSync will have the same security guarantees as if they are held in an Ethereum account without any additional requirements on the user 
1. zkRollup security: no monitoring needed, private keys in cold storage, no validator danger
2. validity proofs: system cannot be in incorrect state 
3. priority queue: emergency exit mechanism if user transaction is ignored
    - user submits exit request to mainnet
    - validators must process within 1 week
    - after 1 week, system is in exodus mode where every user can immediately exit with mainnet transaction
4. upgrade mechanism: users can opt out of upgrades 

## cryptography 
- proof system: plonk (v1), redshift (v2)
- hash function: sha256, rescue
- signature scheme: musig 
- assumptions: collision-resistance, pseudo-randdomness, discrete log problem on elliptic curves & finite fields 

## payments
- operations: 
    - priority (interacts with mainnet)
        - deposit: move funds from mainnet to zksync 
        - fullexit: withdraw from zksync to mainnet without cooperation of zksync server
    - transaction (submitted via zksync operators)
        - changepubkey: sets/changes signing key of account 
        - transfer: transfer funds between zksync accounts 
        - withdraw: withdraw funds from zksync to mainnet
        - forcedexit: withdraw funds from account with no signing key to ethereum
- blocks: 
    - all operations are inside blocks pushed by zksync operators
    - commit: push block to zksync smart contract on mainnet
    - verify: publish proof for block 
    - note: multiple blocks can be committed without being verified 
        - user does not have to wait for block commitment/verification
- flow: 
    - accounts are created by: 
        1. receives deposit from ethereum
        2. transfer of funds 
        note: accounts must set signing key to authorize transactions
    - set signing key 
        - default is 0 ("unowned")
        - call changepubkey with zksync signature of transaction data + ethereum signature of account ownership
    - sending transactions
        - priority
        - normal
            - encode transaction data in bytes
            - create zksync signature with private key
            - generate eth sig or eip1271 sig
            - send transaction
        - batch
## smart contracts
- turing complete: unbounded loops, recursion, vectors, maps of arbitrary length
- inherits ethereum: local var stored in stack/heap, contract storage is global 
    - contracts can call other contracts via interfaces
- sync vm: 
    - execution trace proven in snarks 
    - uses one singl circuit 
- zinc: 
    - secure + safe is number 1 priority 
    - based off rust 
    - smart contract language optimized for zkp circuits 
    - uses llvm middle/backend
    
