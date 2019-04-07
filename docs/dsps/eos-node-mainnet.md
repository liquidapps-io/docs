Mainnet EOS Node
==============

## Recommended Hardware

m4.2xlarge machine - ubuntu 18.04
500GB+ primary disk

## Configuration

```bash
# mainnet backup

## WIP
SNAPFILE=docker-20180712
URL=https://osshkbk01.oss-cn-hongkong.aliyuncs.com/cryptokylin/$SNAPFILE.tar.bz2
P2P_FILE=https://.../
GENESIS=https://raw.githubusercontent.com/cryptokylin/CryptoKylin-Testnet/master/genesis.json

cd $HOME/.local/share/eosio/nodeos/data
wget $URL



cd $HOME/.local/share/eosio/nodeos/config

# download mainnet genesis
wget $GENESIS

# config
cat <<EOF >> $HOME/.local/share/eosio/nodeos/config/config.ini
chain-state-db-size-mb = 65535
EOF

wget $P2P_FILE

# p2p
cat p2p-config.ini >> $HOME/.local/share/eosio/nodeos/config/config.ini

nodeos --disable-replay-opts --hard-replay-blockchain  
```

