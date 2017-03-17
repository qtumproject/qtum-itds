# Average Block Spacing

Check the average time for creating block in the network.
`Average Time = ( Time(Block N + X) - Time(Block N) ) / X`

## Environment

Regardless of the starting difficulty for mining the network will adjust the difficulty according to its size. The start and the end block need to be after the network is adjusted. The number of blocks between the start and the end block need to be sufficiently large due to the probabilistic nature, for example: 256 blocks.

## Test Steps

1. Start `qtumd`
2. Get block hash `qtum-cli getblockhash <start block number>` for the starting block
3. Get the block `qtum-cli getblock <block hash>`
4. Save the `mediantime` for the block
5. Get block hash `qtum-cli getblockhash <end block number>` for the starting block
6. Get the block `qtum-cli getblock <block hash>`
7. Save the `mediantime` for the block
8. Subtract the times for the blocks and divide it with the number of blocks between them

## Expected Results

Step 8 should result in average time close to 128 seconds.  
The same result is expected for checks for PoW and PoS consensus protocols.

