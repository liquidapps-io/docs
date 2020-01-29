Add a Chain to LiquidX
==========

The following steps will cover how to enable the DAPP Network on a new chain.  We'll start by creating the necessary accounts and loading the contracts.  Then we'll create a series of mappings, both on chain and in JSON.  Finally after running a few actions, the new chain will be ready for DSPs to set themselves up.

Guide:

- [Create accounts and set contracts](#create-accounts-and-set-contracts)
- [Creating mapping files for services](#creating-mapping-files-for-services)
- [Creating mapping files for DSPs](#creating-mapping-files-for-dsps)
- [Creating mapping file for Chain](#creating-mapping-file-for-chain)
- [Register chain](#register-chain)
- [Create service contract mappings](#create-service-contract-mappings)

## Create accounts and set contracts

There are a handful of accounts that must be created on the new chain. Note that you do not have to use the names provided, but it may make more sense for everyone if you did.

EOS Mainnet Account:

    - Create an account that will become the name for the chain.  A good name should be chosen that easily represents the new chain as it is used in many places.

New Chain:

    DAPP Service Contract:

    - dappservicex - this contract is used to add new DSPs and to create links between accounts

    Service Contracts (service contract account name - service contract file name - service contract pretty name):

    - ipfsservice1 - ipfsservice - ipfs
    - cronservices - cronservice - cron
    - readfndspsvc - readfnservice - readfn
    - logservices1 - logservice - log
    - oracleservic - oracleservice - oracle
    - accountless1 - vaccountsservice - vaccounts
    - liquidstorag - storageservice - storage
    - authfndspsvc - authservice - auth
    - vcpuservices - vcpuservice - vcpu

After each account is created, the associated contract must be uploaded to each account.  All contracts may be found by unboxing the `liquidx` box from zeus.

```bash
npm i -g @liquidapps/zeus-cmd
zeus unbox liquidx
cd liquidx
zeus compile
cd contracts/eos
```

## Creating mapping files for services

Within the `/models/liquidx-mappings` directory, there are a series of mapping files.  A mapping file for each of the above accounts (dappservicex mapping and service contract mappings) will need to be created for the new chain.  

Here is an example:

File name: `mynewchainnn.ipfs.json` - <sidechain_name><service_pretty_name>.json

- sidechain_name - name of sidechain to add liquidx to (`mynewchainnn` in this example), name of account liquidx contract on mainnet is deployed to
- service_pretty_name - the name found in the `zeus-sdk/boxes/groups/services/${SERVICE}/models/dapp-services/${service_pretty_name}.json`

_For example: [https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/models/dapp-services/ipfs.json#L2](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/models/dapp-services/ipfs.json#L2)_

- mainnet_account - account on EOS mainnet
- chain_account - account on side chain (assuming you got the account above, if not, use the account you created)

_For example, if ipfsservice1 was taken, but you were able to create ipfsservice2, ipfsservice2 would be the `chain_account`_

```json
{
    "sidechain_name":"mynewchainnn",
    "mainnet_account":"ipfsservice1",
    "chain_account":"ipfsservice1"
}
```

## Creating mapping files for DSPs

Using the same syntax, each DSP needs their own mapping file to be stored in the same `./models/liquidx-mappings` directory where the DSP API software is running.

File name: `mynewchainnn.DSP_NAME_HERE.json`

```json
{
    "sidechain_name":"mynewchainnn",
    "mainnet_account":"uuddlrlrbass",
    "chain_account":"uuddlrlrbass"
}
```

## Creating mapping file for Chain

The new chain itself needs its own mapping file to be stored in  `/models/eosio-chains` directory.  This mapping file represents a DSP operating on the new chain.

File name: `mynewchainnn.json`

- dsp_port - port of a DSP API's on the new chain, default 3115 (must be unique per chain)
- nodeos_host - host address of nodeos endpoint
- nodeos_port - port of nodeos endpoint
- secured - true or false (true = https, false = http)
- nodeos_state_history_port - state history port endpoint for nodeos instance
- nodeos_p2p_port - port nodeos is listening on for peers
- nodeos_endpoint - full nodeos endpoint (secured + host + port), "http://localhost:8888"
- demux_port - port demux is running on, default 3195 (must be unique per chain)
- name - mainnet account name used to register chain
- local - bool whether new chain is local or not

```json
{
    "dsp_port":3116,
    "nodeos_host":"localhost",
    "nodeos_port":8888,
    "secured":false,
    "nodeos_state_history_port":8887,
    "nodeos_p2p_port":9876,
    "nodeos_endpoint":"http://localhost:8888",
    "demux_port":3196,
    "name":"mynewchainnn",
    "local":false
}
```

## Register chain

You must execute the `setchain` action on the account that has deployed the `liquidx` code on the eos mainnet.  The syntax is as follows:

- chain_name {name} - name of account deploying chain, e.g., mynewchainnn
- chain_meta {chain_metadata_t} - chain data
    - is_public {bool} - whether the chain is public
    - is_single_node {bool} - whether chain is a single node
    - dappservices_contract {std::string} - contract that dappservicex is deployed to, e.g., dappservicex
    - chain_id {std::string} - chain ID of sidechain
    - type {std::string} - type of blockchain, e.g., EOSIO
    - endpoints {std::vector<std::string>} - list of public endpoints for developers to use
    - chain_json_uri {std::vector<std::string>} - publicly available json file that declares chain statistics

Example cleos command:

```bash
cleos -u https://nodes.get-scatter.com:443 push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"mynewchainnn","name":"setchain","data":{"chain_name":"mynewchainnn","chain_meta":{"is_public":true,"is_single_node":false,"dappservices_contract":"dappservicex","chain_id":"e70aaab8997e1dfce58fbfac80cbbb8fecec7b99cf982a9444273cbc64c41473","type":"EOSIO","endpoints":[],"p2p_seeds":[],"chain_json_uri":""}},"authorization":[{"actor":"mynewchainnn","permission":"active"}]}]}'
```

## Create service contract mappings

On the new chain you will need to run the `setlink` command for each service you created above on the account that has deployed the `dappservicex` contract.  The `setlink` command represents a 1 way mapping between the new chain's service contract account and the mainnet service contract account.

- `owner` - service contract name on new chain, e.g., `ipfsservice1`
- `mainnet_owner` - mainnet version of service contract name, e.g., `ipfsservice1`

Example cleos command:

```bash
cleos -u $NEW_CHAIN_NODEOS_ENDPOINT push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"dappservicex","name":"setlink","data":{"owner":"ipfsservice1","mainnet_owner":"ipfsservice1"},"authorization":[{"actor":"ipfsservice1","permission":"active"}]}]}'
```

Once that's done. The mapping files need to be publicly published. Here's what the finished file hierarchy should look like:

```js
liquidx-mynewchainnn // root box name
└── models // models folder
    ├── liquidx-mappings // generated after npm install
    │   └── mynewchainnn.json // chain config file
    └── eosio-chains // where chain config file should be
        ├── mynewchainnn.dappservices.json // file mapping mainnet dappservices contract to account name dappservicex contract is deployed to
        ├── mynewchainnn.dsp1.json // file mapping mainnet DSP account name to mynewchainnn DSP account name
        ├── mynewchainnn.dsp2.json // another one
        ├── mynewchainnn.ipfs.json // file mapping mainnet vRAM service contract account name to mynewchainnn vRAM service contract  account name
        ├── mynewchainnn.cron.json // file mapping mainnet cron service contract account name to mynewchainnn cron service contract  account name
        ├── mynewchainnn.readfn.json // file mapping mainnet read function service contract account name to mynewchainnn read function service contract  account name
        ├── mynewchainnn.log.json // file mapping mainnet log service contract account name to mynewchainnn log service contract  account name
        ├── mynewchainnn.oracle.json // file mapping mainnet LiquidHarmony (oracle) service contract account name to mynewchainnn LiquidHarmony (oracle) service contract  account name
        ├── mynewchainnn.vaccounts.json // file mapping mainnet LiquidAccounts service contract account name to mynewchainnn LiquidAccounts service contract  account name
        ├── mynewchainnn.storage.json // file mapping mainnet LiquidStorage service contract account name to mynewchainnn LiquidStorage service contract  account name
        ├── mynewchainnn.auth.json // file mapping mainnet LiquidAuth service contract account name to mynewchainnn LiquidAuth service contract  account name
        ├── mynewchainnn.vcpu.json // file mapping mainnet vCPU service contract account name to mynewchainnn vCPU service contract  account name
        ├── mynewchainnn.storage.json // file mapping mainnet LiquidStorage service contract account name to mynewchainnn LiquidStorage service contract  account name
```

The files can be published publicly on Github, through a publicly available API, or with IPFS.

To publish to IPFS with Zeus:

```bash
cd liquidx-mynewchainnn
zeus deploy box --type=ipfs
```

This will provide a URI that you can make publicly available.  With it, a developer can run the following commands to unbox the files as long as the IPFS node is available:

```bash
zeus box add liquidx-mynewchainnn <URI>
zeus unbox liquidx-mynewchainnn
```

And now you're setup to begin configuring DSPs and then enabling users to use DAPP Network services.

*Next*: [Become a DSP](become-a-dsp)
*After that*: [Use Services](use-services)