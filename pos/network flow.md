# Network flow

Check that the nodes are synchronized and are connect to the network. No banned nodes.

## Environment

Set-up new network and connect nodes to the network.

## Test Steps

1. Set-up a pool of miners and add nodes `qtum-cli addnode <IP Address> add`
2. Start `qtumd` and do mining for some time (one, two days ...)
3. Check that there is no banned nodes, `qtum-cli listbanned` returns empty list

## Expected Results

Step 3 should result in no banned nodes in any of the miners.


