# Test Title

Add ability for contracts to call other deployed contracts

## Environment

QTUNCORE-10 Story.
Node started в –regtest
Bitcoin tests passed

## Test Steps

Test 1:
1. Create TX_CREATE (OutOfGasBase):
```
var bitcoin = require('bitcoinjs-lib');
var key = bitcoin.ECPair.fromWIF("cVuXgKeCjzXoBv68qGWnfT3neu3Kz7uJUQoDNyWcxVWmbtAdN2bc", bitcoin.networks.testnet);
var tx = new bitcoin.TransactionBuilder(bitcoin.networks.testnet);
tx.addInput("009131715b72ac2ea5358854cfb0c4e65d6301d43a7f127f550f0c079e1de17d", 0);
var version = Buffer.from([0x01]);
var gasLimit = Buffer.from("50c3000000000000", "hex");
var gasPrice = Buffer.from([0x01]);
var opcodes = Buffer.from("606060405234610000575b60ca806100186000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff168063262fd733146046578063f22a195e146060575b6000565b34600057605e60048080359060200190919050506088565b005b34600057606a6098565b60405180826000191660001916815260200191505060405180910390f35b8040600081600019169055505b50565b600054815600a165627a7a72305820d3ac4575c035ec06e50fa73da3017cd40bcd3aca2c13d1cb63e801e9ba1581890029", "hex");
ret = bitcoin.script.compile([
    version,
    gasLimit,
    gasPrice,
    opcodes,
    bitcoin.opcodes.OP_CREATE
]);
tx.addOutput(ret, 13)
tx.sign(0, key);
console.log(tx.build().toHex());
```
2. Open created transaction (sendrawtransaction).
3. Generate 1 block.
4. Run getblock.
5. Run getrawtransaction for transactions from generated block.

Test 2:
1. Create TX_CREATE:
```
var bitcoin = require('bitcoinjs-lib');
var key = bitcoin.ECPair.fromWIF("cVuXgKeCjzXoBv68qGWnfT3neu3Kz7uJUQoDNyWcxVWmbtAdN2bc", bitcoin.networks.testnet);
var tx = new bitcoin.TransactionBuilder(bitcoin.networks.testnet);
tx.addInput("009131715b72ac2ea5358854cfb0c4e65d6301d43a7f127f550f0c079e1de17d", 0);
var version = Buffer.from([0x01]);
var gasLimit = Buffer.from("20a1070000000000", "hex");
var gasPrice = Buffer.from([0x01]);
var opcodes = Buffer.from("606060405234610000575b60ca806100186000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff168063262fd733146046578063f22a195e146060575b6000565b34600057605e60048080359060200190919050506088565b005b34600057606a6098565b60405180826000191660001916815260200191505060405180910390f35b8040600081600019169055505b50565b600054815600a165627a7a72305820d3ac4575c035ec06e50fa73da3017cd40bcd3aca2c13d1cb63e801e9ba1581890029", "hex");
ret = bitcoin.script.compile([
    version,
    gasLimit,
    gasPrice,
    opcodes,
    bitcoin.opcodes.OP_CREATE
]);
tx.addOutput(ret, 0)
tx.sign(0, key);
console.log(tx.build().toHex());
```
2. Open created transaction (sendrawtransaction).
3. Generate 1 block.
4. Run getblock.
5. Run getrawtransaction for transactions from generated block.

Test 3:
1. Create TX_CALL value = 13 (for unexisting address).
2. Open created transacton (sendrawtransaction).
3. Generate 1 block.
4. Run getblock.
5. Run getrawtransaction for transactions from generated block.

Test 4:
1. Create TX_CALL (gasLimit * gasPrice > fee)
2. Open created transaction (sendrawtransaction).

Test 5:
1. Create TX_CALL (non-standart form)
2. Open created transaction (sendrawtransaction).

## Expected Results

Test 1:
1. Contract was not created.
2. Block contains 3 transactions.
3. Miner received all fees.
4. 3rd transaction sent value back to the sender.

Test 2:
1. Contract was created.
2. Block contains 2 transactions.
3. Miner received fee - refundGas.
4. In coinBaseTX 2 vout:
- vout[0] = fee - refundGas (to miner);
- vout[1] = refundGas (sender);

Test 3:
1. Block contains 3 transactions.
2. Miner receiver all fees.
3. 3rd transaction sent value back to the sender.

Test 4:
1. Message received: "bad-txns-fee-notenough".

Test 5:
1. Message received: "bad-txns-callcontract-incorrect".
