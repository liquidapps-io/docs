Token bridge example
====================
The Token bridge example uses the cron, oracle and ipfs service to enable token pegging between eosio chains.

Code located [here](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/tokenpeg)

### Download and test with zeus

To download and run the unit test for tokenpeg with zeus simply run:

```bash
mkdir bridge; cd bridge
# npm install -g @liquidapps/zeus-cmd
zeus box create
zeus unbox tokenpeg
zeus test -c
```

The unit test will deploy two "bridge contracts", one on the "mainnet" and one on a sidechain, as well as two corresponding token contracts on each chain. It will then transfer a token from the mainnet to sidechain and back from the sidechain to the mainnet. 

### Token bridge settings/initialization

The following are required settings for initializing the bridge contracts to allow cross chain transfers. Note that the code for the bridge contracts are identical, the only differences being the settings/initialization values.

- `name sister_code` - name of bridge contract on other chain
- `string sister_chain_name` - identifier of sister chain. The DSP's servicing the bridge contracts need to support these chains and have appropriate endpoints.
- `name token_contract` - the name of the token contract which holds the balances
- `bool processing_enabled` - flag to enable/disable message processing. Essentially disables outgoing tokens.
- `bool transfers_enabled` - flag to enable/disable token transfers to the bridge contract. Essentially disables incoming tokens.
- `uint64_t last_irreversible_block_num` - stores the last irreversible block number
- `bool can_issue` - boolean as to whether or not the contract can issue tokens. False on the chain where the original token was created, true on the other. 
- `uint64_t last_irreversible_block_num` stores the last irreversible block number
- `uint64_t last_received_releases_id, uint64_t last_received_receipts_id,uint64_t last_confirmed_block_id,uint64_t last_received_transfer_block_id` - counters for tracking the last received block of releases, receipts, confirmed block, and transfers

The example also allows an average or median consensus mechanism for calculating the most recent price.

### Token bridge actions

`
[[eosio::action]] 
void init(
  name sister_code,
  string sister_chain_name,
  name token_contract,
  symbol token_symbol,
  bool processing_enabled,
  bool transfers_enabled,
  uint64_t last_irreversible_block_num,
  bool can_issue,
  uint64_t last_received_releases_id,
  uint64_t last_received_receipts_id,
  uint64_t last_confirmed_block_id,
  uint64_t last_received_transfer_block_id
);
`
- initializes settings and broadcasts cron requests

 `
[[eosio::action]] 
void enable(bool processing_enabled, bool transfers_enabled);
`
- enable/disable processing or transfers
