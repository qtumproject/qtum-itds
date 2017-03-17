# PoS Big Stake

Check that if the stake is more than 200 coins the output contain two entries. Story *QTUMCORE-35*.

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS and some PoS-blocks are mined.

## Test Steps

1.  Start `qtumd`
2.  Find mined PoS-block
3.  Get the block `qtum-cli getblock <block hash>`
4.  Get the PoS transaction `qtum-cli gettransaction <tx id>`
5.  Get the transaction outputs list.

## Expected Results

Step 3 should result in showing block info.
Step 4 should result in showing transaction info.
Step 5 should show that the output contains 2 entries.

---
# POS Small Stake

Check that if the stake is less than 200 coins the output contain one entry. Story *QTUMCORE-35*.

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS and some PoS-blocks are mined.

## Test Steps

1.  Start `qtumd`
2.  Find some mined block
3.  Get the block `qtum-cli getblock <block hash>`
4.  Get the PoS transaction `qtum-cli gettransaction <tx id>`
5.  Get the transaction outputs list.

## Expected Results

Step 3 should result in showing block info.  
Step 4 should result in showing transaction info.  
Step 5 should show that the output contain 1 entry.
