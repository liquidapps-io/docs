Consumer Permissions
============

The consumer of DSP services may optionally create permissions that allow the consumer to pay for all CPU, NET, and RAM costs associated with the DSP services. This permission is optional. Without it, DSP services will continue to operate normally.

**Process**

* The consuming contract creates a `dsp` permission under the `active` permission
* The `dsp` permissions requires that each provider used by the consumer must be added, for example:
    provider1@active, provider2@active
* Each `xaction` required for the services used must be Link Authed to the `dsp` permission

## Example of adding DSP_ACCOUNT_NAME_HERE@active to a DSP permission level

### with Bloks.io

Login with your account's name using the cleos login option: [https://kylin.bloks.io/wallet/permissions/advanced](https://kylin.bloks.io/wallet/permissions/advanced), and the cleos command will be auto generated for you.

- click "Add New Permission
- click on permission to get it to open up
- permission name: dsp
- parent: active
- threshold: 1
- add as many DSP to the accounts section with the active permission (heliosslene - active, uuddlrlrbass - active, etc)
- click save permission to have the cleos command auto generated

### or Cleos

```bash
# CONTRACT_ACCOUNT_HERE - the account name that the consumer contract is deployed to
# DSP_ACCOUNT_NAME_HERE - the account name of the DSP staked to, if staked to multiple DSPs, must add all DSP permissions
cleos -u https://kylin.eos.dfuse.io push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"eosio","name":"updateauth","data":{"account":"CONTRACT_ACCOUNT_HERE","permission":"dsp","parent":"active","auth":{"threshold":1,"keys":[],"accounts":[{"permission":{"actor":"DSP_ACCOUNT_NAME_HERE","permission":"active"},"weight":1}],"waits":[]}},"authorization":[{"actor":"CONTRACT_ACCOUNT_HERE","permission":"active"}]}]}'
```

## Example of linkauthing to each DSP xaction

### with Bloks.io

Go here: [https://kylin.bloks.io/wallet/permissions/link](https://kylin.bloks.io/wallet/permissions/link)

- login using your contract's name for the cleos option
- Permission: dsp
- Contract name: CONTRACT_ACCOUNT_HERE, not the DSP name
- Contract action: xsignal
- Link Auth -> presto you have your cleos command
- You must repeat this process for all of the contract xactions you are using, you may find them by checking your ABI or using a block explorer to view your actions

### or Cleos

```bash
# CONTRACT_ACCOUNT_HERE - the account name that the consumer contract is deployed to
# ACTION_NAME_HERE - action to linkauth dsp permission levle to
cleos -u https://kylin.eos.dfuse.io push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"eosio","name":"linkauth","data":{"account":"CONTRACT_ACCOUNT_HERE","code":"CONTRACT_ACCOUNT_HERE","type":"ACTION_NAME_HERE","requirement":"dsp"},"authorization":[{"actor":"CONTRACT_ACCOUNT_HERE","permission":"active"}]}]}'
```