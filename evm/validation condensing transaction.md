## Validation of correctness of condensing transaction.

After adding to the block a condensing transaction by miner there is a risk of vouts replacement.
As a result: 
1. Users' balances can be lost.
2. Data in dbUTXO and dbState will be corrupted.

## Environment

Modifying miner the way that all balances from the condensing transaction are sent to miner.

## Test Steps

1. Create contract:
```
contract Temp {
    function () payable {}
}
```
2. Mining created block.
3. Adding balances to created contract.
4. Mining created block.

## Expected Results

1. Block not accepted.
