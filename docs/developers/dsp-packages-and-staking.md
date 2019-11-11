Packages and Staking
====================

## List of available Packages
DSPs who have registered their service packages may be found in the [package table](https://bloks.io/account/dappservices?loadContract=true&tab=Tables&account=dappservices&scope=dappservices&limit=100&table=package) under the dappservices account on every supported chain.  

DSP Portals for viewing/interacting with packages:

* [DSP HQ](https://dsphq.io/)
* [Bloks.io](https://bloks.io/dsp)
* [EOS Nation](https://dsp.eosnation.io/)
* [Malta Block](https://dsp.maltablock.org/)
* [Mission Control](https://dsp.mest.net)
* [Aloha EOS](https://dsps.io/)
* [MinerGate](https://minergate.com/eos-vram-providers)

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
The amount of time that must pass before an unstake executes a refund action and returns DAPP or DAPPHDL tokens is either the current time + the minimum unstake time as stated in the package table, or the end of the current package period, whichever is greater.
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

## Third Party Staking
Third parties may stake to a DSP package on behalf of a DAPP by calling the `staketo` and `unstaketo` actions. In order for a DAPP to select a new package after third parties have staked to them, all third party stakes must be removed.

The following two actions remove third party stakes, `preselectpkg` allows users to be removed in batches by supplying a `depth` parameter to indicate how many accounts to remove at a time.  The second action is `retirestakes` which allows for the removal of specific third party stakes from a list of `delegators` account names.

Third party stakers can be identified by supplying the `ID` of the [`accountext` table](https://bloks.io/account/dappservices?loadContract=true&tab=Tables&table=accountext&account=dappservices&scope=DAPP&limit=10000) entry that matches the account, provider, service, and package (or pending package) selected by the account being staked to as the scope of the [`stakingext` table](https://bloks.io/account/dappservices?loadContract=true&tab=Tables&table=stakingext&account=dappservices&scope=150&limit=100), for example: `150`.

### Examples:
```bash
cleos -u $DSP_ENDPOINT push action dappservices retirestake "{\"owner\":\"$MY_ACCOUNT\",\"provider\":\"heliosselene\",\"service\":\"cronservices\",\"package\":\"cronservices\",\"delegators\":["dappservice2","natdeveloper","oracletest22"]}" -p $MY_ACCOUNT

cleos -u $DSP_ENDPOINT push action dappservices preselectpkg "{\"owner\":\"$MY_ACCOUNT\",\"provider\":\"heliosselene\",\"service\":\"cronservices\",\"package\":\"cronservices\",\"depth\":\"10\"}" -p $MY_ACCOUNT

cleos -u $DSP_ENDPOINT push action dappservices staketo "{\"from\":\"$MY_ACCOUNT\",\"to\":\"asdfasdfasdy\",\"provider\":\"heliosselene\",\"service\":\"cronservices\",\"quantity\":\"10.0000 DAPP\"}" -p $MY_ACCOUNT

cleos -u $DSP_ENDPOINT push action dappservices unstaketo "{\"from\":\"$MY_ACCOUNT\",\"to\":\"asdfasdfasdy\",\"provider\":\"heliosselene\",\"service\":\"cronservices\",\"quantity\":\"10.0000 DAPP\"}" -p $MY_ACCOUNT

// if deferred trx for refund fails after unstaketo
cleos -u $DSP_ENDPOINT push action dappservices refundto "{\"from\":\"$MY_ACCOUNT\",\"to\":\"asdfasdfasdy\",\"provider\":\"heliosselene\",\"service\":\"cronservices\",\"symcode\":\"DAPP\"}" -p $MY_ACCOUNT
```

### More on `preselectpkg` and `retirestakes`

`preselectpkg` allows the DAPP to simply supply a `depth` (number of third party stakes) to remove per tx ordered by the `ID` of "payer". The DAPP may simply execute this as many times as required until no more third party stakes remain. If a depth that is too deep is selected (for example, 100) the trx will simply fail, and a smaller depth can be selected. This method requires no external knowledge of who has staked to the account.

`retirestakes` is a more explicit and selective method for a DAPP. A list of third party stake payers (`name[] delegators`) can be provided. If the list is too large, the tx may time out. In order to discover who has staked to the DAPP's package the following steps can be utilized:

1. Determine the `id` of their package's `accountext` table entry. accountext entries all use the scope `DAPP`. The correct entry is the entry that has that same account, service, provider, and package (or pending_package) as the selected package. [bloks.io accountext table](https://bloks.io/account/dappservices?loadContract=true&tab=Tables&table=accountext&account=dappservices&scope=DAPP&limit=100000)

2. Find the `stakingext` entries, using the `id` from the `accountext` table as the scope. [bloks.io stakingext table](https://bloks.io/account/dappservices?loadContract=true&tab=Tables&table=stakingext&account=dappservices&scope=155&limit=100)

3. Each entry under this scope will have payers equal to the account names of the third parties. These payers can be used to populate the delegators for the `retirestakes` action.

For both actions, if the `depth` is deeper than the number of stakes, or the `delegators` list includes payers that haven't actually staked, it will succeed gracefully, ignoring the erroneous entries and executing for the correct ones.

The `dappairhodl1` contract is the exception to all of this. It does not get added to the `stakingext` table since while it is a third party stake mechanically, it is essentially the same as a first party stake. We do not support third party staking using AirHODL'd DAPP.