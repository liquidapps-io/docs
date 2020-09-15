Dapp Client Library
=====================

The [dapp-client library](https://www.npmjs.com/package/@liquidapps/dapp-client) makes using DAPP Network services much easier.  It also allows you to easily tap into the [`dappservices`](https://bloks.io/account/dappservices) (core DAPP Network Smart contract) and [`dappairhodl1`](https://bloks.io/account/dappairhodl1) (Air-HODL smart contract) RAM tables with precision utilizing secondary and tertiary indices without the hassle.

To setup the library, first install it:

```bash
npm i -g @liquidapps/dapp-client
```

From there include the library's client creator script:

```javascript
const { createClient } = require('@liquidapps/dapp-client');
```

Then pass your desired arguments to the creator function

```javascript
/*
    * network: specify your network of choice: mainnet, kylin, jungle 
    * httpEndpoint: you may also specify an endpoint to use instead of our defaults
    * fetch: pass a fetch reference as needed
*/

export const getClient = async() => {
  return await createClient({ network: "kylin", httpEndpoint: endpoint, fetch: window.fetch.bind(window) });
};

```

Finally, setup the service you would like to interact along with your smart contract name:

```javascript
(async () => {
    const service = await (await createClient()).service('ipfs','cardgame1112');
    const response = await service.get_vram_row( "cardgame1112", "cardgame1112", "users", "nattests" );
    console.log(response);
    // { username: 'nattests',
    //     win_count: 0,
    //     lost_count: 0,
    //     game_data:
    // { life_player: 5,
    //     life_ai: 5,
    //     deck_player:
    //     [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17 ],
    //     deck_ai:
    //     [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17 ],
    //     hand_player: [ 0, 0, 0, 0 ],
    //     hand_ai: [ 0, 0, 0, 0 ],
    //     selected_card_player: 0,
    //     selected_card_ai: 0,
    //     life_lost_player: 0,
    //     life_lost_ai: 0,
    //     status: 0 } }
})().catch((e) => { console.log(e); });
```

Here is a full list of service options.  There are DAPP Network service extensions and [`dappservices`](https://bloks.io/account/dappservices) / [`dappairhodl1`](https://bloks.io/account/dappairhodl1) RAM row calls.

### DAPP Network service extensions

#### [vRAM - IPFS](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service)

##### [get_vram_row](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/client/examples/get_vram_row.ts) - get vRAM row from DSP's endpoint

```javascript
/*
    getClient()
    * service name: ipfs
    * contract name

    service.get_vram_row - read vRAM table row from DSP's endpoint
    * code - smart contract account
    * scope - scope of table
    * table - name of table
    * primary key

*/

const service = await (await getClient()).service('ipfs','cardgame1112');
const response = await service.get_vram_row( "cardgame1112", "cardgame1112", "users", "nattests" );
```

#### [LiquidAccounts](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vaccounts-dapp-service)

##### [push_liquid_account_transaction](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/client/examples/push_register_liquid_account.ts) - register new account

```javascript
/*
    getClient()
    * service name: vaccounts
    * contract name

    service.push_liquid_account_transaction
    * account name of contract with LiquidAcount code deployed
    * private key of LiquidAccount
    * action name: regaccount to register new account
    * payload
        * vaccount - name of LiquidAccount

*/

const service = await (await getClient()).service('vaccounts', "vacctstst123");
const response = await service.push_liquid_account_transaction(
    "vacctstst123",
    "5JMUyaQ4qw6Zt816B1kWJjgRA5cdEE6PhCb2BW45rU8GBEDa1RC",
    "regaccount",
    {
        vaccount: 'testing126'
    }
);
```

##### [push_liquid_account_transaction](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/client/examples/push_liquid_account_transaction.ts) - push transaction after creating account

```javascript
/*
    getClient()
    * service name: vaccounts
    * contract name

    service.push_liquid_account_transaction
    * account name of contract with LiquidAcount code deployed
    * private key of LiquidAccount
    * action name: hello
    * payload
        * any - payload must match struct outline in the smart contract

*/

const service = await (await getClient()).service('vaccounts', "vacctstst123");
const response = await service.push_liquid_account_transaction(
    "vacctstst123",
    "5JMUyaQ4qw6Zt816B1kWJjgRA5cdEE6PhCb2BW45rU8GBEDa1RC",
    "hello",
    {
        vaccount: 'testing126'
    }
);
```

### [dappservices](https://bloks.io/account/dappservices)

##### [get_package_info](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/client-lib-base/client/examples/get_package_info.ts) - returns package info

```javascript
/*
    
    dappNetwork.get_package_info
    * account name
    * service name - service names are listed under the services section of the docs as the Contract name

*/

const response = await (await getClient()).dappNetwork.get_package_info( "cardgame1112", "accountless1" );
console.log(response);
// {
//     api: 'https://kylin-dsp-2.liquidapps.io',
//     package_json_uri: 'https://kylin-dsp-2.liquidapps.io/liquidaccts2.dsp-package.json',
//     package_id: 'liquidaccts2',
//     service: 'accountless1',
//     provider: 'heliosselene',
//     quota: '10.0000 QUOTA',
//     package_period: 60,
//     min_stake_quantity: '10.0000 DAPP',
//     min_unstake_period: 3600,
//     enabled: 0
// }
```

##### [get_table_accountext](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/client-lib-base/client/examples/get_table_accountext.ts) - returns entire accountext table

```javascript
/*
    
    dappNetwork.get_table_accountext

*/

const response = await (await getClient()).dappNetwork.get_table_accountext();
for (const row of response.rows) {
    console.log(row);
    // {
    //     id: 144,
    //     account: 'mailcontract',
    //     service: 'ipfsservice1',
    //     provider: 'heliosselene',
    //     quota: '9.9907 QUOTA',
    //     balance: '10.0000 DAPP',
    //     last_usage: '1564112241500',
    //     last_reward: '1564112241500',
    //     package: 'ipfs1',
    //     pending_package: 'ipfs1',
    //     package_started: '1564112241500',
    //     package_end: '1564112301500'
    // }
}
```

##### [get_table_accountext_by_account_service](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/client-lib-base/client/examples/get_table_accountext_by_account_service.ts) - returns entire accountext by account and service specified

```javascript
/*
    
    dappNetwork.get_table_accountext_by_account_service
    * account name
    * service name - service names are listed under the services section of the docs as the Contract name

*/

const response = await (await getClient()).dappNetwork.get_table_accountext_by_account_service('cardgame1112', 'ipfsservice1');
for (const row of response.rows) {
    console.log(row);
    // {
    //     id: 144,
    //     account: 'mailcontract',
    //     service: 'ipfsservice1',
    //     provider: 'heliosselene',
    //     quota: '9.9907 QUOTA',
    //     balance: '10.0000 DAPP',
    //     last_usage: '1564112241500',
    //     last_reward: '1564112241500',
    //     package: 'ipfs1',
    //     pending_package: 'ipfs1',
    //     package_started: '1564112241500',
    //     package_end: '1564112301500'
    // }
}
```

##### [get_table_accountext_by_account_service_provider](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/client-lib-base/client/examples/get_table_accountext_by_account_service_provider.ts) - returns entire accountext by account, service, and provider specified

```javascript
/*
    
    dappNetwork.get_table_accountext_by_account_service_provider
    * account name
    * service name - service names are listed under the services section of the docs as the Contract name
    * provider name - DSP name

*/

const response = await (await getClient()).dappNetwork.get_table_accountext_by_account_service_provider('cardgame1112', 'ipfsservice1','heliosselene');
for (const row of response.rows) {
    console.log(row);
    // {
    //     id: 144,
    //     account: 'mailcontract',
    //     service: 'ipfsservice1',
    //     provider: 'heliosselene',
    //     quota: '9.9907 QUOTA',
    //     balance: '10.0000 DAPP',
    //     last_usage: '1564112241500',
    //     last_reward: '1564112241500',
    //     package: 'ipfs1',
    //     pending_package: 'ipfs1',
    //     package_started: '1564112241500',
    //     package_end: '1564112301500'
    // }
}
```

##### [get_table_package](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/client-lib-base/client/examples/get_table_package.ts) - returns entire package table

```javascript
/*
    
    dappNetwork.get_table_package
    * [limit] - optional limit for how many packages to return

*/

const response = await (await getClient()).dappNetwork.get_table_package({ limit: 500 });
for (const row of response.rows) {
    console.log(row);
    // {
    //     id: 9,
    //     api_endpoint: 'https://kylin-dsp-2.liquidapps.io',
    //     package_json_uri: 'https://kylin-dsp-2.liquidapps.io/package1.dsp-package.json',
    //     package_id: 'package1',
    //     service: 'ipfsservice1',
    //     provider: 'heliosselene',
    //     quota: '1.0000 QUOTA',
    //     package_period: 86400,
    //     min_stake_quantity: '10.0000 DAPP',
    //     min_unstake_period: 3600,
    //     enabled: 1
    // }
}
```

##### [get_table_package_by_package_service_provider](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/client-lib-base/client/examples/get_table_package_by_package_service_provider.ts) - returns packages by package, service, and DSP name

```javascript
/*
    
    dappNetwork.get_table_package_by_package_service_provider
    * package name
    * service name - service names are listed under the services section of the docs as the Contract name
    * DSP name
    * [limit] - optional limit for how many packages to return

*/

const response = await (await getClient()).dappNetwork.get_table_package_by_package_service_provider('package1', 'ipfsservice1', 'heliosselene', { limit: 500 });
for (const row of response.rows) {
    console.log(row);
    // {
    //     id: 9,
    //     api_endpoint: 'https://kylin-dsp-2.liquidapps.io',
    //     package_json_uri: 'https://kylin-dsp-2.liquidapps.io/package1.dsp-package.json',
    //     package_id: 'package1',
    //     service: 'ipfsservice1',
    //     provider: 'heliosselene',
    //     quota: '1.0000 QUOTA',
    //     package_period: 86400,
    //     min_stake_quantity: '10.0000 DAPP',
    //     min_unstake_period: 3600,
    //     enabled: 1
    // }
}
```

##### [get_table_refunds](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/client-lib-base/client/examples/get_table_refunds.ts) - returns refund table details for account name specified

```javascript
/*
    
    dappNetwork.get_table_refunds
    * account name

*/

const response = await (await getClient()).dappNetwork.get_table_refunds('heliosselene');
for (const row of response.rows) {
    console.log(row);
    // {
    //     id: 0,
    //     account: 'heliosselene',
    //     amount: '10.0000 DAPP',
    //     unstake_time: 12345678
    //     provider: 'heliosselene',
    //     service: 'ipfsservice1'
    // }
}
```

##### [get_table_staking](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/client-lib-base/client/examples/get_table_staking.ts) - returns staking table details for account name specified

```javascript
/*
    
    dappNetwork.get_table_staking
    * account name

*/

const response = await (await getClient()).dappNetwork.get_table_staking('cardgame1112');
for (const row of response.rows) {
    console.log(row);
    // {
    //     id: 0,
    //     account: 'cardgame1112',
    //     balance: '10.0000 DAPP',
    //     provider: 'uuddlrlrbass',
    //     service: 'accountless1'
    // }
}
```

### [dappairhodl1](https://bloks.io/account/dappairhodl1)

##### [get_dapphdl_accounts](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/client-lib-base/client/examples/get_dapphdl_accounts.ts) - get an account's DAPPHDL stats

```javascript
/*
    
    airhodl.get_dapphdl_accounts
    * account name

*/

const response = await (await getClient()).airhodl.get_dapphdl_accounts('natdeveloper');
for (const row of response.rows) {
    console.log(row);
    // {
    //     balance: '0.0033 DAPPHDL',
    //     allocation: '0.0199 DAPPHDL',
    //     staked: '0.0000 DAPPHDL',
    //     claimed: 1
    // }
}
```