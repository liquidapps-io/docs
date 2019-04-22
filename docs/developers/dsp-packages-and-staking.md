Packages and Staking
====================

## List of available Packages
DSPs who have registered their service packages may be found in the [package table](https://kylin.eosx.io/account/dappservices?mode=contract&sub=tables&table=package&lowerBound=&upperBound=&limit=100) under the dappservices account on every supported chain.

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