# Database Blocks Verification

Verify the blocks and the state are valid for the chain during loading the database.
The test is useful to check errors during saving the chain. Story *QTUMCORE-40*.

## Environment

Connect to the network and synchronize the block-chain.

## Test Steps

1. Make sure that the database is used and have PoW and PoS blocks inside, also transactions were performed
2. Start `qtumd -checklevel=4`
3. The daemon start normally and there are not logs for errors

## Expected Results

Step 3 should result in starting the daemon without problems.

