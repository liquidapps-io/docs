Cleanup IPFS Entries
========

Sometimes IPFS entries are not evicted from a developer's contract due to the DSP experiencing unpredictable behavior.  This causes the developer's smart contract RAM supply to increase as the ipfsentry table rows are not evicted.  If this happens, you may run the [cleanup.js](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/utils/ipfs-service/cleanup.js) file with the following environment variables:

### Mandatory:

```bash
# contract to clean IPFS entries
export CONTRACT=
export NODEOS_CHAINID="aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906" # < mainnet | kylin > "5fff1dae8dc8e2fc4d5b23b2c7665c97f9e9d8edf2b6485a86ba311c25639191"
```

### Optional:

```bash
export NODEOS_SECURED= # defaults to true
export NODEOS_HOST= # defaults to localhost
export NODEOS_PORT= # defaults to 13115
```

Then run with:

```bash
sudo find / -name cleanup.js
node /root/.nvm/versions/node/v10.16.0/lib/node_modules/@liquidapps/dsp/utils/ipfs-service/cleanup.js
```