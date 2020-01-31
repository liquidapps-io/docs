Unit Testing
====================

Unit testing with Zeus is highly customizable and easy to configure.  The following example explains how to use the main helper functions to write your own test.

## Customize your own unit tests
in tests/mycontract.spec.js
```javascript
require('mocha');
const { assert } = require('chai'); // Using Assert style
const { getCreateKeys } = require('../extensions/helpers/key-utils');
const getDefaultArgs = require('../extensions/helpers/getDefaultArgs');
const { getTestContract } = require('../extensions/tools/eos/utils');
const artifacts = require('../extensions/tools/eos/artifacts');
const deployer = require('../extensions/tools/eos/deployer');
const { genAllocateDAPPTokens, readVRAMData } = require('../extensions/tools/eos/dapp-services');

/*** UPDATE CONTRACT CODE ***/
var contractCode = 'mycontract';
var ctrt = artifacts.require(`./${contractCode}/`);
const delay = ms => new Promise(res => setTimeout(res, ms));

describe(`${contractCode} Contract`, () => {
  var testcontract;

  /*** SET CONTRACT NAME(S) ***/
  const code = 'airairairair';
  const code2 = 'airairairai2';
  var testUser = "tt11";
  var account = code;

  /*** CREATE TEST ACCOUNT NAME ***/
  const getTestAccountName = (num) => {
    var fivenum = num.toString(5).split('');
    for (var i = 0; i < fivenum.length; i++) {
      fivenum[i] = String.fromCharCode(fivenum[i].charCodeAt(0) + 1);
    }
    fivenum = fivenum.join('');
    var s = '111111111111' + fivenum;
    var prefix = 'test';
    s = prefix + s.substr(s.length - (12 - prefix.length));
    console.log(s);
    return s;
  };

  before(done => {
    (async () => {
      try {
        
        /*** DEPLOY CONTRACT ***/
        var deployedContract = await deployer.deploy(ctrt, code);
        
        /*** DEPLOY ADDITIONAL CONTRACTS ***/
        var deployedContract2 = await deployer.deploy(ctrt, code2);
        
        /*** ALLOCATE DAPP TOKENS TO YOUR DEPLOYED CONTRACT ***/
        await genAllocateDAPPTokens(deployedContract, 'ipfs');

        /*** RETURNS EOSJS SMART CONTRACT INSTANCE ***/
        testcontract = await getTestContract(code);

        /*** ENDS UNIT TEST SUCCESSFULLY ***/
        done();
      } catch (e) {
        /*** FAILS UNIT TEST AND PROVIDES ERROR ***/
        done(e);
      }
    })();
  });
        
  /*** DISPLAY NAME FOR TEST, REPLACE 'coldissue' WITH ANYTHING ***/
  it('coldissue', done => {
    (async () => {
      try {        
       
        /*** SETUP VARIABLES ***/
        var symbol = 'AIR';
                
        /*** DEFAULT failed = false FOR ASSERTION ERROR ***/
        /*** SET failed = true IN TRY/CATCH BLOCK TO FAIL TEST ***/
        var failed = false;
                    
        /*** SETUP CHAIN OF ACTIONS ***/
        await testcontract.create({
          issuer: code2,
          maximum_supply: `1000000000.0000 ${symbol}`
        }, {
          authorization: `${code}@active`,
          broadcast: true,
          sign: true
        });

        /*** CREATE ADDITIONAL KEYS AS NEEDED ***/
        var key = await getCreateKeys(code2);
        
        var testtoken = testcontract;
        await testtoken.coldissue({
          to: code2,
          quantity: `1000.0000 ${symbol}`,
          memo: ''
        }, {
          authorization: `${code2}@active`,
          broadcast: true,
          keyProvider: [key.active.privateKey],
          sign: true
        });
        
        /*** ADD DELAY BETWEEN ACTIONS ***/
        await delay(3000);
        
        /*** EXAMPLE TRY/CATCH failed = true ***/
        try {
          await testtoken.transfer({
            from: code2,
            to: code,
            quantity: `100.0000 ${symbol}`,
            memo: ''
          }, {
            authorization: `${code2}@active`,
            broadcast: true,
            keyProvider: [key.active.privateKey],
            sign: true
          });
        } catch (e) {
          failed = true;
        }
        
        /*** ADD CUSTOM FAILURE MESSAGE ***/
        assert(failed, 'should have failed before withdraw');
        
        /*** ADDITIONAL ACTIONS ... ***/

        done();
      } catch (e) {
        done(e);
      }
    })();
  });
        
  /*** USE it.skip TO CONTINUE WITH UNIT TEST IF TEST FAILS ***/
  it.skip('it.skip does not assert and continues test if fails' ...
});
```

## Helper Functions

### getCreateKeys | [Code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/eos-sdk/eos-keystore/extensions/helpers/key-utils.js#L22)

The `getCreateKeys` function is intended to create a new key pair in `~/.zeus/networks/development/accounts` if a key pair does not already exist for the account provided. The sub directory to `~/.zeus/network` can be any chain that you are developing on as well, e.g., `kylin`, `jungle`, `mainnet`.  If an account name has a contract deployed to it with the `deploy` function, then a key pair will automatically be assigned during the deployment.

```javascript
/**
 * @param account - account name to generate or fetch keys for
 */

const { getCreateKeys } = require('../extensions/helpers/key-utils');
var keys = await getCreateKeys(account);
```

### artifacts | [Code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/eos-sdk/seed-eos/extensions/tools/eos/artifacts.js)

The `artifacts` helper pulls the relevant contract files, such as the wasm / ABI, to be used within the unit test.

```javascript
/**
 * @param f - contract name within the /contracts/eos directory
 */

const artifacts = require('../extensions/tools/eos/artifacts');
var contractCode = 'mycontract';
var ctrt = artifacts.require(`./${contractCode}/`);
```

### deployer | [Code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/eos-sdk/seed-eos/extensions/tools/eos/deployer.js#L35)

The `deployer` function deploys a contract to an account based on the contract files and the account name passed.  You may also pass your own args, if not specified, the `getDefaultArgs()` function will be passed.

```javascript
/**
 * @param contract - contract file name to deploy
 * @param account - account name to deploy contract to
 * @param [args=getDefaultArgs()] - arguments to be used for configuring the network's settings
 */

const deployer = require('../extensions/tools/eos/deployer');
var ctrt = artifacts.require(`./${contractCode}/`);
const code = 'airairairair';
var deployedContract = await deployer.deploy(ctrt, code);
```

### genAllocateDAPPTokens | [Code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/dapp-services/extensions/tools/eos/dapp-services.js#L14)

The `genAllocateDAPPTokens` function allocates DAPP tokens to the specified contract provided.  It also issues, selects a package, stakes, and updates auth to include `eosio.code`.

```javascript
/**
 * @param deployedContract - deployed contract
 * @param serviceName - DAPP Services name as determined in the groups/boxes/groups/services/SELECTED_SERVICE/models/dapp-services/SELECTED_SERVICE.json model file under the "name" key
 * @param [provider=''] - DAPP Services Provider name, if non provided, 'pprovider1', 'pprovider2' will be used
 * @param [selectedPackage='default'] - package name to select, defaults to "default"
 */

const { genAllocateDAPPTokens } = require('../extensions/tools/eos/dapp-services');
await genAllocateDAPPTokens(deployedContract, 'ipfs');
```

### readVRAMData | [Code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/dapp-services/extensions/tools/eos/dapp-services.js#L102)

The `readVRAMData` function makes a vRAM get table row call by providing the `contract`, `key`, `table`, and `scope` arguments.

```javascript
/**
 * @param contract - account name contract was deployed to
 * @param key - primary key of the dapp::multi_index container
 * @param table - table name as specified in the ABI
 * @param scope - scope of dapp::multi_index container to read from
 */

const { readVRAMData } = require('../extensions/tools/eos/dapp-services');
var tableRes = await readVRAMData({
    contract: 'airairairair',
    key: `AIRU`,
    table: "accounts",
    scope: 'tt11'
});
```

### getTestContract | [Code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/eos-sdk/seed-eos/extensions/tools/eos/utils.js#L275)

The `getTestContract` function creates an EOSJS instance to be used for sending EOS transactions.

```javascript
/**
 * @param code - account name to use in setting up 
 */

const code = 'airairairair';
const { getTestContract } = require('../extensions/tools/eos/utils');
testcontract = await getTestContract(code);
await testcontract.create({
    issuer: code,
    maximum_supply: `1000000000.0000 ${symbol}`
}, {
    authorization: `${code}@active`,
    broadcast: true,
    sign: true
});
```

## Compile and test
```bash
zeus test
```