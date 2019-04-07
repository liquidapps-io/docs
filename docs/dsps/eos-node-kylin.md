Kylin EOS Node
==============

## Recommended Hardware

m4.2xlarge machine - ubuntu 18.04
500GB+ primary disk

## Configuration

```bash
# kylin backup
SNAPFILE=snapshot-0276f607955f3008bae69fc47a23ac2eb989af1adebeced2d7462ef30423b194.bin
URL=https://s3-ap-northeast-1.amazonaws.com/eosbeijing/$SNAPFILE
P2P_FILE=https://.../
GENESIS=https://raw.githubusercontent.com/cryptokylin/CryptoKylin-Testnet/master/genesis.json


cd $HOME/.local/share/eosio/nodeos/data/snapshots
wget $URL
cd $HOME/.local/share/eosio/nodeos/config


# download genesis
wget $GENESIS

# config
cat <<EOF >> $HOME/.local/share/eosio/nodeos/config/config.ini
chain-state-db-size-mb = 65535
EOF

# wget $P2P_FILE
# cat p2p-config.ini >> $HOME/.local/share/eosio/nodeos/config/config.ini

# p2p
cat <<EOF >> $HOME/.local/share/eosio/nodeos/config/config.ini
p2p-peer-address = kylinnet.eosstore.link:9876
p2p-peer-address = 119.254.15.40:9876
p2p-peer-address = 39.108.231.157:23225
p2p-peer-address = p2p.kylin.eoseco.com:10000
p2p-peer-address = p2p-kylin.eoslaomao.com:443
p2p-peer-address = p2p.kylin-testnet.eospacex.com:88
p2p-peer-address = kylin.fnp2p.eosbixin.com:443
p2p-peer-address = peering-kylin.eosasia.one:80
p2p-peer-address = kylin.meet.one:9876
p2p-peer-address = peer.kylin.alohaeos.com:9876
p2p-peer-address = p2p.kylin.helloeos.com.cn:9876
p2p-peer-address = kylin-testnet.starteos.io:9876
p2p-peer-address = kylin-fn001.eossv.org:443
p2p-peer-address = kylin-fn001.eossv.org:443
p2p-peer-address = api-kylin.eoshenzhen.io:9876
p2p-peer-address = p2p.kylin.eosbeijing.one:8080
p2p-peer-address = testnet.zbeos.com:9876
EOF

nodeos --disable-replay-opts --snapshot $HOME/.local/share/eosio/nodeos/data/snapshots/$SNAPFILE --delete-all-blocks
```


## Faucet
[Faucet](https://kylin-dapp-faucet.liquidapps.io/)
