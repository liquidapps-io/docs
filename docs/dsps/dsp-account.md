Account
=======

## Prerequisites
Install cleos from: https://github.com/EOSIO/eos/releases

## Create Account
### Mainnet

```bash
cleos create key --to-console > keys.txt
export DSP_PRIVATE_KEY=`cat keys.txt | head -n 1 | cut -d ":" -f 2 | xargs echo`
export DSP_PUBLIC_KEY=`cat keys.txt | tail -n 1 | cut -d ":" -f 2 | xargs echo`
```
*Save keys.txt somewhere safe!*

#### Have an exising EOS Account
- [Getting started on eos mainnet](https://hackernoon.com/getting-started-on-eos-mainnet-in-10-minutes-bf61dd9ec787)

#### First EOS Account
Fiat:
- [EOS Account Creator](https://eos-account-creator.com/)
- [EOS Lynx](https://eoslynx.com/)

Bitcoin/ETH/Bitcoin Cash/ALFAcoins:
- [ZEOS](https://www.zeos.co/)

### Kylin
Create an account
```bash
# Create a new available account name (replace 'yourdspaccount' with your account name):
export DSP_ACCOUNT=yourdspaccount
curl http://faucet.cryptokylin.io/create_account?$DSP_ACCOUNT > keys.json
curl http://faucet.cryptokylin.io/get_token?$DSP_ACCOUNT
export DSP_PRIVATE_KEY=`cat keys.json | jq -e '.keys.active_key.private'`
export DSP_PUBLIC_KEY=`cat keys.json | jq -e '.keys.active_key.public'`
```
*Save keys.json somewhere safe!*

## Account Name

```bash
# Create wallet
cleos wallet create --file wallet_password.pwd
```
*Save wallet_password.pwd somewhere safe!*

## Import account
```bash
cleos wallet import $DSP_PRIVATE_KEY
```
