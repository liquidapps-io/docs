latest
========

### [LiquidVRAM Service](https://docs.liquidapps.io/en/v2.0/services/ipfs-service.html)
- add new service responses and requests when `#define USE_ADVANCED_IPFS` is used
- add warmuprow and cleanuprow, these allow for more efficient loading and cleaning of vram shard information
- add warmupcode, which allows for vram to be accessed from external contracts and third party DSPs
- warmupcode is used automatically when required when the `code` specified in a multi-index table is something other than `self`

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- update get table row secodary index changes in 2.0
- cease support for pre nodeos 2.0 nodes
- added max pending messages for demux to prevent memory crashes
- fixed demux get starting block logic, added possibility for head block
- fixes
    - fix demux head block handler using brackets
    - support new `/v1/chain/send_transaction` endpoint in addition to `/v1/chain/push_transaction`

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- split mapping into builtin and local file stored in ~/.zeus/ storage directory
- boxes added to the mapping using zeus deploy box and zeus box add go to the local mapping file to persist between zeus updates
- when unboxing a box found in both files, the local mapping is given priority, but a warning is displayed
- added zeus box remove command to remove boxes from the local mapping
- added RC file ignore flag --rc-ignore to bypass it | [thank you prcolaco](https://github.com/liquidapps-io/zeus-sdk/pull/9)

Example `zeusrc.json`:
```json
{
    "verbose": true,
    "type": "local",
    "update-mapping": true,
    "test": true
}
```
- added utility/tool for deserializing `xvexec` data (vaccount action data)
Example:
`deserializeVactionPayload('dappaccoun.t', '6136465e000000002a00000000000000aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e90600000000000000008090d9572d3ccdcd002370ae375c19feaa49e0d336557df8aa49010000000000000004454f5300000000026869', 'https://mainnet.eos.dfuse.io')`
returns
```json
{
   "payload":{
      "expiry":"1581659745",
      "nonce":"42",
      "chainid":"ACA376F206B8FC25A6ED44DBDC66547C36C6C33E3A119FFBEAEF943642F0E906",
      "action":{
         "account":"",
         "action_name":"transfervacc",
         "authorization":[

         ],
         "action_data":"70AE375C19FEAA49E0D336557DF8AA49010000000000000004454F5300000000026869"
      }
   },
   "deserializedAction":{
      "payload":{
         "vaccount":"dapjwaewayrb",
         "to":"dapjkzepavdy",
         "quantity":"0.0001 EOS",
         "memo":"hi"
      }
   }
}
```
- add RC file to load regular zeus-cmd options from, on ~/.zeus/zeusrc.json by default, changeable with --rc-file option | [thank you prcolaco](https://github.com/liquidapps-io/zeus-sdk/pull/9)
- `zeus create contract <MY_CONTRACT>` now creates `MY_CONTRACT.cpp` instead of `main.cpp`, update cmake to use `MY_CONTRACT.cpp`
- use v2.0.2 nodeos
- fixes
    - fix portfolio app requesting new oracle entries twice on load
    - fix portfolio app double adding eos token balances
    - `Read past end of buffer` - The issue was that an additional parameter was added to IPFS warmup to enable new functionality. This caused a conflict for pre-existing contracts attempting to warmup IPFS data. The parameter was removed.
    - fixed and refactored `get-table` utility to work with new dsp api (`get_uri`) and nodeos >= 2.0.0.

### [@liquidapps/dapp-client](https://www.npmjs.com/package/@liquidapps/dapp-client)

### [docs](https://docs.liquidapps.io/en/stable/)
- update to using nodeos v2+, pre 2.0 no longer supported

### [dappservices contract](http://bloks.io/account/dappservices)
- Moved quota calculation logic into dappservices
- Deprecated individual service contracts for quota management, i.e. `ipfsservice1`,`cronservices`, etc
    - Providers that have already registered packages will have their RAM freed using a new `freeprovider(provider)` action
- billing will default to 0.0001 QUOTA for all actions
    - This allows for new actions to be added to services
- use `pricepkg` action to price action in quota
- add `developers/contract-logs` section and add details on DSP logs
- add @eosio.code note for dappservicex and consumer contracts in liquidx section