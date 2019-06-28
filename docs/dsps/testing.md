Testing
=======

## Test your DSP with vRAM
Please note, if you wish to test on the mainnet, this will require the [purchase of DAPP tokens](https://liquidapps.io/auction) or the use of [DAPPHDL tokens (Air-HODL)](https://medium.com/@liquidapps/air-hodl-dapp-network-tokens-for-eos-holders-f879412f2e49).  In the case of Kylin, we provide a [DAPP token faucet](https://kylin-dapp-faucet.liquidapps.io/).

### Create Mainnet or Kylin Account:

- [Kylin](../developers/kylin-account.md)
- [Mainnet](https://www.eosx.io/guides/how-to-create-account)

### Install Zeus:

```bash
npm install -g @liquidapps/zeus-cmd
```

### Unbox coldtoken contract:

```bash
zeus unbox coldtoken
cd coldtoken
```

### Compile and deploy contract for testing:
```bash
# your DSP's API endpoint
export DSP_ENDPOINT=
# a new account to deploy your contract to
export ACCOUNT=
# your new account's active public key
export ACTIVE_PUBLIC_KEY=
# compile coldtoken contract
zeus compile
cd contracts/eos
# set eosio.code permission
cleos -u $DSP_ENDPOINT set account permission $ACCOUNT active "{\"threshold\":1,\"keys\":[{\"weight\":1,\"key\":\"$ACTIVE_PUBLIC_KEY\"}],\"accounts\":[{\"permission\":{\"actor\":\"$ACCOUNT\",\"permission\":\"eosio.code\"},\"weight\":1}]}" owner -p $ACCOUNT@active
# set contract
cleos -u $DSP_ENDPOINT set contract $ACCOUNT ./coldtoken
```

### Select and stake to DSP:
```bash
# your DSP's account
export DSP_ACCOUNT=
# your DSP's service
export DSP_SERVICE=
# your DSP's package
export DSP_PACKAGE=
# your DSP's minimum stake quantity in DAPP or DAPPHDL (example: 10.0000 DAPP or 10.0000 DAPPHDL)
export MIN_STAKE_QUANTITY=
# select DSP package
cleos -u $DSP_ENDPOINT push action dappservices selectpkg "{\"owner\":\"$ACCOUNT\",\"provider\":\"$DSP_ACCOUNT\",\"service\":\"$DSP_SERVICE\",\"package\":\"$DSP_PACKAGE\"}" -p $ACCOUNT
# stake to DSP package with DAPP
cleos -u $DSP_ENDPOINT push action dappservices stake "{\"owner\":\"$ACCOUNT\",\"provider\":\"$DSP_ACCOUNT\",\"service\":\"$DSP_SERVICE\",\"quantity\":\"$MIN_STAKE_QUANTITY\"}" -p $ACCOUNT
# stake to DSP package with DAPPHDL, only available on mainnet
cleos -u $DSP_ENDPOINT push action dappairhodl1 stake "{\"owner\":\"$ACCOUNT\",\"provider\":\"$DSP_ACCOUNT\",\"service\":\"$DSP_SERVICE\",\"quantity\":\"$MIN_STAKE_QUANTITY\"}" -p $ACCOUNT
```

### Run test commands:

```bash
# create coldtoken
cleos -u $DSP_ENDPOINT push action $ACCOUNT create "{\"issuer\":\"$ACCOUNT\",\"maximum_supply\":\"1000000000 TEST\"}" -p $ACCOUNT
# issue some TEST
cleos -u $DSP_ENDPOINT push action $ACCOUNT issue "{\"to\":\"$ACCOUNT\",\"quantity\":\"1000 TEST\",\"memo\":\"Testing issue\"}" -p $ACCOUNT
```

### Test vRAM get table row:

```bash
# you must be in the root of the box to run this command
cd ../../
zeus get-table-row $ACCOUNT "accounts" $ACCOUNT "TEST" --endpoint $DSP_ENDPOINT | python -m json.tool
# with curl:
curl http://$DSP_ENDPOINT/dsp/ipfsservice1/get_table_row -d '{"contract":"CONTRACT_ACCOUNT","scope":"SCOPE","table":"TABLE_NAME","key":"TABLE_PRIMARY_KEY"}' | python -m json.tool
```

### Transfer:

```bash
cleos -u $DSP_ENDPOINT push action $ACCOUNT transfer "{\"from\":\"$ACCOUNT\",\"to\":\"natdeveloper\",\"quantity\":\"1000 TEST\",\"memo\":\"Testing transfer\"}" -p $ACCOUNT
zeus get-table-row $ACCOUNT "accounts" "natdeveloper" "TEST" --endpoint $DSP_ENDPOINT | python -m json.tool
```

### Check logs on your DSP Node
```bash
pm2 logs
```

### vRAM related actions to look for in a block explorer:

Look for "xcommit" and "xcleanup" actions on your contract: https://bloks.io/

* **xcommit** - The commit request instructs a DSP to write new data to their local IPFS cluster node. A developer can utilize the setData function from within their smart contract to first hash the new data in order to return a URI, before dispatching a commit request which is caught by the DSP node. In a similar way the getData function can be utilized in order to fetch the data for the smart contract or request a Warmup in case it is missing.

* **xcleanup** - A cleanup request sends a request to the DSP to evict a file from the cache. This is an asynchronous request.

More information on vRAM related actions can be found here: https://medium.com/@liquidapps/vram-guide-for-experts-f809c8f82a27