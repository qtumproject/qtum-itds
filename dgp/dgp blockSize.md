## Validation of dynamic blockSize changes.

If DGP contract doesn't contain contract addresses with blockSize, use default value.

## Environment
For blockSize management use reserved contract address 0000000000000000000000000000000000000081.

## Test Steps
1. Generate 50 blocks.
2. Create contract (https://github.com/qtumproject/qtum-dgp/blob/master/dgp-template.sol.js).
3. Mine the block.
4. Call getblock "block hash from step 3".
5. Call method setInitialAdmin(), contract 0000000000000000000000000000000000000081.
6. Mine the block.
7. Create contract:
```
contract blockSize{
  uint32[1] _blockSize=[
    12000 //block size in bytes
  ];
  function getBlockSize() constant returns(uint32[1] _size){
    return _blockSize;
  }
}
```
8. Mine the block.
9. Call method addAddressProposal("contract address from step 7", "2").
10. Mine two blocks.
11. Create contract from step 2.
12. Mine the block.
13. Call getblock "block hash from step 12".
14. Create contract:
```
contract blockSize{
  uint32[1] _blockSize=[
    2000000 //block size in bytes
  ];
  function getBlockSize() constant returns(uint32[1] _size){
    return _blockSize;
  }
}
```
15. Mine the block.
16. Call method addAddressProposal("contract address from step 14", "2").
17. Mine two blocks.
18. Create contract from step 2.
19. Mine the block.
20. Call getblock "block hash from step 19".

## Expected Results
1. In step 4 transaction was added to the block (OP_CREATE) (Block size 12601 byte). 
2. In step 13 transaction was NOT added to the block (OP_CREATE) (Max size on this step equals 12000 byte).
3. In step 20 transaction was added to the block (OP_CREATE) (Maz size on this step equals 2000000 byte).
