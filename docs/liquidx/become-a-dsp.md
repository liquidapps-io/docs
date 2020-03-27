Become a DSP on another chain
==========

LiquidX enables DSPs (DAPP Service Providers) to offer services on new chains.  All existing and newly created packages may be staked to and used by developers without any additional modifications.

To add a chain, a DSP must configure their DSP API's `config.toml` file with the sidechain's details then two way map their EOS mainnet DSP account, the account that will be staked to and will receive rewards, to their sidechain DSP account.  This is done with the [`addaccount`](https://bloks.io/account/liquidx.dsp?loadContract=true&tab=Tables&account=liquidx.dsp&scope=liquidx.dsp&limit=100&action=addaccount) action on the mainnet [`liquidx.dsp`](https://bloks.io/account/liquidx.dsp) account and `adddsp` on the `dappservicex.cpp` contract on the new chain.  As a note, the `dappservicex.cpp` contract's account can be called anything (hopefully `dappservicex` for simplicity), so the name must be found from the community.

To add a chain from an architecture perspective requires adding a new nodeos instance for that chain and having another demux instance running on the DSP's API endpoint.  The nodeos instance can be run external to the DSP API.  There are two additional log files produced for the new chain.  A new dapp-service-node log file for the new gateway port and another demux log file.

See list of example chains to add [here](example-chains).

Guide:

- [Editing config.toml file](#editing-config.toml-file)
- [Push DSP account mapping action](#push-dsp-account-mapping-action)

## Editing config.toml file

In order to enable a new chain, a DSP must add the following to the `config.toml` environment variable file.  This is the file that holds the environment variables for your DSP's API instance.  Each sidechain will need a new `[sidechains.CHAIN_NAME]` section. `CHAIN_NAME` - this is the account on the EOS mainnet that has registered with the [`liquidx.dsp`](https://bloks.io/account/liquidx.dsp) contract as the chain's name.  This name can be found from the block producers of the chain, other DSPs, or the community.

```bash
# sidechain section, if no sidechains, leave as [sidechains], can add additional sidechains with [sidechains.newchain] ...
[sidechains]
  [sidechains.test1]
    # dsp
    dsp_port = 3116 # dsp port to run new chain's services on, this is the port developers will push to, must be unique per new chain
    dsp_account = "test1" # DSP Account on new chain
    dsp_private_key = "" # DSP active private key on new chain
    # nodeos
    nodeos_host = "localhost" # nodeos host running new chain
    nodeos_port = 8888 # nodeos host port
    nodeos_secured = false # nodeos secured bool (true: https, false: http)
    nodeos_chainid = "" # chainid of new chain
    nodeos_websocket_port = 8887 # nodeos websocket port, can be same per nodeos instance
    nodoes_latest = true # if using 2.0+ version of nodeos, true, if less than 2.0, false
    webhook_dapp_port = 8113 # nodeos webhook port, must be unique per chain
    # demux
    demux_webhook_port = 3196 # port demux runs on, must be unique per new chain
    demux_socket_mode = "sub"
    demux_head_block = 1 # head block to sync demux from
    # demux defaults to first pulling head block from toml file if postgres database is not set
    # after database is set, defaults to database, to overwrite and default to toml file, pass true
    demux_bypass_database_head_block = false
    demux_max_pending_messages = 500 # amount of pending messages to add to the stack before disconnecting the demux websocket to allow pending messages to process
    # sidechain 
    liquidx_contract = "liquidx.dsp" # liquidx contract on the EOS mainnet
    name = "test1" # CHAIN_NAME - contract on the EOS mainnet that registered the new chain
    # the mapping below contains the dappservices:dappservicex and mainnet DSP account to the new chain's DSP account mapping
    mapping = "dappservices:dappservicex,heliosselene:heliosselene"
  # [sidechains.ANOTHER_CHAIN_NAME]
    # ...
```

Once this has been configured, the environment variables for the DSP can be updated with:

```bash
systemctl stop dsp
setup-dsp
systemctl start dsp
```

You can also upgrade to the latest version of the DSP software by following the steps: [here](../dsps/upgrade).

## Push DSP account mapping action

On the EOS mainnet, the EOS mainnet's DSP account must be connected to the new chain's DSP account.  This is done using the [`addaccount`](https://bloks.io/account/liquidx.dsp?loadContract=true&tab=Actions&account=liquidx.dsp&scope=liquidx.dsp&limit=100&action=addaccount) command on the [`liquidx.dsp`](https://bloks.io/account/liquidx.dsp) account.

- owner {name} - DSP account name on the EOS mainnet
- chain_account {name} - DSP account name on the new chain
- chain_name {name} - account name of contract on the EOS mainnet that registered the chain

Cleos example:

```bash
cleos -u https://nodes.get-scatter.com:443 push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"liquidx.dsp","name":"addaccount","data":{"owner":"uuddlrlrbass","chain_account":"uuddlrlrbass","chain_name":"mynewchainnn"},"authorization":[{"actor":"uuddlrlrbass","permission":"active"}]}]}'
```

On the new chain you must find the account that has deployed the `dappservicex.cpp` code.  This can be found by asking a BP, a member of the community, or by checking the [`chainentry`](https://bloks.io/account/liquidx.dsp?loadContract=true&tab=Tables&table=chainentry&account=liquidx.dsp&scope=CHAIN_NAME_HERE&limit=100) table on the [`liquidx.dsp`](https://bloks.io/account/liquidx.dsp) contract and providing the scope of the `chain_name` used to register in the previous step.

This will return the `chain_meta` field:

```json
{ "is_public": 1, "is_single_node": 0, "dappservices_contract": "dappservicex", "chain_id": "e70aaab8997e1dfce58fbfac80cbbb8fecec7b99cf982a9444273cbc64c41473", "type": "EOSIO", "endpoints": [], "p2p_seeds": [], "chain_json_uri": "" }
```

The `dappservices_contract` value is the name of the contract that has deployed the `dappservicex.cpp` code, `dappservicex` in this case.

Once you have that then on the new chain, submit an `adddsp` action on that account.

- owner {name} - DSP name on new chain
- dsp {name} - DSP name on mainnet

Cleos example:

```bash
cleos -u $NEW_CHAIN_NODEOS_ENDPOINT push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"dappservicex","name":"adddsp","data":{"owner":"uuddlrlrbass","dsp":"uuddlrlrbass"},"authorization":[{"actor":"uuddlrlrbass","permission":"active"}]}]}'
```

With that you have 2 way mapped your DSP account name.  On the EOS Mainnet, the DSP's account has been linked to the new chain's network.  And on the new network, the EOS Mainnet's DSP account has been verified.

With that, the DSP is ready to offer services.

*Next*: [Use Services](use-services)