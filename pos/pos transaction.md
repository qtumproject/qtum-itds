# PoS Transaction location into block

Check that the PoS transaction is the second transaction into the blocks, the first transaction in case of PoS is empty

## Environment

Connect to the network and have enough matured coins to do staking.

## Test Steps

1. Start `qtumd`
2. Get PoS block `qtum-cli getblock <block hash>`
3. Get that the first transaction `qtum-cli gettransaction <first tx hash>`
4. Get that the second transaction `qtum-cli gettransaction <second tx hash>`

## Expected Results

Step 3 should result in problem due to the first transaction being empty
Step 4 should result in successful operation and reward according to PoS specification

---
# PoS Transaction time

Check that the PoS is younger then the other transactions in the block. Story *QTUMCORE-33*.

## Environment

Connect to the network and have enough matured coins to do staking.

## Test Steps

1. Start `qtumd`
2. Find PoS block with more than 2 transactions that you mined `qtum-cli getblock <block hash>`
3. Get the time from the second transaction in the block (PoS transaction)
4. Get the time for the rest of the transactions
5. Compare the transactions times

## Expected Results

Step 5 should result in confirming that the PoS transaction is younger then the other transactions in the block i.e. have bigger time then the other transactions.


