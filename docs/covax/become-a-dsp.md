Become a DAPP Service Provider
==========

The CoVax chain utilizes the LiquidX technology to enable DAPP Network services to be provided on EOSIO based chains.  To read more on LiquidX, please [see the LiquidX section](../liquidx/getting-started).  To learn more about setting up a DAPP Service Provider, see the [getting started section](../dsps/getting-started).

To obtain a DAPP Service Provider account on CoVax, reach out in the CoVax Telegram channel: [https://t.me/CoVaxApp](https://t.me/CoVaxApp).

Guide:

- [Update config.toml file](#update-config-toml-file) update config.toml environment variable file with the CoVax chain sidechain section
- [Push DSP account mapping action on EOS mainnet](#push-dsp-account-mapping-action-on-eos-mainnet) run `adddsp` action on the `dappservicex` contract on the CoVax chain to link the CoVax chain DSP account to your EOS mainnet DSP account
- [Push DSP account mapping action on CoVax Chain](#push-dsp-account-mapping-action-on-covax-chain) run the `addaccount` action on the `liquidx.dsp` contract on the EOS mainnet to link the EOS mainnet DSP account to the CoVax chain DSP account

## Update config.toml file

The `config.toml` file is the environment variable file used for DAPP Service Providers.

```bash
[sidechains]
  [sidechains.liquidxcovax]
    # dsp
    dsp_port = 3116 # dsp port to run new chain's services on, this is the port developers will push to, must be unique per new chain
    dsp_account = "" # DSP Account on new chain
    dsp_private_key = "" # DSP active private key on new chain
    # nodeos
    nodeos_host = "" # state history nodeos host running new chain
    nodeos_port = 8888 # nodeos host port
    nodeos_secured = false # nodeos secured bool (true: https, false: http)
    nodeos_chainid = "63788f6e75cdb4ec9d8bb64ce128fa08005326a8b91702d0d03e81ba80e14d27" # chainid of new chain
    nodeos_websocket_port = 8887 # nodeos websocket port, can be same per nodeos instance
    nodeos_latest = true # using 2.x nodeos
    webhook_dapp_port = 8113 # nodeos webhook port, must be unique per chain
    # demux
    demux_webhook_port = 3196 # port demux runs on, must be unique per new chain
    demux_socket_mode = "sub"
    demux_bypass_database_head_block = false
    # sidechain 
    name = "liquidxcovax" # CHAIN_NAME - contract on the EOS mainnet that registered the new chain
    # the mapping below contains the dappservices:dappservicex and mainnet DSP account to the new chain's DSP account mapping
    mapping = "dappservices:dappservicex,MAINNET_DSP_ACCOUNT:COVAX_CHAIN_DSP_ACCOUNT"
```

## Push DSP account mapping action on EOS mainnet

On the EOS mainnet, the EOS mainnet's DSP account must be connected to the new chain's DSP account.  This is done using the [`addaccount`](https://bloks.io/account/liquidx.dsp?loadContract=true&tab=Actions&account=liquidx.dsp&scope=liquidx.dsp&limit=100&action=addaccount) command on the [`liquidx.dsp`](https://bloks.io/account/liquidx.dsp) account.

- owner {name} - DSP account name on the EOS mainnet
- chain_account {name} - DSP account name on the new chain, `liquidxcovax`
- chain_name {name} - account name of contract on the EOS mainnet that registered the chain

Cleos example:

```bash
cleos -u https://nodes.get-scatter.com:443 push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"liquidx.dsp","name":"addaccount","data":{"owner":"uuddlrlrbass","chain_account":"uuddlrlrbass","chain_name":"liquidxcovax"},"authorization":[{"actor":"uuddlrlrbass","permission":"active"}]}]}'
```

## Push DSP account mapping action on CoVax Chain

Once you have that then on the CoVax chain, submit an `adddsp` action on that account.

- owner {name} - DSP name on new chain
- dsp {name} - DSP name on mainnet

Cleos example:

```bash
cleos -u http://peer-covax.liquidapps.io:80 push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"dappservicex","name":"adddsp","data":{"owner":"uuddlrlrbass","dsp":"uuddlrlrbass"},"authorization":[{"actor":"uuddlrlrbass","permission":"active"}]}]}'
```