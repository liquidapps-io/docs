Use DAPP Network Services
==========

To utilize the DAPP Network on another chain as a developer, a two way mapping must first be established between the EOS mainnet account that is staking the DAPP to the service package and the account on the side chain that will use the services.  The point of this mapping is to verify that an account on the EOS mainnet has given permission to an account on another chain to be able to bill on the EOS mainnet account's behalf.  This mapping must also be verified on the new chain in question.  A mainnet account can allow multiple new chain accounts to bill for services.

On the EOS mainnet this mapping is performed with the [`addaccount`](https://bloks.io/account/liquidx.dsp?loadContract=true&tab=Actions&table=chainentry&account=liquidx.dsp&scope=CHAIN_NAME_HERE&limit=100&action=addaccount) action on the [`liquidx.dsp`](https://bloks.io/account/liquidx.dsp) account.  On the new chain in question, this is performed with the `setlink` action on the account that has deployed the `dappservicex.cpp` contract (hopefully `dappservicex` for simplicity, but any account name can be used).  To figure out what account name this is, a DSP, BP, or community member can be asked.

Guide:

- [Smart Contract Steps](#smart-contract-steps)
- [Add DSP on New Chain](#add-dsp-on-new-chain)
- [Map Mainnet to New Chain](#map-mainnet-to-new-chain)
- [Map New Chain to Mainnet](#map-new-chain-to-mainnet)

## Smart Contract Steps

At the smart contract level, the `liquidx` box must be unboxed and `#define LIQUIDX` must be added at the top of the smart contract which uses the DAPP Network services.  In order for the compiler to know which network the contract intends to be deployed on the `--sidechain` flag must be passed to `zeus compile --sidechain $SIDE_CHAIN_NAME`.

Ensure that you add `@eosio.code` to the active permission level of the account.  This can be done with the `--add-code` flag on the `cleos set account permission` command.

The side chain name is the account on the EOS mainnet that has registered the chain.  You may find what this contract is by asking a DSP, a BP, or the chain team itself.

2 files must be added.  One to the `/zeus_boxes/liquidx/models/eosio-chains/` directory and one to the `/zeus_boxes/liquidx/models/liquidx-mappings/` directory.

`/zeus_boxes/liquidx/models/liquidx-mappings/sidechain_name.dappservices.json` - this maps the dappservices account on the mainnet to the dappservicex account name on the new chain

- sidechain_name - EOS mainnet account that has registered the chain
- mainnet_account - dappservices account on EOS mainnet
- chain_account - dappservicex account on new chain - enter the sidechain_name as the scope for the [`chainentry`](https://bloks.io/account/liquidx.dsp?loadContract=true&tab=Tables&table=chainentry&account=liquidx.dsp&scope=CHAIN_NAME_HERE&limit=100) table, the `dappservices_contract` key will list the account name needed

```json
{
    "sidechain_name":"",
    "mainnet_account":"dappservices",
    "chain_account":""
}
```

`/zeus_boxes/liquidx/models/eosio-chains/${CHAIN_ACCOUNT}.json` - this maps the chain's configuration details

 - dsp_port - port DSP gateway runs on
 - webhook_dapp_port - webhook port
 - nodeos_host - nodeos host address
 - nodeos_port - nodeos port
 - secured - bool true/false for http/https for the nodeos address
 - nodeos_state_history_port - port for nodeos state history websocket
 - nodeos_p2p_port - nodeos peer 2 peer port
 - nodeos_endpoint - full nodeos endpoint
 - demux_port - demux port
 - name - name of the chain account
 - local - bool whether chain is local or not

```json
{
    "dsp_port":13016,
    "webhook_dapp_port": 8813,
    "nodeos_host":"localhost",
    "nodeos_port":2424,
    "secured":false,
    "nodeos_state_history_port":12341,
    "nodeos_p2p_port":12451,
    "nodeos_endpoint":"http://localhost:2424",
    "demux_port":1232,
    "name":"CHAIN_ACCOUNT_HERE",
    "local":true
}
```

```bash
npm i -g @liquidapps/zeus-cmd
mkir liquidx; cd liquidx
zeus unbox liquidx
export MY_CONTRACT_NAME=
zeus create contract $MY_CONTRACT_NAME
cd zeus_boxes/contracts/eos
# edit MY_CONTRACT_NAME.cpp
cd ../../../
export SIDE_CHAIN_NAME=
zeus compile --sidechain $SIDE_CHAIN_NAME
cd zeus_boxes/contracts/eos
export EOS_ENDPOINT=
export ACCOUNT=
cleos -u $EOS_ENDPOINT set contract $ACCOUNT $MY_CONTRACT_NAME
```

## Add DSP on New Chain

To use a DSP on a new chain, the consumer must submit an `adddsp` command on the new chain on the account that is hosting the `dappservicex.cpp` contract.

- owner {name} - name of consumer contract on new chain
- dsp {name} - dsp name on new chain (could be different from the mainnet DSP name), [enter the mainnet DSP's account](https://bloks.io/account/liquidx.dsp?loadContract=true&tab=Tables&table=accountlink&account=liquidx.dsp&scope=MAINNET_DSP_NAME_HERE&limit=100) into the `accountlink` table on the `liquidx.dsp` contract to find this name

## Map Mainnet to New Chain

To map an EOS mainnet account to a new chain's account, perform the [`addaccount`](https://bloks.io/account/liquidx.dsp?loadContract=true&tab=Actions&account=liquidx.dsp&scope=liquidx.dsp&limit=100&action=addaccount) action on the [`liquidx.dsp`](https://bloks.io/account/liquidx.dsp) account.

- owner {name} - name of account on the EOS mainnet staked to services
- chain_account {name} - name of account on new chain to use services
- chain_name {name} - account on mainnet that has registered the new chain, should be publicly available from a representative of the chain (DSP, BP, community)

Example cleos command:
```bash
cleos -u https://nodes.get-scatter.com:443 push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"liquidx.dsp","name":"addaccount","data":{"owner":"natdeveloper","chain_account":"liquidxcnsmr","chain_name":"mynewchainnn"},"authorization":[{"actor":"natdeveloper","permission":"active"}]}]}'
```

## Map New Chain to Mainnet

To map a new chain's account to the EOS mainnet, navigate to the contract that has deployed the `dappservicex.cpp` contract and perform the `setlink` action.

- owner {name} - name of account on the new chain to link, using services
- mainnet_owner {name} - name of account on mainnet, staking to services

Example cleos command:
```bash
cleos -u $NEW_CHAIN_NODEOS_ENDPOINT push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"dappservicex","name":"setlink","data":{"owner":"liquidxcnsmr","mainnet_owner":"natdeveloper"},"authorization":[{"actor":"liquidxcnsmr","permission":"active"}]}]}'
```

In short you have run the `adddsp` and the `setlink` action on the new chain's `dappservicex` account and the `addaccount` action on the EOS mainet's [`liquidx.dsp`](https://bloks.io/account/liquidx.dsp) account.

The new chain account now has the ability to access any service staked to by the mainnet account.