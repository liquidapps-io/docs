LiquidAccounts Getting Started
====================

```  
  _       _                   _       _      _                                            _         
 | |     (_)   __ _   _   _  (_)   __| |    / \      ___    ___    ___    _   _   _ __   | |_   ___ 
 | |     | |  / _` | | | | | | |  / _` |   / _ \    / __|  / __|  / _ \  | | | | | '_ \  | __| / __|
 | |___  | | | (_| | | |_| | | | | (_| |  / ___ \  | (__  | (__  | (_) | | |_| | | | | | | |_  \__ \
 |_____| |_|  \__, |  \__,_| |_|  \__,_| /_/   \_\  \___|  \___|  \___/   \__,_| |_| |_|  \__| |___/
                 |_|                                                                                

```

LiquidAccounts are EOS accounts that are stored in vRAM instead of RAM.  This drastically reduces the cost of creating accounts on EOS.  Another great place to understand the service is in the [unit tests](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/test/vaccountsconsumer.spec.js).

## Prerequisites

* [Zeus](zeus-getting-started.md) - Zeus installs eos and the eosio.cdt if not already installed
* [Kylin Account](kylin-account.md)

## Unbox LiquidAccounts DAPP Service box
This box contains the LiquidAccounts smart contract libraries, DSP node logic, unit tests, and everything else needed to get started integrating / testing the DAPP Network LiquidAccounts in your smart contract.
```bash
# npm install -g @liquidapps/zeus-cmd
zeus unbox vaccounts-dapp-service
cd vaccounts-dapp-service
zeus test -c
```

## LiquidAccount Consumer Example Contract used in unit tests
in `contract/eos/vaccountsconsumer/vaccountsconsumer.cpp`
The consumer contract is a great starting point for playing around with the LiquidAccount syntax.
```cpp
/* DELAY REMOVAL OF USER DATA INTO VRAM */
/* ALLOWS FOR QUICKER ACCESS TO USER DATA WITHOUT THE NEED TO WARM DATA UP */
#define VACCOUNTS_DELAYED_CLEANUP 120

/* ADD NECESSARY LIQUIDACCOUNT / VRAM INCLUDES */
#include "../dappservices/vaccounts.hpp"
#include "../dappservices/ipfs.hpp"
#include "../dappservices/multi_index.hpp"

/* ADD LIQUIDACCOUNT / VRAM RELATED ACTIONS */
#define DAPPSERVICES_ACTIONS() \
  XSIGNAL_DAPPSERVICE_ACTION \
  IPFS_DAPPSERVICE_ACTIONS \
  VACCOUNTS_DAPPSERVICE_ACTIONS

#define DAPPSERVICE_ACTIONS_COMMANDS() \
  IPFS_SVC_COMMANDS()VACCOUNTS_SVC_COMMANDS() 
  
#define CONTRACT_NAME() vaccountsconsumer 


CONTRACT_START()
  
  /* THE FOLLOWING STRUCT DEFINES THE PARAMS THAT MUST BE PASSED */
  struct dummy_action_hello {
      name vaccount;
      uint64_t b;
      uint64_t c;
  
      EOSLIB_SERIALIZE( dummy_action_hello, (vaccount)(b)(c) )
  };
  
  /* DATA IS PASSED AS PAYLOADS INSTEAD OF INDIVIDUAL PARAMS */
  [[eosio::action]] void hello(dummy_action_hello payload) {
    /* require_vaccount is the equivalent of require_auth for EOS */
    require_vaccount(payload.vaccount);
    
    print("hello from ");
    print(payload.vaccount);
    print(" ");
    print(payload.b + payload.c);
    print("\n");
  }
  
  [[eosio::action]] void hello2(dummy_action_hello payload) {
    print("hello2(default action) from ");
    print(payload.vaccount);
    print(" ");
    print(payload.b + payload.c);
    print("\n");
  }
  
  [[eosio::action]] void init(dummy_action_hello payload) {
  }
  
  /* EACH ACTION MUST HAVE A STRUCT THAT DEFINES THE PAYLOAD SYNTAX TO BE PASSED */
  VACCOUNTS_APPLY(((dummy_action_hello)(hello))((dummy_action_hello)(hello2)))
  
CONTRACT_END((init)(hello)(hello2)(regaccount)(xdcommit)(xvinit))
```

## Compile

See the unit testing section for details on adding unit tests.

```bash
zeus compile
# test without compiling
zeus test
# compile and test with
zeus test -c
```

## Deploy Contract
```bash
export DSP_ENDPOINT=https://kylin-dsp-2.liquidapps.io
export KYLIN_TEST_ACCOUNT=<ACCOUNT_NAME>
export KYLIN_TEST_PUBLIC_KEY=<ACTIVE_PUBLIC_KEY>
# Buy RAM:
cleos -u $DSP_ENDPOINT system buyram $KYLIN_TEST_ACCOUNT $KYLIN_TEST_ACCOUNT "200.0000 EOS" -p $KYLIN_TEST_ACCOUNT@active
# Set contract code and abi
cleos -u $DSP_ENDPOINT set contract $KYLIN_TEST_ACCOUNT vaccountsconsumer -p $KYLIN_TEST_ACCOUNT@active

# Set contract permissions
cleos -u $DSP_ENDPOINT set account permission $KYLIN_TEST_ACCOUNT active "{\"threshold\":1,\"keys\":[{\"weight\":1,\"key\":\"$KYLIN_TEST_PUBLIC_KEY\"}],\"accounts\":[{\"permission\":{\"actor\":\"$KYLIN_TEST_ACCOUNT\",\"permission\":\"eosio.code\"},\"weight\":1}]}" owner -p $KYLIN_TEST_ACCOUNT@active
```

## Select and stake DAPP for DSP package | [DSP Portal Link](https://dsphq.io/packages/heliosselene/accountless1/accountless1?network=kylin)
 * Use [the faucet](https://kylin-dapp-faucet.liquidapps.io/) to get some DAPP tokens on Kylin
 * Information on: [DSP Packages and staking DAPP/DAPPHDL (AirHODL token)](dsp-packages-and-staking.md)
```bash
export PROVIDER=heliosselene
export PACKAGE_ID=accountless1

# select your package: 
export SERVICE=accountless1
cleos -u $DSP_ENDPOINT push action dappservices selectpkg "[\"$KYLIN_TEST_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"$PACKAGE_ID\"]" -p $KYLIN_TEST_ACCOUNT@active

# Stake your DAPP to the DSP that you selected the service package for:
cleos -u $DSP_ENDPOINT push action dappservices stake "[\"$KYLIN_TEST_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"10.0000 DAPP\"]" -p $KYLIN_TEST_ACCOUNT@active
```

## Test
First you'll need to initialize the LiquidAccounts implementation with the `chain_id` of the platform you're operating on.

```bash
# kylin
export CHAIN_ID=5fff1dae8dc8e2fc4d5b23b2c7665c97f9e9d8edf2b6485a86ba311c25639191
cleos -u $DSP_ENDPOINT push action $KYLIN_TEST_ACCOUNT xvinit "[\"$CHAIN_ID\"]" -p $KYLIN_TEST_ACCOUNT
```

Then you can begin registering accounts.  You will need to do this either in a nodejs environment using the [`dapp-client-lib`](https://www.npmjs.com/package/@liquidapps/dapp-client), or you can use the `zeus vaccounts push-action`.  [Here is an example of using the lib to register an account.](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/client/examples/push_register_liquid_account.ts).

*All payloads must include a key value pair with `"vaccount":"vaccountname"` or the transaction will fail*.  This is so the `dapp-client` can fetch the nonce associated with the LiquidAccount.

#### dapp-client

```bash
npm install -g @liquidapps/dapp-client
```

This example takes:

- the contract name the LiquidAccount project is deployed to
- the active private key of that account
- the `regaccount` as the action name
- the payload with the `vaccount` name

After registering an account, you may also use the library to [push LiquidAccount transactions](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/client/examples/push_liquid_account_transaction.ts).  In the linked example, you can see that the action name has changed to `hello` and the payload has changed to include the required parameters.

#### Zeus vaccounts push-action

Push LiquidAccount actions easily with zeus's wrapper of the `dapp-client` library.  You can pass a `--dsp-url` for your DAPP Service Provider's API endpoint.  Then pass the name of the contract that the LiquidAccount code is deployed to, the action name (`regaccount` for example), and the payload.

You also have the ability to store your private keys with or without encryption.  If you choose to encrypt, you can pass the `--encrypted` flag when creating a new account to store the keys.  You can provide a password by command line, or with the flag `--password`.  If you use the account again, zeus will look for the key based on what network you are operating on.  If it finds it, it will use that key to sign and prompt for a password if needed.

```bash
zeus vaccounts push-action <CONTRACT> <ACTION> <PAYLOAD> --dsp-url https://kylin-dsp-2.liquidapps.io

# optional flags:

--dsp-url # url to DAPP Service Provider's API endpoint
# default: https://kylin-dsp-2.liquidapps.io
--private-key # LiquidAccount private key, can be provided or auto generated
# will be auto generated and stored in the storage path if not provided
--encrypted # Encrypt the LiquidAccount keys with a password
# default: false
--password # password to encrypt the keys with
--network # network LiquidAccount contract deployed on (other options: kylin, jungle, mainnet)
# development (local)
--storage-path # path to the wallet which will store the LiquidAccount key
# default: path.join(require('os').homedir(), '.zeus')

# Example:
zeus vaccounts push-action test1v regaccount '{"vaccount":"vaccount1"}'
zeus vaccounts push-action vacctstst123 regaccount '{"vaccount":"vaccount2"}' --private-key 5KJL... -u https://kylin-dsp-2.liquidapps.io
zeus vaccounts push-action vacctstst123 regaccount '{"vaccount":"vaccount3"}' -u http://kylin-dsp-2.liquidapps.io/ --encrypted --network=kylin --password=password
```

## Deserialize Payload | [`des-payload.js`](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/utils/vaccounts-service/des-payload.js)
This file is under the utility/tool and it is for deserializing `xvexec` data (vaccount action data).

Example:

`deserializeVactionPayload('dappaccoun.t', '6136465e000000002a00000000000000aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e90600000000000000008090d9572d3ccdcd002370ae375c19feaa49e0d336557df8aa49010000000000000004454f5300000000026869', 'https://mainnet.eos.dfuse.io')`

returns

```json
{
   "payload":{
      "expiry":"1581659745",
      "nonce":"42",
      "chainid":"ACA376F206B8FC25A6ED44DBDC66547C36C6C33E3A119FFBEAEF943642F0E906",
      "action":{
         "account":"",
         "action_name":"transfervacc",
         "authorization":[

         ],
         "action_data":"70AE375C19FEAA49E0D336557DF8AA49010000000000000004454F5300000000026869"
      }
   },
   "deserializedAction":{
      "payload":{
         "vaccount":"dapjwaewayrb",
         "to":"dapjkzepavdy",
         "quantity":"0.0001 EOS",
         "memo":"hi"
      }
   }
}
```