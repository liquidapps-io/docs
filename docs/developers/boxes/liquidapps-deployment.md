
liquidapps-deployment
====================







## Dependencies
### Boxes
* [`all-dapp-services`](all-dapp-services.md)
* [`coldtoken`](coldtoken.md)
* [`microauctions`](microauctions.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)




## Install
```bash
zeus unbox liquidapps-deployment
```










### Model Instances
#### [dapp-network/production.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidapps-deployment/models/liquidapps-deployment-settings/production.json)
```json
{
  "name": "production",
  "dapptoken": {
    "yearlyInflation": 0.0271
  },
  "auctions": {
    "cycles": {
      "startTimestamp": 1551196800000,
      "iterations": 444,
      "secondsPerCycle": 64800
    },
    "standard": {
      "ratio": 0.5,
      "account": "instanttrack",
      "safeAccount": "lqapmauctis1"
    },
    "whitelisted": {
      "whitelist": "dappverified",
      "ratio": 0.5,
      "account": "regulartrack",
      "safeAccount": "lqapmauctis2"
    },
    "accepted": {
      "contract": "eosio.token",
      "symbol": "EOS",
      "minimum": "0.1000"
    }
  },
  "distribution": {
    "supply": 1000000000,
    "auctions": 0.5,
    "wallets": {
      "lqapwalleta1": 0.1,
      "lqapwalletb1": 0.05,
      "lqapwalletb2": 0.05,
      "lqapwalletb3": 0.05,
      "lqapwalletb4": 0.05,
      "lqapwalletb5": 0.05,
      "lqapwalletba": 0.05,
      "lqapwalletbb": 0.05,
      "lqapwalletbc": 0.05
    }
  },
  "protect": {
    "newOwnerPublicKey": "EOS7vLUnvz3kCZVBjwmRcad7bMGBqYBjP4Dztpj6mQvpdCtniYkLd",
    "newActivePublicKey": "EOS5TuGNWU3g7QtneyNGopZZKuhwy5TsTGDMMXMAqDjc2LaUpJvUp"
  },
  "testUsers": []
}
```,#### [dapp-network/staging.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidapps-deployment/models/liquidapps-deployment-settings/staging.json)
```json
{
  "name": "staging",
  "dapptoken": {
    "yearlyInflation": 0.0271
  },
  "auctions": {
    "cycles": {
      "startTimestamp": 0,
      "iterations": 4444,
      "secondsPerCycle": 120
    },
    "standard": {
      "ratio": 0.5,
      "account": "lqasmauctiv1",
      "safeAccount": "cryptocoders"
    },
    "whitelisted": {
      "whitelist": "dappverified",
      "ratio": 0.5,
      "account": "lqasmauctiv2",
      "safeAccount": "cryptocoders"
    },
    "accepted": {
      "contract": "lqasmtsttokn",
      "symbol": "EOS",
      "minimum": "0.1000"
    }
  },
  "distribution": {
    "supply": 1000000000,
    "auctions": 0.5,
    "wallets": {
      "cryptocoders": 0.5
    }
  },
  "testUsers": [
    "tmuskal.xyz",
    "talmuskaleos"
  ]
}
```

## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidapps-deployment)