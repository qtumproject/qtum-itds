# Stake modifier change

Check the computation of new stake modifier for the block.

## Environment

Connect to the network and have enough matured coins to do staking.

## Test Steps

1. Start `qtumd`
2. Get the PoS block `qtum-cli getblock <block hash>`
3. Get the next PoS block `qtum-cli getblock <next block hash>`
4. Confirm that the stake modifiers are different

## Expected Results

Step 4 should result in different stake modifier.

