**Experiment 1: Simple Blockchain Implementation**

*A blockchain* is a linked structure of blocks where each block securely stores data using cryptographic hashing. The core idea is that each block depends on the previous block’s hash, making the chain resistant to tampering.

*Block elements include:*

* *Index*: position in the chain
* *Timestamp*: when the block was created
* *Data*: transactions or any stored information
* *Previous hash*: connects this block to the earlier block
* *Hash*: produced using SHA-256; depends on all block contents
* *Nonce*: used only in Proof-of-Work systems to vary the hash

*Purpose of each field:*

* *Previous hash* links blocks
* *Hash* ensures integrity
* *Timestamp* orders blocks
* *Data* stores the block’s payload
* *Nonce* allows hash manipulation during mining

*A block’s hash is computed by* combining:

* index
* timestamp
* data
* previous hash
* nonce (if used)

*The SHA-256 output* is a fixed-size digital fingerprint. Any small change in input creates a completely different hash.

*A simple blockchain implementation* contains:

1. *Block class* that stores fields and calculates the SHA-256 hash
2. *Blockchain class* that:

   * creates the *genesis block*
   * appends new blocks by referencing the previous block’s hash
   * verifies the chain by recalculating and comparing hashes

*Genesis block characteristics:*

* first block in the chain
* no previous block exists
* previous hash is typically “0”

*Chain validation logic:*

* recompute every block’s hash
* confirm previous hash matches the hash of the earlier block
* any mismatch implies tampering

*Deep concepts related to real blockchain systems:*

* *Nonce* in mining: a number miners adjust repeatedly to find a valid hash under a difficulty target
* *Transaction structure*: includes sender, receiver, amount, signature, and a transaction nonce to prevent replay
* *Merkle root*: a single hash summarizing all transactions using a Merkle tree, enabling efficient verification
* *Immutability*: modifying a block changes its hash, breaking all subsequent links and exposing tampering

*Why this experiment is foundational:*

* shows hash-based security
* explains block linking
* demonstrates immutability and verification
* introduces ideas used in Bitcoin, Ethereum, and all major blockchains

*Sample concise viva answers:*

* *A block cannot be modified because its hash would no longer match its recalculated hash.*
* *The previous hash field connects blocks and ensures tamper detection.*
* *The genesis block is the first block and has no predecessor.*
* *SHA-256 is used because it is secure, irreversible, and collision-resistant.*
* *Immutability comes from each block storing the hash of the previous one.*

---
**Experiment 2: SHA-256 Hashing for Data Integrity**

*Hashing* is a process that converts any input into a fixed-length output using a mathematical function. *SHA-256* is one such cryptographic hash function widely used in blockchain systems.

*Core properties of SHA-256:*

* *Deterministic*: the same input always produces the same output
* *Fixed-length*: output is always 256 bits (64 hex characters)
* *Irreversible*: original data cannot be retrieved from the hash
* *Collision-resistant*: extremely difficult to find two inputs with the same hash
* *Avalanche effect*: a small change in input results in a drastically different output

*Typical uses of SHA-256 in real systems:*

* ensuring data integrity
* verifying file or message authenticity
* generating transaction IDs
* securing blockchain blocks
* hashing digital signatures and keys

*Steps in hashing data:*

1. *Take input data* as a string or bytes
2. *Encode* the input to bytes
3. *Pass* the encoded data into the SHA-256 function
4. *Produce* a hexadecimal hash output
5. *Compare hashes* before and after changes to verify integrity

*Example insight:*
If the input "Hello" changes to "hello", the SHA-256 output becomes completely different due to the avalanche effect.

*Structure of data before hashing:*

* original message
* encoded bytes
* hashing via SHA-256 function

*Structure of hash output:*

| Component    | Description                       |
| ------------ | --------------------------------- |
| *Length*     | always 256 bits                   |
| *Form*       | 64 hexadecimal characters         |
| *Uniqueness* | practically unique for all inputs |

*Why SHA-256 is used in blockchain:*

* hashing block contents produces a secure block hash
* ensures immutability because any data change alters the hash
* provides a tamper-evident structure for all transactions
* allows nodes to quickly validate block integrity without decrypting anything

*Viva-ready points:*

* *SHA-256 ensures data integrity by producing a unique fingerprint of the data.*
* *Two different inputs will almost never generate the same hash due to collision resistance.*
* *Hashing cannot be reversed, making it suitable for secure storage and verification.*
* *Blockchain uses SHA-256 to link blocks together and detect any tampering.*

---
**Experiment 3: Simulation of Proof-of-Work (PoW) and Proof-of-Stake (PoS) Algorithms**

*Consensus mechanisms* ensure all nodes in a blockchain network agree on the same state. *PoW* and *PoS* are two widely used approaches to maintain security, validate transactions, and add new blocks.

*Purpose of consensus:*

* *prevent double spending*
* *ensure honest participation*
* *secure the chain against attacks*
* *provide a fair way to propose new blocks*

*Proof-of-Work (PoW):*

* miners compete using computational power
* goal is to find a hash below a difficulty target
* hash is manipulated by adjusting a *nonce*
* first miner to find a valid hash adds the new block
* energy-intensive but highly secure

*Steps in PoW mining simulation:*

1. prepare block data
2. choose difficulty (number of leading zeros required)
3. initialize nonce at zero
4. iterate through nonce values until hash meets difficulty
5. record valid nonce and hash as proof of work

*Meaning of difficulty:*
Higher difficulty means more leading zeros, increasing the number of nonce attempts needed.

*Role of nonce in PoW:*

* *a value miners change repeatedly*
* *influences the hash output*
* *reshuffled until the hash satisfies difficulty requirement*

*Proof-of-Stake (PoS):*

* validators are chosen based on the amount of cryptocurrency they stake
* selection probability is proportional to stake size
* avoids high energy usage
* dishonest validators risk losing stake (slashing)

*Steps in PoS simulation:*

1. list validators with associated stakes
2. compute total stake
3. randomly select a validator weighted by stake proportion
4. chosen validator proposes the next block

*Comparison table (concise):*

| Feature        | *PoW*                      | *PoS*                                |
| -------------- | -------------------------- | ------------------------------------ |
| Resource used  | computational power        | cryptocurrency stake                 |
| Energy usage   | high                       | low                                  |
| Block winner   | first miner solving puzzle | validator chosen by stake proportion |
| Security basis | cost of computation        | economic penalty (slashing)          |

*Why PoS is more energy-efficient:*
No miners compete with hardware; selection relies on stake weight.

*Deep concepts relevant to viva:*

* *transaction nonce*: prevents replay attacks and ensures transaction ordering; unrelated to mining nonce
* *difficulty adjustment*: system modifies difficulty to maintain constant block time
* *security assumption*: PoW secures via computational cost; PoS secures via economic penalties

*Viva-ready points:*

* *PoW requires miners to solve a hash puzzle; PoS chooses validators based on stake.*
* *Nonce in PoW helps search for a valid hash under the difficulty target.*
* *PoS reduces energy consumption by removing mining competition.*
* *Both mechanisms aim to secure the blockchain and maintain consensus.*

---
**Experiment 4: Development and Deployment of a Smart Contract Using Solidity**

*A smart contract* is a self-executing program stored on the blockchain, designed to run exactly as written without manual intervention. Solidity is the primary language used to create such contracts for the Ethereum ecosystem.

*Core elements of a smart contract:*

* *state variables*: permanently stored values on the blockchain
* *functions*: operations that read or modify state
* *events*: lightweight logs emitted for off-chain listeners
* *modifiers*: reusable condition wrappers applied to functions

*Structure of a basic contract:*

* declaration of version using *pragma*
* contract definition block
* state variables
* constructor (optional)
* public or private functions

*Typical steps in contract development:*

1. write Solidity code defining logic and storage
2. compile contract to bytecode and ABI
3. deploy to blockchain using a tool like Remix, Truffle, or Hardhat
4. interact using a console, scripts, or web interface

*Details of code compilation:*

* Solidity compiler creates *bytecode* executed by the EVM
* it also generates an *ABI* describing function interfaces
* both are used during deployment and interaction

*Role of Truffle in deployment:*

* provides project scaffolding
* handles compilation and contract building
* uses migration scripts to deploy deterministically
* connects to networks like Ganache or testnets

*Ganache usage:*

* simulates a local Ethereum blockchain
* provides test accounts with ETH
* allows rapid testing without real gas consumption

*Important execution concepts in Solidity:*

* *storage*: permanent and expensive; used for state variables
* *memory*: temporary; used for internal computations
* *calldata*: read-only; used for external function inputs

*Gas concepts relevant to contract deployment:*

* every write to storage costs significant gas
* reading state is cheaper but still charged
* deploying a contract consumes gas because it stores bytecode on-chain

*Function visibility types:*

* *public*: accessible from anywhere
* *external*: called only from outside the contract
* *internal*: used within contract or inherited contracts
* *private*: used only inside the defining contract

*Basic workflow of interacting with a deployed contract:*

1. connect to the network
2. load ABI and contract address
3. create contract instance
4. execute read or write functions
5. receive output or transaction receipt

*Example functions in a simple storage contract:*

* *set()* to store a value
* *get()* to retrieve the stored value
* *event* emitted when a value is updated

*Why this experiment matters:*

* introduces smart contract fundamentals
* explains storage, bytecode, and ABI
* demonstrates deployment and interaction
* serves as foundation for tokens, dApps, and security experiments

*Viva-ready points:*

* *A smart contract executes automatically once deployed and cannot be altered.*
* *Solidity compiles into bytecode that the EVM runs.*
* *Events provide on-chain logging without storing extra data.*
* *Gas represents the computational cost of running functions.*
* *Truffle simplifies compilation and deployment through migrations.*

---
**Experiment 5: Creation and Deployment of an ERC-20 Token on Ethereum**

*An ERC-20 token* is a fungible digital asset that follows a standard set of rules defined by the Ethereum community. These rules ensure that wallets, exchanges, and dApps can interact with any ERC-20 token reliably.

*Key characteristics of ERC-20 tokens:*

* *fungible*: each token is identical and interchangeable
* *standardized functions*: ensures compatibility
* *stored in wallet addresses*: similar to ETH but managed by smart contracts

*Essential functions in the ERC-20 standard:*

* *totalSupply()*: returns total number of tokens
* *balanceOf(address)*: returns balance of a specific user
* *transfer(address, amount)*: transfers tokens to another user
* *approve(address, amount)*: authorizes another address to spend tokens
* *transferFrom(address, address, amount)*: executes spending on behalf of the user
* *allowance(owner, spender)*: shows remaining approved tokens

*Common events included:*

* *Transfer*: emitted when tokens move between addresses
* *Approval*: emitted when an allowance is set or updated

*Basic structure of an ERC-20 contract:*

* declare state variables for balances and total supply
* map addresses to their token balances
* manage allowances using a nested mapping
* write logic for transfer, approve, and transferFrom
* emit events for transparency and tracking

*Deployment workflow using Remix or Truffle:*

1. write ERC-20 token code
2. compile contract to generate ABI and bytecode
3. connect MetaMask to a test network
4. deploy contract to the blockchain
5. record the deployed address
6. interact with functions through Remix, scripts, or dApps

*Role of MetaMask in deployment:*

* signs transactions
* pays gas fees using testnet ETH
* manages accounts and contract interactions

*Allowance mechanism explained simply:*

* user calls *approve(spender, amount)*
* spender gets permission to transfer tokens up to the approved limit
* spender uses *transferFrom()* to move tokens
* allowances prevent unauthorized transfers while enabling automation

*Typical risks and fixes:*

* *approval race condition*: spender may use old allowance before update
* fix by setting allowance to zero before assigning a new value or using *increaseAllowance()* and *decreaseAllowance()*

*Why ERC-20 tokens are important:*

* enable creation of cryptocurrencies, credits, points, and utility tokens
* serve as the base for ICOs, DeFi platforms, and many blockchain applications
* form the fundamental token standard in Web3 ecosystems

*Concise comparison table:*

| Feature            | *Description*                         |
| ------------------ | ------------------------------------- |
| *Fungibility*      | all tokens are identical              |
| *Transferability*  | easily transferred via transfer()     |
| *Interoperability* | supported by all major Ethereum tools |
| *Security*         | relies on smart contract correctness  |

*Viva-ready points:*

* *ERC-20 defines a standard interface for fungible tokens on Ethereum.*
* *transfer() sends tokens directly, while transferFrom() uses allowances.*
* *approve() sets how many tokens a spender can use.*
* *Events keep token transfers visible to wallets and explorers.*
* *Fungibility ensures every token has equal value.*

---
**Experiment 6: Development of a Decentralized Application (dApp) Using Web3/Ethers**

*A decentralized application (dApp)* interacts directly with a smart contract deployed on a blockchain. It removes the need for centralized servers by using the blockchain as the backend and the user's wallet for authentication.

*Core components of a dApp:*

* *frontend interface*: built using HTML, CSS, and JavaScript
* *smart contract*: deployed on Ethereum or similar network
* *library for blockchain interaction*: Web3.js or Ethers.js
* *wallet provider*: usually MetaMask

*Purpose of a dApp:*

* allow users to read data from the blockchain
* allow users to send transactions to modify contract state
* provide trustless and transparent interactions

*Steps in building the dApp:*

1. deploy smart contract to testnet or local node
2. connect frontend to MetaMask
3. load contract address and ABI
4. initialize Web3/Ethers.js
5. call contract functions through user actions
6. display results in UI

*Role of MetaMask:*

* exposes accounts to the dApp
* signs transactions securely
* selects blockchain networks
* sends signed transactions to the connected network

*Web3.js versus Ethers.js (concise comparison):*

| Feature           | *Web3.js*            | *Ethers.js*         |
| ----------------- | -------------------- | ------------------- |
| Size              | heavier              | lightweight         |
| Design            | older, utility-heavy | modern, modular     |
| Provider handling | integrated           | explicit separation |
| Recommended       | legacy dApps         | newer projects      |

*Basic interaction steps using Web3 or Ethers:*

* connect to *window.ethereum* (MetaMask)
* request user accounts
* create a contract instance using ABI + address
* call read functions directly (no gas needed)
* call write functions through signed transactions (gas required)

*Transaction flow initiated from a dApp:*

1. user clicks a button
2. dApp prepares transaction data
3. MetaMask prompts for confirmation
4. user signs the transaction
5. transaction enters mempool
6. miner/validator includes it in a block
7. dApp listens for receipt and updates UI

*Types of contract calls:*

* *read calls*: use call() method; no gas; return data directly
* *write calls*: use send() or transaction methods; cost gas; modify state

*Important concepts for clarity:*

* *ABI*: defines how functions and events are encoded
* *provider*: connects dApp to blockchain network
* *signer*: represents user’s account for sending transactions
* *contract instance*: interface for interacting with deployed contract

*Why dApps matter:*

* enable decentralized finance, voting, gaming, and identity systems
* give users control over their data and assets
* eliminate centralized single points of failure

*Viva-ready points:*

* *A dApp communicates with a smart contract using Web3/Ethers.js.*
* *MetaMask provides accounts and signs transactions.*
* *Read functions require no gas; write functions modify blockchain state.*
* *ABI and contract address are required to create a contract instance.*
* *Ethers.js is modern and lightweight, while Web3.js is older and heavier.*

---
**Experiment 7: Security Analysis of Smart Contracts – Reentrancy Attack**

*A reentrancy attack* occurs when a contract makes an external call before updating its internal state, allowing the called contract to repeatedly re-enter the vulnerable function.

*Core idea of the attack:*

* contract sends Ether using *call()*
* receiver’s fallback or receive function executes
* attacker triggers the same withdrawal function again
* state is not updated yet, so multiple withdrawals succeed

*Conditions required for reentrancy:*

* use of *call()* or low-level external calls
* state updates placed after external calls
* lack of reentrancy protection mechanisms

*Structure of a vulnerable withdrawal function:*

1. check user balance
2. send Ether to user
3. reduce user balance

*Why this is dangerous:*
sending Ether before reducing balance allows the receiver to exploit the delay and recursively drain funds.

*Typical attack sequence:*

1. attacker deposits some Ether
2. attacker calls withdraw()
3. contract sends Ether using *call()*
4. attacker’s fallback triggers
5. fallback calls withdraw() again
6. contract sends Ether again (balance not updated yet)
7. loop continues until contract balance is empty

*Sample structure of an attacker contract:*

* fallback function calls withdraw()
* attack() function initiates first withdrawal
* maintains counter or checks to stop once funds are drained

*Key ways to prevent reentrancy:*

* *Checks-Effects-Interactions pattern*: update state first, then send Ether
* use *ReentrancyGuard* modifier provided by OpenZeppelin
* avoid using *call()* for sending Ether unless necessary
* implement pull payment pattern (users withdraw funds instead of receiving automatically)

*Checks-Effects-Interactions pattern explained:*

* *Check*: validate input and balance
* *Effects*: update balances and internal state
* *Interactions*: external calls placed at the end

*Difference between transfer(), send(), and call():*

| Method       | *Gas forwarded*   | *Safety*                                  |
| ------------ | ----------------- | ----------------------------------------- |
| *transfer()* | 2300 gas          | safer but may fail after opcode repricing |
| *send()*     | 2300 gas          | similar to transfer(), returns boolean    |
| *call()*     | all remaining gas | most flexible but dangerous               |

*Why call() is risky:*
forwards full gas, enabling complex fallback execution including reentrancy.

*Impact of reentrancy vulnerabilities:*

* complete draining of contract funds
* loss of user assets
* major security incidents such as the DAO hack

*Viva-ready points:*

* *Reentrancy happens when external calls occur before state updates.*
* *The attacker re-enters the same function through fallback execution.*
* *Checks-Effects-Interactions prevents the issue by updating state first.*
* *ReentrancyGuard adds a lock to stop multiple entries.*
* *call() is flexible but dangerous due to full gas forwarding.*

---
**Experiment 8: Blockchain-Based Supply Chain Management System**

*A supply chain system on blockchain* tracks the movement of a product from its origin to its final destination using transparent and tamper-evident records.

*Purpose of using blockchain for supply chain:*

* *traceability*: track product lifecycle
* *transparency*: all participants can verify records
* *immutability*: entries cannot be altered once recorded
* *multi-party trust*: no single point of control

*Key actors typically involved:*

* *manufacturer*
* *distributor*
* *wholesaler*
* *retailer*
* *customer*

*Core data stored for each product:*

* *product ID*
* *current owner*
* *status or stage*
* *history of updates* (often stored in events)

*Main functions implemented in a simple supply chain contract:*

1. *addProduct()*: registers a new product
2. *updateStatus()*: modifies product state or stage
3. *transferOwnership()*: moves product to the next actor
4. *getProductDetails()*: retrieves stored product data

*How data flows through the system:*

* manufacturer registers product
* ownership transferred to distributor
* new status updates reflect quality checks or transportation details
* retailer receives product
* customer verifies authenticity using product ID

*On-chain vs off-chain storage considerations:*

* *on-chain*: minimal essential data due to gas costs
* *off-chain*: large details like documents, images, certifications
* *events*: used to log updates without occupying storage

*Advantages of blockchain in supply chain:*

* prevents counterfeiting
* simplifies audits and verification
* reduces disputes through transparent history
* enables real-time status tracking

*Typical structure of product data mapping:*

| Field       | *Description*                      |
| ----------- | ---------------------------------- |
| *productId* | unique identifier                  |
| *owner*     | address of current holder          |
| *status*    | short description of current stage |
| *exists*    | boolean to ensure valid product    |

*Access control considerations:*

* certain operations limited to specific actors
* require checks to ensure only authorized roles update product data
* prevents malicious updates or ownership changes

*Viva-ready points:*

* *Blockchain ensures each product update is permanent and verifiable.*
* *Events help track product history without high storage cost.*
* *Ownership transfer prevents unauthorized manipulation of product identity.*
* *On-chain storage is kept minimal to reduce gas consumption.*
* *Supply chain transparency improves trust among all participants.*

---
**Experiment 9: Implementation of a Private Blockchain Network Using Hyperledger Fabric**

*Hyperledger Fabric* is a permissioned blockchain framework designed for enterprise environments where participants are known and authenticated. It enables private communication, modular consensus, and scalable transaction processing.

*Key characteristics of Fabric:*

* *permissioned access*: only authorized organizations participate
* *identity-based*: every participant is assigned a digital certificate
* *modular architecture*: consensus, membership, and ordering are configurable
* *high throughput*: supports parallel transaction execution

*Core components in a Fabric network:*

* *peers*: hold ledger copies and execute chaincode
* *orderers*: create and broadcast blocks
* *certificate authority (CA)*: issues digital identities
* *MSP (Membership Service Provider)*: defines identity rules
* *channels*: private communication layers between organizations
* *chaincode*: smart contract executed on peers

*Ledger structure in Fabric:*

* *blockchain*: immutable log of transactions
* *world state*: current state stored in LevelDB or CouchDB
* separation allows fast lookups and efficient queries

*Transaction flow in Hyperledger Fabric:*

1. client application sends a transaction proposal to endorsing peers
2. endorsing peers simulate the transaction using chaincode
3. peers sign and return endorsements
4. client submits endorsed transaction to ordering service
5. orderers package transactions into blocks
6. blocks are delivered to peers
7. peers validate endorsements and commit the block

*Role of endorsement policies:*

* determine which organizations must sign a transaction
* ensure multi-party agreement before committing updates
* help prevent unauthorized changes

*Purpose of channels:*

* allow a subset of organizations to share a private ledger
* maintain confidentiality of sensitive data
* isolate business workflows without affecting the entire network

*Chaincode characteristics:*

* written in Go, JavaScript, or Java
* deployed onto peers
* handles logic for asset creation, transfer, and queries
* executed during simulation and validation phases

*Comparison table for clarity:*

| Feature             | *Hyperledger Fabric*                  |
| ------------------- | ------------------------------------- |
| *Network type*      | permissioned                          |
| *Consensus*         | pluggable modular options             |
| *Participants*      | identified and authenticated          |
| *Ledger structure*  | blockchain + world state              |
| *Privacy mechanism* | channels and private data collections |

*Typical setup steps for a Fabric network:*

1. configure organizations, peers, orderers, and MSPs
2. start network components using Docker
3. create a channel for communication
4. join peers to the channel
5. install and approve chaincode definitions
6. commit chaincode to channel
7. invoke and query chaincode

*Advantages of Hyperledger Fabric:*

* enterprise-grade security and authentication
* private and confidential transactions
* flexible chaincode programming languages
* high performance throughput

*Viva-ready points:*

* *Fabric uses identity-based permissioning rather than anonymous participation.*
* *Transactions are simulated then ordered, not immediately committed.*
* *World state speeds up queries by storing current values.*
* *Channels allow private ledgers between selected organizations.*
* *Endorsement policies ensure collaboration and prevent unauthorized updates.*

---
**Experiment 10: Implementation of a Decentralized Voting System on Blockchain**

*A decentralized voting system* uses blockchain to record votes in a transparent, tamper-proof, and verifiable manner. It eliminates manual counting errors, central authority dependence, and vote manipulation risks.

*Core goals of blockchain-based voting:*

* *transparency*: anyone can verify results
* *immutability*: votes cannot be altered once recorded
* *fairness*: one person, one vote
* *security*: prevents unauthorized voting

*Typical actors involved:*

* *administrator*: registers candidates and manages setup
* *voters*: cast votes using their blockchain address
* *blockchain network*: stores vote data immutably

*Essential components of the voting smart contract:*

* *candidate structure*: stores candidate name and vote count
* *array or mapping*: records list of candidates
* *mapping for voter status*: prevents double voting
* *functions*: register candidates, cast votes, get results

*Basic workflow of decentralized voting:*

1. admin adds candidates to contract
2. voters connect their wallet
3. voter selects a candidate and casts a vote
4. contract checks if voter has already voted
5. voteCount for candidate is incremented
6. voter is marked as having voted

*How one-person-one-vote is enforced:*

* a *mapping(address => bool)* marks if the address has already voted
* once true, further vote attempts are rejected

*Data stored for each candidate:*

| Field       | *Description*        |
| ----------- | -------------------- |
| *name*      | candidate’s name     |
| *voteCount* | total votes received |

*Security advantages of blockchain voting:*

* voter address acts as unique identifier
* no central authority can manipulate stored votes
* transparent audit trail available to all participants

*Possible privacy concerns:*

* votes are visible on-chain unless anonymization is used
* solutions include *zero-knowledge proofs*, *mixing*, or *encrypted vote commitments*

*Limitations to address in real deployments:*

* verifying real-world identity of voters requires off-chain KYC
* preventing coercion or vote buying is still challenging
* scalability considerations for very large elections

*Why decentralized voting matters:*

* reduces fraud
* ensures integrity of results
* increases voter trust in the process
* automates tallying instantly without manual intervention

*Viva-ready points:*

* *Blockchain voting ensures transparency, immutability, and fairness.*
* *Double voting is prevented using a hasVoted mapping.*
* *Votes cannot be modified once stored on the blockchain.*
* *Candidate data is stored on-chain with name and voteCount fields.*
* *Blockchain acts as a decentralized and tamper-proof voting database.*

---
