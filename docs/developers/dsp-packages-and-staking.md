Packages and Staking
====================

## List of available Packages
DSPs who have registered their service packages may be found in the [package table](https://bloks.io/account/dappservices?loadContract=true&tab=Tables&account=dappservices&scope=dappservices&limit=100&table=package) under the dappservices account on every supported chain.  

DSP Portals for viewing/interacting with packages:

* [Bloks.io](https://bloks.io/dsp)
* [EOS Nation](https://dsp.eosnation.io/)
* [Malta Block](https://dsp.maltablock.org/)
* [Mission Control](https://dsp.mest.net)
* [Aloha EOS](https://dsps.io/)
* [MinerGate](https://minergate.com/eos-vram-providers)
* [DSP HQ](https://dsphq.io/)

## DSP Package Explanation
DSP packages have several fields which are important to understand:

* **api_endpoint** - endpoint to direct DSP related trxs/api requests to
* **package_id** - ID of the package that can be selected with the **selectpkg** action
* **service** - the DSP service to be used.  Currently LiquidApps supports 6 DSP services; however DSPs are encouraged to create services of their own as well as create bundled DSP services.  The use of these resources is measured in QUOTA.
 1. ipfsservice1 - providing IPFS services to the dapp::multi_index container of a smart contract
 2. cronservices - enable CRON related tasks such as continuous staking
 3. oracleservic - provide oracle related services
 4. readfndspsvc - return a result from a smart contract function based on current conditions without sending an EOSIO trx
 5. accountless1 - virtual accounts that do not require RAM storage for account related data, instead data and accounts are stored in vRAM
* **provider** - DSP account name
* **quota** - QUOTA represents the amount of actions that a DSP supports based on the package_period. You can think of QUOTA like cell phone minutes in a plan.  For a cell phone plan you could pay $10 per month and get 1000 minutes.  1 QUOTA always equals 10,000 actions.  Said differently .0001 QUOTA equals 1 action.  Instead of $10 per month perhaps you would be required to stake 10 DAPP and/or 10 DAPPHDL (Air-HODL) tokens for a day to receive .001 QUOTA or 10 actions.
* **package_period** - period of the package before restaking is required. Upon restaking the QUOTA and package period are reset.
* **min_stake_quantity** - the minimum quantity of DAPP and/or DAPPHDL (Air-HODL) tokens to stake to receive the designated amount of QUOTA for the specified package_period
* **min_unstake_period** - period of time required to pass before **refund** action can be called after **unstake** command is executed
* **enabled** - bool if the package is available or not

## Select a DSP Package
Select a service package from the DSP of your choice.

```bash
export PROVIDER=someprovider
export PACKAGE_ID=providerpackage
export MY_ACCOUNT=myaccount

# select your package: 
export SERVICE=ipfsservice1
cleos -u $DSP_ENDPOINT push action dappservices selectpkg "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"$PACKAGE_ID\"]" -p $MY_ACCOUNT@active
```

## Stake DAPP Tokens for DSP Package
```bash
# Stake your DAPP to the DSP that you selected the service package for:
cleos -u $DSP_ENDPOINT push action dappservices stake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPP\"]" -p $MY_ACCOUNT@active
```

## Stake DAPPHDL (AirHODL) Tokens for DSP Package
If you were a holder of the EOS token on February 26th, 2019 then you should have a balance of DAPPHDL tokens.  These tokens possess the ability to 3rd party stake and unstake tokens throughout the duration of the AirHODL, until February 26th 2021.
```bash
# Stake your DAPPHDL to the DSP that you selected the service package for:
cleos -u $DSP_ENDPOINT push action dappairhodl1 stake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPPHDL\"]" -p $MY_ACCOUNT@active
```

## Unstake DAPP Tokens
```bash
cleos -u $DSP_ENDPOINT push action dappservices unstake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPP\"]" -p $MY_ACCOUNT@active
```

## Unstake DAPPHDL (AirHODL) Tokens
```bash
cleos -u $DSP_ENDPOINT push action dappairhodl1 unstake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPPHDL\"]" -p $MY_ACCOUNT@active
# In case unstake deferred trx fails, you can manually refund the unstaked tokens:
cleos -u $DSP_ENDPOINT push action dappairhodl1 refund "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\"]" -p $MY_ACCOUNT@active
```

## Withdraw DAPPHDL (AirHODL) Tokens
**Please note**: withdrawing your DAPPHDL tokens immediately makes you ineligible for further vesting and forfeits all your unvested tokens. **This action is irrevocable.**  Vesting ends February 26th 2021.  Also, you must hold DAPP token before executing this action.  If you do not, use the *open* action below to open a 0 balance.
```bash
# Withdraw
cleos -u $DSP_ENDPOINT push action dappairhodl1 withdraw "[\"$MY_ACCOUNT\"]" -p $MY_ACCOUNT@active
# Open DAPP balance to withdraw if needed
cleos -u $DSP_ENDPOINT push action dappservices open "[\"$MY_ACCOUNT\",\"4,DAPP\",\"$MY_ACCOUNT\"]" -p $MY_ACCOUNT@active
```

## Check DAPPHDL (AirHODL) Token Balance & Refresh Data
In the [dappairhodl1 contract](https://bloks.io/contract/dappairhodl1/table?table=accounts&scope=YOUR_ACCOUNT_HERE) under the accounts table, enter your account as the scope to retrieve its information.

```bash
# Refresh accounts table data
cleos -u $DSP_ENDPOINT push action dappservices refresh "[\"$MY_ACCOUNT\"]" -p $MY_ACCOUNT@active
```