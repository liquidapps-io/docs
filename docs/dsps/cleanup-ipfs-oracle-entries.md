Cleanup IPFS and Oracle Entries
========

Sometimes IPFS or Oracle entries are not evicted from a developer's contract due to the DSP experiencing unpredictable behavior.  This causes the developer's smart contract RAM supply to increase as the `ipfsentry` / `oracleentry` table rows are not evicted.  If this happens, you may run the [cleanup.js](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/utils/ipfs-service/cleanup.js) file with the following environment variables:

The cleanup script will auto detect which table to cleanup `ipfsentry` or `oracleentry` depending on which one is present on the contract.  If both are set, you can use the `TABLE` env variable to specify which to cleanup.

### Mandatory:

- `CONTRACT` contract to clean IPFS / oracle entries
- `DSP_ENDPOINT` the DSP's endpoint that you staked to for IPFS and/or Oracle services

```bash
export CONTRACT=lqdportfolio
export DSP_ENDPOINT=http://kylin-dsp-2.liquidapps.io
```

### Optional:

- `CHUNK_SIZE` represents the number of async requests for cleanups to send to the DSP at a time
- `TABLE` type is auto detected based on the contract name (ipfs table: `ipfsentry` or oracle table: `oracleentry`), if a contract has both tables, you will use this variable to target both

```bash
export CHUNK_SIZE= # defaults to 5
export TABLE= # defaults to ipfsentry or oracleentry by detecting from contract
```

Then run with:

```bash
sudo find / -name cleanup.js
node /root/.nvm/versions/node/v10.16.0/lib/node_modules/@liquidapps/dsp/utils/cleanup.js
```