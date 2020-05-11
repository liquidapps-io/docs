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

LiquidScheduler is an on chain scheduling solution for EOS based actions.  LiquidScheduler takes a `name timer`, a `std::vector<char> payload` object, and an `uint32_t seconds` interval as its parameters.  Multiple timers may be used within the same contract.  For example, if a contract deals with multiple accounts, a timer can be set up for each account.  The `payload` parameter can be used to pass data to the `timer_callback` function which is used to run the scheduled task.  At the end of the `timer_callback` is a return response.  If the return is truthy, the task will reschedule again based on the interval passed, if it is false, the task will not be rescheduled. 

One use case would be setting up LiquidHarmony (oracle) fetches on a continual basis.  An example of this can be seen in the [portfolio example](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/portfolio) which fetches 3 prices on an infinite loop.

Another great place to understand the service is in the [unit tests](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/cron-dapp-service/test/cron.spec.js).

The price feed example uses LiquidHarmony web oracles and the LiquidScheduler to periodically update a price on chain only when the new price is more or less than 1% of the last updated price, conserving CPU by only running actions when necessary. See [here](price-feed)

## Prerequisites

* [Zeus](zeus-getting-started.md) - Zeus installs eos and the eosio.cdt if not already installed
* [Kylin Account](kylin-account.md)

## Unbox LiquidScheduler DAPP Service box
This box contains the LiquidScheduler smart contract libraries, DSP node logic, unit tests, and everything else needed to get started integrating / testing the DAPP Network LiquidScheduler in your smart contract.
```bash
mkdir cron-dapp-service; cd cron-dapp-service
# npm install -g @liquidapps/zeus-cmd
zeus box create
zeus unbox cron-dapp-service
zeus test -c
```

## LiquidScheduler Consumer Example Contract used in unit tests
in `zeus_boxes/contracts/eos/cronconsumer/cronconsumer.cpp`
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

  TABLE stat {
      uint64_t   counter = 0;
  };
  typedef eosio::singleton<"stat"_n, stat> stats_def;

  TABLE payloadtbl {
      string   payload = "";
  };
  typedef eosio::singleton<"payloadtbl"_n, payloadtbl> payload_def;
  
  bool timer_callback(name timer, std::vector<char> payload, uint32_t seconds){
    stats_def statstable(_self, timer.value);
    stat newstats;
    if(!statstable.exists()){
      statstable.set(newstats, _self);
    }
    else{
      newstats = statstable.get();
    }
    if(payload.size() > 0) {
      payload_def payloadtable(_self, timer.value);
      payloadtbl newpayload;
      string payload_string(payload.begin(), payload.end());
      newpayload.payload = payload_string;
      payloadtable.set(newpayload, _self);
      return false;
    }
    newstats.counter++;
    statstable.set(newstats, _self);
    // reschedule
    return (newstats.counter < 45);
    // for infinite loop, return true
  }

  // test scheduling timer scoped to _self with 2s interval
  [[eosio::action]] void testschedule() {
      // optional payload for data to be passed to timer_callback
      std::vector<char> payload;
      schedule_timer(_self, payload, 2);
  }

  // test multiple timers by scoping each timer by account with 2s interval
  [[eosio::action]] void multitimer(name account) {
      // optional payload for data to be passed to timer_callback
      std::vector<char> payload;
      schedule_timer(account, payload, 2);
  }

  // remove timer by scope
  [[eosio::action]] void removetimer(name account) {
      remove_timer(account);
  }

  // test passing payload to timer_callback
  [[eosio::action]] void testpayload(name account, std::vector<char> payload, uint32_t seconds) {
      schedule_timer(account, payload, seconds);
  }
CONTRACT_END((testschedule)(multitimer)(removetimer)(testpayload))
```

## Compile

See the [unit testing](unit-testing) section for details on adding unit tests.

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

# Set contract permissions, add eosio.code
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

### Test actions
```bash
# setup scheduling task to run 45 times for multiple timers/accounts
cleos -u $DSP_ENDPOINT push action $KYLIN_TEST_ACCOUNT testschedule '[INTERVAL]' -p $KYLIN_TEST_ACCOUNT
```

### Custom eosio assertion message

If `shouldAbort` is included in an `eosio::check` assertion, the DSP will cease to process the request and will reschedule it.  This prevents the DSP from using CPU to schedule the next cron.