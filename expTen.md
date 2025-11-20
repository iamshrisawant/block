# Experiment No-10 — Decentralized Voting System on Blockchain

## Title

Implementation of a Decentralized Voting System on Blockchain

## Aim

To design and implement a blockchain-based voting system using Ethereum smart contracts that enables secure, transparent, tamper-proof, and fair voting.

---

## Theory

### Problems in Traditional Voting

* Possible vote tampering
* Fake / duplicate votes
* Lack of transparency
* Slow counting process

### Blockchain as a Solution

Blockchain provides:

* **Immutability** — Votes cannot be changed once recorded
* **Transparency** — Anyone can verify candidates and vote counts
* **Fairness** — One person, one vote enforced via smart contract
* **Security** — Cryptographically secured storage

### Smart Contract Role

* Registers candidates
* Records voter participation
* Stores all votes permanently
* Publishes results instantly

### Tools Used

* **Solidity** for smart contracts
* **Remix IDE** for deployment
* **MetaMask** as wallet
* **Ethereum Testnet** (Sepolia/Goerli)

---

## Key Characteristics

1. **Decentralization** – No central authority controlling the election
2. **Security** – Votes cannot be altered or erased
3. **Transparency** – Public, verifiable vote counts
4. **One Person, One Vote** – Prevents double voting
5. **Instant Results** – Vote counts update on-chain real-time

---

## Steps of Execution

### Step 1 — Setup

* Install MetaMask
* Connect to Testnet
* Get test ETH from a faucet
* Open Remix IDE in browser

### Step 2 — Write Smart Contract

Create file: `Voting.sol` and implement:

* Candidate registration
* Vote casting
* Query functions

### Step 3 — Compile

* Open Solidity Compiler
* Choose version **0.8.x**
* Compile without errors

### Step 4 — Deploy

* Environment → **Injected Web3** (MetaMask)
* Deploy & approve in MetaMask

### Step 5 — Add Candidates

Using Remix interface:

```
addCandidate("Alice")
addCandidate("Bob")
```

### Step 6 — Voting

Different MetaMask accounts execute:

```
vote(candidateId)
```

### Step 7 — View Results

Use:

```
getCandidate(candidateId)
```

To see:

* ID
* Name
* Vote count

---

## Smart Contract Code — VotingSystem.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract VotingSystem {

    struct Candidate {
        uint id;
        string name;
        uint voteCount;
    }

    mapping(uint => Candidate) public candidates;
    mapping(address => bool) public hasVoted;
    uint public candidatesCount;

    // Add a new candidate
    function addCandidate(string memory _name) public {
        candidatesCount++;
        candidates[candidatesCount] = Candidate(candidatesCount, _name, 0);
    }

    // Cast a vote
    function vote(uint _candidateId) public {
        require(!hasVoted[msg.sender], "You already voted!");
        require(_candidateId > 0 && _candidateId <= candidatesCount, "Invalid candidate ID");

        hasVoted[msg.sender] = true;
        candidates[_candidateId].voteCount++;
    }

    // Get candidate details
    function getCandidate(uint _candidateId) public view returns (uint, string memory, uint) {
        Candidate memory c = candidates[_candidateId];
        return (c.id, c.name, c.voteCount);
    }
}
```

---

## Remix IDE Procedure (Clean Overview)

1. Open Remix → create `Voting.sol`
2. Paste the smart contract code
3. Compile using Solidity 0.8.20+
4. Deploy using Injected Web3 (MetaMask)
5. Add candidates
6. Switch MetaMask accounts to simulate different voters
7. Cast votes and view live results

---

## Key Points

* Each Ethereum address can vote only **once**
* Vote data is immutable after storage
* Easy result verification via `getCandidate()`
* Fully transparent and tamper-resistant

---

## Conclusion

In this experiment, we successfully implemented a decentralized voting system using Ethereum smart contracts. The system enforces fairness, prevents vote manipulation, ensures transparency, and provides instant, verifiable results — demonstrating the power of blockchain in secure election systems.

---

## Viva Questions

**1. What is a decentralized voting system?**
A decentralized voting system is an election mechanism built on blockchain where votes are recorded on a distributed ledger. It eliminates central authority control, enhances transparency, and prevents tampering or manipulation.

**2. Why is blockchain suitable for voting applications?**
Blockchain provides immutability, transparency, decentralization, and security. These features ensure that votes cannot be altered, double voting is prevented, and results can be independently verified by anyone.

**3. How does the smart contract ensure one person can vote only once?**
The smart contract maintains a mapping (e.g., `hasVoted[address]`) that stores whether a voter has already voted. Before accepting a vote, the contract checks this mapping, and rejects the transaction if the voter has voted before.

**4. Which tools are used for compiling and deploying the contract?**
Tools such as **Remix IDE**, **MetaMask**, **Truffle**, **Hardhat**, or **Ganache** can be used to compile, deploy, and test the voting smart contract.

**5. How is blockchain voting different from traditional voting?**
Blockchain voting ensures transparency, real-time verification, immutability, and removes reliance on a central authority. Traditional voting relies on centralized systems that can be prone to tampering, delays, and auditing difficulties.

**6. Can votes be modified once stored on blockchain? Why?**
No, votes cannot be modified because blockchain is **immutable**. Once a vote is recorded in a block and added to the chain, it becomes permanent and cannot be altered without breaking the entire chain's cryptographic integrity.

**7. Which function retrieves candidate details?**
Usually, a function like `getCandidate()`, `getCandidateDetails()`, or `candidates(id)` is implemented in the smart contract to retrieve candidate information such as name, vote count, or ID.

