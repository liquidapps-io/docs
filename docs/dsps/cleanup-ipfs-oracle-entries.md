Cleanup IPFS and Oracle Entries
========

Sometimes IPFS or Oracle entries are not evicted from a developer's contract due to the DSP experiencing unpredictable behavior.  This causes the developer's smart contract RAM supply to increase as the `ipfsentry` / `oracleentry` table rows are not evicted.  If this happens, you may run the [cleanup.js](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/utils/ipfs-service/cleanup.js) file with the following environment variables:

The cleanup script will auto detect which table to cleanup `ipfsentry` or `oracleentry` depending on which one is present on the contract.  If both are set, you can use the `TABLE` env variable to specify which to cleanup.

### Mandatory:

- `CONTRACT` contract to clean IPFS / oracle entries
- `DSP_ENDPOINT` the DSP's endpoint that you staked to for IPFS and/or Oracle services
- `TABLE` specify table name to be cleaed: (ipfs (vRAM) table: `ipfsentry` or oracle table: `oracleentry`)
- `DSP_ALLOW_API_NON_BROADCAST` enables the `/event` DSP API endpoint to accept non-blocking service events such as xcommits.

```bash
export CONTRACT=lqdportfolio
export DSP_ENDPOINT=http://kylin-dsp-2.liquidapps.io
export TABLE=ipfsentry
export DSP_ALLOW_API_NON_BROADCAST=true
```

### Sidechain:

If cleaning an account on a sidechain, must add the following environment variables. 

- `SIDECHAIN` if using a sidechain, must specify sidechain name (sidechain names can be found [here](../liquidx/example-chains))
- `SIDECHAIN_DSP_PORT` if using a sidechain, must specify sidechain DSP's port
- `DSP_LIQUIDX_CONTRACT` the liquidx contract name must be set `liquidx.dsp` on mainnet if cleaning a sidechain
- `NODEOS_MAINNET_ENDPOINT` set mainnet nodeos endpoint

### Optional:

- `CHUNK_SIZE` represents the number of async requests for cleanups to send to the DSP at a time

```bash
export CHUNK_SIZE= # defaults to 5
export TABLE= # defaults to ipfsentry or oracleentry by detecting from contract
# if using dfuse
export DFUSE_PUSH_ENABLE=true
export DFUSE_API_KEY="" # long-lived API key, see https://docs.dfuse.io/guides/core-concepts/authentication/, e.g. (server_abcdef123123123000000000000000000)
export DFUSE_PUSH_GUARANTEE="in-block" # handoff:1, handoffs:2, handoffs:3, irreversible
export DFUSE_NETWORK="mainnet"
```

Then run with:

```bash
sudo find / -name cleanup.js
node /root/.nvm/versions/node/v10.16.3/lib/node_modules/@liquidapps/dsp/zeus_boxes/seed-utils-cleanup/utils/cleanup.js
```