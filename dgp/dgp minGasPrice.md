## Validation of dynamic minGasPrice changes.

If DGP contract doesn't contain contract addresses with minGasPrice, use default value.

## Environment
For minGasPrice management use reserved contract address 0000000000000000000000000000000000000082.

## Test Steps
1. Generate 50 blocks.
2. Create contract (createcontract "code" 500000 0.00000001):
```
contract testDGP{
    uint a1 = 100;
    uint a2 = 100;
    uint a3 = 100;
    uint a4 = 100;
    uint a5 = 100;
    function() payable{}
}
```
3. Mine the block.
4. Call getblock "block hash from step 3".
5. Call method setInitialAdmin(), contract 0000000000000000000000000000000000000082.
6. Mine the block.
7. Create contract:
```
contract minGasPrice{
  uint32[1] _minGasPrice=[
    10 //min gas price in satoshis
  ];
  function getMinGasPrice() constant returns(uint32[1] _gasPrice){
    return _minGasPrice;
  }
}
```
8. Mine the block.
9. Call method addAddressProposal("contract address from step 7", "2").
10. Mine two blocks.
11. Create contract from step 2 (createcontract "code" 500000 0.00000001).
12. Mine the block.
13. Call getblock "block hash from step 12".
14. Create contract:
```
contract minGasPrice{
  uint32[1] _minGasPrice=[
    1 //min gas price in satoshis
  ];
  function getMinGasPrice() constant returns(uint32[1] _gasPrice){
    return _minGasPrice;
  }
}
```
15. Mine the block.
16. Call method addAddressProposal("contract address from step 14", "2").
17. Mine two blocks.
18. Create contract from step 2 (createcontract "code" 500000 0.00000001).
19. Mine the block.
20. Call getblock "block hash from step 19".

## Expected Results
1. In step 4 transaction was added to the block (OP_CREATE) (minGasPrice = 1). 
2. In step 13 transaction was NOT added to the block (OP_CREATE) (minGasPrice = 10).
3. In step 20 transaction was added to the block (OP_CREATE) (minGasPrice = 1).
