latest
========

### [docs](https://docs.liquidapps.io/en/stable/)
- add CoVax chain section for becoming BP or DSP
- add example chains section of LiquidX chains

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- use 8887 instead of 8889 for state history port to match DSP docs
- skip `01-dapp-client.js` if built
- add price feed example, price feed uses LiquidHarmony's oracles and LiquidScheduler's cron to fetch a price periodically and only use CPU when the price has changed from the last recorded price by more or less than 1%

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- added warning to ensure `trace-history = true` set in nodeos `config.ini`
- add how many blocks behind the head block in demux heartbeat

### [LiquidStorage Service](https://docs.liquidapps.io/en/stable/services/storage-service.html)
- add dapp-client examples for `get_uri.ts`, `unpin_public_file.ts`, `upload_archive_to_liquidstorage.ts`, `upload_file_to_liquidstorage.ts`, `upload_public_file_from_vaccount.ts`
- add sidechain storage unit test
- add `zeus storage upload <ACCOUNT_NAME> package.json <ACCOUNT_PRIVATE_KEY>` and `zeus storage unpin <ACCOUNT_NAME> <IPFS_URI_RETURNED_ABOVE> <ACCOUNT_PRIVATE_KEY>` zeus commands

### [docs](https://docs.liquidapps.io/en/stable/)
- add [LiquidStorage section](../developers/storage-getting-started)

### [LiquidHarmony Service](https://docs.liquidapps.io/en/stable/developers/harmony-getting-started.html)
- add check to ensure each DSP only returns one oracle response
- add hook to assert on oracle fetch before geturi is fired
- add `Oracle minimum threshold check` unit test
- reduce oracle retries to 10 from 100
- add `shouldAbort` `eosio::check` handler for aborting oracle service request

### [LiquidScheduler Service](https://docs.liquidapps.io/en/stable/developers/cron-getting-started.html)
- run exponential backoff forever, was 10 retries max
- add `shouldAbort` `eosio::check` handler for rescheduling cron without CPU