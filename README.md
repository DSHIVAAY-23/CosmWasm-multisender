# CosmWasm Multisend Contract

This is a multisend smart contracts in Rust built to run on
[Cosmos SDK](https://github.com/cosmos/cosmos-sdk) module on all chains that enable it.

## What it does

This project covers all that is required to build, deploy and interact with the smart contract .
The main parts of the project are:

- The contract (this page)
- The deployment scripts in [scripts](./scripts)
- A basic CLI client application to send and receive  as well as interact with the deployed contract: [multisender-client](./multisender-client)

Each part has its own README and instructions

# Multisend Contract

The contract is written in `Rust` and is compiled to wasm. The contract receives a list of addresses and amounts, and distributes the passed coins to these addresses.
The amount sent to the contract must be enough to cover the outgoing payments.

The contract also includes a possible fee, set on contract initialization. The fee is paid to the contract owner and must be added to the total payment received by the contract when creating a transaction.
The script in [multisender-client](./multisender-client) queries the contract and adds the required fee to the transaction.

## Prerequisites

Building the contract requires an up-to-date `Rust` version with `wasm` support.

```sh
rustup default stable
rustup target add wasm32-unknown-unknown
```

## Building the contract

Clone the contract to a new environment.

```
git clone https://github.com/DSHIVAAY-23/CosmWasm-multisender.git
```

Make sure the project compiles by running

```sh
cargo wasm
```

This will build an unoptimized version of the contract, just to make sure the compilation works. To build an optimized version that can be uploaded to the blockchain, run

```sh
docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.10.3
```

More instructions on building and developing the contract in [Developing](./Developing.md)


## Contract use cases and limitations

The contract supports sending multiple payments simultaneously via a `MsgMultiSend`. Still, the contract could be useful for several use cases:

- Integration with other smart contracts
- Sending ERC20/CW20 tokens (in future versions)
- Collecting payment for the provided service

The contract can be also easily extended to perform additional tasks, such as swapping tokens to the required coin etc.



## Misc

[Publishing](./Publishing.md) contains useful information on how to publish your
contract, once you are ready to deploy it on a running blockchain.
[Importing](./Importing.md) contains information about pulling in other contracts or crates
that have been published.
