latest
========

### [docs](https://docs.liquidapps.io/en/stable/)

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- use simple eos example in gitpod, create a simple eosio contract with 1 command, test in under 10 seconds with zeus test, and migrate within 15 seconds to kylin or another network : )
- add network to zeus contract deployment to allow deployments to each multiple networks
- enable flexible migration of contracts to other networks `zeus migrate --creator natdeveloper --network=development`, e.g., deploys all migrations for creator natdeveloper on development
- allow only specific services to be run with `--services "cron,ipfs,oracle"`, etc, add to start-localenv
- add `zeus start-localenv --kill` to kill all existing nodes and processes
- add `zeus start-localenv --enable-features` which runs the latest eosio contracts and enables EOSIO features such as bill first auth, etc.
- add `zeus start-localenv --single-chain` to only run the provisioning chain if say a box like tokenpeg in unboxed, so you're not running additional nodeos instances
- add `zeus start-localenv --basic-env` which spins up the most basic nodeos env
- allow `zeus migrate CONTRACT_NAME`
- move working contracts dir from `./zeus_boxes/contracts` to `./contracts`
- move sql logs to `./logs` folder
- update eosio contracts to latest, still access legacy contracts if not using `--enable-features`, add rex
- add `--plugin eosio::producer_api_plugin` and `--plugin eosio::chain_plugin` to local tests
- update to 1.8.0-rc2 default eosio.cdt install, 1.7 has boost errors that appear to be on b1's side
- allow contracts keys to be fetched automatically if not supplied to deployer
- add jungle3 network option, update kylin endpoint
- add initial transfer for created accounts
- don't run IPFS node if not needed
- update self history unit test
- update ethtokenpeg contract to issue then transfer
- add various command console logs and emojis : )
- add `getTestAccountName` to unit test library
- add `testinterval` and `rminterval` actions to cronconsumer unit test and example contract
- add simple contract template `zeus create contract mycontract --template=simplecontract `

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- reduce default `DSP_BACKOFF_EXPONENT` from 1.5 to 1.1

### [@liquidapps/dapp-client](https://www.npmjs.com/package/@liquidapps/dapp-client)

### [@liquidapps/box-utils](https://www.npmjs.com/package/@liquidapps/box-utils)
- support existing boxes to create local directory

### dappservices contract

## DSP Services:

### [LiquidAccount Service](https://docs.liquidapps.io/en/v2.0/services/vaccounts-service.html)

### [LiquidVRAM Service](https://docs.liquidapps.io/en/stable/services/ipfs-service.html)

### [LiquidStorage Service](https://docs.liquidapps.io/en/stable/services/storage-service.html)

### [LiquidHarmony Service](https://docs.liquidapps.io/en/stable/developers/harmony-getting-started.html)

### [LiquidScheduler Service](https://docs.liquidapps.io/en/stable/developers/cron-getting-started.html)
- add interval service for running real realiable crons, crons stored in postgresql and fetched when DSP starts, `interval`, and `rminterval`
- upgrade sign service to beta stage

### LiquidLink Service
- add additional checks for external requests and undocumented models

### Link Library
- update EOSIO side to use new intervals to handle crons
