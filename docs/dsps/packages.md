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
            "95": 500,
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

**Warning: packages are read only and can't be removed yet.**

```bash

zeus register dapp-service-provider-package \
    ipfs dspaccount package1 \
    --key yourdspprivatekey \
    --min-stake-quantity "10.0000" \
    --package-period 86400 \
    --quota "1.0000" \
    --network mainnet \
    --api-endpoint $MYAPI \
    --package-json-uri https://acme-dsp.com/package1.dsp-package.json
```

replace https://api.acme-dsp.com with the service endpoint from 

For more options:
```bash
zeus register dapp-service-provider-package --help 
```

#### Modify Package metadata:
Currently only package_json_uri & api_endpoint are modifyable.
To modify package metadata: use the "modifypkg" action of the dappservices contract. (https://bloks.io/account/dappservices)
