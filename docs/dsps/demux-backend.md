Demux Backend
=============

In the [`config.toml`](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/dapp-services-deploy/sample-config.toml) file in the [DSP Node Setup](./dsp-node.md) you can configure the `state_history_plugin`.

You can also configure the `HEAD_BLOCK` to sync demux from.  If you experience any issues with demux syncing from an older block, you may try syncing demux at the head block by finding it at [bloks.io](bloks.io) and setting it to `head_block`.

```bash
[demux]
backend = "state_history_plugin"

# head block to sync demux from
head_block = 35000000
```