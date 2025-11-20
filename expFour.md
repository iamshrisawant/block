# Experiment 4 — Development and Deployment of a Smart Contract using Solidity

## Aim

To understand the fundamentals of blockchain and smart contracts, and to develop and deploy a simple smart contract on the Ethereum blockchain using the Solidity programming language — performed using **Remix IDE**.

---

## Theory (brief)

**Smart Contracts:** Self-executing programs with contract terms written in code and deployed on a blockchain. Once deployed they are immutable and run deterministically when called.

**Solidity:** A statically-typed language for writing Ethereum smart contracts. Solidity source files compile to EVM bytecode which the Ethereum Virtual Machine executes.

**Ethereum:** A decentralized platform that runs smart contracts and dApps. Ether (ETH) is used to pay gas (transaction/compute fees).

**Key Solidity concepts:** state variables, functions (view, pure, payable), modifiers, events, visibility (public/private/internal/external), gas cost for operations, and the EVM execution model.

---

## Key Concepts (quick)

* **State variables:** Persisted on-chain storage (costly to write).
* **Functions & Visibility:** `public`, `private`, `internal`, `external` control who can call functions.
* **Events:** Used to log activity that can be listened to off-chain.
* **Gas:** Units paid to execute transactions; more complex operations cost more gas.
* **EVM:** The runtime that executes compiled bytecode.

---

## Simple project overview (Remix workflow)

This lab uses **Remix IDE** ([https://remix.ethereum.org](https://remix.ethereum.org)) for quick development, compilation and deployment. Remix removes the need for local frameworks (Truffle/Ganache) for the basic experiment.

### Files

* `SimpleStorage.sol` — the contract source.

---

## Stepwise Procedure (Remix-specific)

### 1. Open Remix IDE

1. Go to [https://remix.ethereum.org](https://remix.ethereum.org).
2. Create a new file in the `contracts/` area named `SimpleStorage.sol`.

### 2. Write the smart contract

Paste the following clean, well-documented Solidity code into `SimpleStorage.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/// @title SimpleStorage
/// @notice A minimal contract to store and retrieve a single unsigned integer
contract SimpleStorage {
    // Stored value (state variable) — persisted on-chain
    uint256 private storedData;

    // Event emitted when storedData changes
    event DataStored(address indexed setter, uint256 data);

    /// @notice Stores a new value in the contract
    /// @param x The unsigned integer value to store
    function set(uint256 x) public {
        storedData = x;
        emit DataStored(msg.sender, x);
    }

    /// @notice Retrieves the currently stored value
    /// @return The stored unsigned integer
    function get() public view returns (uint256) {
        return storedData;
    }
}
```

Notes on the code:

* `pragma solidity ^0.8.0;` ensures compilation with Solidity 0.8.x — this version includes built-in overflow checks.
* `event DataStored` allows off-chain listeners (UIs, log analyzers) to react when the state changes.
* `msg.sender` is the transaction origin (address) that invoked the function.

### 3. Compile the contract

1. In Remix, open the **Solidity Compiler** plugin.
2. Choose a compiler version compatible with `^0.8.0` (for example 0.8.19) and click **Compile SimpleStorage.sol**.
3. Fix any compiler warnings if necessary.

### 4. Deploy and run — local testing in Remix

Remix offers multiple environments under the **Deploy & Run Transactions** panel:

* **JavaScript VM (default):** An in-browser ephemeral blockchain useful for testing — no real ETH required.
* **Injected Web3:** Connects to your browser wallet (e.g., MetaMask). Use this to deploy to testnets (e.g., Goerli) or mainnet.
* **Web3 Provider:** Connect to a local node or remote provider (for advanced setups).

To test locally (JavaScript VM):

1. Select **JavaScript VM**.
2. Set the appropriate account and gas options (defaults are fine for the lab).
3. Click **Deploy** next to `SimpleStorage`.
4. After deployment, the contract instance appears under **Deployed Contracts**.

### 5. Interact with the deployed contract (Remix UI)

* To call the **set** function: expand the deployed contract panel, enter a number in the `set` textbox, and click the `set` button. This sends a transaction (costs gas in real networks).
* To call the **get** function: click `get` — this is a `view` call and does **not** consume gas (when called locally or via `call`).
* Observe emitted events in the **Transactions** and **Logs** sections.

### 6. Deploy to a Test Network (Injected Web3 + MetaMask)

1. Configure MetaMask to use a testnet (e.g., Goerli). Obtain test ETH from a faucet.
2. In Remix, switch the environment to **Injected Web3**.
3. Compile again if necessary and click **Deploy**; MetaMask will prompt to confirm the transaction and gas fee.
4. After confirmation and block inclusion, the contract is live on the chosen testnet — use the deployed address to interact off-chain.

**Note:** When using Remix + MetaMask you do not use Truffle migrations — Remix performs the deployment directly.

---

## What to observe / Important points for lab report

* Show compilation output and any warnings.
* Show a transaction trace (parameters, gas used) after calling `set`.
* Show event logs generated by `DataStored`.
* Explain gas consumed for `set` vs. `get` and why writes cost more.
* For testnet deployment include the transaction hash and contract address.

---

## Viva Questions (with brief answers)

1. **What is a smart contract, and how does it differ from a traditional contract?**

   * A smart contract is code that executes automatically on a blockchain when conditions are met. Unlike traditional contracts, it is self-executing, tamper-resistant, and transparent.

2. **Explain the role of `pragma` in Solidity.**

   * `pragma` specifies compiler version constraints to ensure source code compiles with compatible compiler versions.

3. **What are state variables in Solidity?**

   * Variables whose values are stored on the blockchain and persisted across transactions.

4. **What is the purpose of gas in Ethereum?**

   * Gas measures and pays for computational work on Ethereum, preventing abuse and compensating miners/validators.

5. **What is the purpose of the `event` keyword in Solidity?**

   * `event` declares a log entry that can be emitted by the contract; these are inexpensive ways to expose state changes to off-chain listeners.

6. **Explain the difference between `public`, `private`, and `internal` functions in Solidity.**

   * `public`: callable internally and externally; `private`: callable only within the declaring contract; `internal`: callable within the contract and derived contracts.

7. **What is the role of `deployer.deploy()` in the Truffle migration script?**

   * In Truffle migrations, `deployer.deploy(Contract)` instructs Truffle to deploy that contract to the configured network during migration.

8. **How do you test a smart contract before deploying it to the mainnet?**

   * Use local testing (Remix JS VM, Ganache), automated tests (Truffle/Hardhat tests in JavaScript/TypeScript), and deploy to testnets (Goerli) using test ETH.

9. **What is the Ethereum Virtual Machine (EVM)?**

   * The EVM is the runtime environment that executes smart contract bytecode deterministically across all Ethereum nodes.

10. **What is a migration script in Truffle?**

    * A migration is a JavaScript file used by Truffle to manage deployments in sequence (e.g., `1_initial_migration.js`, `2_deploy_contracts.js`).

---

## Conclusion

This experiment demonstrates writing, compiling, deploying and interacting with a simple Solidity contract using Remix IDE. It highlights important blockchain concepts such as immutability, gas costs, events, and the EVM. Remix provides a fast feedback loop for learning smart contract development without setting up local toolchains.

---

## References / Further reading

* Solidity docs: [https://docs.soliditylang.org](https://docs.soliditylang.org)
* Remix IDE: [https://remix.ethereum.org](https://remix.ethereum.org)
* Ethereum docs: [https://ethereum.org](https://ethereum.org)

