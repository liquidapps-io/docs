Kylin Testnet Account
=====================
## Account

```bash
# Create a new available account name (replace 'yourtestaccount' with your account name):
export ACCOUNT=yourtestaccount

# Configure endpoint
export DSP_ENDPOINT=https://kylin-dsp-1.liquidapps.io

# Create wallet
cleos wallet create --file wallet_password.pwd

# Create account and import key
curl http://faucet.cryptokylin.io/create_account?$ACCOUNT > keys.json
export ACTIVE_PRIVATE_KEY=`cat keys.json | jq -e '.keys.active_key.private'`
export ACTIVE_PUBLIC_KEY=`cat keys.json | jq -e '.keys.active_key.public'`
cleos wallet import --private-key $ACTIVE_PRIVATE_KEY
# if this does not work, import key directly

# Get some tokens, stake CPU/NET, buy RAM for contract
curl http://faucet.cryptokylin.io/get_token?$ACCOUNT
curl http://faucet.cryptokylin.io/get_token?$ACCOUNT
cleos -u $DSP_ENDPOINT system buyram $ACCOUNT $ACCOUNT "100.0000 EOS" -p $ACCOUNT@active
cleos -u $DSP_ENDPOINT system delegatebw $ACCOUNT $ACCOUNT "20.0000 EOS" "80.0000 EOS" -p $ACCOUNT@active
```
*Save wallet_password.pwd and keys.json somewhere safe!*

## Kylin DAPP Tokens

[DAPP Faucet](https://kylin-dapp-faucet.liquidapps.io/)
