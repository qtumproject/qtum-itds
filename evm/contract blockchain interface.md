# Test Title
Initialization of EVM environment (EnvInfo)
## Environment
QTUNCORE-45 Story.
Node started в –regtest
Bitcoin tests passed
## Test Steps
1. Generate 250 blocks.
2. Create contract:
```
contract BlockHash{
    bytes32 public blockHash;
    function storeBlockHash(uint blockNumber){
        blockHash = block.blockhash(blockNumber);
    }
}
```
3. Call method storeBlockHash with blockNumber = 13 for BlockHash contract.
4. Call blockHash variable for BlockHash contract.
5. Run getblockhash 13.
## Expected Results
1. Hash received = blockHash.
Using same sequence next paramenetrs can be tested:
- block.blockhash(uint blockNumber) returns (bytes32): hash of the given block - only works for 256 most recent blocks excluding current
- block.coinbase (address): current block miner’s address
- block.difficulty (uint): prev block difficulty
- block.gaslimit (uint): current block gaslimit
- block.number (uint): current block number
- block.timestamp (uint): prev block timestamp
