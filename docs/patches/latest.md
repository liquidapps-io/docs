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
- fixes
    - add custom permissions for xcallback in generic-dapp-service-node file

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- update eos 1.8.6
- add example portolio dapp
- update LiquidStorage unit test
- add boxes: oracle-web oracle-self-history oracle-foreign-chain oracle-sister-chain oracle-wolframalpha oracle-random oracle-sql oracle-vcpu
- split up oracle services
- add functional LiquidStorage unit test
- modified and fixed ipfs cleanup script to support oracle cleanups
- allow `zeus compile <CONTRACT_NAME>`, zeus now allows you to only compile a contract by its name if you like, or you can run `zeus compile` to run all
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

### [dappservices contract](http://bloks.io/account/dappservices)
- add usagex for LiquidX and other off chain service billing LiquidStorage, LiquidLens, LiquidAuth