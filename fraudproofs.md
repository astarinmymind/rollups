## overview
- optimistic view: assumption that blocks represent correct L2 states until proven otherwise
- proofs are not needed for every state transition: 
    - fewer computational resources
    - better fit for highest scalability 
- requires **liveness** by party claiming fraud 
- problem: protocol interpretation of silence as implicit consent (attacker may create fake silence with DDoS attack)

## concept
1. block includes incorrect state transition
2. protocol allows a dispute time delay (dtd) to dispute incorrect state
3. case a: if no fraud proof is submitted within dtd, L2 state transition considered correct 
4. case b: if valid fraud proof is submitted within dtd, smart contract reverts state to last correct L2 state (+ penalty on violating party)

note: there is a tradeoff in picking a good dtd: longer dtds increase the probability of incorrect state transition detection, but also means users have to wait longer to withdraw funds 

## 51% attack 
fraud proofs are vulnerable to 51% attacks where an attacker can put the blockchain in a fraudulent state 
![51% attack by fraud proof](https://miro.medium.com/max/1400/0*zvXk_5LhszEMyJsB)
1. fraudulent state transition by including transfer of funds to account
2. add dtd blocks which includes withdrawal of funds 
3. extend chain beyond dtd 
note: cost of 51% attack is independent of how much money is stolen, so the greater the funds are, the higher chance it has of getting attacked