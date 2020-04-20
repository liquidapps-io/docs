Become a Block Producer
==========

To obtain a Block Producer account on CoVax, reach out in the CoVax Telegram channel: [https://t.me/CoVaxApp](https://t.me/CoVaxApp).

Hyperion: [https://covax.eosrio.io/v2/docs/index.html](https://covax.eosrio.io/v2/docs/index.html) | courtesy of [`eosriobrazil`](https://eosrio.io/)

[Block Explorer](https://local.bloks.io/?nodeUrl=covax.eosdsp.com&coreSymbol=COVAX&systemDomain=eosio&hyperionUrl=https%3A%2F%2Fcovax.eosdsp.com) | courtesy of [EOSUSA](https://bp.eosusa.news/)

[Block Producer Scorecard](https://validate.eosnation.io/covax/producers/) | courtesy of [EOS Nation](https://eosnation.io/)

Guide:

- [Genesis JSON](#genesis-json)
- [Peers](#peers)
- [API Endpoints](#api-endpoints)
- [Snapshots](#snapshots)
- [Scripts](#scripts)

## Genesis JSON:

```json
{
  "initial_timestamp": "2018-03-18T08:55:11.000",
  "initial_key": "EOS6HyUZskuWbHzhZx4Vi8ZxcaW28hte5MVGhejFGJeDbd6iYYXBn",
  "initial_configuration": {
    "max_block_net_usage": 1048576,
    "target_block_net_usage_pct": 1000,
    "max_transaction_net_usage": 524288,
    "base_per_transaction_net_usage": 12,
    "net_usage_leeway": 500,
    "context_free_discount_net_usage_num": 20,
    "context_free_discount_net_usage_den": 100,
    "max_block_cpu_usage": 100000,
    "target_block_cpu_usage_pct": 500,
    "max_transaction_cpu_usage": 50000,
    "min_transaction_cpu_usage": 100,
    "max_transaction_lifetime": 3600,
    "deferred_trx_expiration_window": 600,
    "max_transaction_delay": 3888000,
    "max_inline_action_size": 4096,
    "max_inline_action_depth": 4,
    "max_authority_depth": 6
  },
  "initial_chain_id": "63788f6e75cdb4ec9d8bb64ce128fa08005326a8b91702d0d03e81ba80e14d27"
}
```

## Peers:

```bash
eosnode-covax.liquidapps.io:9876
node1.eosdsp.com:9888
dsp1.dappsolutions.app:9875
covax.maltablock.org:9876
covax.eosrio.io:8132
covax.eosn.io:9876
node3.blockstartdsp.com:8132
```

## API Endpoints:

- [`http://eosnode-covax.liquidapps.io`](http://eosnode-covax.liquidapps.io/v1/chain/get_info)
- [`https://covax.eosn.io`](https://covax.eosn.io/v1/chain/get_info)
- [`https://covax.eosdsp.com`](https://covax.eosdsp.com/v1/chain/get_info)
- [`http://node3.blockstartdsp.com:8200`](http://node3.blockstartdsp.com:8200/v1/chain/get_info)

## Snapshots:

[https://snapshots.eosnation.io/](https://snapshots.eosnation.io/) | courtesy of [EOS Nation](https://eosnation.io/)

## Scripts:

- [`genesis_start.sh`](#genesis-start-sh)
- [`start.sh`](#start-sh)
- [`stop.sh`](#stop-sh)
- [`hard_replay.sh`](#hard-replay-sh)
- [`clean.sh`](#clean-sh)

The following are a list of scripts from the bios boot sequence tutorial located [here](https://developers.eos.io/welcome/latest/tutorials/bios-boot-sequence/).  The `PURBLIC_KEY_HERE` and `PRIVATE_KEY_HERE` fields must be updated in the `genesis_start.sh`, `start.sh`, and `hard_replay.sh` scripts.

To start the chain from genesis, run the `genesis_start.sh` file, then if you need to stop the chain, run `stop.sh`, if you need to start again, run `start.sh`.  If you get a dirty flag, run `hard_replay.sh`.

If you need to wipe everything, run `stop.sh`, `clean.sh`, `genesis_start.sh`.

If you need to install eosio, see the [eosio node](../dsps/eosio-node) section of the docs.

```bash
mkdir biosboot
touch genesis.json
nano genesis.json
mkdir genesis
cd genesis
mkdir YOUR_BP_NAME_HERE
cd YOUR_BP_NAME_HERE
touch genesis_start.sh
nano genesis_start.sh
chmod 755 genesis_start.sh
./genesis_start.sh
# create start.sh, stop.sh, hard_replay.sh, and clean.sh
```

### `genesis_start.sh`

```bash
#!/bin/bash
DATADIR="./blockchain"
CURDIRNAME=${PWD##*/}
if [ ! -d $DATADIR ]; then
  mkdir -p $DATADIR;
fi
nodeos \
--genesis-json $DATADIR"/../../../genesis.json" \
--signature-provider PURBLIC_KEY_HERE=KEY:PRIVATE_KEY_HERE \
--plugin eosio::producer_plugin \
--plugin eosio::producer_api_plugin \
--plugin eosio::chain_plugin \
--plugin eosio::chain_api_plugin \
--plugin eosio::http_plugin \
--plugin eosio::history_api_plugin \
--plugin eosio::history_plugin \
--data-dir $DATADIR"/data" \
--blocks-dir $DATADIR"/blocks" \
--config-dir $DATADIR"/config" \
--producer-name $CURDIRNAME \
--http-server-address 127.0.0.1:8888 \
--p2p-listen-endpoint 127.0.0.1:9876 \
--p2p-peer-address localhost:9877 \
--access-control-allow-origin=* \
--contracts-console \
--http-validate-host=false \
--verbose-http-errors \
--enable-stale-production \
--wasm-runtime=eos-vm \
--eos-vm-oc-enable \
--p2p-peer-address eosnode-covax.liquidapps.io:9876 \
--p2p-peer-address node1.eosdsp.com:9888 \
--p2p-peer-address dsp1.dappsolutions.app:9875 \
--p2p-peer-address covax.maltablock.org:9876 \
--p2p-peer-address covax.eosrio.io:8132 \
--p2p-peer-address covax.eosn.io:9876 \
--p2p-peer-address node3.blockstartdsp.com:8132 \
>> $DATADIR"/nodeos.log" 2>&1 & \
echo $! > $DATADIR"/eosd.pid"
```

### `start.sh`

```bash
#!/bin/bash
DATADIR="./blockchain"
        CURDIRNAME=${PWD##*/}
​
if [ ! -d $DATADIR ]; then
  mkdir -p $DATADIR;
fi
​
nodeos \
--signature-provider PURBLIC_KEY_HERE=KEY:PRIVATE_KEY_HERE \
--plugin eosio::producer_plugin \
--plugin eosio::producer_api_plugin \
--plugin eosio::chain_plugin \
--plugin eosio::chain_api_plugin \
--plugin eosio::http_plugin \
--plugin eosio::history_api_plugin \
--plugin eosio::history_plugin \
--data-dir $DATADIR"/data" \
--blocks-dir $DATADIR"/blocks" \
--config-dir $DATADIR"/config" \
--producer-name $CURDIRNAME \
--http-server-address 0.0.0.0:8888 \
--p2p-listen-endpoint 0.0.0.0:9876 \
--access-control-allow-origin=* \
--contracts-console \
--http-validate-host=false \
--verbose-http-errors \
--enable-stale-production \
--wasm-runtime=eos-vm \
--eos-vm-oc-enable \
--p2p-peer-address eosnode-covax.liquidapps.io:9876 \
--p2p-peer-address node1.eosdsp.com:9888 \
--p2p-peer-address dsp1.dappsolutions.app:9875 \
--p2p-peer-address covax.maltablock.org:9876 \
--p2p-peer-address covax.eosrio.io:8132 \
--p2p-peer-address covax.eosn.io:9876 \
--p2p-peer-address node3.blockstartdsp.com:8132 \
>> $DATADIR"/nodeos.log" 2>&1 & \
echo $! > $DATADIR"/eosd.pid"
```

### `stop.sh`

```bash
#!/bin/bash
DATADIR="./blockchain/"
​
if [ -f $DATADIR"/eosd.pid" ]; then
pid=`cat $DATADIR"/eosd.pid"`
echo $pid
kill $pid
rm -r $DATADIR"/eosd.pid"
echo -ne "Stoping Node"
while true; do
[ ! -d "/proc/$pid/fd" ] && break
echo -ne "."
sleep 1
done
echo -ne "\rNode Stopped. \n"
fi
```

### `hard_replay.sh`

```bash
#!/bin/bash
DATADIR="./blockchain"
CURDIRNAME=${PWD##*/}
​
if [ ! -d $DATADIR ]; then
  mkdir -p $DATADIR;
fi
​
nodeos \
--signature-provider PURBLIC_KEY_HERE=KEY:PRIVATE_KEY_HERE \
--plugin eosio::producer_plugin \
--plugin eosio::producer_api_plugin \
--plugin eosio::chain_plugin \
--plugin eosio::chain_api_plugin \
--plugin eosio::http_plugin \
--plugin eosio::history_api_plugin \
--plugin eosio::history_plugin \
--data-dir $DATADIR"/data" \
--blocks-dir $DATADIR"/blocks" \
--config-dir $DATADIR"/config" \
--producer-name $CURDIRNAME \
--http-server-address 127.0.0.1:8888 \
--p2p-listen-endpoint 127.0.0.1:9876 \
--p2p-peer-address localhost:9877 \
--access-control-allow-origin=* \
--contracts-console \
--http-validate-host=false \
--verbose-http-errors \
--enable-stale-production \
--wasm-runtime=eos-vm \
--eos-vm-oc-enable \
--hard-replay-blockchain \
--p2p-peer-address eosnode-covax.liquidapps.io:9876 \
--p2p-peer-address node1.eosdsp.com:9888 \
--p2p-peer-address dsp1.dappsolutions.app:9875 \
--p2p-peer-address covax.maltablock.org:9876 \
--p2p-peer-address covax.eosrio.io:8132 \
--p2p-peer-address covax.eosn.io:9876 \
--p2p-peer-address node3.blockstartdsp.com:8132 \
>> $DATADIR"/nodeos.log" 2>&1 & \
echo $! > $DATADIR"/eosd.pid"
```

### `clean.sh`

```bash
#!/bin/bash
rm -fr blockchain
ls -al
```