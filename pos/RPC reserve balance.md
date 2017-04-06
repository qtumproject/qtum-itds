# RPC calls for Reserve Balance

Check RPC calls for Reserve Balance with different values

## Environment

Connect to the network and have enough matured coins.

## Test Steps

 1. Start `qtum-cli`
 2. Call `reservebalance false`
 3. Call `reservebalance false 0`
 4. Call `reservebalance false <value greater than 0>`
 5. Call `reservebalance true 0`
 6. Call `reservebalance true <value greater than 0>`
 7. Call `reservebalance`

## Expected Results

Steps 2, 3, 4 and 5 should result in Reserve balance set to 0.  
Step 6 should result in Reserve Balance set to `<value>`  
Step 7 should result in only showing Reserve Balance.
