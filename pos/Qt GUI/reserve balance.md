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

---
# Reserve balance configuration

There are 3 ways for setting the reserve balance:

1. Set the reserve balance in aplication configuration. Set in `qtum.conf` `reservebalance=<value>` or during starting the deamon `qtum-qt -reservebalance=<value>`
2. Set the reserve balance in `optionsdialog`
3. Set the reserve balance using RPC call. `reservebalance true <value>`

The sources for the reserve balance are defined above in the list with the respected priority. The test will check different sources that will be used to get the reserve balance.

## Environment

The test can be executed in any network.

## Test Steps

 1. Start`qtum-qt -reservebalance=500`
 2. Check that the reserve balance in `optionsdialog` and RCP call `reservebalance` is 500
 3. Change the reserve balance in the `optionsdialog` or the RPC call `reservebalance true 1000` to 1000
 4. Restart the Qt Wallet `qtum-qt -reservebalance=500`
 5. Check that the reserve balance in `optionsdialog` and RCP call `reservebalance` is 500
 6. Start`qtum-qt`
 7. Check that the reserve balance in `optionsdialog` and RCP call `reservebalance` is 0
 8. Change the reserve balance in the `optionsdialog` to 1000 
 9. Restart the Qt Wallet `qtum-qt`
 10. Check that the reserve balance in `optionsdialog` and RCP call `reservebalance` is 1000
 11. Change the reserve balance using the RPC call `reservebalance true 500` to 500
 12. Restart the Qt Wallet `qtum-qt`
 13. Check that the reserve balance in `optionsdialog` and RCP call `reservebalance` is 1000

## Expected Results

Step 5 should result in confirmation that the reserve balance in the application configuration is used for initialization of the wallet.  
Step 10 should result in confirmation that the reserve balance saved in the `optionsdialog` is used for initialization of the wallet due to not presence of application configuration for reserve balance.  
Step 13 should result in confirmation that the reserve balance set using the RPC call `reservebalance` have only in process scope.  

