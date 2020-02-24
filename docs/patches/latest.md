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
- fixes
    - fix demux head block handler using brackets

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
- add RC file to load regular zeus-cmd options from, on ~/.zeus/zeusrc.json by default, changeable with --rc-file option | [thank you prcolaco](https://github.com/liquidapps-io/zeus-sdk/pull/9)
- use v2.0.2 nodeos
- fixes
    - fix portfolio app requesting new oracle entries twice on load
    - fix portfolio app double adding eos token balances
    - `Read past end of buffer` - The issue was that an additional parameter was added to IPFS warmup to enable new functionality. This caused a conflict for pre-existing contracts attempting to warmup IPFS data. The parameter was removed.

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