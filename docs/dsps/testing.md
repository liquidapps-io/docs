Testing
=======

## Test your DSP with vRAM

### Run a sample contract using your DSP:
On a remote machine, follow The [vRAM Getting Started Tutorial](../developers/vram-getting-started.md)

### Check logs on your DSP Node
```bash
pm2 logs
```

in kubernetes:
```bash
kubectl logs dsp-dspnode-0 -c dspnode-ipfs-svc
```

### Look for "xcommit" and "xcleanup" actions for your contract:

[mycoldtoken1 at bloks.io](https://bloks.io/account/mycoldtoken1)

