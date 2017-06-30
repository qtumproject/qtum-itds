# Qt wallet staking icon and info

Check the GUI info about staking.

## Environment

Connect to the network and mine blocks.

## Test Steps

1. Start `qtum-qt` on one node with enabled staking, `staking=1` in `qtum.conf` - file or with command-line argument `--staking`.
2. Check the Staking icon and icon-tooltip for staking information.
3. Start `qtum-qt` on two more nodes, connect them and check Staking icon and icon-tooltip again.
4. Start with generating initial PoW blocks and after finished check Staking icon and icon-tooltip.
5. Lock the wallet(go to Settings->Encrypt wallet and enter passphrase to encrypt and lock the wallet) and check Staking icon and tooltip.
6. Stop `qtum-qt` on one node and let the other to stake for some time (more is better).
7. Start the `qtum-qt` again and check the Staking icon and tooltip while node is syncing.

## Expected Results

Step 2 should result in showing staking_off icon with tooltip "Not staking because wallet is offline"  
Step 3 should result in showing staking_off icon with tooltip "Not staking because you don't have mature coins"  
Step 4 should result in showing staking_on icon with tooltip with info about Staking as: your weight, network weight and expected time to earn reward.  
Step 5 should result in showing staking_off icon with tooltip "Not staking because wallet is locked"  
Step 7 should result in showing staking_off icon with tooltip "Not staking because wallet is syncing"  

##Note
Between steps wait for up to 30 seconds for GUI update.
