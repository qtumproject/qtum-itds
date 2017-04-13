# `sendtocontract` rpc call

QTUMCORE-13: Add "sendtocontract" RPC call

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
        owner=test;
    }
    function setSenderAsOwner() payable{
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
6060604052341561000c57fe5b5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b5b6102218061005f6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af40351461005c5780632bcf51b41461008a5780635e01eb5a14610094578063893d20e8146100e6575bfe5b610088600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610138565b005b61009261017d565b005b341561009c57fe5b6100a46101c1565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100ee57fe5b6100f66101ca565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b80600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a72305820791895230daae0ed58cc374ad5b639044408f1942d7b85689a616caee50dc42e0029
```

Functions' signatures:

```
893d20e8 getOwner()
5e01eb5a getSender()
13af4035 setOwner(address)
2bcf51b4 setSenderAsOwner()
```
### `sendtocontract` syntax:

```
sendtocontract "contractaddress" "data" (amount gaslimit gasprice senderaddress broadcast)
Send funds and data to a contract.

Arguments:
1. "contractaddress" (string, required) The contract address that will receive the funds and data.
2. "datahex"  (string, required) data to send.
3. "amount"      (numeric or string, optional) The amount in QTUM to send. eg 0.1, default: 0
4. gasLimit  (numeric or string, optional) gasLimit, default: 10000
5. gasPrice  (numeric or string, optional) gasPrice Qtum price per gas unit, default: 0.00001
6. "senderaddress" (string, optional) The quantum address that will be used as sender.
7. "broadcast" (bool, optional, default=true) Whether to broadcast the transaction or not.

Result:
[
  {
    "txid" : (string) The transaction id.
    "sender" : (string) QTUM address of the sender.
    "hash160" : (string) ripemd-160 hash of the sender.
  }
]

Examples:
> qtum-cli sendtocontract "c6ca2697719d00446d4ea51f6fac8fd1e9310214" "54f6127f"
> qtum-cli sendtocontract "c6ca2697719d00446d4ea51f6fac8fd1e9310214" "54f6127f" 12.0015 6000000 0.00000001 "QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd"
```

For all tests in this document, we will need to deploy the contract using `createcontract` and use its address to test `sendtocontract`:

1. Start `qtumd` with:  

	`qtumd --regtest`
	
2. Generate 50 blocks:  

	`/qtum-cli --regtest generate 50`
	
3. Create the contract transaction:  

	`qtum-cli --regtest createcontract 6060604052341561000c57fe5b5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b5b6102218061005f6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af40351461005c5780632bcf51b41461008a5780635e01eb5a14610094578063893d20e8146100e6575bfe5b610088600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610138565b005b61009261017d565b005b341561009c57fe5b6100a46101c1565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100ee57fe5b6100f66101ca565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b80600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a72305820791895230daae0ed58cc374ad5b639044408f1942d7b85689a616caee50dc42e0029`
	
4. Mine the contract:  

	`/qtum-cli --regtest generate 1`
	
	
Our test contract address after deployment is: `4d8241c63e8d213d7d3b587af391a906ab6948d2`

## Test Steps

### Test 1 - Send message to contract:

1. Call the contract setOwner function to set the contract owner to `8888888888888888888888888888888888888888`:

	`/qtum-cli --regtest sendtocontract 4d8241c63e8d213d7d3b587af391a906ab6948d2 13af40350000000000000000000000008888888888888888888888888888888888888888`
	
2. Mine the 1 block:  

	`/qtum-cli --regtest generate 1`
	
### Test 2 - Send message and funds to contract:

1. Call the contract setOwner function to set the contract owner to `9999999999999999999999999999999999999999` and send `100 QTUM` with the call:

	`/qtum-cli --regtest sendtocontract 4d8241c63e8d213d7d3b587af391a906ab6948d2 13af40350000000000000000000000009999999999999999999999999999999999999999 100`
	
2. Mine the 1 block:  

	`/qtum-cli --regtest generate 1`
	
### Test 3 - Send message and funds to contract and specify a sender address:

1. Import test address `qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT`:  

	`qtum-cli --regtest importprivkey cQWxca9y9XBf4c6ohTwRQ9Kf4GZyRybhGBfzaFgkvRpw8HjbRC58`

2. Send 0.1 QTUM to the test address and mine 1 block to confirm it:  

	`qtum-cli --regtest sendtoaddress qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT 0.1`
	
	`/qtum-cli --regtest generate 1`

3. Call the contract setSenderAsOwner function to set the contract owner to `qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT` (hash160: `baede766c1a4844efea1326fffc5cd8c1c3e42ae`) and send `100 QTUM` with the call:

	`/qtum-cli --regtest sendtocontract 4d8241c63e8d213d7d3b587af391a906ab6948d2 2bcf51b4 100 10000 0.00001 qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT`
	
4. Mine the 1 block:  

	`/qtum-cli --regtest generate 1`
	
### Test 4 - No broadcast:

1. Send 0.1 QTUM to the test address and mine 1 block to confirm it:  

	`qtum-cli --regtest sendtoaddress qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT 0.1`
	
	`/qtum-cli --regtest generate 1`

2. Call the contract with broadcast flag set to false:

	`/qtum-cli --regtest sendtocontract 4d8241c63e8d213d7d3b587af391a906ab6948d2 2bcf51b4 100 10000 0.00001 qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT false`
	
## Expected Results

### Test 1 - Send message to contract (Expected results):

Step 1 should result in `sendtocontract` returning contract call info for example:

```
{
  "txid": "a3f08482e4b82a8a7ac8463879b8474f8cde254454f52b2269089e165472fb14",
  "sender": "qe4qM2q7JoVLbNgkPmH3EGvndLBnNRrGoP",
  "hash160": "e0f470cb89c0f88942ff9771cfbe72f468441c17"
}

```
Step 2 should result in the call being executed and contract `owner` variable set to `8888888888888888888888888888888888888888`.

We can use `getaccountinfo` to check that:  

```
qtum-cli --regtest getaccountinfo 4d8241c63e8d213d7d3b587af391a906ab6948d2
```

Result should be:

```
{
  "address": "4d8241c63e8d213d7d3b587af391a906ab6948d2",
  "balance": 0,
  "storage": {
    "290decd9548b62a8d60345a988386fc84ba6bc95484008f6362f93160ef3e563": {
      "0000000000000000000000000000000000000000000000000000000000000000": "0000000000000000000000008888888888888888888888888888888888888888"
    }
  },
  "code": "60606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af40351461005c5780632bcf51b41461008a5780635e01eb5a14610094578063893d20e8146100e6575bfe5b610088600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610138565b005b61009261017d565b005b341561009c57fe5b6100a46101c1565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100ee57fe5b6100f66101ca565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b80600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a72305820791895230daae0ed58cc374ad5b639044408f1942d7b85689a616caee50dc42e0029"
}
```
* Please note that the address and other contract results data will be different on each test, change the values accordingly.


### Test 2 - Send message and funds to contract (Expected results):

Step 1 should result in `sendtocontract` returning contract call info for example:

```
{
  "txid": "a3f08482e4b82a8a7ac8463879b8474f8cde254454f52b2269089e165472fb14",
  "sender": "qe4qM2q7JoVLbNgkPmH3EGvndLBnNRrGoP",
  "hash160": "e0f470cb89c0f88942ff9771cfbe72f468441c17"
}

```
Step 2 should result in the call being executed and contract `owner` variable set to `9999999999999999999999999999999999999999` and the contract's balance to be `100 QTUM` (10000000000 QTUM satoshis).

We can use `getaccountinfo` to check that:  

```
qtum-cli --regtest getaccountinfo 4d8241c63e8d213d7d3b587af391a906ab6948d2
```

Result should be:

```
{
  "address": "4d8241c63e8d213d7d3b587af391a906ab6948d2",
  "balance": 10000000000,
  "storage": {
    "290decd9548b62a8d60345a988386fc84ba6bc95484008f6362f93160ef3e563": {
      "0000000000000000000000000000000000000000000000000000000000000000": "0000000000000000000000009999999999999999999999999999999999999999"
    }
  },
  "code": "60606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af40351461005c5780632bcf51b41461008a5780635e01eb5a14610094578063893d20e8146100e6575bfe5b610088600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610138565b005b61009261017d565b005b341561009c57fe5b6100a46101c1565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100ee57fe5b6100f66101ca565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b80600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a72305820791895230daae0ed58cc374ad5b639044408f1942d7b85689a616caee50dc42e0029"
}
```
* Please note that the address and other contract results data will be different on each test, change the values accordingly.

### Test 3 - Send message and funds to contract and specify a sender address (Expected results):


Step 3 should result in `sendtocontract` returning contract call info with the sender being `qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT` (hash160: `baede766c1a4844efea1326fffc5cd8c1c3e42ae`) for example:

```
{
  "txid": "97bc9e6a4b8b47b9baaf7b0e883bc89c08d51f5269d6df0b8743c0de6facf23b",
  "sender": "qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT",
  "hash160": "baede766c1a4844efea1326fffc5cd8c1c3e42ae"
}

```
Step 4 should result in the call being executed and contract `owner` variable set to `qabmqZk3re5b9UpUcznxDkCnCsnKdmPktT` (hash160: `baede766c1a4844efea1326fffc5cd8c1c3e42ae`) and the contract's balance to be `200 QTUM` (20000000000 QTUM satoshis), as previous balance from test #2 was `100 QTUM`.

We can use `getaccountinfo` to check that:  

```
qtum-cli --regtest getaccountinfo 4d8241c63e8d213d7d3b587af391a906ab6948d2
```

Result should be:

```
{
  "address": "4d8241c63e8d213d7d3b587af391a906ab6948d2",
  "balance": 20000000000,
  "storage": {
    "290decd9548b62a8d60345a988386fc84ba6bc95484008f6362f93160ef3e563": {
      "0000000000000000000000000000000000000000000000000000000000000000": "000000000000000000000000baede766c1a4844efea1326fffc5cd8c1c3e42ae"
    }
  },
  "code": "60606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806313af40351461005c5780632bcf51b41461008a5780635e01eb5a14610094578063893d20e8146100e6575bfe5b610088600480803573ffffffffffffffffffffffffffffffffffffffff16906020019091905050610138565b005b61009261017d565b005b341561009c57fe5b6100a46101c1565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b34156100ee57fe5b6100f66101ca565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b80600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b50565b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505b565b60003390505b90565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1690505b905600a165627a7a72305820791895230daae0ed58cc374ad5b639044408f1942d7b85689a616caee50dc42e0029"
}
```
* Please note that the address and other contract results data will be different on each test, change the values accordingly.

### Test 4 - No broadcast (Expected results):

step 2 should result in getting the raw transaction data for the contract call:

```
{
  "raw transaction": "03000000023f8ecf08571f6ee33e7de17a38ebbda874b5f9d57adfd5dde60cb15182cb7420000000006a473044022071b1752464eae0736d78eafe6dceeb4ca1c8c000a3207a8a178d0f1518e46afb0220211f5588286020a19a88f7da8d5d32abce6ba1283d3b17f3632876bc34007741012103cedbc3099b504b72b36b8d1898bf249e1e64712728faba5fef929a1a37f72455feffffff0d68e3b68946c583389b802e4b81f02c8842ec2f3f19028f0efd6d05c0157c4c000000006a473044022000e28505cf652ae0cd91518496163111c6c2ce4ab9b14ee2d72cdd3936befddf02200646fa1598d974b3dfef23a6f38f8c3a5559713ba19940cea70b4557ab1795080121034ef47035d13b543ca24a4c6d11dc35b300b5ae8867ecc909da0b445f17a5df4bfeffffff0290759900cd0100001976a914e58e7fe7a06365daf454a279588249d2b8f1bb4988ac00e40b540200000023010102102702e803042bcf51b4144d8241c63e8d213d7d3b587af391a906ab6948d2c24f00000000000000"
}

```

Decoding the raw data using:

```
qtum-cli --regtest decoderawtransaction 03000000023f8ecf08571f6ee33e7de17a38ebbda874b5f9d57adfd5dde60cb15182cb7420000000006a473044022071b1752464eae0736d78eafe6dceeb4ca1c8c000a3207a8a178d0f1518e46afb0220211f5588286020a19a88f7da8d5d32abce6ba1283d3b17f3632876bc34007741012103cedbc3099b504b72b36b8d1898bf249e1e64712728faba5fef929a1a37f72455feffffff0d68e3b68946c583389b802e4b81f02c8842ec2f3f19028f0efd6d05c0157c4c000000006a473044022000e28505cf652ae0cd91518496163111c6c2ce4ab9b14ee2d72cdd3936befddf02200646fa1598d974b3dfef23a6f38f8c3a5559713ba19940cea70b4557ab1795080121034ef47035d13b543ca24a4c6d11dc35b300b5ae8867ecc909da0b445f17a5df4bfeffffff0290759900cd0100001976a914e58e7fe7a06365daf454a279588249d2b8f1bb4988ac00e40b540200000023010102102702e803042bcf51b4144d8241c63e8d213d7d3b587af391a906ab6948d2c24f00000000000000
```

Should result in a valid OP_CALL transaction:

```
{
  "txid": "f0dd8c504768f9b7ee4145f8fbfe411dff283f47ab4ff7ef6511bf60abd67db9",
  "hash": "f0dd8c504768f9b7ee4145f8fbfe411dff283f47ab4ff7ef6511bf60abd67db9",
  "size": 386,
  "vsize": 386,
  "version": 3,
  "locktime": 79,
  "vin": [
    {
      "txid": "2074cb8251b10ce6ddd5df7ad5f9b574a8bdeb387ae17d3ee36e1f5708cf8e3f",
      "vout": 0,
      "scriptSig": {
        "asm": "3044022071b1752464eae0736d78eafe6dceeb4ca1c8c000a3207a8a178d0f1518e46afb0220211f5588286020a19a88f7da8d5d32abce6ba1283d3b17f3632876bc34007741[ALL] 03cedbc3099b504b72b36b8d1898bf249e1e64712728faba5fef929a1a37f72455",
        "hex": "473044022071b1752464eae0736d78eafe6dceeb4ca1c8c000a3207a8a178d0f1518e46afb0220211f5588286020a19a88f7da8d5d32abce6ba1283d3b17f3632876bc34007741012103cedbc3099b504b72b36b8d1898bf249e1e64712728faba5fef929a1a37f72455"
      },
      "sequence": 4294967294
    }, 
    {
      "txid": "4c7c15c0056dfd0e8f02193f2fec42882cf0814b2e809b3883c54689b6e3680d",
      "vout": 0,
      "scriptSig": {
        "asm": "3044022000e28505cf652ae0cd91518496163111c6c2ce4ab9b14ee2d72cdd3936befddf02200646fa1598d974b3dfef23a6f38f8c3a5559713ba19940cea70b4557ab179508[ALL] 034ef47035d13b543ca24a4c6d11dc35b300b5ae8867ecc909da0b445f17a5df4b",
        "hex": "473044022000e28505cf652ae0cd91518496163111c6c2ce4ab9b14ee2d72cdd3936befddf02200646fa1598d974b3dfef23a6f38f8c3a5559713ba19940cea70b4557ab1795080121034ef47035d13b543ca24a4c6d11dc35b300b5ae8867ecc909da0b445f17a5df4b"
      },
      "sequence": 4294967294
    }
  ],
  "vout": [
    {
      "value": 19799.89980560,
      "n": 0,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 e58e7fe7a06365daf454a279588249d2b8f1bb49 OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a914e58e7fe7a06365daf454a279588249d2b8f1bb4988ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "qeVAc1Rsfb8R45H6a3gqHXcjG8tKESAjAw"
        ]
      }
    }, 
    {
      "value": 100.00000000,
      "n": 1,
      "scriptPubKey": {
        "asm": "1 10000 1000 -877776683 4d8241c63e8d213d7d3b587af391a906ab6948d2 OP_CALL",
        "hex": "010102102702e803042bcf51b4144d8241c63e8d213d7d3b587af391a906ab6948d2c2",
        "type": "call"
      }
    }
  ]
}

```


## References

createcontract ITD: <https://github.com/qtumproject/qtum-itds/blob/74143f9db76ef00133ab3e8ddd2c4ffa009bf447/evm/createcontract%20rpc%20call.md>

Ethereum Contract ABI: <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>

