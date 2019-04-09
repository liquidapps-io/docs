Testing
=======

## Test your DSP with vRAM

### Unbox and Compile
```bash
zeus unbox vgrab
cd vgrab
zeus compile
```

### Upload contract 

```bash
export TEST_ACCOUNT=mycoldtoken1
PKEY=mycoldtoken1_active_public_key

cleos set abi $TEST_ACCOUNT vgrab.abi
cleos set code $TEST_ACCOUNT vgrab.wasm
PERMISSIONS=`echo "{\"threshold\":1,\"keys\":[{\"key\":\"$PKEY\",\"weight\":1}],\"accounts\":[{\"permission\":{\"actor\":\"$TEST_ACCOUNT\",\"permission\":\"eosio.code\"},\"weight\":1}]}"`
cleos set account permission $TEST_ACCOUNT active $PERMISSIONS owner -p $TEST_ACCOUNT
cleos push action $TEST_ACCOUNT create "[\"$TEST_ACCOUNT\",\"100000000.0000 VTST\"]" -p $TEST_ACCOUNT
```

### Select your service package and stake towards you DSP

```bash
cleos push action dappservices selectpkg "[\"$TEST_ACCOUNT\",\"$DSP_ACCOUNT\",\"ipfsservice1\",\"package1\"]" -p $TEST_ACCOUNT
cleos push action dappservices stake "[\"$TEST_ACCOUNT\",\"$DSP_ACCOUNT\",\"ipfsservice1\",\"1.0000 DAPP\"]" -p $TEST_ACCOUNT
cleos set account permission $TEST_ACCOUNT dsp "{\"threshold\":1,\"keys\":[],\"accounts\":[{\"permission\":{\"actor\":\"$DSP_ACCOUNT\",\"permission\":\"active\"},\"weight\":1}]}" owner -p $TEST_ACCOUNT
```

### Test your contract and DSP
```bash
cleos -u $DSP_ENDPOINT push action $TEST_ACCOUNT coldissue '["talmuskaleos","1.0000 VTST","hello world"]' -p $TEST_ACCOUNT
```


### Check logs
```bash
pm2 logs
```

in kubernetes:
```bash
kubectl logs dsp-dspnode-0 -c dspnode-ipfs-svc
```

### Look for "xcommit" and "xcleanup" actions for your contract:

(https://bloks.io/account/mycoldtoken1)

