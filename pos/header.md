# PoW Header

Check block's header for PoW-blocks.

## Environment

PoW blocks for checking need to be mined.

## Test Steps

1. Start `qtumd`
2. Get some PoW block's header `qtum-cli getblockheader <block hash>`
3. Get the `nonce` field
4. Get the `flags` field

## Expected Results

Step 2 should result in showing the block's header.
Step 3 should confirm that there is `nonce` field that is greater than 0, usually some big number.
Step 4 should result in  `flags = proof-of-work` to confirm that the block is PoW- block.

---
# PoS Header

Check block's header for PoS-blocks.

## Environment

PoS blocks for checking need to be mined.

## Test Steps

1. Start `qtumd`
2. Get PoS block's header `qtum-cli getblockheader <block hash>`
3. Get the `nonce`  field
4. Get the `flags` field 
5. Get the `entropybit` and `modifier` exist

## Expected Results

Step 2 should result in showing the block's header.  
Step 3 should result in `nonce = 0` because `nonce` is not used.  
Step 4 should result in `flags = proof-of-stake` to confirm that the block is PoS-block.  
Step 5 should show that `entropybit` and `modifier` exist in PoS-block.
