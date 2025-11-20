# Experiment No-8 — Blockchain-Based Supply Chain Traceability System

## Title

Development of a Blockchain-Based Supply Chain Traceability System

## Aim

To design and implement a blockchain-based system that ensures transparency, immutability, and traceability across all stages of a supply chain using Ethereum smart contracts.

---

## Theory

### Supply Chain Traceability

The process of tracking a product’s journey across different stages:
**Manufacturer → Distributor → Retailer → Customer**.

### Problems in Traditional Supply Chains

* Data manipulation is possible
* Limited transparency
* Difficult to verify authenticity
* No shared system across all participants

### Blockchain Solution

Blockchain provides:

* **Tamper-proof records** — once added, data can’t be altered
* **Complete transparency** — product journey viewable by all participants
* **Trustless environment** — no need for intermediaries
* **Smart contract automation** — rules are enforced automatically

### Smart Contract

A decentralized program deployed on Ethereum that securely stores product data and updates.

### Tools Used

* **Solidity** → smart contract
* **Remix IDE** → write/compile/deploy
* **MetaMask** → wallet and transaction signer
* **Ethereum Testnet (Sepolia/Goerli)** → safe testing environment

---

## Key Characteristics of a Blockchain Supply Chain dApp

1. **Transparency** — Everyone can verify product movement
2. **Immutability** — No record can be edited once added
3. **Trustless** — No central authority controls data
4. **Security** — Data secured with cryptography
5. **Traceability** — Full product lifecycle can be tracked

---

## Steps of Execution

### Step 1 — Setup Environment

* Install MetaMask
* Connect to a Testnet (Sepolia/Goerli)
* Get test ETH from faucet
* Open Remix IDE

### Step 2 — Write Smart Contract

* Create file: `SupplyChain.sol`
* Define product structure: ID, name, owner, status

### Step 3 — Compile Contract

* Select Solidity **0.8.x** compiler
* Fix warnings if any

### Step 4 — Deploy Contract

* Select **Injected Provider - MetaMask**
* Deploy and confirm the transaction

### Step 5 — Add Product

* Call `addProduct("ProductName")`
* A new product is created with status `Created`

### Step 6 — Update Product Status

* Call `updateProduct(id, status)`
* Example status flow:

  * Created → Manufactured → Shipped → Delivered

### Step 7 — Retrieve Product History

* Call `getProduct(id)` to verify all details

---

## Smart Contract Code — SupplyChain.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SupplyChain {
    struct Product {
        uint id;
        string name;
        string status;
        address currentOwner;
    }

    mapping(uint => Product) public products;
    uint public productCount;

    event ProductAdded(uint id, string name, address owner);
    event ProductUpdated(uint id, string status, address owner);

    // Add a new product
    function addProduct(string memory _name) public {
        productCount++;
        products[productCount] = Product(productCount, _name, "Created", msg.sender);
        emit ProductAdded(productCount, _name, msg.sender);
    }

    // Update product status
    function updateProduct(uint _id, string memory _status) public {
        require(_id > 0 && _id <= productCount, "Invalid product ID");
        Product storage p = products[_id];

        p.status = _status;
        p.currentOwner = msg.sender;

        emit ProductUpdated(_id, _status, msg.sender);
    }

    // Get product details
    function getProduct(uint _id) public view returns (uint, string memory, string memory, address) {
        require(_id > 0 && _id <= productCount, "Invalid product ID");
        Product memory p = products[_id];
        return (p.id, p.name, p.status, p.currentOwner);
    }
}
```

---

## Remix IDE Deployment Steps (Clean & Simple)

### 1. Add Contract

* Create file `SupplyChain.sol`
* Paste the above code

### 2. Compile

* Solidity compiler → 0.8.20 or compatible
* Press **Compile**

### 3. Deploy

* Environment → **Injected Provider (MetaMask)**
* Deploy contract → approve MetaMask transaction

### 4. Add Product

* Under Deployed Contracts → `addProduct("Laptop")`

### 5. Update Status

* Call: `updateProduct(1, "Shipped")`

### 6. Verify

* Call: `getProduct(1)`
* View ID, name, status, and current owner

---

## Key Points

* Blockchain enables secure and transparent tracking of product movement
* Every product has a unique ID and immutable history
* Each update is permanently recorded on-chain
* Builds trust between all supply chain participants

---

## Conclusion

In this experiment, we developed a blockchain-powered supply chain traceability system. Using smart contracts, we securely stored and updated product information at each stage. This demonstrated how blockchain enhances transparency, immutability, and trust in supply chain management.

---

## Viva Questions

**1. What is supply chain traceability?**
Supply chain traceability is the ability to track a product’s entire journey—from raw materials to manufacturing, transportation, and final delivery. It ensures transparency, accountability, and authenticity at every stage.

**2. How does blockchain improve supply chain management?**
Blockchain provides a decentralized, tamper-proof ledger that records every supply chain event. This improves transparency, prevents data manipulation, reduces fraud, enhances tracking, and enables real-time updates accessible to all stakeholders.

**3. What is immutability in blockchain?**
Immutability means that once data is recorded on the blockchain, it cannot be altered or deleted. This ensures the integrity and reliability of supply chain records, preventing fraud and unauthorized modifications.

**4. Why is Ethereum suitable for supply chain dApps?**
Ethereum supports smart contracts, has a large developer community, offers reliable infrastructure, and allows automation of supply chain processes like verification, status updates, and payments — making it ideal for supply chain decentralized applications.

**5. What functions are required in a supply chain contract?**
Typical functions include:

* Adding product details
* Updating product status at each stage
* Transferring product ownership
* Verifying authenticity
* Recording timestamps and stakeholders
  These enable complete tracking and secure data management.

**6. What are the advantages of storing product data on blockchain?**
Advantages include tamper-proof records, improved transparency, better traceability, reduced fraud, faster audits, real-time updates, and increased consumer trust.

**7. How does blockchain improve trust among supply chain partners?**
Blockchain ensures that all participants share the same verified, immutable data. Since no single entity controls the system, partners can trust the accuracy of records, reducing disputes, enhancing collaboration, and improving overall efficiency.

