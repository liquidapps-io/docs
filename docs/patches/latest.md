latest
========

### [docs](https://docs.liquidapps.io/en/stable/)
- add CoVax chain section for becoming BP or DSP
- add example chains section of LiquidX chains
- use eos 2.0.5
- update documentation to support `zeus_boxes` refactor

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- use 8887 instead of 8889 for state history port to match DSP docs
- skip `01-dapp-client.js` if built
- add price feed example, price feed uses LiquidHarmony's oracles and LiquidScheduler's cron to fetch a price periodically and only use CPU when the price has changed from the last recorded price by more or less than 1%
- add `'--eos-vm-oc-compile-threads=4'` and `--chain-threads=4` to local nodeos
- update `zeus` to modularize logic into `zeus_boxes` directory making `zeus` more like `npm`, to create or unbox a new box start with `zeus box create [name]` then if you wish to unbox and existing box `zeus unbox <BOX>`
- zeus now offers versioning of boxes
    - zeus now offers optional `zeus unbox <BOX>@[VERSION]`
    - can add and remove boxes with `zeus box add <BOX> [VERSION] [URI]` `zeus box remove <BOX> [VERSION]`
    - to update an existing box, run `zeus unbox <BOX>@[VERSION]`, if no version specified, latest used, will unbox everything again with new version
    - to only add new boxes, unbox after update with `--no-update`
- fixes
    - if Mac, detect and skip `--eos-vm-oc-enable` flags as they are not supported

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- added warning to ensure `trace-history = true` set in nodeos `config.ini`
- add how many blocks behind the head block in demux heartbeat
- add `DATABASE_TIMEOUT` to sample toml, adjust time before database connection times out
- add `DEMUX_PROCESS_BLOCK_CHECKPOINT` to sample toml, amount of blocks to pass before updating database with last processed block
- add `disabledServices` to ecosystem file to prevent pre-alpha services from being setup

### [LiquidVRAM Service](https://docs.liquidapps.io/en/stable/services/ipfs-service.html)
- add cross chain reading of vRAM table data

### [LiquidStorage Service](https://docs.liquidapps.io/en/stable/services/storage-service.html)
- add dapp-client examples for `get_uri.ts`, `unpin_public_file.ts`, `upload_archive_to_liquidstorage.ts`, `upload_file_to_liquidstorage.ts`, `upload_public_file_from_vaccount.ts`
- add sidechain storage unit test
- add `zeus storage upload <ACCOUNT_NAME> package.json <ACCOUNT_PRIVATE_KEY>` and `zeus storage unpin <ACCOUNT_NAME> <IPFS_URI_RETURNED_ABOVE> <ACCOUNT_PRIVATE_KEY>` zeus commands

### [docs](https://docs.liquidapps.io/en/stable/)
- add [LiquidStorage section](../developers/storage-getting-started)
- update [LiquidVRAM section](../developers/vram-getting-started)

### [LiquidHarmony Service](https://docs.liquidapps.io/en/stable/developers/harmony-getting-started.html)
- add check to ensure each DSP only returns one oracle response
- add hook to assert on oracle fetch before geturi is fired
- add `Oracle minimum threshold check` unit test
- reduce oracle retries to 10 from 100
- add `shouldAbort` `eosio::check` handler for aborting oracle service request

### [LiquidScheduler Service](https://docs.liquidapps.io/en/stable/developers/cron-getting-started.html)
- run exponential backoff forever, was 10 retries max
- add `shouldAbort` `eosio::check` handler for rescheduling cron without CPU