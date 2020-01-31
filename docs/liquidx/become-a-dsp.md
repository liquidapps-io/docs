Become a DSP on another chain
==========

LiquidX enables DSPs (DAPP Service Providers) to offer services on new chains.  To do so, a DSP can acquire the necessary mapping files, verify the DSP's account name on the existing and new networks, and launch a DSP based on that new network.

Becoming a DSP on a new chain requires having an existing DSP account on the EOS mainnet.  The mainnet account will remain the account that claims DSP rewards.  To add a chain from an architecture perspective requires adding a new nodeos instance for that chain, enabling a new demux instance for the new nodeos instance, and opening a new port to receive services.

Guide:

- [Create Mapping Files](#create-mapping-files)
- [Push DSP account mapping action](#push-dsp-account-mapping-action)
- [Register Provider Package with Service Contracts](#register-provider-package-with-service-contracts)

## Create mapping files

There are two kinds of mappings when it comes to LiquidX, there are JSON files that must be acquired and translated into the `mapping` env variable in the `config.toml` file and there are actions that must be run on the new chain and the EOS mainnet. The mapping files should be publicly available on the new chain's Github, by API, or with IPFS.

If the file is provided with IPFS, Zeus can be used to unbox the file with:

```bash
npm i -g @liquidapps/zeus-cmd
zeus box add liquidx-mynewchainnn <URI> # URI being the IPFS URI
zeus unbox liquidx-mynewchainnn
```

The architecture of the mapping files is as follows:

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

The DSP must be aware of each of the mappings in the `eosio-chains` section.  Below is an example `config.toml` with the necessary account mappings. 

In order to enable a new chain, a DSP must add the following to the `config.toml` file.  This is the file that holds the environment variables for your DSP's API instance.  Each sidechain will need a new `[sidechains.LIQUIDX_CONTRACT_NAME]` section.

The `mapping` var can be created from the mapping files and the `name` is the `chain_account` in the mapping files

```bash
# LIQUIDX_CONTRACT_NAME - this is the name of the account that has the LiquidX code deployed to it. 

[sidechains]
  [sidechains.LIQUIDX_CONTRACT_NAME]
    # dsp
    dsp_port = 3116 # dsp port to run new chain's services on, this is the port developers will push to, must be unique per new chain
    dsp_account = "" # DSP Account on new chain
    dsp_private_key = "" # DSP active private key on new chain
    # nodeos
    nodeos_host = "" # nodeos host running new chain
    nodeos_port = 8888 # nodeos host port
    nodeos_secured = false # nodeos secured bool (true: https, false: http)
    nodeos_chainid = "" # chainid of new chain
    nodeos_websocket_port = 8887 # nodeos websocket port, can be same per nodeos instance
    webhook_dapp_port = 8113 # nodeos webhook port, must be unique per chain
    # demux
    demux_webhook_port = 3196 # webhook port for demux, must be unique per instance
    demux_backend = "state_history_plugin" # demux backend plugin
    demux_socket_mode = "sub" # demux socket mode
    demux_head_block = 1 # head block to start new chain's demux instance at
    # sidechain 
    liquidx_contract = "liquidxxxxxx" # liquidx contract on the EOS mainnet
    name = "" # contract on the EOS mainnet that registered the new chain
    mapping = "dappservices:dappservicex,cronservices:cronservices,ipfsservice1:ipfsservice1,readfndspsvc:readfndspsvc,logservices1:logservices1,oracleservic:oracleservic,accountless1:accountless1,liquidstorag:liquidstorag,authfndspsvc:authfndspsvc,vcpuservices:vcpuservices"
    # mapping of all service files (main_chain:new_chain), and the (dappservices:new_chain_dappservicex_account)
```

A further explanation of each file's key/value pairs:

Example service file mapping `./models/liquidx-mappings/mynewchainnn.ipfs.json`

- sidechain_name - account name of contract on the EOS mainnet that registered the chain
- mainnet_account - service contract on main chain
- chain_account - service contract on new chain

```json
{
  "sidechain_name": "mynewchainnn",
  "mainnet_account": "ipfsservice1",
  "chain_account": "ipfsservice1"
}
```

Example DSP file mapping `./models/liquidx-mappings/mynewchainnn.uuddlrlrbass.json`

- sidechain_name - account name of contract on the EOS mainnet that registered the chain
- mainnet_account - DSP account on main chain
- chain_account - DSP account on new chain

```json
{
    "sidechain_name":"mynewchainnn",
    "mainnet_account":"uuddlrlrbass",
    "chain_account":"uuddlrlrbass"
}
```

Example sidechain file mapping `./models/eosio-chains/mynewchainnn.json`

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

## Push DSP account mapping action

On the EOS mainnet, the EOS mainnet's DSP account must be connected to the new chain's DSP account.  This is done using the `addaccount` command on the account on the EOS mainnet that has deployed the LiquidX contract, i.e., `liquidxxxxxx`.

- owner {name} - DSP account name on the EOS mainnet
- chain_account {name} - DSP account name on the new chain
- chain_name {name} - account name of contract on the EOS mainnet that registered the chain

Cleos example:

```bash
cleos -u https://nodes.get-scatter.com:443 push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"liquidxxxxxx","name":"addaccount","data":{"owner":"uuddlrlrbass","chain_account":"uuddlrlrbass","chain_name":"mynewchainnn"},"authorization":[{"actor":"uuddlrlrbass","permission":"active"}]}]}'
```

Then on the new chain, submit an `adddsp` action on the account that is marked as the `chain_account` in the `mynewchainnn.dappservices.json` file.

- owner {name} - DSP name on new chain
- dsp {name} - DSP name on mainnet or Jungle

Cleos example:

```bash
cleos -u $NEW_CHAIN_NODEOS_ENDPOINT push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"dappservicex","name":"adddsp","data":{"owner":"uuddlrlrbass","dsp":"uuddlrlrbass"},"authorization":[{"actor":"uuddlrlrbass","permission":"active"}]}]}'
```

With that you have 2 way mapped your DSP account name.  On the EOS Mainnet, the DSP's account has been linked to the new chain's network.  And on the new network, the EOS Mainnet's DSP account has been verified.

## Register Provider Package with Service Contracts

In order to provide the same package that is provided on the EOS Mainnet, a DSP must register as a provider with each package they intend to offer with the service contract of the sidechain.  This is done with the `regprovider` action on each service contract.  The Zeus command to register a package did this in the background previously.

Here is an example of registering a provider package with the `ipfsservice1` contract:

```bash
cleos -u $NEW_CHAIN_NODEOS_ENDPOINT push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"ipfsservice1","name":"regprovider","data":{"provider":"heliosselene","model":{"model":{"cleanup_model_field":{"cost_per_action":1},"commit_model_field":{"cost_per_action":1},"warmup_model_field":{"cost_per_action":1}},"package_id":"package1"}},"authorization":[{"actor":"heliosselene","permission":"active"}]}]}'
```

With that, the DSP is ready to offer services.

*Next*: [Use Services](use-services)