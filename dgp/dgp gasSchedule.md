## Validation of dynamic gasSchedule changes.
If DGP contract doesn't contain contract addresses with gasSchedule, use EIP158Schedule value.
## Environment
For gasSchedule management use reserved contract address 0000000000000000000000000000000000000080.
## Test Steps
1. Generate 50 blocks.
2. Create contract:
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
4. Call getblock "block hash блока from step 3".
5. Call getrawtransaction hash_coinbasetx.
```
"vout": [
    {
      "value": 20000.00176768,
      "n": 0,
      "scriptPubKey": {
        ...
        "addresses": [
          "qL3i9pEpY9DHEycyA9kwCQQgKscgAwVqt8"
        ]
      }
    }, 
    {
      "value": 0.09828952,
      "n": 1,
      "scriptPubKey": {
        ...
        "addresses": [
          "qKzJ5WzqiurmQo45Nw7fgNELQ5PZnNqB1P"
        ]
      }
    }, 
    ...
  ]
```
6. Note down sum spent.
7. Create contract from step 2. Mine the block.
8. Call getblock "block hash from step 7".
9. Call getrawtransaction hash_coinbasetx.
```
"vout": [
    {
      "value": 20000.00177448,
      "n": 0,
      "scriptPubKey": {
        ...
        "addresses": [
          "qbVmiEkavoKwai93h74AFJoCjXMY5skh7U"
        ]
      }
    }, 
    {
      "value": 0.09828952,
      "n": 1,
      "scriptPubKey": {
        ...
        "addresses": [
          "qakDL5n49cKN88nAo986WrgXGBeAVmy3mV"
        ]
      }
    }, 
    ...
  ]
```
10. Note down sum spent.
11. Create contract from step 2. Mine the block.
12. Call getblock "block hash from step 11".
13. Call getrawtransaction hash_coinbasetx.
```
"vout": [
    {
      "value": 20000.00177448,
      "n": 0,
      "scriptPubKey": {
        ...
        "addresses": [
          "qYcfQCsLYEdz5FfML8PKvG4CbsnubUJUvC"
        ]
      }
    }, 
    {
      "value": 0.09828952,
      "n": 1,
      "scriptPubKey": {
        ...
        "addresses": [
          "qba2Hy8KHKTcCvS3uFBw1XWQUXRydKqRgV"
        ]
      }
    }, 
    ...
  ]
```
14. Call method setInitialAdmin(), contract 0000000000000000000000000000000000000080.
15. Mine the block.
16. Create contract:
```
contract gasSchedule{
uint32[39] _gasSchedule=[
0, //0: tierStepGas0
4, //1: tierStepGas1
5, //2: tierStepGas2
10, //3: tierStepGas3
16, //4: tierStepGas4
20, //5: tierStepGas5
40, //6: tierStepGas6
0, //7: tierStepGas7
20, //8: expGas
100, //9: expByteGas
60, //10: sha3Gas
12, //11: sha3WordGas
400, //12: sloadGas
40000, //13: sstoreSetGas
10000, //14: sstoreResetGas
30000, //15: sstoreRefundGas
2, //16: jumpdestGas
750, //17: logGas
16, //18: logDataGas
750, //19: logTopicGas
64000, //20: createGas
1400, //21: callGas
4600, //22: callStipend
18000, //23: callValueTransferGas
50000, //24: callNewAccountGas
48000, //25: suicideRefundGas
6, //26: memoryGas
1024, //27: quadCoeffDiv
400, //28: createDataGas
42000, //29: txGas
106000, //30: txCreateGas
8, //31: txDataZeroGas
136, //32: txDataNonZeroGas
6, //33: copyGas
1400, //34: extcodesizeGas
1400, //35: extcodecopyGas
800, //36: balanceGas
10000, //37: suicideGas
24576 //38: maxCodeSize
];
function getSchedule() constant returns(uint32[39] _schedule){
    return _gasSchedule;
}
}
```
17. Mine the block.
18. Call method addAddressProposal("contract addresses from points 16", "2").
19. Mine two blocks.
20. Create contract from step 2. Mine the block.
21. Call getblock "block hash from step 20".
22. Call getrawtransaction hash_coinbasetx.
```
"vout": [
    {
      "value": 20000.00348495,
      "n": 0,
      "scriptPubKey": {
        ...
        "addresses": [
          "qWubq7LMpe9vqdRAwYdPEdrwS934wgTkhR"
        ]
      }
    }, 
    {
      "value": 0.09657905,
      "n": 1,
      "scriptPubKey": {
        ...
        "addresses": [
          "qQcSV8rZ6feFpp4JA9SWMQZs9ivEPADkok"
        ]
      }
    }, 
    ...
  ]
```
23. Note down sum spent.
24. Create contract from step 2. Mine the block.
25. Call getblock "block hash from step 24".
26. Call getrawtransaction hash_coinbasetx.
```
"vout": [
    {
      "value": 20000.00348495,
      "n": 0,
      "scriptPubKey": {
        ...
        "addresses": [
          "qX6on5hvXbNqNbrzNa4nqqpzc5J8neYjc5"
        ]
      }
    }, 
    {
      "value": 0.09657905,
      "n": 1,
      "scriptPubKey": {
        ...
        "addresses": [
          "qf4mZ137TnEWWcE4PA8jvW1V7ekmML9kvN"
        ]
      }
    }, 
    ...
  ]
```
27. Note down sum spent.
28. Create contract from step 2. Mine the block.
29. Call getblock "block hash from step 28".
30. Call getrawtransaction hash_coinbasetx.
```
"vout": [
    {
      "value": 20000.00348495,
      "n": 0,
      "scriptPubKey": {
        ...
        "addresses": [
          "qdffNZfKaqTrc7trPeNBgJSXx5GDfmC7va"
        ]
      }
    }, 
    {
      "value": 0.09657905,
      "n": 1,
      "scriptPubKey": {
        ...
        "addresses": [
          "qQbpXAt21j54Ybioh7U9Dqzbb7Pc86MJ2x"
        ]
      }
    }, 
    ...
  ]
```
31. Note down sum spent.
32. Create contract:
```
contract gasSchedule{
uint32[39] _gasSchedule=[
0, //0: tierStepGas0
2, //1: tierStepGas1
3, //2: tierStepGas2
5, //3: tierStepGas3
8, //4: tierStepGas4
10, //5: tierStepGas5
20, //6: tierStepGas6
0, //7: tierStepGas7
10, //8: expGas
50, //9: expByteGas
30, //10: sha3Gas
6, //11: sha3WordGas
200, //12: sloadGas
20000, //13: sstoreSetGas
5000, //14: sstoreResetGas
15000, //15: sstoreRefundGas
1, //16: jumpdestGas
375, //17: logGas
8, //18: logDataGas
375, //19: logTopicGas
32000, //20: createGas
700, //21: callGas
2300, //22: callStipend
9000, //23: callValueTransferGas
25000, //24: callNewAccountGas
24000, //25: suicideRefundGas
3, //26: memoryGas
512, //27: quadCoeffDiv
200, //28: createDataGas
21000, //29: txGas
53000, //30: txCreateGas
4, //31: txDataZeroGas
68, //32: txDataNonZeroGas
3, //33: copyGas
700, //34: extcodesizeGas
700, //35: extcodecopyGas
400, //36: balanceGas
5000, //37: suicideGas
24576 //38: maxCodeSize
];
function getSchedule() constant returns(uint32[39] _schedule){
    return _gasSchedule;
}
}
