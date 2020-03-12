Add a Chain to LiquidX
==========

The following steps will cover how to enable the DAPP Network on a new chain.  After performing these steps DSPs will be able to set themselves up on the new chain.

Guide:

- [Create accounts and set dappservicex contract](#create-accounts-and-set-dappservicex-contract)
- [Register chain](#register-chain)

## Create accounts and set dappservicex contract

There are two accounts that must be created to enable LiquidX.  One mainnet account to represent the chain name, and one sidechain account to handle the DAPP service logic.

EOS Mainnet Account:

- Create an account that will become the name for the chain.  A good name should be chosen that easily represents the new chain as it is used in many places.  This account will register the chain with the [`liquidx.dsp`](https://bloks.io/account/liquidx.dsp) contract on the mainnet.  This account does not have a contract set to it.

New Chain:

- dappservicex - this contract is used to add new DSPs and to create links between accounts.  This account will need to be known to DSPs and developers wishing to operate on the network.

After both accounts are created, the `dappservicex.cpp` contract must be set to the account created on the side chain.  This contract can be found in the [Zeus-sdk](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/liquidx/contracts/eos/dappservicex/dappservicex.cpp) repo, or by unboxing the `liquidx` box with the following commands:

```bash
npm i -g @liquidapps/zeus-cmd
zeus unbox liquidx
cd liquidx
zeus compile
cd contracts/eos
```

Ensure that you add `dappservicex@eosio.code` to the active permission level of the account.

## Register chain

You must execute the [`setchain`](https://bloks.io/account/liquidx.dsp?loadContract=true&tab=Tables&account=liquidx.dsp&scope=liquidx.dsp&limit=100&action=setchain) action on the [`liquidx.dsp`](https://bloks.io/account/liquidx.dsp) account on the EOS mainnet with the mainnet account created to represent the chain name.  The syntax is as follows:

- chain_name {name} - name of EOS mainnet account deploying chain, e.g., mynewchainnn
- chain_meta {chain_metadata_t} - chain data
    - is_public {bool} - whether the chain is public
    - is_single_node {bool} - whether chain is a single node
    - dappservices_contract {std::string} - account that `dappservicex.cpp` is deployed to, e.g., dappservicex
    - chain_id {std::string} - chain ID of sidechain
    - type {std::string} - type of blockchain, e.g., EOSIO
    - endpoints {std::vector<std::string>} - list of public endpoints for developers to use
    - chain_json_uri {std::vector<std::string>} - publicly available json file that declares chain statistics

Example cleos command:

```bash
cleos -u https://nodes.get-scatter.com:443 push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"mynewchainnn","name":"setchain","data":{"chain_name":"mynewchainnn","chain_meta":{"is_public":true,"is_single_node":false,"dappservices_contract":"dappservicex","chain_id":"e70aaab8997e1dfce58fbfac80cbbb8fecec7b99cf982a9444273cbc64c41473","type":"EOSIO","endpoints":[],"p2p_seeds":[],"chain_json_uri":""}},"authorization":[{"actor":"mynewchainnn","permission":"active"}]}]}'
```

After that, you must run the `init` action on the `dappservicex` contract.

- `chain_name` {name} - name of EOS mainnet account deploying chain, e.g., mynewchainnn

Example cleos command:

```bash
cleos -u https://api.jungle.alohaeos.com push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"dappservicex","name":"init","data":{"chain_name":"liquidjungle"},"authorization":[{"actor":"dappservicex","permission":"active"}]}]}'
```

And now you're setup to begin configuring DSPs and then enabling users to use DAPP Network services.  DSPs and developers will need to know the `chain_name` used on the mainnet and the account the `dappservicex.cpp` contract was set to on the new chain.

*After that*: [Become a DSP](become-a-dsp)

*After that*: [Use Services](use-services)