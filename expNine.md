# Experiment No-9 — Private Blockchain Setup using Hyperledger Fabric

## Title

Setup and Configuration of a Private Blockchain Network using Hyperledger Fabric

## Aim

To set up, configure, and run a private blockchain network using Hyperledger Fabric, deploy chaincode, and test transactions between peers.

---

## Theory

### Private Blockchain

A blockchain where access is limited to verified and permissioned participants. Commonly used in enterprises (banking, supply chain, healthcare, etc.).

### Hyperledger Fabric

An open‑source, permissioned, modular blockchain framework developed by the **Linux Foundation** and widely used in enterprise applications.

### Key Components of Hyperledger Fabric

1. **Peer Nodes** – Maintain ledger + execute smart contracts (chaincode)
2. **Orderer Node** – Orders transactions and manages network consensus
3. **Channel** – Private communication path for selected organizations
4. **Chaincode** – Smart contracts written in Go, Node.js, or Java
5. **Certificate Authority (CA)** – Issues digital identities to participants

### Characteristics of Fabric‑based dApps

* **Permissioned:** Only approved members can join
* **Modular:** Consensus, identity, and policies fully configurable
* **Privacy:** Private channels for sensitive data
* **Smart Contracts:** Chaincode written in common languages
* **High Performance:** Supports thousands of TPS

---

## Steps of Execution (High‑Level)

1. Install prerequisites (Docker, Go, Node.js, Fabric binaries)
2. Download Hyperledger Fabric samples
3. Generate cryptographic materials
4. Start the network using Docker
5. Create a channel
6. Deploy chaincode
7. Test and verify transactions

---

## Detailed Stepwise Procedure

### **1. Install Prerequisites**

Install:

* Docker
* Docker‑Compose
* cURL
* Go
* Node.js

Verify installations:

```
docker --version
go version
node -v
```

---

### **2. Download Fabric Samples**

Run the bootstrap script:

```
curl -sSL https://bit.ly/2ysbOFE | bash -s
cd fabric-samples/test-network
```

The script downloads:

* Fabric binaries
* Docker images
* Sample networks

---

### **3. Generate Certificates & Start Network**

Fabric uses CAs to generate MSP (Membership Service Provider) identities.

Run:

```
./network.sh down
./network.sh up
```

This will:

* Generate crypto materials
* Create MSP directories
* Start peer + orderer nodes in Docker

---

### **4. Create a Channel**

Create a new channel called **mychannel**:

```
./network.sh createChannel -c mychannel
```

---

### **5. Deploy Chaincode (Smart Contract)**

Deploy the default chaincode (Go version of Asset Transfer):

```
./network.sh deployCC -c mychannel -ccn basic -ccp ../asset-transfer-basic/chaincode-go/ -ccl go
```

---

### **6. Interact with Chaincode**

Query all assets:

```
peer chaincode query -C mychannel -n basic -c '{"Args":["GetAllAssets"]}'
```

Create a new asset:

```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com \
--tls --cafile $ORDERER_CA -C mychannel -n basic \
-c '{"Args":["CreateAsset","asset6","blue","30","Tom","400"]}'
```

Verify the asset state:

```
peer chaincode query -C mychannel -n basic -c '{"Args":["ReadAsset","asset6"]}'
```

---

## Sample Chaincode (Go) — Asset Transfer

```go
package main

import (
    "encoding/json"
    "fmt"
    "github.com/hyperledger/fabric-contract-api-go/contractapi"
)

type SmartContract struct {
    contractapi.Contract
}

type Asset struct {
    ID             string `json:"ID"`
    Color          string `json:"color"`
    Size           int    `json:"size"`
    Owner          string `json:"owner"`
    AppraisedValue int    `json:"appraisedValue"`
}

func (s *SmartContract) CreateAsset(ctx contractapi.TransactionContextInterface, id string, color string, size int, owner string, value int) error {
    asset := Asset{id, color, size, owner, value}
    assetJSON, _ := json.Marshal(asset)
    return ctx.GetStub().PutState(id, assetJSON)
}

func (s *SmartContract) ReadAsset(ctx contractapi.TransactionContextInterface, id string) (*Asset, error) {
    assetJSON, err := ctx.GetStub().GetState(id)
    if err != nil || assetJSON == nil {
        return nil, fmt.Errorf("Asset not found")
    }

    var asset Asset
    _ = json.Unmarshal(assetJSON, &asset)
    return &asset, nil
}

func main() {
    chaincode, _ := contractapi.NewChaincode(new(SmartContract))
    chaincode.Start()
}
```

---

## Key Points

1. Hyperledger Fabric is a **permissioned**, high‑performance framework.
2. Identity management is handled using **Certificate Authorities**.
3. Smart contracts are called **Chaincode**.
4. The **Orderer** provides consensus.
5. **Channels** ensure private communication between organizations.

---

## Conclusion

In this experiment, we successfully set up a private blockchain network using Hyperledger Fabric. We generated cryptographic identities, created a channel, deployed chaincode, and executed transactions. This demonstrates how Fabric enables secure, scalable, permissioned blockchain applications for enterprise environments.

---

## Viva Questions

**1. What is the difference between public and private blockchains?**
Public blockchains are open to everyone, permissionless, fully decentralized, and transparent (e.g., Ethereum, Bitcoin).
Private blockchains are permissioned, controlled by organizations, restrict access, and focus on privacy and performance (e.g., Hyperledger Fabric).

**2. What is Hyperledger Fabric used for?**
Hyperledger Fabric is an enterprise-grade, permissioned blockchain framework used to build secure, scalable, and private blockchain applications for industries like supply chain, finance, healthcare, and manufacturing.

**3. What is a peer node in Fabric?**
A peer node is a network participant that stores the ledger, runs chaincode, validates transactions, and endorses proposals. Peers maintain the distributed ledger and execute smart contract logic.

**4. What role does the Orderer play in Fabric?**
The Orderer (Ordering Service) collects transactions from peers, orders them into blocks, and delivers those blocks to all peers. It ensures consensus and maintains the correct sequence of transactions.

**5. What is Chaincode?**
Chaincode in Hyperledger Fabric is the equivalent of a smart contract. It defines the business logic, handles data validation, and runs on endorsing peers to process application-specific operations.

**6. Why do we use channels in Fabric?**
Channels enable private communication between specific network members. They allow organizations to maintain separate ledgers, ensuring confidentiality, data isolation, and controlled access within the same blockchain network.

**7. What advantages does Fabric offer over Ethereum for enterprise solutions?**
Fabric offers permissioned access, higher transaction throughput, modular architecture, private channels, pluggable consensus, and strong privacy controls — making it more suitable for enterprise applications that require confidentiality and performance.

