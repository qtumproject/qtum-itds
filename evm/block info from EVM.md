# Block Info From EVM

There should be an interface to access some details about the current block from the EVM. These details in Solidity are tied to the following global variables: (the yellowpaper has the EVM opcodes themselves)

	block.coinbase (address): current block miner’s address
	block.difficulty (uint): current block difficulty
	block.gaslimit (uint): current block gaslimit
	block.number (uint): current block number
	block.timestamp (uint): current block timestamp

Note that we change behavior from Ethereum somewhat in this ITD. In Ethereum you can not access `block.blockhash(block.number)`. It will return 0. However, in Qtum, we instead treat `block.number` as the previous block, and thus allow full access to all data available about it. In Qtum we do not allow access to any data in the block currently being mined (ie, the block the contract is placed in).

Say we have Ethereum and Qtum. Both chains are at block #500. In Ethereum `block.number` would report 500. However, in Qtum, `block.number` would report 499.

The rationale for this behavior change is due to design differences in Ethereum and Qtum. In Ethereum, the standard block time is 15 seconds, but in Qtum the standard blocktime is 2 minutes. During mining/staking, several variables can change during the mining of a block. This includes the timestamp and coinbase. We do not expose the coinbase (yet). With the timestamp being exposed to contracts, this means that when a staker needs to increment the timestamp, they must also process every contract again, to ensure their behavior has not been altered. Thus, this encourages inaccurate timestamps to be put into blocks because keeping accurate timestamps is more work.

## Environment

Normal network in any mode. LOG opcode logging enabled (to see the results). 

## Test Steps

1. Compile the contract in the Code listing using browser-solidity
2. Deploy the contract
3. Send a message (using OP_CALL) to the CallTest method. 
4. Call the contract using off-chain execution to call the BlockLogs method

## Expected Results

Steps 2 and 3 should result in the following results being logged:

    block number: [last block number]
    timestamp: [last block's timestamp]
    difficulty: [last block's difficulty]
    coinbase: 0
    gas limit: [gas limit TBD, should match the block's gas limt]

Note that block number is actually the "last" block. So, if the height of the block containing this contract's execution is 500, the reported block number for this would be 499. All difficulty, timestamp info, etc should for block 499 as well. 

Step 4 should result in the following being logged:

    block number: [current block number]
    timestamp: [current time]
    difficulty: [current block's difficulty]
    coinbase: 0
    gas limit: [gas limit TBD, should match the block's gas limt]

For off-chain execution, the block info should be the current in-progress block. So, if the last block received is at height 500, then the reported block number and all associated info is for the currently in progress block 501. 

## Code

    pragma solidity ^0.4.0;
    contract BlockTest {
       uint num;
       /// Create a new ballot with $(_numProposals) different proposals.
       function BlockTest() {
           CallTest();
       }
       function CallTest() public{
           num++; //not a constant function.. 
           BlockLogs();
       }
       function BlockLogs() constant{
            log1(
               "block number: ",
               uintToBytes(block.number)
               );
           log1(
               "timestamp: ",
               bytes32(block.timestamp)
               );
           log1(
               "difficulty: ",
               bytes32(block.difficulty)
               );
           log1(
               "coinbase: ",
               bytes32(block.coinbase)
               );
           log1(
               "gas limit: ",
               bytes32(block.gaslimit)
               );
       }
       function uintToBytes(uint v) constant returns (bytes32 ret) {
           if (v == 0) {
               ret = '0';
           }
           else {
               while (v > 0) {
                   ret = bytes32(uint(ret) / (2 ** 8));
                   ret |= bytes32(((v % 10) + 48) * 2 ** (8 * 31));
                   v /= 10;
               }
           }
           return ret;
       }
    }
