Become a DSP on another chain
==========

LiquidX enables DSPs (DAPP Service Providers) to offer services on new chains.  To do so, a DSP can aquire the necessary mapping files, verify the DSP's account name on the existing and new networks, and launch an eosio node based on that new network.

Becoming a DSP on a new chain requires having an existing DSP account on the EOS mainnet.  The mainnet account will remain the account that claims DSP rewards.  

Guide:

- [Get Mapping Files](#get-mapping-files)
- [Create DSP account mapping](#create-dsp-account-mapping)
- [Add to config.toml file](#add-to-config.toml-file)

## Get mapping files

There are two kinds of mapping when it comes to LiquidX, there are JSON files that must be added on both DSPs and there are actions that must be run on the new chain and the primary chain.

The JSON files mapping the DAPP Network contracts on the new chain should be available from the network that deployed the `dappservicex` contract.  This should get you:

- All the service file mappings, e.g., `liquidjungle.ipfs.service.json`

The JSON mapping files are needed to inform the DSP (DAPP Service Provider) of the account names hosting the DAPP Network contracts, e.g., `liquidjungle.ipfs.service.json`.  

Here we will setup the JSON mapping files.  This step must be performed on the primary network DSP and the new chain DSP.

For Jungle:

```bash
export BOX=liquidx-jungle
cd $(readlink -f `which setup-dsp` | xargs dirname)
cd models
npm i -g @liquidapps/zeus-cmd
zeus box add $BOX https://s3.us-east-2.amazonaws.com/liquidapps.artifacts/boxes/60295170699618ff20b212195afaa0700e4f17a893974095782a069ad31809d9.zip
zeus unbox $BOX
mv ./$BOX/models/liquidx-mappings/ .
mv ./$BOX/models/local-sidechains/ .
rm -rf ./$BOX
```

Example service file mapping `./models/liquidx-mappings/liquidjungle.ipfs.service.json`

```json
{
  "sidechain_name": "liquidjungle",
  "mainnet_account": "ipfsservice1",
  "chain_account": "ipfsservice1"
}
```

Example DSP file mapping `./models/liquidx-mappings/liquidjungle.uuddlrlrbass.json`

```json
{
    "sidechain_name":"liquidjungle",
    "mainnet_account":"uuddlrlrbass",
    "chain_account":"uuddlrlrbass"
}
```

Example sidechain file mapping `./models/local-sidechains/liquidjungle.json`

```json
{
    "dsp_port":3115,
    "nodeos_port":8888,
    "nodeos_endpoint":"http://localhost:8888",
    "nodeos_host":"localhost",
    "nodeos_state_history_port":8887,
    "nodeos_p2p_port":9876,
    "demux_port":3195,
    "name":"liquidjungle"
}
```

## Create DSP account mapping

On the eos mainnet, or jungle, you will need to connect your DSP account to the new chain's DSP account.  This is done using the `addaccount` command.

- owner {name} - DSP account name on mainnet or jungle
- chain_account {name} - DSP account name on new chain
- chain_name {name} - Account name LiquidX contract is deployed to

Cleos example:

```bash
cleos -u https://kylin.eos.dfuse.io push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"liquidjungle","name":"addaccount","data":{"owner":"uuddlrlrbass","chain_account":"uuddlrlrbass","chain_name":"liquidjungle"},"authorization":[{"actor":"uuddlrlrbass","permission":"active"}]}]}'
```

Then on the new chain, submit an `adddsp` action.

- owner {name} - DSP name on new chain
- dsp {name} - DSP name on mainnet or Jungle

Cleos example:

```bash
cleos -u https://api.jungle.alohaeos.com push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"dappservicex","name":"adddsp","data":{"owner":"uuddlrlrbass","dsp":"uuddlrlrbass"},"authorization":[{"actor":"uuddlrlrbass","permission":"active"}]}]}'
```

With that you have 2 way mapped your DSP account name.  On the primary network, the DSP's account has been linked to the new chain's network.  And on the new network, the primary network's DSP account has been verified.

## Add to config.toml file

In order to enable a sister chain, you must add the following to your mainnet or Jungle's `config.toml` file.  This is the file that holds the environment variables for your DSP's API instance.

```bash
# LIQUIDX_CONTRACT_NAME - this is the name of the account that has the LiquidX code deployed to it. 

[sidechains]
  [sidechains.LIQUIDX_CONTRACT_NAME]
    dsp_port = 12346
    nodeos_secured = false
    nodeos_host = "localhost"
    nodeos_port = 2424
    nodeos_state_history_port = 12341
    demux_port = 1232
    name = "<LIQUIDX_CONTRACT_NAME>"
    dsp_account = "<DSP SIDECHAIN ACCOUNT>"
    dsp_private_key = "<DSP SIDECHAIN PRIVATE KEY>"
    chainid = "<SIDECHAIN CHAIN ID>"
    mapping = "dappservices:dappservicex,cronservices:cronservicex,ipfsservice1:ipfsservice2"
```