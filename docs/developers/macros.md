DAPP Network Macros
====================

The DAPP Network utilizes a series of macros and service actions to perform different logic.  Many of the macros use a special syntax to interact with the DAPP Service Providers.

In this portion of the docs we'll have a look at the macros associated with the DAPP Network's core services (beta and above).  We'll also explore some of the macros exposed by the `dappservices` contract (core contract to the DAPP Network that handles staking, the DAPP token, and packages).  This is intended to be an additional reading piece to the getting started sections.

- [dappservices](#dappservices)
- [vRAM](#vRAM)
- [LiquidAccounts](#LiquidAccounts)
- [LiquidHarmony (oracles)](#LiquidHarmony)
- [LiquidScheduler (cron)](#LiquidScheduler)

## dappservices

### CONTRACT_START() | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/dapp-services/contracts/eos/dappservices/dappservices.hpp#L486)

```cpp
/**
  *  Wraps the eosio::contract class and provides the DAPP service actions needed for each service utilized as well as a specified CONTRACT_NAME. Intended to be used with CONTRACT_END.
  *
  *  @param CONTRACT_NAME - defines smart contract's name
  *  @param DAPPSERVICES_ACTIONS - specifies DAPP Service actions that must be included to perform a service
  *
  *  @return eosio::contract class with DAPP service actions defined under DAPPSERVICES_ACTIONS() and CONTRACT_NAME
  *
  *  Example:
  *
  *  @code
  *  #define DAPPSERVICES_ACTIONS() \
  *  XSIGNAL_DAPPSERVICE_ACTION \
  *  ORACLE_DAPPSERVICE_ACTIONS
  *  
  *  #define CONTRACT_NAME() oracleconsumer 
  *  
  *  CONTRACT_START()
  *  @endcode
  */

#define CONTRACT_START() \
CONTRACT CONTRACT_NAME() : public eosio::contract { \
  using contract::contract; \
public: \
DAPPSERVICES_ACTIONS()
```

### CONTRACT_END() | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/dapp-services/contracts/eos/dappservices/dappservices.hpp#L492)

```cpp
/**
  *  Generates the EOSIO_DISPATCH_SVC list of actions for a smart contract.  Intended to be used with CONTRACT_START.
  *
  *  @param CONTRACT_NAME - contract name for eosio smart contract
  *  @param methods - list of actions to be included in the smart contract's ABI
  *
  *  @return EOSIO_DISPATCH_SVC list of actions
  *
  *  Example:
  *
  *  @code
  *  #define CONTRACT_NAME() oracleconsumer 
  *  
  *  CONTRACT_END((testget)(testrnd))
  *  @endcode
  */

#define CONTRACT_END(methods) \
}; \
EOSIO_DISPATCH_SVC(CONTRACT_NAME(),methods)
```

## vRAM

### dapp::multi_index | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/contracts/eos/dappservices/multi_index.hpp)

```cpp
/**
  *  DAPP Network version of the eosio::multi_index container.  Enables storage of information in IPFS (vRAM) when not needed in RAM.  When data is warmed up, it is checked against the merkle root stored on-chain to ensure integrity and prevent a DAPP Service Provider from needing to be a trusted entity.
  *
  *  @param {name} code - account that owns table
  *  @param {uint64_t} scope - scope identifier within the code hierarchy
  *  @param {uint32_t} [shards=1024] - amount of shards to include for each table
  *  @param {buckets_per_shard} [buckets_per_shard=64] - number of buckets to use per shard
  *  @param {bool} [pin_shards=false] - persist shards to RAM
  *  @param {bool} [pin_buckets=false] - persist shard buckets to RAM
  *  @param {uint32_t} [cleanup_delay=0] - time in seconds before data loaded in RAM is committed to vRAM (IPFS)
  *
  *  @return advanced_multi_index container
  *
  *  Notes
  *  - by utilizing the cleanup_delay param, data persists to RAM and can be used until committed.  One use case for this is a session based application where a user does not need their data committed to RAM after each transaction.  The cleanup_delay is reset each time a user uses the data.  If a user is inactive for say 120 seconds, then their data can be committed.  Utilizing the cleanup_delay also prevents the latency associated with warming up data into RAM from vRAM (IPFS).
  *  - by selecting pin_shards = true, the shards will not be evicted from the ipfsentry table after the transaction that required the data is run
  * 
  * 
  *  Example:
  *
  *  @code
  *  TABLE testindex {
  *   uint64_t id;
  *   uint64_t sometestnumber;
  *   uint64_t primary_key()const {return id;}
  *  };
  *  
  *  typedef dapp::multi_index<"test"_n, testindex> testindex_t;
  *  typedef eosio::multi_index<".test"_n, testindex> testindex_t_v_abi;
  *  typedef eosio::multi_index<"test"_n, testindex_shardbucket> testindex_t_abi;
  *  @endcode
  */

  TABLE testindex {
    uint64_t id;
    uint64_t sometestnumber;
    uint64_t primary_key()const {return id;}
  };
  
  typedef dapp::multi_index<"test"_n, testindex> testindex_t;
  typedef eosio::multi_index<".test"_n, testindex> testindex_t_v_abi;
  typedef eosio::multi_index<"test"_n, testindex_shardbucket> testindex_t_abi;

  // get some data
  [[eosio::action]] void testget(uint64_t id) {
    testindex_t testset(_self,_self.value, 1024, 64, false, false, 0);
    auto const& data = testset.get(id,"data not found");
  }

  // add new data row with .emplace
  [[eosio::action]] void testemplace(uint64_t id) {
    testindex_t testset(_self,_self.value, 1024, 64, false, false, 0);
    testset.emplace(_self, [&]( auto& a ){
      a.id = id;
    });
  }

  // modify existing data row with .modify
  [[eosio::action]] void testmodify(uint64_t id, uint64_t new_id) {
    testindex_t testset(_self,_self.value, 1024, 64, false, false, 0);
    auto existing = testset.find(id);
    testset.modify(existing,_self, [&]( auto& a ){
      a.id = new_id;
    });
  }  

  // test adding a delayed cleanup
  [[eosio::action]] void testdelay(uint64_t id, uint64_t value, uint32_t delay_sec) {
    testindex_t testset(_self,_self.value, 1024, 64, false, false, delay_sec);
    auto existing = testset.find(id);
    if(existing == testset.end())
      testset.emplace(_self, [&]( auto& a ){
        a.id = id;
        a.sometestnumber = value;
      });
    else
      testset.modify(existing,_self, [&]( auto& a ){
        a.sometestnumber = value;
      });
  }
  
  // test loading a new manifest file
  // a manifest file is a snapshot of the current state of the table
  // manifests can be used to version the database or to revert back
  [[eosio::action]] void testman(dapp::manifest man) {
    testindex_t testset(_self,_self.value);
    testset.load_manifest(man,"Test");
  }

  // increment revision number, reset shards and buckets_per_shard params and next_available_key in vconfig table
  [[eosio::action]] void testclear() {
    testindex_t testset(_self,_self.value, 1024, 64, false, false, 0);
    testset.clear();
  }
```

## LiquidAccounts

### payload definition | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/contracts/eos/vaccountsconsumer/vaccountsconsumer.cpp#L20)

```cpp
/**
  *  LiquidAccounts use a payload syntax in order to pass params.  This payload is setup as a struct and uses the EOSLIB_SERIALIZE to create the payload type
  *
  *  @param {name} vaccount - vaccount that owns table
  *
  *  Notes
  *  - primary key for the vaccount payload table must be "vaccount" for client side library support
  * 
  *  Example:
  *
  *  @code
  *  struct dummy_action_hello {
  *   name vaccount;
  *   uint64_t b;
  *   uint64_t c;
  *
  *   EOSLIB_SERIALIZE( dummy_action_hello, (vaccount)(b)(c) )
  *  };
  *
  *  [[eosio::action]] void hello(dummy_action_hello payload) {
  *   ...
  *  }
  *  @endcode
  */
```

### VACCOUNTS_APPLY | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/contracts/eos/vaccountsconsumer/vaccountsconsumer.cpp#L49)

```cpp
/**
  *  LiquidAccounts use the VACCOUNTS_APPLY macro to map which payload structs are associated with which actions.  It also defines the LiquidAccount action.
  *
  *  @param ((payload_struct)(action_name))
  * 
  *  Example:
  *
  *  @code
  *  VACCOUNTS_APPLY(((dummy_action_hello)(hello))((dummy_action_hello)(hello2)))
  *  @endcode
  */
```

### require_vaccount | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/contracts/eos/dappservices/_vaccounts_impl.hpp#L115)

```cpp
/**
  *  LiquidAccounts use the require_vaccount macro in place of the require_auth macro for authenticating a LiquidAccount against the key assigned when calling regaccount
  *
  *  @param {name} - vaccount from payload
  * 
  *  Example:
  *
  *  @code
  *  require_vaccount(payload.vaccount);
  *  @endcode
  */

void required_key(const eosio::public_key& pubkey){ \
    eosio::check(_pubkey == pubkey, "wrong public key"); \
} \

void require_vaccount(name vaccount){ \
    auto pkey = handleNonce(vaccount); \
    required_key(pkey); \
} \
```

### VACCOUNTS_DELAYED_CLEANUP | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/contracts/eos/vaccountsconsumer/vaccountsconsumer.cpp#L1)

```cpp
/**
  *  LiquidAccounts use the VACCOUNTS_DELAYED_CLEANUP time in seconds to prevent data from being committed to IPFS from RAM.  
  *
  *  @param {uint32_t} VACCOUNTS_DELAYED_CLEANUP - time delay in seconds before data is removed from RAM and committed to vRAM (IPFS)
  *
  *  Notes
  *  - VACCOUNTS_DELAYED_CLEANUP is intended to allow DAPPs to operate in a session based way.  Data persists to RAM for the time specified to avoid the warmup process associated with vRAM data.  After the user has become inactive, the data is committed.
  * 
  *  Example:
  *
  *  @code
  *  #define VACCOUNTS_DELAYED_CLEANUP 120
  *  @endcode
  */
```

## LiquidHarmony

### geturi | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/oracle-dapp-service/contracts/eos/dappservices/_oracle_impl.hpp#L63)

```cpp
/**
  *  LiquidHarmony uses the geturi action to perform an oracle request
  *
  *  @param {std::vector<char>} uri - hex conversion of URL syntax to perform an oracle request
  *  @param {Lambda&&} combinator - lambda to return the results of an oracle request
  * 
  *  Example:
  *
  *  @code
  *  [[eosio::action]] void testrnd(std::vector<char> uri) {
  *    getURI(uri, [&]( auto& results ) { 
  *      return results[0].result;
  *    });
  *  }
  *  @endcode
  */

TABLE oracleentry {  \
   uint64_t                      id; \
   std::vector<char>             uri; \
   std::vector<provider_result>     results; \
   checksum256 hash_key() const { return hashData(uri); }  \
   uint64_t primary_key()const { return id; }  \
};  \
typedef eosio::multi_index<"oracleentry"_n, oracleentry, indexed_by<"byhash"_n, const_mem_fun<oracleentry, checksum256, &oracleentry::hash_key>>> oracleentries_t; \

static std::vector<char> getURI(std::vector<char> uri, Lambda&& combinator){  \
    checksum256 trxId = transaction_id(); \
    auto trxIdp = trxId.data(); \
    std::string trxIdStr(trxIdp, trxIdp + trxId.size()); \
    auto pubTime = tapos_block_prefix(); \
    std::string uristr(uri.begin(), uri.end()); \
    auto s = fc::base64_encode(trxIdStr) + "://" + fc::to_string(pubTime) + "://" + uristr;\
    std::vector<char> idUri(s.begin(), s.end()); \
    return _getURI(idUri, combinator); \
}\

static std::vector<char> _getURI(std::vector<char> uri, Lambda&& combinator){  \
    auto _self = name(current_receiver()); \
    oracleentries_t entries(_self, _self.value);  \
    auto cidx = entries.get_index<"byhash"_n>(); \
    auto existing = cidx.find(hashData(uri)); \
    if(existing == cidx.end()){  \
        SEND_SVC_REQUEST(geturi, uri); \
    } \
    else {\
        auto results = _extractResults(*existing, combinator);\
        cidx.erase(existing);\
        return results; \
    } \
    return std::vector<char>();\
}  \
```

## LiquidScheduler

### schedule_timer | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/cron-dapp-service/contracts/eos/dappservices/_cron_impl.hpp#L21)

```cpp
/**
  *  LiquidScheduler uses the schedule_timer macro to schedule a cron action on chain by adding a timer to the timerentry singleton
  *
  *  @param {name} timer - account name to scope the timer within
  *  @param {std::vector<char>} payload - payload to be accessed within the timer_callback function in the consumer contract
  *  @param {uint32_t} seconds - seconds to repeat the cron
  * 
  *  Example:
  *
  *  @code
  *  [[eosio::action]] void testschedule() {
  *    std::vector<char> payload;
  *    schedule_timer(_self, payload, 2);
  *  }
  *  @endcode
  */

TABLE timerentry { \
    int64_t   set_timestamp = 0; \
    int64_t   fired_timestamp = 0; \
};\
typedef eosio::singleton<"timer"_n, timerentry> timers_def;\

static void schedule_timer(name timer,std::vector<char> payload, uint32_t seconds){  \
    timers_def timers(current_receiver(), timer.value); \
    timerentry newtimer; \
    if(timers.exists()){ \
        newtimer = timers.get(); \
    } \
    newtimer.fired_timestamp = 0;\
    newtimer.set_timestamp = eosio::current_time_point().time_since_epoch().count();\
    timers.set(newtimer, current_receiver()); \
    SEND_SVC_REQUEST(schedule, timer, payload, seconds); \
}  \

SVC_RESP_CRON(schedule)(name timer,std::vector<char> payload, uint32_t seconds,name current_provider){ \
    timers_def timers(current_receiver() , timer.value); \
    if(!timers.exists()) \
        return; \
    auto current_timer = timers.get(); \
    if(current_timer.fired_timestamp != 0 || (current_timer.set_timestamp + (seconds * 1000000) > eosio::current_time_point().time_since_epoch().count()))\
        return; \
    current_timer.fired_timestamp = eosio::current_time_point().time_since_epoch().count(); \
    timers.set(current_timer, current_receiver()); \
    if(!timer_callback(timer, payload, seconds)) \
        return; \
    schedule_timer(timer, payload, seconds);\
} \
```

### remove_timer | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/cron-dapp-service/contracts/eos/dappservices/_cron_impl.hpp#L32)

```cpp
/**
  *  LiquidScheduler uses the remove_timer macro to remove a timer from the timerentry singleton
  *
  *  @param {name} timer - account name to scope the timer within
  *  @param {std::vector<char>} payload - payload to be accessed within the timer_callback function in the consumer contract
  *  @param {uint32_t} seconds - seconds to repeat the cron
  * 
  *  Example:
  *
  *  @code
  *  [[eosio::action]] void testsremove() {
  *    std::vector<char> payload;
  *    remove_timer(_self, payload, 2);
  *  }
  *  @endcode
  */

static void remove_timer(name timer,std::vector<char> payload, uint32_t seconds){  \
    timers_def timers(current_receiver(), timer.value); \
    if(timers.exists()){ \
        timers.remove(); \
    } \
}  \
```

### timer_callback | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/cron-dapp-service/contracts/eos/cronconsumer/cronconsumer.cpp#L18)

```cpp
/**
  *  LiquidScheduler uses the timer_callback to run the cron logic within the consumer contract
  *
  *  @param {name} timer - account name to scope the timer within
  *  @param {std::vector<char>} payload - payload to be accessed within the timer_callback function in the consumer contract
  *  @param {uint32_t} seconds - seconds to repeat the cron
  *
  *  Notes
  *  - can return true to create an infinite loop, false breaks the loop
  * 
  *  Example:
  *
  *  @code
  *  bool timer_callback(name timer, std::vector<char> payload, uint32_t seconds){
  *    stats_def statstable(_self, _self.value);
  *    stat newstats;
  *    if(!statstable.exists()){
  *      statstable.set(newstats, _self);
  *    }
  *    else {
  *      newstats = statstable.get();
  *    }
  *    newstats.counter++;
  *    statstable.set(newstats, _self);
  *    return (newstats.counter < 10);
  *  }
  *  @endcode
  */
```