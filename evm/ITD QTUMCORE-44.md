QTUMCORE-44
Check hashStateRoot to CBlockHeader.

## Environment

QTUMCORE-44. Add hashStateRoot to CBlockHeader
Before running the test make sure that:
Node started –regtest
Bitcoin tests passed

## Test Steps

Test 1:
1. Run getblockhash 0.
2. Run getblock 'hash' (hash from p.1).

Test 2:
1. Run generate 150.
2. Run getblockhash 150.
3. Run getblock 'hash' (hash from p.2).
4. Create contract.
5. Run generate 1.
6. Run getblockhash 151.
7. Run getblock 'hash' (hash from p.6).

Test 3:
1. Run generate 150.
2. Run getblockhash 150.
3. Run getblock 'hash' (hash from p.2).
4. Create contract.
5. Run generate 1.
6. Run getblockhash 151.
7. Run getblock 'hash' (hash from p.6).
8. Run node.
9. Run node.
10. Run generate 1.
11. Run getblockhash 152.
12. Run getblock 'hash' (hash from p.11).

## Expected Results

Test 1:
1. Received json message for genesis block that contains fiels hashStateRoot != 0;

Test 2:
1. hashStateRoot in block 150 != hashStateRoot in block 151.

Test 3:
1. hashStateRoot in block 152 = hashStateRoot in block 151.
