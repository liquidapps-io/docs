latest
========

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- separated pm2 log files
- add dsp version endpoint /v1/dsp/version
- add `keytype` parameter to `/v1/dsp/get_table_row` request (`"keytype":"symbol"` if passing a symbol or string as primary key, `"keytype":"number"` if passing number). The keytype field adds precision to ensure the correct primary key is returned and it is an optional parameter
- add support pass body to oracle POST request
- fixes
    - demux database sync issue
    - speed up demux sync and fix log messages
    - allow demux to sync from head_block in config.toml
    - prevent demux block processing from hanging
    - enable DSP API to use non-local nodeos instance
    - auto generate dsp node index files
    - fix demux high CPU issue

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- add vcpu-dapp-service
- add chess game zeus unbox chess
- enable large LiquidAccount payload sizes
- add unit test for oracle POST request
- add `--phase` command to specify dapp services file `dapp-services-eos.js`, install npm files `npm`, or compile eos files `eos`
- fixes
    - change instantiateBuffer to instantiateSync for vcpu vrun.js
    - fix debian install for eosio.cdt due to syntax change in download link

### [@liquidapps/dapp-client](https://www.npmjs.com/package/@liquidapps/dapp-client)

### [docs](https://docs.liquidapps.io/en/stable/)
- add unit testing section
- add email support: support@liquidapps.io
- add vCPU & LiquidChess

### [dappservices contract](http://bloks.io/account/dappservices)