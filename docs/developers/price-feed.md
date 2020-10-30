Price feed example
====================
The price feed example uses either `LiquidScheduler` and `LiquidHarmony` or just `LiquidHarmony` oracles to fetch a price every X seconds.  If the price does not deviate by more or less than 1% (default) then an assertion will be thrown preventing the DSP from using CPU on the oracle request or the cron rescheduling action.

Code located [here](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/price-feed)

### Download and test with zeus

To download and run the unit test the price feed with zeus simply run:

```bash
mkdir price-feed; cd price-feed
# npm install -g @liquidapps/zeus-cmd
zeus box create
zeus unbox price-feed
zeus test -c
```

The unit test will run several requests, none of which will increment the CPU used past the first action unless the price deviates by 1% from the first price logged during the test.

### Price feed settings

The following are the settings that can be customized with the `settings` action.

- `uint32_t threshold = 1;` - this represents the minimum amount of DSPs that must respond to deem a price fetch valid, defaults to 1
- `float lower_bound_filter = 1;` // lower bound check to see if a price has dropped by more than x%, if so, update the price and spend CPU to do so, if not assert reschedule the cron and spend no CPU, defaults 1%
- `float upper_bound_filter = 1;` // upper bound check to see if a price has increased by more than x%, if so, update the price and spend CPU to do so, if not assert reschedule the cron and spend no CPU, defaults 1%
- `float lower_bound_dsp = 1;` // lower bound check to compare all DSP results against, if any DSP result is less than the first reported price by x%, assert, defaults to 1%
- `float upper_bound_dsp = 1;` // upper bound check to compare all DSP results against, if any DSP result is more than the first reported price by x%, assert, defaults to 1%

The example also allows an average or median consensus mechanism for calculating the most recent price.

### Price feed actions

- `testfeed (std::vector<char> uri, uint32_t interval)` - takes a LiquidHarmony URI and an interval to reschedule the price feed request
- `settings (uint32_t new_threshold_val, float lower_bound_filter, float upper_bound_filter, float lower_bound_dsp, float upper_bound_dsp)` - update settings for price feed
- `stopstart (bool stopstart_bool)` - toggles price feed cron on or off
- `testfetch (std::vector<char> uri)` - fetches price feed oracle request without cron

cleos examples:

```bash
# set settings
cleos -u $EOS_ENDPOINT push action $KYLIN_TEST_ACCOUNT settings '[1,1,1,1,1]' -p $KYLIN_TEST_ACCOUNT
# start feed for EOS price fetch every 15 seconds
cleos -u $EOS_ENDPOINT push action $KYLIN_TEST_ACCOUNT testfeed '["68747470732b6a736f6e3a2f2f5553442f6d696e2d6170692e63727970746f636f6d706172652e636f6d2f646174612f70726963653f6673796d3d454f53267473796d733d555344266170695f6b65793d64356132346639653535616265633938316163396565346337366230346132663237643138303234613134313564663830666130306137393466343864636162",15]' -p $KYLIN_TEST_ACCOUNT
# stop feed / enable feed to be restarted
cleos -u $EOS_ENDPOINT push action $KYLIN_TEST_ACCOUNT stopstart '[false]' -p $KYLIN_TEST_ACCOUNT
# test once time fetch only using oracles
cleos -u $EOS_ENDPOINT push action $KYLIN_TEST_ACCOUNT testfetch '["68747470732b6a736f6e3a2f2f5553442f6d696e2d6170692e63727970746f636f6d706172652e636f6d2f646174612f70726963653f6673796d3d454f53267473796d733d555344266170695f6b65793d64356132346639653535616265633938316163396565346337366230346132663237643138303234613134313564663830666130306137393466343864636162"]' -p $KYLIN_TEST_ACCOUNT
```