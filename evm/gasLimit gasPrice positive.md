issue#42
Signedness problem in gas limit/gas price.

## Environment

issue#42.
Before running the test make sure that:
Node started –regtest
Bitcoin tests passed

## Test Steps

Test 1:
1. Create TX_CREATE (gasLimit < 0).
2. Run decoderawtransaction 'tx' (tx from p.1).

Test 2:
1. Create TX_CREATE (gasLimit > 0).
2. Run decoderawtransaction 'tx' (tx from p.1).

Test 3:
1. Create TX_CREATE (gasLimit < 0).
2. Run sendrawtransaction 'tx' (tx from p.1).

Test 4:
1. Create TX_CREATE (gasLimit > 0).
2. Run sendrawtransaction 'tx' (tx from p.1).

## Expected Results

Test 1:
1. Transaction type nonstandard.

Test 2:
1. Transaction type create.

Test 3:
1. Return exception (bad-txns-incorrect-format).

Test 4:
1. Return hash tx.
