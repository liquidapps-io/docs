Packages
========

## Register

These json files are primarily used by DSP portals to display information.

### Prepare and host dsp.json 
```JSON
{
    "name": "acme DSP",
    "website": "https://acme-dsp.com",
    "code_of_conduct":"https://...",
    "ownership_disclosure" : "https://...",
    "email":"dsp@acme-dsp.com",
    "branding":{
      "logo_256":"https://....",
      "logo_1024":"https://....",
      "logo_svg":"https://...."
    },
    "location": {
      "name": "Atlantis",
      "country": "ATL",
      "latitude": 2.082652,
      "longitude": 1.781132
    },
    "social":{
      "steemit": "",
      "twitter": "",
      "youtube": "",
      "facebook": "",
      "github":"",
      "reddit": "",
      "keybase": "",
      "telegram": "",
      "wechat":""      
    }
    
}

```
### Prepare and host dsp-package.json 
```JSON
{
    "name": "Package 1",
    "description": "Best for low vgrabs",
    "dsp_json_uri": "https://acme-dsp.com/dsp.json",
    "logo":{
      "logo_256":"https://....",
      "logo_1024":"https://....",
      "logo_svg":"https://...."
    },
    "service_level_agreement": {
        "availability":{
            "uptime_9s": 5
        },
        "performance":{
            "95": 500
        }
    },
    "pinning":{
        "ttl": 2400,
        "public": false
    },
    "locations":[
        {
          "name": "Atlantis",
          "country": "ATL",
          "latitude": 2.082652,
          "longitude": 1.781132
        }
    ]
}
```

### Register Package

Packages are needed for consumers to stake to in order for DAPP Service Providers to provide services. Packages are disabled by default.

Once the register command has been run with cleos or zeus, **the package must be enabled!** See [Enable/Disable Package](#enable/disable-package)

* [Mainnet DSP packages](https://bloks.io/account/dappservices?loadContract=true&tab=Tables&account=dappservices&scope=dappservices&limit=100&table=package)
* [Kylin DSP packages](https://kylin.bloks.io/account/dappservices?loadContract=true&tab=Tables&account=dappservices&scope=dappservices&limit=100&table=package)

```bash
npm install -g @liquidapps/zeus-cmd
# the package must be chosen from the following list:
# packages: (ipfs, cron, log, oracle, readfn, vaccounts, storage, auth)
export PACKAGE=ipfs
export DSP_ACCOUNT=
# active key to sign package creation trx
export DSP_PRIVATE_KEY=
# customizable and unique name for your package
export PACKAGE_ID=package1
export EOS_CHAIN=mainnet
# or
export EOS_CHAIN=kylin
# the minimum stake quantity is the amount of DAPP and/or DAPPHDL that must be staked to meet the package's threshold for use
export MIN_STAKE_QUANTITY="10.0000"
# package period is in seconds, so 86400 = 1 day, 3600 = 1 hour
export PACKAGE_PERIOD=86400
# the time to unstake is the greater of the package period remaining and the minimum unstake period, which is also in seconds
export MIN_UNSTAKE_PERIOD=3600
# QUOTA is the measurement for total actions allowed within the package period to be processed by the DSP.  1.0000 QUOTA = 10,000 actions. 0.0001 QUOTA = 1 action
export QUOTA="1.0000"
export DSP_ENDPOINT=https://acme-dsp.com
# package json uri is the link to your package's information, this is customizable without a required syntax
export PACKAGE_JSON_URI=https://acme-dsp.com/package1.dsp-package.json
# The annual inflation rate of the DAPP token may be tuned by the community. This is done by DSP's specifying a desired inflation rate during package registration. All existing packages default to the original annual inflation rate of 2.71%
export ANNUAL_INFLATION=2.71

cd $(readlink -f `which setup-dsp` | xargs dirname)/../..
zeus register dapp-service-provider-package \
    $PACKAGE $DSP_ACCOUNT $PACKAGE_ID \
    --key $DSP_PRIVATE_KEY \
    --min-stake-quantity $MIN_STAKE_QUANTITY \
    --package-period $PACKAGE_PERIOD \
    --quota $QUOTA \
    --network $EOS_CHAIN \
    --api-endpoint $DSP_ENDPOINT \
    --package-json-uri $PACKAGE_JSON_URI \
    --min-unstake-period $MIN_UNSTAKE_PERIOD \
    --inflation $ANNUAL_INFLATION
```

### Or in cleos:

```bash
# currently available services: (ipfsservice1, cronservices, logservices1, oracleservic, readfndspsvc, accountless1)
# the services use EOS account names to fascilitate usage
# service contract names may be found in the boxes/groups/services/SERVICE_NAME/models/dapp-services/*.json file as the ( contract ) parameter
export SERVICE=ipfsservice1
# zeus command automatically adds QUOTA / DAPP, so we must add it here
export QUOTA="1.0000 QUOTA"
export MIN_STAKE_QUANTITY="10.0000 DAPP"
export EOS_ENDPOINT=https://kylin-dsp-2.liquidapps.io # or mainnet: https://api.eosnewyork.io
cleos -u $EOS_ENDPOINT push action dappservices regpkg "{\"newpackage\":{\"api_endpoint\":\"$DSP_ENDPOINT\",\"enabled\":0,\"id\":0,\"min_stake_quantity\":\"$MIN_STAKE_QUANTITY\",\"min_unstake_period\":\"$MIN_UNSTAKE_PERIOD\",\"package_id\":\"$PACKAGE_ID\",\"package_json_uri\":\"$PACKAGE_JSON_URI\",\"package_period\":\"$PACKAGE_PERIOD\",\"provider\":\"$DSP_ACCOUNT\",\"quota\":\"$QUOTA\",\"service\":\"$SERVICE\"},
\"annual_inflation\":$ANNUAL_INFLATION}" -p $DSP_ACCOUNT
```

Example service contract name for LiquidAccounts: https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/vaccounts-dapp-service/models/dapp-services/vaccounts.json#L7

List of all services, please note some of which may not be testable yet: https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services, see the [stage](https://docs.liquidapps.io/en/v1.4/services/history-service.html#stage) for each service to monitor its development maturity (WIP - work in progress, Alpha, Beta, Stable).

output should be:
```
⚡registering package:package1
✔️package:package1 registered successfully
```

For more options:
```bash
zeus register dapp-service-provider-package --help 
```

Don't forget to stake CPU/NET to your DSP account:
```bash
cleos -u $DSP_ENDPOINT system delegatebw $DSP_ACCOUNT $DSP_ACCOUNT "5.000 EOS" "95.000 EOS" -p $DSP_ACCOUNT@active
```

#### Modify Package metadata:
Currently only `package_json_uri` and `api_endpoint` are modifyable.  To signal to DSP Portals / Developers that your package is no longer in service, set your `api_endpoint` to `null`.

To modify package metadata: use the "modifypkg" action of the dappservices contract.

Using cleos:
```bash
cleos -u $DSP_ENDPOINT push action dappservices modifypkg "[\"$DSP_ACCOUNT\",\"$PACKAGE_ID\",\"ipfsservice1\",\"$DSP_ENDPOINT\",\"https://acme-dsp.com/modified-package1.dsp-package.json\"]" -p $DSP_ACCOUNT@active
```

#### Enable/Disable Package:
Packages are disabled by default. A package may be disabled or enabled by using the `disablepkg` or `enablepkg` on the [`dappservices`](https://bloks.io/account/dappservices) contract.  If a package is disabled, the consumer will no longer be able to utilize DSP services for that package.

### Update cost per action in QUOTA
The `pricepkg` action on the `dappservices` contract allows a DSP to set how much QUOTA to bill for per action.  For example, a DSP could decide to charge 0.0002 QUOTA per vRAM warmup.  The default for each action is 0.0001 QUOTA.  The billable actions for all services may be found in the `zeus-sdk/boxes/groups/services/SERVICE_NAME-dapp-service/models/dapp-services/SERVICE_NAME.json`, for example: [vRAM](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/services/ipfs-dapp-service/models/dapp-services/ipfs.json).

- name provider - DSP name
- name package_id - package name
- name service - service name, e.g., ipfsservice1
- name action - action name, e.g., warmup, commit, geturi, etc
- uint64_t cost - QUOTA cost per action, e.g., 1 = 0.0001 QUOTA, 5, = 0.0005 QUOTA, etc