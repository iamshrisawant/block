# Experiment Five — Creation & Deployment of an ERC-20 Token (Remix + MetaMask)

**Title:** Creation and Deployment of an ERC-20 Token on Ethereum Testnet
**Aim:** Create and deploy a basic ERC-20 token smart contract on an Ethereum testnet (Sepolia/Goerli) using Remix IDE and MetaMask, then test token functions.

---

# Theory (brief)

* **Ethereum:** a programmable blockchain that runs smart contracts.
* **Smart contract:** code deployed on a blockchain that executes deterministically.
* **ERC-20:** a widely used standard for fungible tokens on Ethereum (all tokens are identical in value).
* **Testnet (Sepolia/Goerli):** practice networks with test ETH; no real money is used.
* **Remix IDE:** web-based Solidity editor + compiler + deployer.
* **MetaMask:** browser wallet that connects to Ethereum networks and signs transactions.

**Key characteristics of ERC-20 tokens**

* Fungible, Transferable, Interoperable.
* Supply can be fixed at deployment (or minted later in extended contracts).
* Standard functions: `totalSupply()`, `balanceOf()`, `transfer()`, `approve()`, `transferFrom()`, `allowance()`.

---

# Files & Where to run

* Open: `https://remix.ethereum.org`
* Create a new file named: `MyToken.sol`
* Use Solidity compiler version **0.8.20** (or compatible 0.8.x).

---

# Clean Solidity Program (`MyToken.sol`)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract MyToken is IERC20 {
    string public name = "MyToken";
    string public symbol = "MTK";
    uint8 public decimals = 18;

    uint256 private _totalSupply;
    mapping(address => uint256) private balances;
    mapping(address => mapping(address => uint256)) private allowed;

    constructor(uint256 initialSupply) {
        // initialSupply is in whole tokens (e.g., 1_000_000)
        _totalSupply = initialSupply * 10 ** uint256(decimals);
        balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return balances[account];
    }

    function transfer(address to, uint256 value) public override returns (bool) {
        require(balances[msg.sender] >= value, "Not enough balance");
        require(to != address(0), "Invalid recipient");

        balances[msg.sender] -= value;
        balances[to] += value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public override returns (bool) {
        require(spender != address(0), "Invalid spender");

        allowed[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return allowed[owner][spender];
    }

    function transferFrom(address from, address to, uint256 value) public override returns (bool) {
        require(balances[from] >= value, "Not enough balance");
        require(allowed[from][msg.sender] >= value, "Allowance exceeded");
        require(to != address(0), "Invalid recipient");

        balances[from] -= value;
        balances[to] += value;
        allowed[from][msg.sender] -= value;

        emit Transfer(from, to, value);
        return true;
    }
}
```

> **Note:** This is a minimal, educational ERC-20 implementation. For production tokens you should use audited, battle-tested libraries (e.g., OpenZeppelin) and consider added features (minting, access control, pausing, safe math checks, events, and edge-case handling).

---

# Stepwise Procedure (detailed)

### 1. Set up MetaMask

* Install MetaMask browser extension.
* Create/import a wallet and securely store the seed phrase.
* Switch MetaMask network to testnet: **Sepolia** or **Goerli** (choose the one you prefer; Sepolia is common).
* Get test ETH from a faucet for the chosen testnet (search “Sepolia faucet” or “Goerli faucet” and follow instructions).

### 2. Open Remix IDE

* Go to `https://remix.ethereum.org`.
* In the File Explorer create `MyToken.sol` and paste the Solidity code above.

### 3. Compile

* Open the **Solidity Compiler** plugin.
* Select compiler version **0.8.20** (or latest 0.8.x).
* Enable optimization if desired, then click **Compile MyToken.sol**.
* Fix any compile errors if present.

### 4. Deploy

* Open **Deploy & Run Transactions**.
* Environment: choose **Injected Web3** (this connects Remix to MetaMask).
* MetaMask will prompt you to connect — approve it.
* In the contract dropdown select `MyToken`.
* In the constructor field supply an `initialSupply` integer (e.g., `1000000` for 1,000,000 tokens).

  * Remember decimals are 18 internally: `initialSupply * 10**18`.
* Click **Deploy** — MetaMask will ask to confirm the transaction.
* After confirmation, wait for the transaction to be mined (confirmations in Remix logs).

### 5. Verify on Etherscan (Testnet)

* Copy the deployed contract address from Remix (Deployed Contracts panel).
* Go to the Testnet Etherscan (Sepolia or Goerli) and paste the address to see the contract.
* Optionally verify the source code on Etherscan by using the **Verify & Publish** tool and matching compiler settings.

### 6. Interact & Test

* In Remix, under **Deployed Contracts**, expand your contract.
* Call `totalSupply()` — should return `initialSupply * 10**18`.
* Call `balanceOf(<your address>)` — deployer should hold the full supply initially.
* Use `transfer(<recipient>, amount)` to send tokens (amount in wei-like smallest units).
* Use `approve(spender, amount)` then `transferFrom(from, to, amount)` to test allowance flow.
* Add token to MetaMask:

  * In MetaMask, choose **Import tokens** → paste contract address → symbol should auto-fill (MTK) and decimals = 18.

---

# Testing checklist (example)

* [ ] Compiler version set to 0.8.20 and contract compiles cleanly.
* [ ] MetaMask connected to the same testnet (Sepolia/Goerli) as Remix.
* [ ] Deployed transaction confirmed and contract address received.
* [ ] `totalSupply()` returns expected value.
* [ ] `balanceOf(deployer)` equals total supply.
* [ ] `transfer()` moves tokens between accounts and emits `Transfer`.
* [ ] `approve()` / `allowance()` / `transferFrom()` follow the expected approval pattern.
* [ ] Token appears in MetaMask after import.

---

# Key Points & Best Practices

* Always **test** on a testnet before mainnet deployment to avoid losing funds.
* Use **OpenZeppelin** libraries for hardened, audited implementations when creating real tokens.
* Keep constructor parameters and initial supply clear (show units to the user).
* Never share private keys or seed phrases; store them offline.
* Keep gas usage in mind — testnets give you test ETH to experiment.

---

# Conclusion

This experiment demonstrated writing a minimal ERC-20 token in Solidity, compiling and deploying it via Remix, and using MetaMask to interact with it on an Ethereum testnet. You verified token basics: supply, balances, transfers, approvals, and allowance flows.

---

# Viva Questions (with concise answers you should be ready to explain)

1. **What is an ERC-20 token?**
   A standard interface for fungible tokens on Ethereum, which defines functions like `transfer`, `approve`, `transferFrom`, `balanceOf`, `totalSupply`.

2. **Why use an Ethereum Testnet?**
   To develop and test contracts without spending real ETH; it prevents loss from bugs and enables safe experimentation.

3. **What is the role of MetaMask in this experiment?**
   MetaMask is the wallet that stores keys, connects to testnets, and signs transactions required to deploy and interact with the contract.

4. **Explain the function of `transfer()` in ERC-20.**
   Moves tokens from the caller’s address to another address, updating balances and emitting a `Transfer` event.

5. **What is the use of `approve()`?**
   Allows an owner to authorize a spender to transfer up to a specified amount on their behalf. `allowance()` checks remaining approved amount. `transferFrom()` performs the authorized transfer.

6. **How can you check your token balance?**
   Call `balanceOf(address)` (via Remix, Etherscan read functions, or a web3 call). In MetaMask, import the token’s contract address to see balances.

7. **Difference between Mainnet and Testnet?**
   Mainnet is the production Ethereum network with real economic value; testnets (Sepolia/Goerli) are for testing, using valueless test ETH.

---

# Quick deployment example (constructor input)

* If you want **1,000,000** tokens visible to humans, deploy with `initialSupply = 1000000`. Internally the contract stores `1000000 * 10**18`.

