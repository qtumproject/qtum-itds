# MAX Amount of coins

Check the maximum amount of coins that will be ever generated is not limited to 21,000,000. Story *QTUMCORE-31*.

## Environment

Set-up private Quantum network for testing. Change the parameters of the network if needed in order to simulate the check.

## Test Steps

1. Start `qtumd`
2. Mining more than 2100 PoW blocks with reward of 10,000
3. Get the balance `qtum-cli getbalance`
4. Check that the balance is more then 21,000,000 which is the limit for Bitcoin

## Expected Results

Step 3 should result in balance bigger then 21,000,000.

