Demux Backend
=============

In the `config.toml` file in the [DSP Node Setup](./dsp-node.md) you can configure either the `state_history_plugin` or eosrio's `zmq_plugin`.

```bash
[demux]
backend = "state_history_plugin"
# zmq: "zmq_plugin" only if using nodeos with eosrio's version of the ZMQ plugin: https://github.com/eosrio/eos_zmq_plugin
```