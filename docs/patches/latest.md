latest
========

**breaking changes**
- if DSP package is not enabled in package table, signaled with boolean "1", then service will throw error on DSP, DSP package may be enabled with the `enablepkg` command: https://bloks.io/account/dappservices?loadContract=true&tab=Actions&account=dappservices&scope=dappservices&limit=100&action=enablepkg

### [docs](https://docs.liquidapps.io/en/stable/)

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- add `testfetch` price feed action / unit test for only using LiquidHarmony oracles for price feed fetch

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- throw error if package not enabled for DSP services

### [@liquidapps/dapp-client](https://www.npmjs.com/package/@liquidapps/dapp-client)
- patch new secondary index RPC API support
- updated Dapp Client to support cross chain Liquid Accounts

## DSP Services:

### [LiquidAccount Service](https://docs.liquidapps.io/en/v2.0/services/vaccounts-service.html)
- add cross chain support for LiquidAccounts using LiquidX
- add time to live option for zeus vaccounts push-action command

### [LiquidVRAM Service](https://docs.liquidapps.io/en/stable/services/ipfs-service.html)

### [LiquidStorage Service](https://docs.liquidapps.io/en/stable/services/storage-service.html)

### [LiquidHarmony Service](https://docs.liquidapps.io/en/stable/developers/harmony-getting-started.html)

### [LiquidScheduler Service](https://docs.liquidapps.io/en/stable/developers/cron-getting-started.html)
