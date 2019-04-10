vRAM Getting Started
====================

```            
       _____            __  __ 
      |  __ \     /\   |  \/  |
__   _| |__) |   /  \  | \  / |
\ \ / /  _  /   / /\ \ | |\/| |
 \ V /| | \ \  / ____ \| |  | |
  \_/ |_|  \_\/_/    \_\_|  |_|
            
```
## Prerequisites

* [Zeus](zeus-getting-started.md)
* [Kylin Account](kylin-account.md)

## Unbox template
```bash
zeus unbox vram-getting-started
```
## Add your contract logic
in contract/eos/mycontract/mycontract.cpp
```cpp
/* TODO */
```

## Add your contract test
in tests/mycontract.spec.js
```javascript
// TODO
```
## Compile and test
```bash
zeus test
```

## Deploy Contract
```bash
export EOS_ENDPOINT=https://kylin-dsp-1.liquidapps.io
# Buy RAM:
cleos -u $EOS_ENDPOINT system buyram $KYLIN_TEST_ACCOUNT $KYLIN_TEST_ACCOUNT "50.0000 EOS" -p $KYLIN_TEST_ACCOUNT@active
# Set contract code and abi
cleos -u $EOS_ENDPOINT set contract $KYLIN_TEST_ACCOUNT ../contract -p $KYLIN_TEST_ACCOUNT@active

# Set contract permissions
cleos -u $EOS_ENDPOINT set account permission $KYLIN_TEST_ACCOUNT active "{\"threshold\":1,\"keys\":[{\"weight\":1,\"key\":\"$KYLIN_TEST_PUBLIC_KEY\"}],\"accounts\":[{\"permission\":{\"actor\":\"$KYLIN_TEST_ACCOUNT\",\"permission\":\"eosio.code\"},\"weight\":1}]}" owner -p $KYLIN_TEST_ACCOUNT@active
```


```bash
# TBD: 
#   zeus import key $KYLIN_TEST_ACCOUNT $KYLIN_TEST_PRIVATE_KEY
#   zeus create contract-deployment contractcode $KYLIN_TEST_ACCOUNT
#   zeus migrate --network=kylin
```

## Select and stake DAPP for DSP package

```bash
export PROVIDER=uuddlrlrbass
export PACKAGE_ID=package1
export MY_ACCOUNT=$KYLIN_TEST_ACCOUNT

# select your package: 
export SERVICE=ipfsservice1
cleos -u $EOS_ENDPOINT push action dappservices selectpkg "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"$PACKAGE_ID\"]" -p $MY_ACCOUNT@active

# Stake your DAPP to the DSP that you selected the service package for:
cleos -u $EOS_ENDPOINT push action dappservices stake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPP\"]" -p $MY_ACCOUNT@active
```

[DSP Package and staking](dsp-packages-and-staking.md)

## Test
Finally you can now test your vRAM implementation by sending an action through your DSP's API endpoint

```bash
cleos -u $EOS_ENDPOINT push action $KYLIN_TEST_ACCOUNT youraction1 "[\"param1\",\"param2\"]" -p $KYLIN_TEST_ACCOUNT@active
```

The result should look like:
```
executed transaction: 865a3779b3623eab94aa2e2672b36dfec9627c2983c379717f5225e43ac2b74a  104 bytes  67049 us
#  yourcontract <= yourcontract::youraction1         {"param1":"param1","param2":"param2"}
>> {"version":"1.0","etype":"service_request","payer":"yourcontract","service":"ipfsservice1","action":"commit","provider":"","data":"DH......"}
```