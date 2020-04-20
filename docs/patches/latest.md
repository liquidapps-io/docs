latest
========

### [docs](https://docs.liquidapps.io/en/stable/)
- add CoVax chain section for becoming BP or DSP
- add example chains section of LiquidX chains

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- use 8887 instead of 8889 for state history port to match DSP docs
- add price feed example, price feed uses LiquidHarmony's oracles and LiquidScheduler's cron to fetch a price periodically and only use CPU when the price has changed from the last recorded price by more or less than 1%

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- added warning to ensure `trace-history = true` set in nodeos `config.ini`

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