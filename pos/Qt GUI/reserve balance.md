# Reserve balance

Check if the reservebalance is specified.

## Environment

Set reserve balance with some of the three possible ways:
 1. Set in `qtum.conf` `reservebalance=<value>` before starting `qtum-qt` or
 2. Start  `qtum-qt -reservebalance=<value>` or
 3. Call `reservebalance true <value>` from Console in Help-dialog after starting `qtum-qt`

## Test Steps

 1. Start `qtum-qt`
 2. Check for reserve balance in `optionsdialog`.
  
## Expected Results

Step 2 should result in showing the Reserve Balance.

---
# Staking with Reserve Balance

Check how reservebalance work.

## Environment

Set reserve balance with some of the three possible ways:
 1. Set in `qtum.conf` `reservebalance=<value>` before starting `qtum-qt` or
 2. Start  `qtum-qt -reservebalance=<value>` or
 3. Call `reservebalance true <value>` from Console in Help-dialog after starting `qtum-qt`  
Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS.

## Test Steps

 1. Start `qtum-qt`
 2. Mine blocks (Staking) for some time and watch the value of Available balance in Overview-screen.
 
## Expected Results
 
Step 2 should result in showing that Available Balance is never lower than Reserve Balance.
 
