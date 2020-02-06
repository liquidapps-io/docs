latest
========

### [LiquidVRAM Service](https://docs.liquidapps.io/en/v2.0/services/ipfs-service.html)

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)

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
- fixes
    - fix portfolio app requesting new oracle entries twice on load
    - fix portfolio app double adding eos token balances
    - `Read past end of buffer` - The issue was that an additional parameter was added to IPFS warmup to enable new functionality. This caused a conflict for pre-existing contracts attempting to warmup IPFS data. The parameter was removed.

### [@liquidapps/dapp-client](https://www.npmjs.com/package/@liquidapps/dapp-client)

### [docs](https://docs.liquidapps.io/en/stable/)

### [dappservices contract](http://bloks.io/account/dappservices)
