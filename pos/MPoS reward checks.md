
# MPoS Reward

Check the reward for Mutualized Proof Of Stake block.

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS.

## Test Steps

1. Start `qtumd` and wait to mine at least 500 PoS blocks.
2. After those 500 blocks when you mine block B1, get the PoS transaction to check the reward info:  `getblock <B1 hash>`, `gettransaction <Second(PoS)tx ID>` from B1.
3. Check if "amount" field is `1/10` (from the "fee" field).
4. Wait for next 499 block to be mined.
5. After 500-th block is mined check reward info as explained in step 2 and do it after every block mined until 510.
6. After 510-th block check reward info again.

## Expected Results

In step 3 you should see that the miner don't get the full reward he get only `1/10` (from the mined block subsidy + fee).

Step 5 should result in getting `1/10` from the reward(subsidy + fee) from every block from 500 to 509 ("amount" field = `1/10` from "fee" field)

Step 6 should result in not getting more rewards, because the reward for block B1 is completed (you got `1/10` rewards from 10 blocks).

## Note

This scenario is true in case, the next 9 blocks after B1 are mined by other miners, not by you. If you mine some of the next 9 blocks ex. B2 on height `B1 height + 3`. In blocks on height from `B1 height + 503` to `B1 height + 509` which is equal to `B2 height + 500` to `B2 height + 506` you will have reward `2/10` from that block's rewards(subsidy + fee). In the next blocks until `B2 height + 509` you will get `1/10` from the reward again.  



# MPoS transaction outputs

Check the transaction outputs for Mutualized Proof Of Stake block.

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS.

## Test Steps

1. Start `qtumd` and wait to mine at least 500 PoS blocks.
2. Mine other PoS block and get block hash `qtum-cli getbestblockhash`
3. Get the block `qtum-cli getblock <bestblockhash>`
4. Get the transaction info `qtum-cli gettransaction <PoS transaction hash>`
5. Check transaction outputs.

## Expected Results

Step 5 should result in showing that MPoS transaction have minimum 10 Outputs, first for creator and 9 for the other Stakers.



# MPoS first blocks

Check the first 500 PoS blocks.

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS.

## Test Steps

1. Start `qtumd` 
2. Mine PoS blocks.
3. Check the PoS block reward for random blocks on `height < (nLastPoWBlock + 500)`

## Expected results

Step 3 should show that the block reward for the first 500 PoS blocks is complete, it's not splited between mutual miners.
