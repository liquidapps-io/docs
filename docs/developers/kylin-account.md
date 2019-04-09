Kylin Testnet Account
=====================
## Account

```bash
# Create a new available account name (replace 'yourtestaccount' with your account name):
export KYLIN_TEST_ACCOUNT=yourtestaccount
# Create wallet
cleos wallet create --file wallet_password.pwd

# Create Account
curl http://faucet.cryptokylin.io/create_account?$KYLIN_TEST_ACCOUNT > keys.json
export KYLIN_TEST_PRIVATE_KEY=`cat keys.json | jq -e '.keys.active_key.private'`
export KYLIN_TEST_PUBLIC_KEY=`cat keys.json | jq -e '.keys.active_key.public'`
cleos wallet import $KYLIN_TEST_PRIVATE_KEY

# Configure endpoint
export EOS_ENDPONT=https://kylin.eoscanada.com
```
*Save wallet_password.pwd and keys.json somewhere safe!*

## Kylin EOS Tokens
```bash
# Get some Kylin EOS tokens
curl http://faucet.cryptokylin.io/get_token?$KYLIN_TEST_ACCOUNT
```

## Kylin DAPP Tokens

[DAPP Faucet](https://kylin-dapp-faucet.liquidapps.io/)
