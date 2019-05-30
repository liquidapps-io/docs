Packages and Staking
====================

## List of available Packages
DSPs who have registered their service packages may be found in the [package table](https://kylin.eosx.io/account/dappservices?mode=contract&sub=tables&table=package&lowerBound=&upperBound=&limit=100) under the dappservices account on every supported chain.  

Soon there will be DSP portals that can be used to compare and interact with DSP packages.

## Select a DSP Package
Select a service package from the DSP of your choice.  

```bash
export PROVIDER=someprovider
export PACKAGE_ID=providerpackage
export MY_ACCOUNT=myaccount

# select your package: 
export SERVICE=ipfsservice1
cleos -u $EOS_ENDPOINT push action dappservices selectpkg "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"$PACKAGE_ID\"]" -p $MY_ACCOUNT@active
```

## Stake DAPP Tokens for DSP Package
```bash
# Stake your DAPP to the DSP that you selected the service package for:
cleos -u $EOS_ENDPOINT push action dappservices stake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPP\"]" -p $MY_ACCOUNT@active
```

## Stake DAPPHDL (AirHODL) Tokens for DSP Package
If you were a holder of the EOS token on February 26th, 2019 then you should have a balance of DAPPHDL tokens.  These tokens possess the ability to 3rd party stake and unstake tokens throughout the duration of the AirHODL, until February 26th 2021.
```bash
# Stake your DAPPHDL to the DSP that you selected the service package for:
cleos -u $EOS_ENDPOINT push action dappairhodl1 stake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPPHDL\"]" -p $MY_ACCOUNT@active
```

## Unstake DAPP Tokens
```bash
cleos -u $EOS_ENDPOINT push action dappservices unstake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPP\"]" -p $MY_ACCOUNT@active
```

## Unstake DAPPHDL (AirHODL) Tokens
```bash
cleos -u $EOS_ENDPOINT push action dappairhodl1 unstake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPPHDL\"]" -p $MY_ACCOUNT@active
# In case unstake deferred trx fails, you can manually refund the unstaked tokens:
cleos -u $EOS_ENDPOINT push action dappairhodl1 refund "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\"]" -p $MY_ACCOUNT@active
```

## Withdraw DAPPHDL (AirHODL) Tokens
**Please note**: withdrawing your DAPPHDL tokens immediately makes you ineligible for further vesting and forfeits all your unvested tokens. **This action is irrevocable.**  Vesting ends February 26th 2021.  Also, you must hold DAPP token before executing this action.  If you do not, use the *open* action below to open a 0 balance.
```bash
# Withdraw
cleos -u $EOS_ENDPOINT push action dappairhodl1 withdraw "[\"$MY_ACCOUNT\"]" -p $MY_ACCOUNT@active
# Open DAPP balance to withdraw if needed
cleos -u $EOS_ENDPOINT push action dappservices open "[\"$MY_ACCOUNT\",\"4,DAPP\",\"$MY_ACCOUNT\"]" -p $MY_ACCOUNT@active
```

## Check DAPPHDL (AirHODL) Token Balance & Refresh Data
In the dappairhodl1 contract under the accounts table, enter your account as the scope to retrieve its information.

```bash
# Refresh accounts table data
cleos -u $EOS_ENDPOINT push action dappservices refresh "[\"$MY_ACCOUNT\"]" -p $MY_ACCOUNT@active
```