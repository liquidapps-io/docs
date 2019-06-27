Architecture
============

* EOS full node - to run a DSP requires running a full EOS node.  The EOS node is also configured with a backend storage mechanism, whether that be the `state_history_plugin` or the [zmq_plugin from eosrio](https://github.com/eosrio/eos_zmq_plugin).
* IPFS Cluster node - locally hosts your vRAM related data in IPFS for fast response times.
* DSP Node - the DSP node is responsible for performing actions like `get_table_row` for vRAM related data and in the case of LiquidAccounts, parsing related trx's and sending them to the chain.