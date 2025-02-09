---
title: Simulate a Network
description: Start a private blockchain network using predefined accounts as authorized validators.
keywords:
  - blockchain
  - node
  - validators
  - private networks
---

> [!WARNING]
> Some links are broken, support is on the way.

This tutorial provides a basic introduction to starting a private blockchain network with an **authority set** of private **validators**.

The Substrate Node Template uses an authority consensus model that limits block production to a rotating list of authorized accounts. These authorized accounts—**authorities**—are responsible for creating blocks in a round-robin fashion.

In this tutorial, you'll simulate a network by using two predefined accounts (`alice` and `bob`) as the authorities. Both nodes will run on a single computer but use different accounts and keys.

## Before You Begin

Before starting, verify the following:

- You have configured your environment for Substrate development by installing [Rust and the Rust toolchain](/install/).
- You have completed the [Build a Local Blockchain](/tutorials/build-a-blockchain/build-local-blockchain/) tutorial and have the Substrate Node Template installed locally.
- You are familiar with software development and command-line interfaces.
- You have a basic understanding of blockchains and smart contract platforms.

## Tutorial Objectives

By completing this tutorial, you will:

- Start a blockchain node using a predefined account.
- Learn key command-line options for starting a node.
- Determine if a node is running and producing blocks.
- Connect a second node to a running network.
- Verify that peers produce and finalize blocks.

## Start the First Blockchain Node

To simulate a private network, you'll run two Substrate nodes on a single computer using predefined accounts (`alice` and `bob`). Start by running the first node with the `alice` account.

To start the blockchain:

1. Open a terminal shell on your computer.
2. Navigate to the root directory where you compiled the Substrate Node Template.
3. Purge old chain data by running:

```bash
./target/release/node-template purge-chain --base-path /tmp/alice --chain local
```

Confirm the operation by typing `y` when prompted.

4. Start the local blockchain node using the `alice` account:

```bash
./target/release/node-template \
--base-path /tmp/alice \
--chain local \
--alice \
--port 30333 \
--rpc-port 9945 \
--node-key 0000000000000000000000000000000000000000000000000000000000000001 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator
```

### Review the Command-Line Options

Here’s a breakdown of the key options used to start the node:

| **Option**            | **Description**                                                                                                                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--base-path`          | Specifies the directory for storing chain data.                                                                                                                                                           |
| `--chain local`        | Specifies the chain specification to use (`local`, `development`, or `staging`).                                                                                                                          |
| `--alice`              | Adds the predefined keys for the `alice` account to the node's keystore. This account is used for block production and finalization.                                                                      |
| `--port 30333`         | Specifies the port for peer-to-peer (`p2p`) traffic. Since both nodes run on the same computer, you must specify different ports for each node.                                                            |
| `--rpc-port 9945`      | Specifies the port for JSON-RPC traffic via WebSocket and HTTP. The default port is `9944`.                                                                                                               |
| `--node-key <key>`     | Specifies the Ed25519 secret key for `libp2p` networking. Use this option only for development and testing.                                                                                               |
| `--telemetry-url`      | Specifies where to send telemetry data. This tutorial uses a Parity-hosted server.                                                                                                                        |
| `--validator`          | Specifies that this node participates in block production and finalization.                                                                                                                               |

For more details on available options, run:

```bash
./target/release/node-template --help
```

### Review the Node Output

If the node starts successfully, the terminal displays messages describing network operations. For example:

```text
2022-08-16 15:29:55 Substrate Node    
2022-08-16 15:29:55 ✌️  version 4.0.0-dev-de262935ede    
2022-08-16 15:29:55 ❤️  by Substrate DevHub <https://github.com/substrate-developer-hub>, 2017-2022    
2022-08-16 15:29:55 📋 Chain specification: Local Testnet    
2022-08-16 15:29:55 🏷  Node name: Alice    
2022-08-16 15:29:55 👤 Role: AUTHORITY    
2022-08-16 15:29:55 💾 Database: RocksDb at /tmp/alice/chains/local_testnet/db/full    
2022-08-16 15:29:55 ⛓  Native runtime: node-template-100 (node-template-1.tx1.au1)    
2022-08-16 15:29:55 🔨 Initializing Genesis block/state (state: 0x6894…033d, header-hash: 0x2cdc…a07f)    
2022-08-16 15:29:55 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.    
2022-08-16 15:29:56 Using default protocol ID "sup" because none is configured in the chain specs    
2022-08-16 15:29:56 🏷  Local node identity is: 12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp    
2022-08-16 15:29:56 💻 Operating system: macos    
2022-08-16 15:29:56 💻 CPU architecture: x86_64    
2022-08-16 15:29:56 📦 Highest known block at #0    
2022-08-16 15:29:56 〽️ Prometheus exporter started at 127.0.0.1:9615    
2022-08-16 15:29:56 Running JSON-RPC server: addr=127.0.0.1:9945, allowed origins=Some(["http://localhost:*", "http://127.0.0.1:*", "https://localhost:*", "https://127.0.0.1:*", "https://polkadot.js.org"])      
2022-08-16 15:29:56 creating instance on iface 192.168.1.125    
2022-08-16 15:30:01 💤 Idle (0 peers), best: #0 (0x2cdc…a07f), finalized #0 (0x2cdc…a07f), ⬇ 0 ⬆ 0
...
```

Key messages to note:

- `🔨 Initializing Genesis block/state` identifies the genesis block. Verify this matches when starting the second node.
- `🏷 Local node identity is: 12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp` uniquely identifies this node using the `--node-key`.
- `💤 Idle (0 peers)` indicates no other nodes are connected, and no blocks are being produced.

## Add a Second Node to the Network

Now that the first node is running, add a second node using the `bob` account. This node will join the existing network.

To add the second node:

1. Open a **new** terminal shell.
2. Navigate to the root directory of the Substrate Node Template.
3. Purge old chain data:

```bash
./target/release/node-template purge-chain --base-path /tmp/bob --chain local -y
```

The `-y` flag skips the confirmation prompt.

4. Start the second node using the `bob` account:

```bash
./target/release/node-template \
--base-path /tmp/bob \
--chain local \
--bob \
--port 30334 \
--rpc-port 9946 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp
```

Key differences from the first command:

- Different `--base-path`, `--port`, and `--rpc-port` values.
- The `--bootnodes` option specifies the first node (`alice`) as the boot node.

The `--bootnodes` option includes:

- `ip4` for IPv4 format.
- `127.0.0.1` for the local IP address.
- `tcp` for the protocol.
- `30333` for the port.
- `12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp` for the node identifier.

## Verify Blocks Are Produced and Finalized

After starting the second node, the nodes should connect as peers and start producing blocks.

To verify block production:

1. Check the terminal where the first node is running. You should see output similar to:

```text
2022-08-16 15:32:33 discovered: 12D3KooWBCbmQovz78Hq7MzPxdx9d1gZzXMsn6HtWj29bW51YUKB /ip4/127.0.0.1/tcp/30334
2022-08-16 15:32:33 discovered: 12D3KooWBCbmQovz78Hq7MzPxdx9d1gZzXMsn6HtWj29bW51YUKB /ip6/::1/tcp/30334
2022-08-16 15:32:36 🙌 Starting consensus session on top of parent 0x2cdce15d31548063e89e10bd201faa63c623023bbc320346b9580ed3c40fa07f
2022-08-16 15:32:36 🎁 Prepared block for proposing at 1 (5 ms) [hash: 0x9ab34110e4617454da33a3616efc394eb1ce95ee4bf0daab69aa4cb392d4104b; parent_hash: 0x2cdc…a07f; extrinsics (1): [0x4634…cebf]] 
2022-08-16 15:32:36 🔖 Pre-sealed block for proposal at 1. Hash now 0xf0869a5cb8ebd0fcc5f2bc194ced84ca782d9749604e888c8b9b515517179847, previously 0x9ab34110e4617454da33a3616efc394eb1ce95ee4bf0daab69aa4cb392d4104b.
2022-08-16 15:32:36 ✨ Imported #1 (0xf086…9847)
2022-08-16 15:32:36 💤 Idle (1 peers), best: #1 (0xf086…9847), finalized #0 (0x2cdc…a07f), ⬇ 1.0kiB/s ⬆ 1.0kiB/s
2022-08-16 15:32:41 💤 Idle (1 peers), best: #1 (0xf086…9847), finalized #0 (0x2cdc…a07f), ⬇ 0.6kiB/s ⬆ 0.6kiB/s
2022-08-16 15:32:42 ✨ Imported #2 (0x0d5e…2a7f)
2022-08-16 15:32:46 💤 Idle (1 peers), best: #2 (0x0d5e…2a7f), finalized #0 (0x2cdc…a07f), ⬇ 0.6kiB/s ⬆ 0.6kiB/s
2022-08-16 15:32:48 🙌 Starting consensus session on top of parent 0x0d5ef31979c2aa17fb88497018206d3057151119337293fe85d9526ebd1e2a7f
2022-08-16 15:32:48 🎁 Prepared block for proposing at 3 (0 ms) [hash: 0xa307c0112bce39e0dc689132452154da2079a27375b44c4d94790b46a601346a; parent_hash: 0x0d5e…2a7f; extrinsics (1): [0x63cc…39a6]]    
2022-08-16 15:32:48 🔖 Pre-sealed block for proposal at 3. Hash now 0x0c55670e745dd12892c9e7d5205085a78ccea98df393a822fa9b3865cfb3d51b, previously 0xa307c0112bce39e0dc689132452154da2079a27375b44c4d94790b46a601346a.
2022-08-16 15:32:48 ✨ Imported #3 (0x0c55…d51b)
2022-08-16 15:32:51 💤 Idle (1 peers), best: #3 (0x0c55…d51b), finalized #1 (0xf086…9847), ⬇ 0.7kiB/s ⬆ 0.9kiB/s    
...
```

Key details:

- The second node is discovered (`12D3KooWBCbmQovz78Hq7MzPxdx9d1gZzXMsn6HtWj29bW51YUKB`).
- The node has one peer (`1 peers`).
- Blocks are being produced (`best: #3 (0x0c55…d51b)`).
- Blocks are being finalized (`finalized #1 (0xf086…9847)`).

2. Verify similar output in the terminal where the second node is running.

3. Shut down one of the nodes by pressing `Control-c`. The remaining node will stop producing blocks:

```text
2022-08-16 15:53:45 💤 Idle (1 peers), best: #143 (0x8f11…1684), finalized #141 (0x5fe3…5a25), ⬇ 0.8kiB/s ⬆ 0.7kiB/s
2022-08-16 15:53:50 💤 Idle (0 peers), best: #143 (0x8f11…1684), finalized #141 (0x5fe3…5a25), ⬇ 83 B/s ⬆ 83 B/s
```

4. Shut down the second node by pressing `Control-c`.

To remove the chain state, use the `purge-chain` subcommand with the `--base-path` options for `/tmp/bob` and `/tmp/alice`.

## Next Steps

In this tutorial, you learned how to:

- Use key command-line options to start a node.
- Start two blockchain nodes that communicate as peers.
- Verify block production and finalization.

The next tutorial, [Add Trusted Nodes](/tutorials/build-a-blockchain/add-trusted-nodes/), will teach you how to:

- Generate your own secret key pairs.
- Create a custom chain specification.
- Add validators to a private network.

If you encounter any issues, feel free to:

- [Submit an issue](https://github.com/substrate-developer-hub/substrate-docs/issues/new/choose).
- Ask questions on [Substrate Stack Exchange](https://substrate.stackexchange.com/).
