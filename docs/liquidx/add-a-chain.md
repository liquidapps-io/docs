Add a Chain to LiquidX
==========

The following steps will cover how to enable the DAPP Network on a new chain.  We'll start by creating the necessary accounts and loading the contracts.  Then we'll create a series of mappings, both on chain and in JSON.  Finally after running a few actions, you're good to go.

Guide:

- [Create accounts and set contracts](#create-accounts-and-set-contracts)
- [Creating mapping files for services](#creating-mapping-files-for-services)
- [Creating mapping files for DSPs](#creating-mapping-files-for-dsps)
- [Creating mapping file for Chain](#creating-mapping-file-for-chain)
- [Create accounts and set contracts](#create-accounts-and-set-contracts)
- [Register chain](#register-chain)
- [Create service contract mappings](#create-service-contract-mappings)

## Create accounts and set contracts
There are a handlful of accounts that must be created on the new chain.  Accounts can be created with the account creator: [https://monitor.jungletestnet.io/#account](https://monitor.jungletestnet.io/#account).  Note that you do not have to use these names, but it may make more sense for everyone if you tried to.

account name - contract name - pretty name

```bash
# for key creation
cleos create key --to-console
```

Mainnet:

- This account name is intended to represent the new chain - liquidx

_For example I used `liquidjungle` to link Kylin <> Jungle_

New Chain:

- dappservicex - dappservicex
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

Set each contract, will need to hit up the faucet: [https://monitor.jungletestnet.io/#faucet](https://monitor.jungletestnet.io/#faucet).
```bash
# rinse wash repeat
export JUNGLE_TEST_ACCOUNT=
# active key
export JUNGLE_TEST_PUBLIC_KEY=
export CONTRACT_NAME=
export EOS_ENDPOINT=http://jungle.atticlab.net:8888

cleos -u $EOS_ENDPOINT system buyram $JUNGLE_TEST_ACCOUNT $JUNGLE_TEST_ACCOUNT "50.0000 EOS" -p $JUNGLE_TEST_ACCOUNT@active
cleos -u $EOS_ENDPOINT system delegatebw $JUNGLE_TEST_ACCOUNT $JUNGLE_TEST_ACCOUNT "10.0000 EOS" "40.0000 EOS" -p $JUNGLE_TEST_ACCOUNT@active
cleos -u $EOS_ENDPOINT set account permission $JUNGLE_TEST_ACCOUNT active "{\"threshold\":1,\"keys\":[{\"weight\":1,\"key\":\"$JUNGLE_TEST_PUBLIC_KEY\"}],\"accounts\":[{\"permission\":{\"actor\":\"$JUNGLE_TEST_ACCOUNT\",\"permission\":\"eosio.code\"},\"weight\":1}]}" owner -p $JUNGLE_TEST_ACCOUNT@active
cleos -u $EOS_ENDPOINT set contract $JUNGLE_TEST_ACCOUNT $CONTRACT_NAME
```

## Creating mapping files for services

Within the `/models/liquidx-mappings` directory, you will see a series of mapping files.  You will need to create a mapping file for each of the accounts above on the new chain.

Here is an example:

File name: `liquidjungle.ipfs.service.json` - <sidechain_name><service_pretty_name>.service.json

- sidechain_name - name of sidechain to add liquidx to, name of account liquidx contract on mainnet is deployed to
- service_pretty_name - the name found in the `zeus-sdk/boxes/groups/services/${SERVICE}/models/dapp-services/${service_pretty_name}.json`

_For example: [https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/models/dapp-services/ipfs.json#L2](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/models/dapp-services/ipfs.json#L2)_

- mainnet_account - account on EOS mainnet
- chain_account - account on side chain (assuming you got the account above, if not, use the account you got)

_For example, if ipfsservice1 was taken, but you got ipfsservice2, ipfsservice2 would be the `chain_account`_

```json
{
    "sidechain_name":"liquidjungle",
    "mainnet_account":"ipfsservice1",
    "chain_account":"ipfsservice1"
}
```

## Creating mapping files for DSPs

Using the same syntax, each DSP needs their own mapping file to be stored in the same `/models/liquidx-mappings` directory.

File name: `liquidjungle.DSP_NAME_HERE.json`

```json
{
    "sidechain_name":"liquidjungle",
    "mainnet_account":"uuddlrlrbass",
    "chain_account":"uuddlrlrbass"
}
```

## Creating mapping file for Chain

The new chain itself needs its own mapping file to be stored in  `/models/local-sidechains` directory.  This mapping file represents a DSP operating on the new chain.

File name: `liquidjungle.json`

```json
{
    "dsp_port":3115,
    "nodeos_port":8888,
    "nodeos_endpoint":"http://localhost:8888",
    "nodeos_host":"localhost",
    "nodeos_state_history_port":8887,
    "nodeos_p2p_port":9876,
    "demux_port":3195,
    "name":"liquidjungle"
}
```

## Register chain

You must execute the `setchain` action on the account that has deployed the `liquidx` code on the eos mainnet.  The syntax is as follows:

- chain_name {name} - name of account deploying chain, e.g., liquidjungle
- chain_meta {chain_metadata_t} - chain data
    - is_public {bool} - whether the chain is public
    - is_single_node {bool} - whether chain is a single node
    - dappservices_contract {std::string} - 
    - chain_id {std::string} - chain ID of sidechain
    - type {std::string} - type of blockchain, e.g., EOSIO
    - endpoints {std::vector<std::string>} - list of public endpoints for developers to use
    - type {std::string} - type of blockchain, e.g., EOSIO
    - chain_json_uri {std::vector<std::string>} - publicly available json file that declares chain statistics

Example cleos command:

```bash
cleos -u https://kylin.eos.dfuse.io push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"liquidjungle","name":"setchain","data":{"chain_name":"liquidjungle","chain_meta":{"is_public":true,"is_single_node":false,"dappservices_contract":"dappservicex","chain_id":"e70aaab8997e1dfce58fbfac80cbbb8fecec7b99cf982a9444273cbc64c41473","type":"EOSIO","endpoints":[],"p2p_seeds":[],"chain_json_uri":""}},"authorization":[{"actor":"liquidjungle","permission":"active"}]}]}'
```

## Create service contract mappings

On the new chain you will need to run the `setlink` command for each service you created above.  The `setlink` command represents a 1 way mapping, only a 1 way mapping is required for the service contracts.

- `owner` - service contract name on new chain, e.g., `ipfsservice1`
- `mainnet_owner` - mainnet version of service contract name, e.g., `ipfsservice1`

Example cleos command:

```bash
cleos -u https://api.jungle.alohaeos.com push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"dappservicex","name":"setlink","data":{"owner":"ipfsservice1","mainnet_owner":"ipfsservice1"},"authorization":[{"actor":"ipfsservice1","permission":"active"}]}]}'
```

And now you're setup to begin configuring DSPs and then enabling user to use DAPP Network services.