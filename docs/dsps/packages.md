Packages
========

## Register
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
### If not using Kubernetes
```bash
npm install -g @liquidapps/zeus-cmd
cd $(readlink -f `which setup-dsp` | xargs dirname)
```
### Register Package

**Warning: packages are read only and can't be removed yet.**

```bash
export PACKAGE_ID=package1
export EOS_CHAIN=mainnet
#or
export EOS_CHAIN=kylin

export DSP_ENDPOINT=https://acme-dsp.com
zeus register dapp-service-provider-package \
    ipfs $DSP_ACCOUNT $PACKAGE_ID \
    --key $DSP_PRIVATE_KEY \
    --min-stake-quantity "10.0000" \
    --package-period 86400 \
    --quota "1.0000" \
    --network $EOS_CHAIN \
    --api-endpoint $DSP_ENDPOINT \
    --package-json-uri https://acme-dsp.com/package1.dsp-package.json
```

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
cleos -u $EOS_ENDPOINT system delegatebw $DSP_ACCOUNT $DSP_ACCOUNT "5.000 EOS" "95.000 EOS" -p $DSP_ACCOUNT@active
```

#### Modify Package metadata:
Currently only `package_json_uri` and `api_endpoint` are modifyable.

To modify package metadata: use the "modifypkg" action of the dappservices contract.

Using cleos:
```bash
cleos -u $EOS_ENDPOINT push action dappservices modifypkg "[\"$DSP_ACCOUNT\",\"$PACKAGE_ID\",\"ipfsservice1\",\"$DSP_ENDPOINT\",\"https://acme-dsp.com/modified-package1.dsp-package.json\"]" -p $DSP_ACCOUNT@active
```