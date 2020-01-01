latest
========

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- add DSP console log in `common.js` if minimum stake threshold not met for account's DAPP stake to DSP's service package
- add reconnect mechanism to demux nodeos websocket
- update eos 1.8.7 nodeos
- fixes
    - add custom permissions for xcallback in generic-dapp-service-node file
    - fix cron reschedule on error, use `nextTrySeconds` time.

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- add `--type=local` flag to `zeus deploy box` command: deploys boxes locally to `~/.zeus/boxes/` instead of `IPFS` or `s3`. *Must use with the `--update-mapping` flag*. Together both flags (`zeus deploy box --type=local --update-mapping`) updates the `mapping.js` file with `file://`.. as the pointer | [thank you prcolaco](https://github.com/liquidapps-io/zeus-sdk/pull/11)
- made `--type=local` and `--update-mapping` flags default for `zeus deploy box` command
- only use invalidation of ipfs with `zeus deploy box` command when the `--type` is `ipfs` | [thank you prcolaco](https://github.com/liquidapps-io/zeus-sdk/pull/11)
- modified and fixed ipfs cleanup script to support oracle cleanups
- allow `zeus compile <CONTRACT_NAME>`, zeus now allows you to only compile a contract by its name if you like, or you can run `zeus compile` to run all
- add `kill-port` npm dependency to `eos-extensions` box
- move `ipfs-daemon` dependency from `boxes/groups/core/build-extensions/zeus-box.json` to `boxes/groups/dapp-network/dapp-services/zeus-box.json` as `IPFS` is only needed with the `dapp-services` box
- add `utils/ipfs-service/get-table.js` - Reads all vRAM tables of a smart contract and stores them with the naming syntax: `${contract_name}-${table_name}-table.json`
- add `utils/ipfs-service/get-ordered-keys.js` - Prints ordered vRAM table keys in ascending order account/table/scope.  This can be used to iterate over the entire table client side
- allow `zeus test <CONTRACT_NAME>`, zeus now allows you to only compile/test a contract by its name if you like, or you can run `zeus test` to compile/test all
- add `zeus vaccounts push-action test1v regaccount '{"vaccount":"vaccount1"}'`
- add ability to import/export LiquidAccount keys
- update eos 1.8.7 nodeos
- implement storage `dapp-client` into storage service test `storage-dapp-service/test/storage.spec.js`
- use base58 instead of default base32 for LiquidStorage's `ipfs.files.add` to match ipfs service
- fixes
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
    - remove prints from vaccount code to prevent `required service` error
    - remove `all-dapp-services` box from `templates-emptycontract-eos-cpp` (zeus create contract)

### [@liquidapps/dapp-client](https://www.npmjs.com/package/@liquidapps/dapp-client)
- fixes
    - add fix text encode/decode in vaccounts service

### [docs](https://docs.liquidapps.io/en/stable/)
- removed `read-mode = head` from default `config.ini` setup for eosio node
- clarified `wasm-runtime = wabt` must be used over `wasm-runtime = wavm` due to bugs in `wavm`
- add `zeus compile <CONTRACT_NAME>` syntax to [zeus-getting-started](../developers/zeus-getting-started)
- update path for `cleanup.js` script for DSPs
- add cleanup oracle info to [Cleanup IPFS and Oracle Entries](../dsps/cleanup-ipfs-oracle-entries)
- fixed little mistakes in [vram-getting-started](../developers/vram-getting-started)
- added usage docs for `get-table` and `get-ordered-keys`
- update `chain-state-db-size-mb` from `131072` to `16384` see [here](https://github.com/EOSIO/eos/issues/7664#issuecomment-560266833)
- update eos 1.8.7 nodeos
- update cardgame link to: [http://elemental.liquidapps.io/](http://elemental.liquidapps.io/)

### [dappservices contract](http://bloks.io/account/dappservices)
- add usagex for LiquidX and other off chain service billing LiquidStorage, LiquidLens, LiquidAuth
- fixes
    - add DAPP token assertion to `regpkg` command to ensure DAPP symbol and 4 decimals of precision used
