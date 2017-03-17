# PoW block reward

Check the reward for Proof Of Work block.

## Environment

Connect to the network and mine blocks.

## Test Steps

1. Start `qtumd`
2. Get the block `qtum-cli getblock <block hash>` mined with PoW
3. Check that the `flags = proof-of-work`
4. Get the first transaction from the list `qtum-cli gettransaction <tx hash>`
5. Check that the `amount = 10000`

## Expected Results

Step 5 should result in confirmation that the reward for the PoW block is 10000 coins.

---
# PoS block reward

Check the reward for Proof Of Stake block.

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS.

## Test Steps

1. Start `qtumd`
2. Get the block `qtum-cli getblock <block hash>` mined with PoS
3. Check that the `flags = proof-of-stake`
4. Get the second transaction from the list `qtum-cli gettransaction <tx hash>`
5. Check that the `fee = 1`

## Expected Results

Step 5 should result in confirmation that the reward for the PoS block is 1 coins.

