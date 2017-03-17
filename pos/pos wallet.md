# PoS Wallet Information

Check the wallet information during staking. Story *QTUMCORE-39*

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS.

## Test Steps

 1. Start `qtum-qt`
 2. The GUI is displayed
 3. Mine some PoS blocks
 4. Check the Overview-screen information about stake 
 5. Check that the coins involved into staking are subtracted from the available coins

## Expected Results

Step 4 should result in showing that the Overview-screen contains label for coins involved into the staking.  
Step 5 should result in `stake + available = total`
