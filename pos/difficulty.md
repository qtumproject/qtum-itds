# Difficulty

Check that the difficulty for PoS is not constant. 

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS and some PoS-blocks are mined.

## Test Steps

1. Start `qtumd`
2. Get the PoS difficulty `qtum-cli getdifficulty`
3. Wait until new block is mined
4. Get the PoS difficulty `qtum-cli getdifficulty`
5. Compare the difficulty values

## Expected Results

Step 2 should result in showing the difficulty for PoW and PoS blocks.
Step 3 should mine new block.
Step 4 should result in showing the difficulty after new block is mined.
Step 5 should show that the `difficulty_1 != difficulty_2`