# MAX Amount of coins

Check the maximum amount of coins that will be ever generated is limited to 107,822,406.2500. Story *QTUMCORE-31*.

## Environment

Set-up private Quantum network for mainnet or testnet. Change the parameters of the network so that blocks can be mined instantly for testing (like regtest, but without the changed regtest halving/PoS schedule)

## Test Steps

1. Start `qtumd`
2. Mine 7.4 million blocks
3. Get the balance `qtum-cli getbalance`

## Expected Results

Step 3 should result in balance of exactly 107,822,406.2500 

