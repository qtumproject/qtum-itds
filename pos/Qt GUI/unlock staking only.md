# Qt wallet unlock for staking only

Check unlock for staking only option.

## Environment

Connect to the network and mine blocks.

## Test Steps

1. Start `qtum-qt` on one node with enabled staking, `staking=1` in `qtum.conf` - file or with command-line argument `--staking`.
2. Check the Staking icon and tooltip, for staking information.
3. Send money to address (go to `send dialog` enter address and amount and click `send` - button).
4. Lock the wallet(go to Settings->Encrypt wallet and enter passphrase to encrypt and lock the wallet) and check Staking icon and tooltip.
5. Send money to address (go to `send dialog` enter address and amount and click `send` - button).
6. Unlock wallet for staking only (`Options -> Unlock Wallet`),  enable `For staking only`, enter passphrase and click `Ok` - button.
7. Check the Staking icon and tooltip, for staking information.
8. Send money to address (go to `send dialog` enter address and amount and click `send` - button).

## Expected Results

Step 2 should result in showing staking_on icon with tooltip with info about Staking as: your weight, network weight and expected time to earn reward.  
Step 3 should result in sending money without requiring passphrase.  
Step 4 should result in showing staking_off icon with tooltip "Not staking because wallet is locked".  
Step 5 should result with wallet asking for passphrase to unlock and send money.  
Step 7 should result in showing staking_on icon with tooltip with info about Staking as: your weight, network weight and expected time to earn reward.  
Step 8 should result in wallet asking for passphrase to send money, because it is unlocked only for staking.
