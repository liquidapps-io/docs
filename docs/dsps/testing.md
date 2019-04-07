Testing
=======

## Test your DSP
### Upload a contract that uses vram 

```bash
zeus unbox vgrab
cd vgrab
zeus compile
cleos set abi mycoldtoken1 vgrab.abi
cleos set code mycoldtoken1 vgrab.wasm
PKEY=mycoltoken1_active_public_key
PERMISSIONS=`echo "{\"threshold\":1,\"keys\":[{\"key\":\"$PKEY\",\"weight\":1}],\"accounts\":[{\"permission\":{\"actor\":\"mycoltoken1\",\"permission\":\"eosio.code\"},\"weight\":1}]}"`
cleos set account permission mycoldtoken1 active $PERMISSIONS owner -p mycoltoken1
cleos push action mycoldtoken1 create '["mycoltoken1","100000000.0000 VTST"]}' -p mycoltoken1
```

### Select your service package and stake towards you DSP

```bash
cleos push action dappservices selectpkg '["mycoltoken1","dspaccount","ipfsservice1","package1"]}' -p mycoltoken1
cleos push action dappservices stake '["mycoltoken1","dspaccount","ipfsservice1","1.0000 DAPP"]}' -p mycoltoken1
cleos set account permission mycoldtoken1 dsp '{"threshold":1,"keys":[],"accounts":[{"permission":{"actor":"dspaccount","permission":"active"},"weight":1}]}' owner -p mycoltoken1
```

### Test your contract and DSP
```bash
cleos -u $MYAPI push action mycoldtoken1 coldissue '["talmuskaleos","1.0000 VTST","hello world"]' -p mycoltoken1
```


### Check logs
```bash
pm2 logs
```

in kubernetes:
```bash
kubectl logs dsp-dspnode-0 -c dspnode-ipfs-svc
```

### look for "xcommit" and "xcleanup" actions for your contract:

https://bloks.io/account/mycoltoken1

