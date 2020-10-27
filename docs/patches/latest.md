latest
========

**breaking changes**
- to use new dfuse push_transaction, must compile consumer contract with new version of dappservices contract
- if DSP package is not enabled in package table, signaled with boolean "1", then service will throw error on DSP, DSP package may be enabled with the `enablepkg` command: https://bloks.io/account/dappservices?loadContract=true&tab=Actions&account=dappservices&scope=dappservices&limit=100&action=enablepkg

### [docs](https://docs.liquidapps.io/en/stable/)
- document ability to use dfuse for cleanup script
- add typescript compile step for dfuse
- add `zeus box create` section to zeus getting started section
- update LiquidStorage upload file example with new params
- add `eth_keys_per_consumer = [{ETH_PRIVATE_KEY_CONSUMER1:PRIVATE_KEY_HERE}, {ETH_PRIVATE_KEY_CONSUMER2:PRIVATE_KEY_HERE}]` to toml
- prevent non-broadcast action from being broadcast

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- add `testfetch` price feed action / unit test for only using LiquidHarmony oracles for price feed fetch
- copy contracts and test folder to root so that `zeus_boxes` directory can be git ignored 
- add `enablepkg` command to dappservices unit tests

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- add option to use dfuse web socket and dfuse push_transaction guarantee in place of demux state history node on main DSP instance and supported side chains
    - this enables a DSP to not use a SHiP node and to instead read on chain events from dfuse and to push transactions using dfuse's push_guarantee making transactions more reliable, a free API key is suitable enough to support low levels of traffic
    - to use the dfuse backend, under the new dfuse section of the toml file, set `enable` to true and provide an api key
    - to use the dfuse push guarantee, set the `push_enable` to true and provide a dfuse api key
    - to use both, enable both
    - add dfuse section `network` to toml, select supported dfuse network: testnet (eosio testnet), kylin, worbli, wax
    - add `debug` to dfuse section to enable dfuse debug logs 
- if dsp `head_block` set to 0, dsp will pull head block from `get_info` RPC call automatically for demux
- throw error if package not enabled for DSP services
- enable cleanup script support for sidechain
- add `authorization` option under dfuse to use the EOS Nation community edition: https://t.me/dfusece | https://eos.dfuse.eosnation.io/
- add `max_request_retries` option to config.toml, specifies how many times to retry a blocking DSP actions such as an IPFS warmup or oracle request
- fixes
    - add better error handling to CONFIRMING USAGE
    - fix replay contract script to enable DSPs to replay contract vRAM/IPFS data to populate a new IPFS instance
    - fix `0xANON` payer empty object issue

### [@liquidapps/dapp-client](https://www.npmjs.com/package/@liquidapps/dapp-client)
- patch new secondary index RPC API support
- updated Dapp Client to support cross chain Liquid Accounts
- add dfuse as option for dapp client, able to pass API key, push guarantee, and network

### dappservices contract
- added required service pending console output to assertion message as dfuse does not return pending console output
- added always false assert service pending console output to assertion message as dfuse does not return pending console output

## DSP Services:

### [LiquidAccount Service](https://docs.liquidapps.io/en/v2.0/services/vaccounts-service.html)
- add cross chain support for LiquidAccounts using LiquidX
- add time to live option for zeus vaccounts push-action command

### [LiquidVRAM Service](https://docs.liquidapps.io/en/stable/services/ipfs-service.html)

### [LiquidStorage Service](https://docs.liquidapps.io/en/stable/services/storage-service.html)
- add small / large photo and small video upload and read unit tests to LiquidStorage
- configure options object to be passed, updated unit test
    - add optional `rawLeaves` IPFS client option for backend API, example, and `client-library`

### [LiquidHarmony Service](https://docs.liquidapps.io/en/stable/developers/harmony-getting-started.html)

### [LiquidScheduler Service](https://docs.liquidapps.io/en/stable/developers/cron-getting-started.html)
