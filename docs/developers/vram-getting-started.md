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
mkdir mydapp; cd mydapp
zeus unbox dapp --no-create-dir
zeus create contract mycontract
```
## Add your contract logic
in contract/eos/mycontract/mycontract.cpp
```cpp
#pragma once

#include "../dappservices/log.hpp"
#include "../dappservices/plist.hpp"
#include "../dappservices/plisttree.hpp"
#include "../dappservices/multi_index.hpp"

#define DAPPSERVICES_ACTIONS() \
  XSIGNAL_DAPPSERVICE_ACTION \
  LOG_DAPPSERVICE_ACTIONS \
  IPFS_DAPPSERVICE_ACTIONS

/*** IPFS: (xcommit)(xcleanup)(xwarmup) | LOG: (xlogevent)(xlogclear) ***/
#define DAPPSERVICE_ACTIONS_COMMANDS() \
  IPFS_SVC_COMMANDS()LOG_SVC_COMMANDS() 

/*** UPDATE CONTRACT NAME ***/
#define CONTRACT_NAME() mycontract

using std::string;

CONTRACT_START()
  public:

  /*** YOUR LOGIC ***/

  private:
    struct [[eosio::table]] vramaccounts {
      asset    balance;
      uint64_t primary_key()const { return balance.symbol.code().raw(); }
    };

    /*** VRAM MULTI_INDEX TABLE ***/
    typedef dapp::multi_index<"vaccounts"_n, vramaccounts> cold_accounts_t;

    /*** FOR CLIENT SIDE QUERY SUPPORT ***/
    typedef eosio::multi_index<".vaccounts"_n, vramaccounts> cold_accounts_t_v_abi;
    TABLE shardbucket {
      std::vector<char> shard_uri;
      uint64_t shard;
      uint64_t primary_key() const { return shard; }
    };
    typedef eosio::multi_index<"vaccounts"_n, shardbucket> cold_accounts_t_abi;

/*** ADD ACTIONS ***/
CONTRACT_END((your)(actions)(here))
```

## Add your contract test
in tests/mycontract.spec.js
```javascript
import 'mocha';
require('babel-core/register');
require('babel-polyfill');
const { assert } = require('chai');
const { getNetwork, getCreateKeys } = require('../extensions/tools/eos/utils');
var Eos = require('eosjs');
const getDefaultArgs = require('../extensions/helpers/getDefaultArgs');
const artifacts = require('../extensions/tools/eos/artifacts');
const deployer = require('../extensions/tools/eos/deployer');
const { genAllocateDAPPTokens } = require('../extensions/tools/eos/dapp-services');

/*** UPDATE CONTRACT CODE ***/
var contractCode = 'mycontract';

var ctrt = artifacts.require(`./${contractCode}/`);
const delay = ms => new Promise(res => setTimeout(res, ms));

describe(`${contractCode} Contract`, () => {
  var testcontract;

  /*** SET CONTRACT NAME(S) ***/
  const code = 'airairairai1';
  const code2 = 'testuser5';
  var account = code;

  before(done => {
    (async () => {
      try {
        
        /*** DEPLOY CONTRACT ***/
        var deployedContract = await deployer.deploy(ctrt, code);
        
        /*** DEPLOY ADDITIONAL CONTRACTS ***/
        var deployedContract2 = await deployer.deploy(ctrt, code2);
        
        await genAllocateDAPPTokens(deployedContract, 'ipfs');
        var selectedNetwork = getNetwork(getDefaultArgs());
        var config = {
          expireInSeconds: 120,
          sign: true,
          chainId: selectedNetwork.chainId
        };
        if (account) {
          var keys = await getCreateKeys(account);
          config.keyProvider = keys.privateKey;
        }
        var eosvram = deployedContract.eos;
        config.httpEndpoint = 'http://localhost:13015';
        eosvram = new Eos(config);
        testcontract = await eosvram.contract(code);
        done();
      } catch (e) {
        done(e);
      }
    })();
  });
        
  /*** DISPLAY NAME FOR TEST, REPLACE 'coldissue' WITH ANYTHING ***/
  it('coldissue', done => {
    (async () => {
      try {        
       
        /*** SETUP VARIABLES ***/
        var symbol = 'AIR';
                
        /*** DEFAULT failed = false, SET failed = true IN TRY/CATCH BLOCK TO FAIL TEST ***/
        var failed = false;
                    
        /*** SETUP CHAIN OF ACTIONS ***/
        await testcontract.create({
          issuer: code2,
          maximum_supply: `1000000000.0000 ${symbol}`
        }, {
          authorization: `${code}@active`,
          broadcast: true,
          sign: true
        });

        /*** CREATE ADDITIONAL KEYS AS NEEDED ***/
        var key = await getCreateKeys(code2);
        
        var testtoken = testcontract;
        await testtoken.coldissue({
          to: code2,
          quantity: `1000.0000 ${symbol}`,
          memo: ''
        }, {
          authorization: `${code2}@active`,
          broadcast: true,
          keyProvider: [key.privateKey],
          sign: true
        });
        
        /*** ADD DELAY BETWEEN ACTIONS ***/
        await delay(3000);
        
        /*** EXAMPLE TRY/CATCH failed = true ***/
        try {
          await testtoken.transfer({
            from: code2,
            to: code,
            quantity: `100.0000 ${symbol}`,
            memo: ''
          }, {
            authorization: `${code2}@active`,
            broadcast: true,
            keyProvider: [key.privateKey],
            sign: true
          });
        } catch (e) {
          failed = true;
        }
        
        /*** ADD CUSTOM FAILURE MESSAGE ***/
        assert(failed, 'should have failed before withdraw');
        
        /*** ADDITIONAL ACTIONS ... ***/

        done();
      } catch (e) {
        done(e);
      }
    })();
  });
});

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