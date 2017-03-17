# Changing block signature results in invalid block

In PoS blocks, a signature is included to verify that the block was minted by the staking party. This signature should be valid in valid blocks

## Environment

Normal functioning network (any mode) with enough blocks to stake. Requires 3 peers. 

## Test Steps

A is honest node, B is malicious node, C is an honest bystander

1. Change B's code so that block signatures are not validated
2. Mine a new block on A, but do not broadcast it to the network (this can be easily done by mining the block alone, and then deleting the blockchain and resyncing)
2. Take the generated block from A as hex, and change it's signature and/or nonce. (any change should be ok for this test) 
3. Add and broadcast this modified block from node B
4. Add and broadcast the unmodified block from node A

## Expected Results

At step 3, B's block height should be 1 higher than A and C. A and C should put relevant messages in debug.log that indicates the modified block was received, but invalid due to bad signature. 

At step 4, A and C should now have the same block height as B, but they should be on different forks (different tip blockhash)

## References

Any references useful for understanding and creating this test
