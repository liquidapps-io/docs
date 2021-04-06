Replay Contract
========

As a DSP, you will want the ability to replay a contract's vRAM (IPFS) related transactions to load that data into your IPFS cluster.  We provide a file that does just that [replay-contract.js](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/utils/ipfs-service/replay-contract.js).

To do this you will need to sign up for an API key from [dfuse.io](https://www.dfuse.io), you can select the *Server to Server* option from the dropdown when creating it. Dfuse offers free keys that last 24 hours, so there's no need to pay.

There are some mandatory and optional environment variables.

Hyperion may also be used thanks to Christoph Michel: [replay-contract-hyperion.js](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/utils/ipfs-service/replay-contract-hyperion.js). List of endpoints here: [https://hyperion.docs.eosrio.io/endpoint/](https://hyperion.docs.eosrio.io/endpoint/), status of endpoint (UP/DOWN) here: [https://bloks.io/hyperion](https://bloks.io/hyperion)

### Mandatory:

```bash
# contract to replay
export CONTRACT=
export NODEOS_CHAINID="aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906" # < mainnet | kylin > "5fff1dae8dc8e2fc4d5b23b2c7665c97f9e9d8edf2b6485a86ba311c25639191"
export DSP_ALLOW_API_NON_BROADCAST=true # enables the `/event` DSP API endpoint to accept non-blocking service events such as xcommits.
export DSP_ENDPOINT=
# using Dfuse?
export DFUSE_API_KEY=
# using Hyperion?
export HYPERION_ENDPOINT=https://eos.hyperion.eosrio.io
export HYPERION_SORT_DIRECTION=asc # can also be desc, ascending starts from first block, descending starts from head block of chains
```

### Sidechain:

If replaying an account on a sidechain, must add the following environment variables. 

- `SIDECHAIN` if using a sidechain, must specify sidechain name (sidechain names can be found [here](../liquidx/example-chains))
- `SIDECHAIN_DSP_PORT` if using a sidechain, must specify sidechain DSP's port
- `DSP_LIQUIDX_CONTRACT` the liquidx contract name must be set `liquidx.dsp` on mainnet if cleaning a sidechain
- `NODEOS_MAINNET_ENDPOINT` set mainnet nodeos endpoint

### Optional:

```bash
export LAST_BLOCK= # defaults to 35000000, this is the last block to sync from, find the first vRAM transaction for the contract and set the block before it
export DFUSE_ENDPOINT= # defaults to 'mainnet.eos.dfuse.io', can set to `kylin.eos.dfuse.io`
export BLOCK_COUNT_PER_QUERY= # defaults to 1000000
export NODEOS_SECURED= # defaults to true
export NODEOS_HOST= # defaults to localhost
export NODEOS_PORT= # defaults to 13115
```

Once you've set those, simply run with:

```bash
sudo find / -name replay-contract.js
node /root/.nvm/versions/node/v10.16.0/lib/node_modules/@liquidapps/dsp/utils/ipfs-service/replay-contract.js
# or hyperion
node /root/.nvm/versions/node/v10.16.0/lib/node_modules/@liquidapps/dsp/utils/ipfs-service/replay-contract-hyperion.js

# sent 6513 saved 7725.26KB 6.42KB/s Block:77756949
```