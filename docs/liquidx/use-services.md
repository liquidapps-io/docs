Use DAPP Network Services
==========

To utilize the DAPP Network on another chain as a developer, a two way mapping must first be established between the EOS mainnet account that is staking the DAPP to the service package and the account on the side chain that will use the services.  The point of this mapping is to verify that an account on the EOS mainnet has given permission to an account on another chain to be able to bill on the EOS mainnet account's behalf.  This mapping must also be verified on the new chain in question.

On the EOS mainnet this mapping is performed with the `addaccount` action on the account that has deployed the `liquidx` contract.  On the new chain in question, this is performed with the `setlink` action on the account that has deployed the `dappservicex` contract.

Guide:

- [Smart Contract Steps](#smart-contract-steps)
- [Add DSP on New Chain](#add-dsp-on-new-chain)
- [Map Mainnet to New Chain](#map-mainnet-to-new-chain)
- [Map New Chain to Mainnet](#map-new-chain-to-mainnet)

## Smart Contract Steps

At the smart contract level, the `liquidx` box must be unboxed and the `#define LIQUIDX` define must be added at the top of the smart contract.  In order for the compiler to know which network the contract intends to be deployed on the `/models/eosio-chains/network_name.json` file must be present.  It also must be the only file in this directory, in other words, if there are two files, the compiler will choose the first it finds and use it.  To avoid this issue, only place the chain json file you intend to use in this folder.

Example sidechain file `./models/eosio-chains/mynewchainnn.json`

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

## Add DSP on New Chain

To use a DSP on a new chain, the consumer must submit an `adddsp` command on the new chain on the account that is hosting the `dappservicex` contract.  This contract should be publicly available.

- owner {name} - name of consumer contract on new chain
- dsp {name} - dsp name on new chain

## Map Mainnet to New Chain

To map an EOS mainnet account to a new chain's account, navigate to the contract that has deployed the `liquidx` contract and perform the `addaccount` action.

- owner {name} - name of account on the EOS mainnet to link
- chain_account {name} - name of account on new chain
- chain_name {name} - account on mainnet that has registered the new chain, should be publicly available from a representative of the chain

Example cleos command:
```bash
cleos -u https://nodes.get-scatter.com:443 push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"liquidxxxxxx","name":"addaccount","data":{"owner":"natdeveloper","chain_account":"liquidxcnsmr","chain_name":"mynewchainnn"},"authorization":[{"actor":"natdeveloper","permission":"active"}]}]}'
```

## Map Mainnet to New Chain

To map a new chain's account to the EOS mainnet, navigate to the contract that has deployed the `dappservicex` contract and perform the `setlink` action.

- owner {name} - name of account on the new chain to link
- mainnet_owner {name} - name of account on mainnet

Example cleos command:
```bash
cleos -u $NEW_CHAIN_NODEOS_ENDPOINT push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"dappservicex","name":"setlink","data":{"owner":"liquidxcnsmr","mainnet_owner":"natdeveloper"},"authorization":[{"actor":"liquidxcnsmr","permission":"active"}]}]}'
```

In short you have run the `adddsp` and the `setlink` action on the new chain's `dappservicex` account and the `addaccount` action on the EOS mainet's `liquidx` account.

The new chain account now has the ability to access any service staked to by the mainnet account.