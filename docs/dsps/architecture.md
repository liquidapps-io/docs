Architecture
============

A DSP consists of an EOS state history node (non block producing node without a full history setup), an IPFS cluster, a PostgreSQL Database, and a DSP API endpoint.  All 4 of these operations may be run on the same machine or separately.  All necessary settings may be found in the [`config.toml`](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/dapp-services-deploy/sample-config.toml) file.

* EOS state history node - to run a DSP requires running a non block producing EOS node.  A full history node is also not required.  The EOS node is configured with a backend storage mechanism: `state_history_plugin`.
* IPFS Cluster node - the IPFS cluster stores all vRAM information so that it may be loaded into RAM as a caching mechanism.  The InterPlanetary File System is a protocol and peer-to-peer network for storing and sharing data in a distributed file system. IPFS uses content-addressing to uniquely identify each file in a global namespace connecting all computing devices.  The documentation shows how to setup a local IPFS cluster, but you may also configure an external IPFS cluster in the `config.toml` file.
* PostgreSQL Database - a PostgreSQL database is utilized to prevent duplicate transactions by creating, updating, and retrieving transaction data.
* DSP API - the DSP API is responsible for performing all DAPP service logic such as a vRAM `get_table_row` call, parsing and sending a LiquidAccount transaction, servicing an oracle call, etc.