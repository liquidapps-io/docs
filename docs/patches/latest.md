latest
========

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- add `'Content-Type': 'application/json'` to oracle `https+post+json` request
- add timeout proxy for database calls
- add LiquidX
- add LiquidHarmony, extension oracle service that allows plug and play oracle options (Web/IBC/XIBC/VCPU/SQL Services)
- add LiquidSQL, state storage alternative for smart contracts
- dappservicesx contract - add setlink (create link between side chain account and mainnet owner), adddsp (side chain account add DSP name), rmvdsp (side chain - account remove DSP name)
- liquidx contract - add addaccount (add sidechain account to allow billing to another chain account) and rmvaccount (remove link) actions
- add sidechain billing to dapp-services-node/common.js
- add LiquidKMS boilerplate
- add LiquidStorage node logic for unpin / upload_public
- add LiquidBilling boilerplate
- add reconnect mechanism to demux nodeos websocket
- fixes
    - add custom permissions for xcallback in generic-dapp-service-node file

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- update eos 1.8.6
- add example portolio dapp
- update LiquidStorage unit test
- add boxes: oracle-web oracle-self-history oracle-foreign-chain oracle-sister-chain oracle-wolframalpha oracle-random oracle-sql oracle-vcpu
- split up oracle services
- add functional LiquidStorage unit test
- add `--type=local` flag to `zeus deploy box` command: deploys boxes locally to `~/.zeus/boxes/` instead of `IPFS` or `s3`. *Must use with the `--update-mapping` flag*. Together both flags (`zeus deploy box --type=local --update-mapping`) updates the `mapping.js` file with `file://`.. as the pointer | [thank you prcolaco](https://github.com/liquidapps-io/zeus-sdk/pull/11)
- made `--type=local` and `--update-mapping` flags default for `zeus deploy box` command
- only use invalidation of ipfs with `zeus deploy box` command when the `--type` is `ipfs` | [thank you prcolaco](https://github.com/liquidapps-io/zeus-sdk/pull/11)
- modified and fixed ipfs cleanup script to support oracle cleanups
- allow `zeus compile <CONTRACT_NAME>`, zeus now allows you to only compile a contract by its name if you like, or you can run `zeus compile` to run all
- add `kill-port` npm dependency to `eos-extensions` box
- move `ipfs-daemon` dependency from `boxes/groups/core/build-extensions/zeus-box.json` to `boxes/groups/dapp-network/dapp-services/zeus-box.json` as `IPFS` is only needed with the `dapp-services` box
- add `utils/ipfs-service/get-table.js` - Reads all vRAM tables of a smart contract and stores them with the naming syntax: `${contract_name}-${table_name}-table.json`
- add `utils/ipfs-service/get-ordered-keys.js` - Prints ordered vRAM table keys in ascending order account/table/scope.  This can be used to iterate over the entire table client side
- fixes
    - replace unzip install with unzipper, allow node v11
    - update create contract example unit test eosjs2
    - update example frontend to eosjs2 and latest scatter
    - update cleanup script to work with new dsp logic
    - add CONTRACT_END syntax to example contract
    - fix cardgame unit test
        - use dapp-client for vaccounts
        - move xvinit for vaccounts to happen in migration
        - add xvinit to coldtoken contract
        - update to eosj2
    - fix chess.json to enable migration by updating contract / account
    - fix OSX `zeus deploy box` breaking issue

### [@liquidapps/dapp-client](https://www.npmjs.com/package/@liquidapps/dapp-client)
- add LiquidStorage client extension to upload / unpin
- fixes
    - add fix text encode/decode in vaccounts service

### [docs](https://docs.liquidapps.io/en/stable/)
- add local postgresql info
- add zeus-ide
- replace unzip install with unzipper, allow node v11
- update overview section with new links / videos
- add dapp-client section
- add example portolio dapp
- update oracle getting started links
- LiquidAuthenticator - WIP → Alpha
- LiquidBilling - WIP - Transaction signing service for resource payment
- LiquidKMS - WIP - Key Management Service
- LiquidStorage - WIP → Alpha
- LiquidSQL - Alpha
- removed `read-mode = head` from default `config.ini` setup for eosio node
- clarified `wasm-runtime = wabt` must be used over `wasm-runtime = wavm` due to bugs in `wavm`
- add `zeus compile <CONTRACT_NAME>` syntax to [zeus-getting-started](../developers/zeus-getting-started)
- update path for `cleanup.js` script for DSPs
- add cleanup oracle info to [Cleanup IPFS and Oracle Entries](../dsps/cleanup-ipfs-oracle-entries)
- fixed little mistakes in [vram-getting-started](../developers/vram-getting-started)
- added usage docs for `get-table` and `get-ordered-keys`
- update `chain-state-db-size-mb` from `131072` to `16384` see [here](https://github.com/EOSIO/eos/issues/7664#issuecomment-560266833)

### [dappservices contract](http://bloks.io/account/dappservices)
- add usagex for LiquidX and other off chain service billing LiquidStorage, LiquidLens, LiquidAuth