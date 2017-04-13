# `createcontract` rpc call

QTUMCORE-7: Add "createcontract" RPC call

## Environment

Before running the test make sure that: Node started –regtest and unit tests pass.

### Contract code:

Make sure you have a sample contract to test with, in this ITD we use the one below:

```
pragma solidity ^0.4.0;

contract Test {
    address owner;
    function Test(){
        owner=msg.sender;
    }
    function setOwner(address test) payable{
        owner=msg.sender;
    }
    function getOwner() constant returns (address cOwner){
        return owner;
    }
    function getSender() constant returns (address cSender){
        return msg.sender;
    }
}
```
Compiled bytecode:
```
6060604052341561000c57fe5b5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b5b6101c88061005f6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af4035146100515780635e01eb5a1461007f578063893d20e8146100d1575bfe5b61007d600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610123565b005b341561008757fe5b61008f610168565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100d957fe5b6100e1610171565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a7230582050b454be91cd91e08099814fc2192c5fded81b632733f6c808e4d75e85a766a50029
```

### `createcontract` syntax:

```
Create a contract with bytcode.

Arguments:
1. "bytecode"  (string, required) contract bytcode.
2. gasLimit  (numeric or string, optional) gasLimit, default: 10000
3. gasPrice  (numeric or string, optional) gasPrice QTUM price per gas unit, default: 0.00001
4. "senderaddress" (string, optional) The quantum address that will be used to create the contract.
5. "broadcast" (bool, optional, default=true) Whether to broadcast the transaction or not.

Result:
[
  {
    "txid" : (string) The transaction id.
    "sender" : (string) QTUM address of the sender.
    "hash160" : (string) ripemd-160 hash of the sender.
    "address" : (string) expected contract address.
  }
]

Examples:
> qtum-cli createcontract "60606040525b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff02191690836c010000000000000000000000009081020402179055506103786001600050819055505b600c80605b6000396000f360606040526008565b600256"
> qtum-cli createcontract "60606040525b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff02191690836c010000000000000000000000009081020402179055506103786001600050819055505b600c80605b6000396000f360606040526008565b600256" 6000000 0.00000001 "QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" true
```

## Test Steps

### Test 1 - Successful deployment:

1. Start `qtumd` with:  

	`qtumd --regtest`
	
2. Generate 50 blocks:  

	`/qtum-cli --regtest generate 50`
	
3. Create the contract transaction:  

	`qtum-cli --regtest createcontract 6060604052341561000c57fe5b5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b5b6101c88061005f6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af4035146100515780635e01eb5a1461007f578063893d20e8146100d1575bfe5b61007d600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610123565b005b341561008757fe5b61008f610168565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100d957fe5b6100e1610171565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a7230582050b454be91cd91e08099814fc2192c5fded81b632733f6c808e4d75e85a766a50029`
	
4. Mine the contract:  

	`/qtum-cli --regtest generate 1`
	
### Test 2 - Successful deployment with specified sender:

1. Start `qtumd` with:  

	`qtumd --regtest`
2. Generate 50 blocks:  

	`/qtum-cli --regtest generate 50`

3. Import test address `qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT`:  

	`qtum-cli --regtest importprivkey cQWxca9y9XBf4c6ohTwRQ9Kf4GZyRybhGBfzaFgkvRpw8HjbRC58`

4. Send 0.1 QTUM to the test address and mine 1 block to confirm it:  

	`qtum-cli --regtest sendtoaddress qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT 0.1`
	
	`/qtum-cli --regtest generate 1`
	
5. Create the contract transaction:  

	`qtum-cli --regtest createcontract 6060604052341561000c57fe5b5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b5b6101c88061005f6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af4035146100515780635e01eb5a1461007f578063893d20e8146100d1575bfe5b61007d600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610123565b005b341561008757fe5b61008f610168565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100d957fe5b6100e1610171565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a7230582050b454be91cd91e08099814fc2192c5fded81b632733f6c808e4d75e85a766a50029 10000 0.0001 qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT`
	
	
6. Mine the contract:  

	`/qtum-cli --regtest generate 1`
	
### Test 3 - No broadcast:

1. Start `qtumd` with:  

	`qtumd --regtest`
	
2. Generate 50 blocks:  

	`/qtum-cli --regtest generate 50`
	
3. Import test address `qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT`:  

	`qtum-cli --regtest importprivkey cQWxca9y9XBf4c6ohTwRQ9Kf4GZyRybhGBfzaFgkvRpw8HjbRC58`

4. Send 0.1 QTUM to the test address and mine 1 block to confirm it:  

	`qtum-cli --regtest sendtoaddress qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT 0.1`
	
	`/qtum-cli --regtest generate 1`
	
5. Create the contract transaction:  

	`qtum-cli --regtest createcontract 6060604052341561000c57fe5b5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b5b6101c88061005f6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af4035146100515780635e01eb5a1461007f578063893d20e8146100d1575bfe5b61007d600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610123565b005b341561008757fe5b61008f610168565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100d957fe5b6100e1610171565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a7230582050b454be91cd91e08099814fc2192c5fded81b632733f6c808e4d75e85a766a50029 10000 0.0001 qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT false`
	

## Expected Results

### Test 1 - Successful deployment (Expected results):

Step 2 should result in `createcontract` returning contract creation info for example:

```
{
  "txid": "ea627872c30b87fbfe2023141c2e71e88bb3fa6b7c8118e7cd6a6d249f36a4ba",
  "sender": "qXFh7KsoAyq3oiQK3hiHB9d31WcYGSRSLm",
  "hash160": "963942a7bc49514103ccd04466ea1b9ca37931f8",
  "address": "691cd4bbdf30a6b12366c218cbeacee940328c45"
}
```
Step 3 should result in the contract being mined and added to the state.

We can use `getaccountinfo` to check that:  

```
qtum-cli --regtest getaccountinfo 691cd4bbdf30a6b12366c218cbeacee940328c45
```

Result should be:

```
{
  "address": "691cd4bbdf30a6b12366c218cbeacee940328c45",
  "balance": 0,
  "storage": {
    "290decd9548b62a8d60345a988386fc84ba6bc95484008f6362f93160ef3e563": {
      "0000000000000000000000000000000000000000000000000000000000000000": "000000000000000000000000963942a7bc49514103ccd04466ea1b9ca37931f8"
    }
  },
  "code": "60606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af4035146100515780635e01eb5a1461007f578063893d20e8146100d1575bfe5b61007d600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610123565b005b341561008757fe5b61008f610168565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100d957fe5b6100e1610171565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a7230582050b454be91cd91e08099814fc2192c5fded81b632733f6c808e4d75e85a766a50029"
}
```
* Please note that the address and other contract results data will be different on each test, change the values accordingly.

### Test 2 - Successful deployment with specified sender (Expected results):

Step 5 should result in `createcontract` returning contract creation info, Having `qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT` as sender, for example:

```
{
  "txid": "156304a1a323723a89bbc6d37f0128f71cd58a7e8fcd245ec642ef6974118f52",
  "sender": "qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT",
  "hash160": "baede766c1a4844efea1326fffc5cd8c1c3e42ae",
  "address": "947a98e40867ae4e4348e39e81aac47a2731db64"
}
```
Step 6 should result in the contract being mined and added to the state.

We can use `getaccountinfo` to check that:  

```
qtum-cli --regtest getaccountinfo 947a98e40867ae4e4348e39e81aac47a2731db64
```

Result should be:

```
{
  "address": "947a98e40867ae4e4348e39e81aac47a2731db64",
  "balance": 0,
  "storage": {
    "290decd9548b62a8d60345a988386fc84ba6bc95484008f6362f93160ef3e563": {
      "0000000000000000000000000000000000000000000000000000000000000000": "000000000000000000000000baede766c1a4844efea1326fffc5cd8c1c3e42ae"
    }
  },
  "code": "60606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af4035146100515780635e01eb5a1461007f578063893d20e8146100d1575bfe5b61007d600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610123565b005b341561008757fe5b61008f610168565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100d957fe5b6100e1610171565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a7230582050b454be91cd91e08099814fc2192c5fded81b632733f6c808e4d75e85a766a50029"
}
```
Make sure the right sender's `qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT ` hash160 `baede766c1a4844efea1326fffc5cd8c1c3e42ae` was stored in the contract: 

```
"storage": {
    "290decd9548b62a8d60345a988386fc84ba6bc95484008f6362f93160ef3e563": {
      "0000000000000000000000000000000000000000000000000000000000000000": "000000000000000000000000baede766c1a4844efea1326fffc5cd8c1c3e42ae"
    }
```

* Please note that the address and other contract results data will be different on each test, change the values accordingly.

### Test 3 - No broadcast (Expected results):

step 5 should result in getting the raw transaction data for the contract creation:

```
{
  "raw transaction": "03000000028e25c914c3dcabd8715d9f32b3af6a53a9d84e73b3c9daa403b8e8e2cfe77fba000000006b483045022100a008cc46fbd217e4b893d3e95019fd3d01c1de182b8b6a29ce175e4faf648ca4022077751bdebb64fa76aa2d13cf188662b7c45a83392e9ce425d815757510e509ac012103cedbc3099b504b72b36b8d1898bf249e1e64712728faba5fef929a1a37f72455feffffff8e25c914c3dcabd8715d9f32b3af6a53a9d84e73b3c9daa403b8e8e2cfe77fba010000006b483045022100db95ffe32c858df773aca181ac907dcc046935ef77e30248ac95de69c6a5b77202205049623d199c593eee35b749852758473d4cce4f19180907aff70150df28745c012102513611747c3139705ef57253f33e024c505d01edd52cfce36ef70b83df802be6feffffff020000000000000000fd330201010210270210274d27026060604052341561000c57fe5b5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b5b6101c88061005f6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af4035146100515780635e01eb5a1461007f578063893d20e8146100d1575bfe5b61007d600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610123565b005b341561008757fe5b61008f610168565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100d957fe5b6100e1610171565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a7230582050b454be91cd91e08099814fc2192c5fded81b632733f6c808e4d75e85a766a50029c1f472025ad10100001976a9140ab4db616bb4535198f0290a33df037a4859f3df88ac4200000000000000"
}
```
Decoding the raw data using:

```
qtum-cli --regtest decoderawtransaction 03000000028e25c914c3dcabd8715d9f32b3af6a53a9d84e73b3c9daa403b8e8e2cfe77fba000000006b483045022100a008cc46fbd217e4b893d3e95019fd3d01c1de182b8b6a29ce175e4faf648ca4022077751bdebb64fa76aa2d13cf188662b7c45a83392e9ce425d815757510e509ac012103cedbc3099b504b72b36b8d1898bf249e1e64712728faba5fef929a1a37f72455feffffff8e25c914c3dcabd8715d9f32b3af6a53a9d84e73b3c9daa403b8e8e2cfe77fba010000006b483045022100db95ffe32c858df773aca181ac907dcc046935ef77e30248ac95de69c6a5b77202205049623d199c593eee35b749852758473d4cce4f19180907aff70150df28745c012102513611747c3139705ef57253f33e024c505d01edd52cfce36ef70b83df802be6feffffff020000000000000000fd330201010210270210274d27026060604052341561000c57fe5b5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b5b6101c88061005f6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af4035146100515780635e01eb5a1461007f578063893d20e8146100d1575bfe5b61007d600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610123565b005b341561008757fe5b61008f610168565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100d957fe5b6100e1610171565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a7230582050b454be91cd91e08099814fc2192c5fded81b632733f6c808e4d75e85a766a50029c1f472025ad10100001976a9140ab4db616bb4535198f0290a33df037a4859f3df88ac4200000000000000
```

Should result in a valid OP_CREATE transaction:

```
{
  "txid": "cf614a1331350a2fc1340fb531f00a06a434c5959452695f9aaada27e45348cf",
  "hash": "cf614a1331350a2fc1340fb531f00a06a434c5959452695f9aaada27e45348cf",
  "size": 918,
  "vsize": 918,
  "version": 3,
  "locktime": 66,
  "vin": [
    {
      "txid": "ba7fe7cfe2e8b803a4dac9b3734ed8a9536aafb3329f5d71d8abdcc314c9258e",
      "vout": 0,
      "scriptSig": {
        "asm": "3045022100a008cc46fbd217e4b893d3e95019fd3d01c1de182b8b6a29ce175e4faf648ca4022077751bdebb64fa76aa2d13cf188662b7c45a83392e9ce425d815757510e509ac[ALL] 03cedbc3099b504b72b36b8d1898bf249e1e64712728faba5fef929a1a37f72455",
        "hex": "483045022100a008cc46fbd217e4b893d3e95019fd3d01c1de182b8b6a29ce175e4faf648ca4022077751bdebb64fa76aa2d13cf188662b7c45a83392e9ce425d815757510e509ac012103cedbc3099b504b72b36b8d1898bf249e1e64712728faba5fef929a1a37f72455"
      },
      "sequence": 4294967294
    }, 
    {
      "txid": "ba7fe7cfe2e8b803a4dac9b3734ed8a9536aafb3329f5d71d8abdcc314c9258e",
      "vout": 1,
      "scriptSig": {
        "asm": "3045022100db95ffe32c858df773aca181ac907dcc046935ef77e30248ac95de69c6a5b77202205049623d199c593eee35b749852758473d4cce4f19180907aff70150df28745c[ALL] 02513611747c3139705ef57253f33e024c505d01edd52cfce36ef70b83df802be6",
        "hex": "483045022100db95ffe32c858df773aca181ac907dcc046935ef77e30248ac95de69c6a5b77202205049623d199c593eee35b749852758473d4cce4f19180907aff70150df28745c012102513611747c3139705ef57253f33e024c505d01edd52cfce36ef70b83df802be6"
      },
      "sequence": 4294967294
    }
  ],
  "vout": [
    {
      "value": 0.00000000,
      "n": 0,
      "scriptPubKey": {
        "asm": "1 10000 10000 6060604052341561000c57fe5b5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b5b6101c88061005f6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af4035146100515780635e01eb5a1461007f578063893d20e8146100d1575bfe5b61007d600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610123565b005b341561008757fe5b61008f610168565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100d957fe5b6100e1610171565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a7230582050b454be91cd91e08099814fc2192c5fded81b632733f6c808e4d75e85a766a50029 OP_CREATE",
        "hex": "01010210270210274d27026060604052341561000c57fe5b5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b5b6101c88061005f6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af4035146100515780635e01eb5a1461007f578063893d20e8146100d1575bfe5b61007d600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610123565b005b341561008757fe5b61008f610168565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100d957fe5b6100e1610171565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a7230582050b454be91cd91e08099814fc2192c5fded81b632733f6c808e4d75e85a766a50029c1",
        "type": "create"
      }
    }, 
    {
      "value": 19986.69902580,
      "n": 1,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 0ab4db616bb4535198f0290a33df037a4859f3df OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a9140ab4db616bb4535198f0290a33df037a4859f3df88ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "qJXza7QD67sphbXnmxQJw2Y6vg3eYyfHGT"
        ]
      }
    }
  ]
}

```