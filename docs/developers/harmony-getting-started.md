LiquidHarmony Getting Started
====================

```
 _      _             _     _ _    _                                        
| |    (_)           (_)   | | |  | |                                       
| |     _  __ _ _   _ _  __| | |__| | __ _ _ __ _ __ ___   ___  _ __  _   _ 
| |    | |/ _` | | | | |/ _` |  __  |/ _` | '__| '_ ` _ \ / _ \| '_ \| | | |
| |____| | (_| | |_| | | (_| | |  | | (_| | |  | | | | | | (_) | | | | |_| |
|______|_|\__, |\__,_|_|\__,_|_|  |_|\__,_|_|  |_| |_| |_|\___/|_| |_|\__, |
             | |                                                       __/ |
             |_|                                                      |___/ 

```

LiquidHarmony includes all oracle related logic from the DAPP Network.  This includes things like http fetches, IBC and XIBC fetches, vCPU and more.  The full list of options can be explored in the oracles directory of the repo: [https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/oracles](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/oracles)

The DAPP Network offers the ability to fetch information using DAPP Service Providers (DSPs). A developer may choose as many or as few oracles to use in fetching data points from the internet.  If a developer wishes to prevent a scenario where all oracles have an incentive to return false information, the developer may run a DSP themselves and set the threshold of acceptance for the information to all parties.  Another great place to understand the service is in the [unit tests](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/oracles) within each sub directory.

The price feed example uses LiquidHarmony web oracles and the LiquidScheduler to periodically update a price on chain only when the new price is more or less than 1% of the last updated price, conserving CPU by only running actions when necessary. See [here](price-feed)

## Prerequisites

* [Zeus](zeus-getting-started.md) - Zeus installs eos and the eosio.cdt if not already installed
* [Kylin Account](kylin-account.md)

The DAPP Network currently supports the following oracle requests:

- HTTP(S) Get
- HTTP(S) Post
- HTTP(S)+JSON Get
- HTTP(S)+JSON Post
- Nodeos History Get
- IBC Block Fetch - (Mainnet, BOS, Telos, Kylin, Worbli, Jungle, Meetone)
- Oracle XIBC - read for foreign chain (Ethereum, Tron, Cardano, Ripple, Bitcoin, Litecoin, Bitcoin Cash)
- Wolfram Alpha
- Random Number
- Stockfish - chess engine AI
- SQL

## Unbox Oracle DAPP Service box
This box contains the oracle smart contract libraries, DSP node logic, unit tests, and everything else needed to get started integrating / testing the DAPP Network oracles in your smart contract.
```bash
# npm install -g @liquidapps/zeus-cmd
zeus unbox oracle-dapp-service
cd oracle-dapp-service
zeus test -c
```

## Creating an Oracle Request URI
Each of the following oracle request types comes equipped with its own syntax that gets encoded with `Buffer.from("<URI HERE>", 'utf8')`.  The following guide will explain the syntax to generate your URI.  Each URI should be passed through the buffer as plaintext is not accepted.

- HTTP(S) Get & Post: `https://ipfs.io/ipfs/Qmaisz6NMhDB51cCvNWa1GMS7LU1pAxdF4Ld6Ft9kZEP2a` - simply add the full URL path
- HTTP(S)+JSON Get: `https+json://name/api.github.com/users/tmuskal` - prepend your uri with `https+json`, then specify the key mapping path of your desired data point, in the example, the `name` key is used as the requested data point.  To request nested values beneath the first layer of keys, simple separate the desired data point with a `.`, e.g., `name.value`.  Then add the path to your desired data point: `api.github.com/users/tmuskal`.  Note you may use `http+json` or `https+json`.
- HTTP(S)+JSON Post: `https+post+json://timestamp/${body}/nodes.get-scatter.com:443/v1/chain/get_block` - where body is `const body = Buffer.from('{"block_num_or_id":"36568000"}').toString('base64')`.  In this example you specify the type of request: `https+post+json` then the key mapping `timestamp` then the body of the POST request, encoded in base64, then the URL path `nodes.get-scatter.com:443/v1/chain/get_block`.
- Nodeos History Get: `self_history://${code}/0/0/0/action_trace.act.data.account` - where code is `const code = 'test1';`
- IBC Block Fetch: `sister_chain_block://bos/10000000/transaction_mroot` - the `sister_chain_block` specifies the type of oracle request, followed by the chain of choice `bos` then the requested data point.
- Oracle XIBC: `foreign_chain://ethereum/history/0x100/result.transactionsRoot` - here the `foreign_chain` oracle type is used followed by the foreign chain of choice: `ethereum`, the type of data point (`block_number`, `history`, `balance`, `storage`).  To see other blockchain data point options, see [this file](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/oracles/oracle-foreign-chain/services/oracle-dapp-service-node/protocols/foreign_chain.js). Then the required data parameter is passed `0x100` followed by the object key mapping `result.transactionsRoot`.
  - You may also see more examples in the [unit test](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/oracles/oracle-foreign-chain/test/oracle-foreign-chain.spec.js)
- Wolfram Alpha: `wolfram_alpha://What is the average air speed velocity of a laden swallow?` - here the `wolfram_alpha` oracle type is used followed by the question: `What is the average air speed velocity of a laden swallow?`.

## LiquidHarmony Consumer Example Contract used in unit tests
in `contracts/eos/oracleconsumer/oracleconsumer.cpp`
The consumer contract is a great starting point for playing around with the LiquidHarmony syntax.
```cpp
/* INCLUDE ORACLE LOGIC */
#include "../dappservices/oracle.hpp"

/* ADD DAPP NETWORK RELATED ORACLE ACTIONS */
#define DAPPSERVICES_ACTIONS() \
  XSIGNAL_DAPPSERVICE_ACTION \
  ORACLE_DAPPSERVICE_ACTIONS

#define DAPPSERVICE_ACTIONS_COMMANDS() \
  ORACLE_SVC_COMMANDS() 

#define CONTRACT_NAME() oracleconsumer 

CONTRACT_START()

  /* 
  
    testget - provide a URI using the DAPP Network Oracle syntax and an expected result, 
    if the result does not match the expected field, the transaction fails 
    
    testrnd - fetch oracle request based on URI without expected field assertion
  
  */

 [[eosio::action]] void testget(std::vector<char>  uri, std::vector<char> expectedfield) {
    /* USE EOSIO'S ASSERTION TO CHECK FOR REQUIRED THREHSHOLD OF ORACLES IS MET */
    eosio::check(getURI(uri, [&]( auto& results ) { 
      eosio::check(results.size() > 0, "require multiple results for consensus");
      auto itr = results.begin();
      auto first = itr->result;
      ++itr;
      /* SET CONSENSUS LOGIC FOR RESULTS */
      while(itr != results.end()) {
        eosio::check(itr->result == first, "consensus failed");
        ++itr;
      }
      return first;
    }) == expectedfield, "wrong data");
  }
  
  [[eosio::action]] void testrnd(std::vector<char> uri) {
    getURI(uri, [&]( auto& results ) { 
      return results[0].result;
    });
  }
CONTRACT_END((testget)(testrnd))
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
cleos -u $DSP_ENDPOINT set contract $KYLIN_TEST_ACCOUNT oracleconsumer -p $KYLIN_TEST_ACCOUNT@active

# Set contract permissions
cleos -u $DSP_ENDPOINT set account permission $KYLIN_TEST_ACCOUNT active "{\"threshold\":1,\"keys\":[{\"weight\":1,\"key\":\"$KYLIN_TEST_PUBLIC_KEY\"}],\"accounts\":[{\"permission\":{\"actor\":\"$KYLIN_TEST_ACCOUNT\",\"permission\":\"eosio.code\"},\"weight\":1}]}" owner -p $KYLIN_TEST_ACCOUNT@active
```

## Select and stake DAPP for DSP package | [DSP Portal Link](https://dsphq.io/packages/heliosselene/oracleservic/oracleservic?network=kylin)
 * Use [the faucet](https://kylin-dapp-faucet.liquidapps.io/) to get some DAPP tokens on Kylin
 * Information on: [DSP Packages and staking DAPP/DAPPHDL (AirHODL token)](dsp-packages-and-staking.md)
```bash
export PROVIDER=heliosselene
export PACKAGE_ID=oracleservic

# select your package: 
export SERVICE=oracleservic
cleos -u $DSP_ENDPOINT push action dappservices selectpkg "[\"$KYLIN_TEST_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"$PACKAGE_ID\"]" -p $KYLIN_TEST_ACCOUNT@active

# Stake your DAPP to the DSP that you selected the service package for:
cleos -u $DSP_ENDPOINT push action dappservices stake "[\"$KYLIN_TEST_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"10.0000 DAPP\"]" -p $KYLIN_TEST_ACCOUNT@active
```

## Test
Finally you can now test your LiquidHarmony implementation by sending an action through your DSP's API endpoint

```bash
# oracleconsumer contract (testrnd / testget):
# uri: Buffer.from("https://ipfs.io/ipfs/Qmaisz6NMhDB51cCvNWa1GMS7LU1pAxdF4Ld6Ft9kZEP2a", 'utf8')
export URI=68747470733a2f2f697066732e696f2f697066732f516d6169737a364e4d68444235316343764e576131474d53374c55317041786446344c64364674396b5a45503261
export EXPECTED_FIELD=48656c6c6f2066726f6d2049504653204761746577617920436865636b65720a
cleos -u $DSP_ENDPOINT push action $KYLIN_TEST_ACCOUNT testrnd "[\"$URI\"]" -p $KYLIN_TEST_ACCOUNT
cleos -u $DSP_ENDPOINT push action $KYLIN_TEST_ACCOUNT testget "[\"$URI\",\"$EXPECTED_FIELD\"]" -p $KYLIN_TEST_ACCOUNT
```

### Custom eosio assertion message

If `shouldAbort` is included in an `eosio::check` assertion, the DSP will cease to process the request instead of retrying.  Retrying is done automatically in the event a transaction fails for any reason.

### Pre geturi hook

Traditionally, an oracle request will be run on each DSP a consumer is staked to with each DSP returning the result of that request before the final array of oracle responses is returned to the smart contract.  In order to access the response each DSP is returning before it is returned, a hook has been added which can be accessed with the following syntax.  This hook can be used to throw an assertion preventing the DSP from using CPU to process a transaction under certain circumstances:

```cpp
// define custom filter
#undef ORACLE_HOOK_FILTER
#define ORACLE_HOOK_FILTER(uri, data) filter_result(uri, data);

void filter_result(std::vector<char> uri, std::vector<char> data){
  // if assertion thrown here, DSP will not respond nor use CPU to process the geturi action
  // shouldAbort is included here to prevent the DSP from retrying the oracle request
  eosio::check(data.size() > 3, "shouldAbort, result too small");
}
```