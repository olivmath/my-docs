---
title: Blockchain Basics
description: Introduces common blockchain concepts, components, and terminology.
keywords:
  - blockchain
  - block
  - consensus
  - state machine
  - limitations
---

> [!WARNING]
> Some links are broken, support is on the way

A blockchain is a decentralized ledger that records information in a sequence of blocks. Each block contains an ordered set of instructions that may result in a state change.

In a blockchain network, individual computers—called nodes—communicate to form a decentralized peer-to-peer (P2P) network. There is no central authority controlling the network, and typically, each node involved in block production stores a copy of the blocks that make up the canonical chain.

Users usually interact with a blockchain by submitting requests that may lead to state changes, such as transferring funds or changing file ownership. These transaction requests are propagated across the network and assembled into blocks by a block author. To ensure data security and chain progress, nodes use consensus mechanisms to agree on the state of the data in each block and the order of transactions.

## What is a Blockchain Node?

At a high level, all blockchain nodes require the following core components:

- **Data storage** for recording state changes resulting from transactions.
- **Peer-to-peer networking** for decentralized communication between nodes.
- **Consensus methodology** to protect against malicious activity and ensure chain progress.
- **Logic** for ordering and processing incoming transactions.
- **Cryptography** for generating block hashes and signing/verifying transaction signatures.

Due to the complexity of building these components, most blockchain projects start by forking an existing blockchain codebase. For example, Bitcoin's repository was forked to create Litecoin, ZCash, and Bitcoin Cash, while Ethereum's repository was forked for Quorum, POA Network, and others.

However, most blockchain platforms are not designed for easy modification, leading to limitations such as scalability issues. Before exploring how Substrate addresses these limitations, it's important to understand the common properties shared by most blockchains.

## State Transitions and Conflicts

A blockchain is essentially a [state machine](https://en.wikipedia.org/wiki/Finite-state_machine). At any point, it has a current internal state. Transactions cause state changes, transitioning the blockchain from one state to another. However, multiple valid transitions may exist, and the blockchain must agree on a single one. All operations within a blockchain must be deterministic to ensure agreement on state transitions, including:

- The **genesis state** (initial state of the chain).
- The series of state transitions from executed transactions.
- The final state of a block to be included in the chain.

In decentralized networks, nodes see transactions in different orders, requiring an elaborate method to select transactions and resolve conflicting state transitions. This method is called the **consensus model** or **consensus algorithm**. The most common model is **proof-of-work**, where the first node to solve a computational problem earns the right to submit a block.

To ensure fault tolerance and consistency, some consensus models require at least two-thirds of nodes to agree on the state at all times. This majority ensures the network can withstand malicious behavior or network outages.

## Blockchain Economics

Blockchains require resources—processors, memory, storage, and network bandwidth—to operate. Nodes provide these resources, creating a distributed, decentralized network. To sustain the network, users typically pay transaction fees, which require user identities to be associated with accounts holding assets. These assets are often represented by tokens, which users purchase through exchanges and deposit to pay for transactions.

## Blockchain Governance

Some blockchains allow participants to submit and vote on proposals affecting network operations or the community. This on-chain governance enables a democratic process for blockchain evolution. However, participation often requires holding a significant stake of tokens or being elected as a representative.

## Applications Running on a Blockchain

Applications on a blockchain—often called **decentralized applications (dApps)**—are typically web applications with front-end frameworks and backend **smart contracts** for state changes. Smart contracts are programs that execute transactions under specific conditions, ensuring outcomes are recorded and tamper-proof. However, smart contracts operate within the fixed rules of the blockchain, limiting access to underlying functionality like consensus or storage layers.

## Where to Go Next

All blockchains share common characteristics. Substrate, a blockchain builders' toolkit, provides a modular framework for creating custom blockchains. With Substrate, you can combine or modify common blockchain components like storage, consensus, and cryptography to suit your project's needs.

Explore the following resources to learn more:

#### Tell Me

- [Learn](/learn/)
- [Welcome to Substrate](/learn/welcome-to-substrate/)
- [Architecture and Rust Libraries](/learn/architecture/)
- [Networks and Blockchains](/learn/node-and-network-types/)

#### Guide Me

- [Build a Local Blockchain](/tutorials/build-a-blockchain/build-local-blockchain/)
- [Simulate a Network](/tutorials/build-a-blockchain/simulate-network/)
- [Add Trusted Nodes](/tutorials/build-a-blockchain/add-trusted-nodes/)
