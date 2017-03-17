# PoS Block Removed Form Top

Check the wallet when PoS block was removed from top of the chain due to block with bigger work.

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS.

## Test Steps

 1. Start `qtum-qt`
 2. The GUI is displayed
 3. Mine some PoS blocks
 4. Check that the Overview screen contains label for coins involved into the staking
 5. Check that the coins involved into staking is subtracted from the available coins
 6. Wait until mined PoS block is removed from the top due to some user have block with more work (can happen sometimes)
 7. The coins involved for staking is returned to the available coins if the block was not accepted

## Expected Results

Step 7 should result in returning the staked coins into available but not getting the reward for the rejected block.