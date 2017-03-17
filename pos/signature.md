# PoW Signature

Check the signature in PoW blocks. Story *QTUMCORE-34*

## Environment

Connect to the network and mine blocks.

## TEST STEPS

1. Start `qtumd`
2. Get PoW block `qtum-cli getblock <block hash>`
3. Check if there is `signature`

## Expected Results

Step 2 should result in showing block info.
Step 3 should show that `signature` field does not exist in PoW blocks.

---
# PoS Signature

Check the signature in PoS blocks. Story *QTUMCORE-34*

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS and some PoS blocks are mined.

## TEST STEPS

1. Start `qtumd`
2. Get PoS block `qtum-cli getblock <block hash>`
3. Check that there is `signature`

## Expected Results

Step 2 should result in showing block info.  
Step 3 should show that `signature` field exist in PoS blocks.
