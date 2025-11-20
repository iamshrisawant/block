# Experiment No-6 — Building a Decentralized Application (dApp) with Ethers.js

## Title

Building a Decentralized Application (dApp) with Web3.js or Ethers.js

## Aim

To design and build a simple decentralized application (dApp) that connects to the Ethereum blockchain using Web3.js or Ethers.js. The dApp will read and update data on a deployed smart contract.

---

## Theory (short)

* **dApp (Decentralized Application):** An application that runs (partially or fully) on a blockchain network instead of a single centralized server.
* **Smart Contract:** Self-executing code deployed on the blockchain (here: Solidity on Ethereum).
* **Web3.js / Ethers.js:** JavaScript libraries for interacting with the Ethereum network from a browser or Node.js app. Web3.js is older and widely used; Ethers.js is lighter and developer-friendly.
* **MetaMask:** Browser wallet that injects `window.ethereum` and manages accounts, signing and sending transactions.

**Key characteristics of dApps:** decentralized, transparent, trustless, secure, often token-based.

---

## Tools / Environment

* Node.js (recommended v18+)
* Browser with MetaMask extension
* Remix IDE ([https://remix.ethereum.org](https://remix.ethereum.org))
* Testnet (Sepolia or Goerli — use whichever is available in Remix / MetaMask)

---

## Steps (high-level)

1. Write the smart contract in Solidity.
2. Compile & deploy the smart contract to an Ethereum testnet using Remix.
3. Save the deployed contract address and ABI.
4. Build a small frontend (index.html + Ethers.js) and connect it to MetaMask.
5. Interact (read/write) with the contract from the frontend.

---

## Smart Contract — HelloWorld.sol

Create this file in Remix and compile with Solidity ^0.8.20.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract HelloWorld {
    string public message;

    constructor(string memory _message) {
        message = _message;
    }

    function setMessage(string memory _message) public {
        message = _message;
    }

    function getMessage() public view returns (string memory) {
        return message;
    }
}
```

---

## Frontend — index.html (Ethers.js)

Save this as `index.html`. Replace `YOUR_CONTRACT_ADDRESS_HERE` and `/* ABI from Remix */` with values from Remix after deploying.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>My dApp — HelloWorld</title>
  <style>
    body { font-family: system-ui, Arial; padding: 24px; max-width: 720px; }
    input { padding: 8px; width: 70%; }
    button { padding: 8px 12px; }
    .row { margin-top: 12px; }
  </style>
</head>
<body>
  <h2>Decentralized App — HelloWorld</h2>
  <p id="currentMessage">Loading…</p>

  <div class="row">
    <input type="text" id="newMessage" placeholder="Enter new message" />
    <button id="setBtn">Update Message</button>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/ethers/dist/ethers.min.js"></script>
  <script>
    // TODO: replace with your deployed contract address and ABI
    const contractAddress = "YOUR_CONTRACT_ADDRESS_HERE";
    const contractABI = [ /* ABI from Remix */ ];

    async function loadContract() {
      if (window.ethereum === undefined) {
        alert('MetaMask not detected — please install MetaMask.');
        return;
      }

      // request account access
      await ethereum.request({ method: 'eth_requestAccounts' });

      // create provider & signer
      const provider = new ethers.BrowserProvider(window.ethereum);
      const signer = await provider.getSigner();

      // instantiate contract
      window.contract = new ethers.Contract(contractAddress, contractABI, signer);

      // wire UI
      document.getElementById('setBtn').addEventListener('click', setMessage);

      // load current message
      await getMessage();
    }

    async function getMessage() {
      try {
        const msg = await window.contract.getMessage();
        document.getElementById('currentMessage').innerText = 'Current Message: ' + msg;
      } catch (err) {
        console.error(err);
        document.getElementById('currentMessage').innerText = 'Error reading message — check console.';
      }
    }

    async function setMessage() {
      const newMsg = document.getElementById('newMessage').value;
      if (!newMsg) return alert('Enter a message first');

      try {
        const tx = await window.contract.setMessage(newMsg);
        document.getElementById('currentMessage').innerText = 'Transaction sent — waiting to confirm...';
        await tx.wait();
        await getMessage();
      } catch (err) {
        console.error(err);
        alert('Transaction failed — see console');
      }
    }

    // initialize
    loadContract();
  </script>
</body>
</html>
```

---

## Detailed Remix IDE procedure (covers Experiment No-4: Deploying a Smart Contract)

Follow these steps inside Remix to compile and deploy `HelloWorld.sol`:

1. Open [https://remix.ethereum.org](https://remix.ethereum.org).
2. Create a new file `HelloWorld.sol` and paste the contract code above.
3. Go to the **Solidity Compiler** plugin:

   * Select compiler `0.8.20` (or compatible `^0.8.20`).
   * Compile `HelloWorld.sol`. Fix any warnings if necessary.
4. Go to the **Deploy & Run Transactions** plugin:

   * Environment: choose **Injected Provider - MetaMask** (this connects Remix to the MetaMask selected network).
   * Make sure MetaMask is connected to a Testnet (Sepolia is commonly available). Fund the account with test ETH from a faucet if needed.
   * Under **Contract**, select `HelloWorld` and in the constructor arguments field type an initial message e.g. `"Hello from Remix"`.
   * Click **Deploy** and confirm the transaction in MetaMask.
5. After deployment completes, Remix will show the deployed contract under **Deployed Contracts**. Expand it to see the contract address and the functions (`getMessage`, `setMessage`).
6. Click the small clipboard icon to copy the contract address; click the ABI button (or use the Compile tab) to copy the contract ABI.
7. Record the **contract address** and **ABI** — you'll paste these into your `index.html`.

**Notes:**

* If you prefer to deploy from Remix without MetaMask, you can use `Remix VM` for local testing; but to interact with MetaMask & a real testnet, use `Injected Provider - MetaMask`.
* Use Sepolia (or whichever testnet is accessible in your MetaMask) and be aware some testnets deprecate over time.

---

## Testing / Running the dApp

1. Host the `index.html` locally (e.g., open the file in the browser or serve with a tiny HTTP server: `npx http-server` or `python -m http.server 8000`).
2. Ensure MetaMask is on the same testnet you used for deployment and that the account holds some test ETH.
3. Open the page, approve MetaMask connection when prompted.
4. You should see the current message read from the contract. Enter a new message and click **Update Message** — confirm the transaction in MetaMask. After confirmation the UI will refresh with the new message.

---

## Key Points / Reminders

* dApps combine a frontend and a smart contract; the frontend holds the UI only and calls contract methods via Ethers.js/Web3.js.
* ABI (Application Binary Interface) defines how to encode calls to the contract — required by Ethers/Web3 to interact with the contract.
* Always test on a testnet before deploying to mainnet.

---
## Viva Questions (Prepare answers)

**1. What is a dApp?**
A dApp (Decentralized Application) is an application that runs on a blockchain instead of a centralized server. It uses smart contracts for backend logic and provides transparency, immutability, and decentralization.

**2. Difference between Web3.js and Ethers.js?**
Web3.js is an older, feature-rich JavaScript library for interacting with Ethereum, while Ethers.js is a modern, lightweight, more secure alternative with better TypeScript support and cleaner APIs.

**3. Why do we use MetaMask?**
MetaMask is a crypto wallet and browser extension that allows users to manage accounts, sign transactions, and interact with decentralized applications securely. It acts as a bridge between the browser and the Ethereum blockchain.

**4. What is an ABI in Ethereum?**
ABI (Application Binary Interface) defines how to interact with a smart contract—its functions, events, inputs, and outputs. Frontend applications use the ABI to encode/decode data when calling contract methods.

**5. How does a frontend connect to blockchain?**
The frontend connects to blockchain using libraries like Web3.js or Ethers.js, which communicate with a blockchain node (like Infura, Alchemy, or MetaMask). These libraries allow the frontend to read data and send transactions to smart contracts.

**6. What is the role of a smart contract in a dApp?**
A smart contract contains the core business logic of the dApp. It executes automatically when conditions are met, performs computations, stores data, and ensures trustless interactions without a central authority.

**7. Why do we first deploy on Testnet?**
Testnets like Goerli or Sepolia allow developers to test smart contracts without spending real money. They help catch bugs, verify functionality, and ensure security before deploying on the expensive Mainnet.

---

## Conclusion

This experiment demonstrates how to write, deploy, and interact with an Ethereum smart contract using Remix and Ethers.js. You built a minimal dApp that reads and updates on-chain state using MetaMask as the signer.

---

## References / Further reading

* Remix IDE — [https://remix.ethereum.org](https://remix.ethereum.org)
* Ethers.js documentation — [https://docs.ethers.org](https://docs.ethers.org)
* Solidity docs — [https://docs.soliditylang.org](https://docs.soliditylang.org)
