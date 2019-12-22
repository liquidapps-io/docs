LiquidScheduler Getting Started
====================

```  
  _       _                   _       _   ____           _                  _           _               
 | |     (_)   __ _   _   _  (_)   __| | / ___|    ___  | |__     ___    __| |  _   _  | |   ___   _ __ 
 | |     | |  / _` | | | | | | |  / _` | \___ \   / __| | '_ \   / _ \  / _` | | | | | | |  / _ \ | '__|
 | |___  | | | (_| | | |_| | | | | (_| |  ___) | | (__  | | | | |  __/ | (_| | | |_| | | | |  __/ | |   
 |_____| |_|  \__, |  \__,_| |_|  \__,_| |____/   \___| |_| |_|  \___|  \__,_|  \__,_| |_|  \___| |_|   
                 |_|                                                                                    

```

LiquidScheduler is an on chain cron solution for EOS based actions.  One use case would be setting up LiquidHarmony fetches on a continual basis.  Another great place to understand the service is in the [unit tests](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/cron-dapp-service/test/cron.spec.js).

## Prerequisites

* [Zeus](zeus-getting-started.md) - Zeus installs eos and the eosio.cdt if not already installed
* [Kylin Account](kylin-account.md)

## Unbox LiquidScheduler DAPP Service box
This box contains the LiquidScheduler smart contract libraries, DSP node logic, unit tests, and everything else needed to get started integrating / testing the DAPP Network LiquidScheduler in your smart contract.
```bash
# npm install -g @liquidapps/zeus-cmd
zeus unbox cron-dapp-service
cd cron-dapp-service
zeus test
```

## LiquidScheduler Consumer Example Contract used in unit tests
in `contract/eos/cronconsumer/cronconsumer.cpp`
The consumer contract is a great starting point for playing around with the LiquidScheduler syntax.
```cpp
/* IMPORT DAPP NETWORK SERVICE */
#include "../dappservices/cron.hpp"

/* ADD LIQUIDSCHEDULER ACTIONS */
#define DAPPSERVICES_ACTIONS() \
  XSIGNAL_DAPPSERVICE_ACTION \
  CRON_DAPPSERVICE_ACTIONS \

#define DAPPSERVICE_ACTIONS_COMMANDS() \
  CRON_SVC_COMMANDS()

#define CONTRACT_NAME() cronconsumer

CONTRACT_START()
  /* SETUP COUNTER TABLE FOR HOW MANY TIME TO REPEAT CRON */
  TABLE stat {
      uint64_t   counter = 0;
  };

  typedef eosio::singleton<"stat"_n, stat> stats_def;
  /* CONFIGURE LOGIC FOR EACH CRON TASK */
  bool timer_callback(name timer, std::vector<char> payload, uint32_t seconds){

    stats_def statstable(_self, _self.value);
    stat newstats;
    if(!statstable.exists()){
      statstable.set(newstats, _self);
    }
    else{
      newstats = statstable.get();
    }
    newstats.counter++;
    statstable.set(newstats, _self);

    // reschedule
    /* CAN return true TO CREATE INFINITE LOOP */
    return (newstats.counter < 10);
  }
  /*  */
 [[eosio::action]] void testschedule() {
    std::vector<char> payload;
    /* SCHEDULE TIMER WITH 2 BEING SECONDS BETWEEN EACH CRON */
    schedule_timer(_self, payload, 2);
  }
CONTRACT_END((testschedule))
```

## Compile

See the unit testing section for details on adding unit tests.

```bash
zeus compile
# compile and test with
zeus test
# test without compiling
zeus test --no-compile-all
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

## Select and stake DAPP for DSP package | [DSP Portal Link](https://dsphq.io/packages/heliosselene/cronservices/cronservices?network=kylin)
 * Use [the faucet](https://kylin-dapp-faucet.liquidapps.io/) to get some DAPP tokens on Kylin
 * Information on: [DSP Packages and staking DAPP/DAPPHDL (AirHODL token)](dsp-packages-and-staking.md)
```bash
export PROVIDER=heliosselene
export PACKAGE_ID=cronservices

# select your package: 
export SERVICE=cronservices
cleos -u $DSP_ENDPOINT push action dappservices selectpkg "[\"$KYLIN_TEST_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"$PACKAGE_ID\"]" -p $KYLIN_TEST_ACCOUNT@active

# Stake your DAPP to the DSP that you selected the service package for:
cleos -u $DSP_ENDPOINT push action dappservices stake "[\"$KYLIN_TEST_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"10.0000 DAPP\"]" -p $KYLIN_TEST_ACCOUNT@active
```

## Test
To test the sample contract, first check the stat table for the current counter value which should increase after the cron.  Then simply send a `testschedule` action to your smart contract's account name.

### Check RAM Table Row for Counter
Add your `account_name`z to the following curl to see if the counter increases.
```bash
curl --request POST \
  --url $DSP_ENDPOINT/v1/chain/get_table_rows \
  --header 'accept: application/json' \
  --header 'content-type: application/json' \
  --data '{"code":"account_name","table":"stat","scope":"account_name"}'
```

### testschedule
```bash
cleos -u $DSP_ENDPOINT push action $KYLIN_TEST_ACCOUNT testschedule "[\"\"]" -p $KYLIN_TEST_ACCOUNT
```