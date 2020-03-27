Example Chains to Add
==========

The following chains have the `dappservicex` contract deployed and mapped.

Guide:

- [CoVax](#covax) - CoVax, powered by the DAPP Network, is a community-driven project to fight against COVID-19 collectively, read more [here](../covax/getting-started)
- [WAX](#wax) 
- [WAX Test](#wax-test) 
- [Telos](#telos) 
- [Telos Test](#telos-test) 
- [BOS](#bos) 

*as a note, the `dsp_port`, `webhook_dapp_port`, `demux_webhook_port` must be unique per chain*

## CoVax

```bash
[sidechains.liquidxcovax]
    # dsp
    dsp_port = 3116
    dsp_account = ""
    dsp_private_key = ""
    # nodeos
    nodeos_host = ""
    nodeos_port = 8888
    nodeos_secured = false
    nodeos_chainid = "63788f6e75cdb4ec9d8bb64ce128fa08005326a8b91702d0d03e81ba80e14d27"
    nodeos_websocket_port = 8887
    nodeos_latest = true
    webhook_dapp_port = 8113
    # demux
    demux_webhook_port = 3196
    demux_socket_mode = "sub"
    demux_bypass_database_head_block = false
    demux_max_pending_messages = 500
    # sidechain 
    name = "liquidxcovax"
    mapping = "dappservices:dappservicex,EOS_MAINNET_DSP_ACCOUNT:COVAX_DSP_ACCOUNT"
```

## WAX

```bash
[sidechains.liquidxxxwax]
    # dsp
    dsp_port = 3117
    dsp_account = ""
    dsp_private_key = ""
    # nodeos
    nodeos_host = ""
    nodeos_port = 8888
    nodeos_secured = false
    nodeos_chainid = "1064487b3cd1a897ce03ae5b6a865651747e2e152090f99c1d19d44e01aea5a4"
    nodeos_websocket_port = 8887
    nodeos_latest = true
    webhook_dapp_port = 8114
    # demux
    demux_webhook_port = 3197
    demux_socket_mode = "sub"
    demux_bypass_database_head_block = false
    demux_max_pending_messages = 500
    # sidechain 
    name = "liquidxxxwax"
    mapping = "dappservices:dappservicex,EOS_MAINNET_DSP_ACCOUNT:WAX_DSP_ACCOUNT"
```

## WAX Test

```bash
[sidechains.liquidxxtwax]
    # dsp
    dsp_port = 3118
    dsp_account = ""
    dsp_private_key = ""
    # nodeos
    nodeos_host = ""
    nodeos_port = 8888
    nodeos_secured = false
    nodeos_chainid = "f16b1833c747c43682f4386fca9cbb327929334a762755ebec17f6f23c9b8a12"
    nodeos_websocket_port = 8887
    nodeos_latest = true
    webhook_dapp_port = 8115
    # demux
    demux_webhook_port = 3198
    demux_socket_mode = "sub"
    demux_bypass_database_head_block = false
    demux_max_pending_messages = 500
    # sidechain 
    name = "liquidxxtwax"
    mapping = "dappservices:dappservicex,EOS_MAINNET_DSP_ACCOUNT:WAX_TEST_DSP_ACCOUNT"
```

## Telos

```bash
[sidechains.liquidxxtlos]
    # dsp
    dsp_port = 3119
    dsp_account = ""
    dsp_private_key = ""
    # nodeos
    nodeos_host = ""
    nodeos_port = 8888
    nodeos_secured = false
    nodeos_chainid = "4667b205c6838ef70ff7988f6e8257e8be0e1284a2f59699054a018f743b1d11"
    nodeos_websocket_port = 8887
    nodeos_latest = true
    webhook_dapp_port = 8116
    # demux
    demux_webhook_port = 3199
    demux_socket_mode = "sub"
    demux_bypass_database_head_block = false
    demux_max_pending_messages = 500
    # sidechain 
    name = "liquidxxtlos"
    mapping = "dappservices:dappservicex,EOS_MAINNET_DSP_ACCOUNT:TELOS_DSP_ACCOUNT"
```

## Telos Test

```bash
[sidechains.liquidxttlos]
    # dsp
    dsp_port = 3120
    dsp_account = ""
    dsp_private_key = ""
    # nodeos
    nodeos_host = ""
    nodeos_port = 8888
    nodeos_secured = false
    nodeos_chainid = "1eaa0824707c8c16bd25145493bf062aecddfeb56c736f6ba6397f3195f33c9f"
    nodeos_websocket_port = 8887
    nodeos_latest = true
    webhook_dapp_port = 8117
    # demux
    demux_webhook_port = 3200
    demux_socket_mode = "sub"
    demux_bypass_database_head_block = false
    demux_max_pending_messages = 500
    # sidechain 
    name = "liquidxttlos"
    mapping = "dappservices:dappservicex,EOS_MAINNET_DSP_ACCOUNT:TELOS_TEST_DSP_ACCOUNT"
```

## BOS

```bash
[sidechains.liquidxxxbos]
    # dsp
    dsp_port = 3121
    dsp_account = ""
    dsp_private_key = ""
    # nodeos
    nodeos_host = ""
    nodeos_port = 8888
    nodeos_secured = false
    nodeos_chainid = "d5a3d18fbb3c084e3b1f3fa98c21014b5f3db536cc15d08f9f6479517c6a3d86"
    nodeos_websocket_port = 8887
    nodeos_latest = true
    webhook_dapp_port = 8118
    # demux
    demux_webhook_port = 3201
    demux_socket_mode = "sub"
    demux_bypass_database_head_block = false
    demux_max_pending_messages = 500
    # sidechain 
    name = "liquidxxxbos"
    mapping = "dappservices:dappservicex,EOS_MAINNET_DSP_ACCOUNT:BOS_DSP_ACCOUNT"
```