# Test Title

QTUM Address for new contracts

## Environment

QTUNCORE-3 Story. Test that qtum address is generated after contract creation. Before running the test make sure that: 
Node started в –regtest
Bitcoin tests passed

## Test Steps

1. Create transaction TX_DEPLOYMENT in bitcoinlib-js, run decoderawtransaction.
2. Create transaction TX_ASSIGN_SC in bitcoinlib-js, run decoderawtransaction.
3. Create transaction TX_DEPLOYMENT in bitcoinlib-js, run sendrawtransaction.
3. Create transaction TX_ASSIGN_SC in bitcoinlib-js, run sendrawtransaction.

## Expected Results

1. Transaction marked as deployment.
2. Transaction marked as assign.
3. Contract created (address and balance are displayed on the console).
4. Contract state changed (address and balance are displayed on the console).
