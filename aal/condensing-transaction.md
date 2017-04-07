# Condensing Transaction Behavior

In the initial prototype we had an unsolved DoS problem where an attacker could send a contract many tiny outputs, and then spend them all at once, potentially exceeding the block size limit. 

The solution which solves this is named "condensing transactions". Basically after a contract has completed it's execution, a single UTXO should hold all of it's tokens. 

## Environment

Fairly involved setup process, but can be done on mainnet or regtest


## Test Steps

1. Deploy each Sender contract
2. Use `setSenders` for each contract to set them up to know about each other
3. Mine a block (block B1)
4. Send 8 coins to Sender1, method `share`(Transaction T1)
5. Mine a block (block B2)
6. Send 2 coins to Sender1, method `keep` (Transaction T2)
7. Send 2 coins to Sender1, method `sendAll` (Transaction T3)
8. Mine a block (Block B3)
9. Send 2 coins to Sender2, method share. Send a very low gas limit that causes it to run out of gas (Transaction T4)
10. Call Sender2, method `withdrawAll` (Transaction T5, sender address is A1)
10. Mine a block (Block B4)

## Expected Results

At step 3, all 3 contracts should have a 0 balance and no UTXOs associated with them other than their creation transaction. 

### Block B2

At step 5, block B2 should contain 4 transactions: 

1. Coinbase
2. Stake
3. T1
4. Condensing TX (Transaction C1)

T1 is of course unmodified. 

C1 should contain 1 vin:

* Spend contract send (OP_CALL) in T1 using OP_TXHASH

And 3 vouts:

* C1.V0: version 0 OP_CALL to Sender1 of value 5 (8 initially, -4 for send to Sender2, then +1 for callback from Sender3)
* C1.V1: version 0 OP_CALL to Sender2 of value 2.5 (4 from Sender1, -2 for send to Sender3, then +0.5 for callback from Sender3)
* C1.V2: version 0 OP_CALL to Sender3 of value 0.5 (2 from Sender2, -1 for send to Sender1, -0.5 for send to Sender2)

If you check the balance of these contracts after this block, it should exactly match the outputs of C1. Furthermore, there should NEVER be more than 1 UTXO owned by a contract after contract execution is complete and the condensing tx is complete. This could even be a network rule for safety against hidden attack vectors. 

Now we do the sends for T2 and T3 and mine block C. 

#### Block B3

Block B3 should contain 6 transactions:

1. Coinbase
2. Stake
3. T2
4. Condensing TX C2
5. T3
6. Condensing TX C3


C2 vins:

* T2 contract spend vout spent with OP_TXHASH
* C1.V0 --Sender1's balance

C2 vouts:

* C2.V0: version 0 OP_CALL to Sender1 of value 7 (5 from previous balance, +2 from T2)

C3 vins:

* T3 contract spend vout spent with OP_TXHASH
* C2.V0 -- Sender1's balance
* C1.V1 -- Sender2's balance


C3 vouts:

* C3.V0: version 0 OP_CALL to Sender2 of value 11.5 (2.5 from previous balance, +9 from Sender1's balance plus T3's contract coins)

Note that at this point Sender1 should now have no UTXOs owned by it at all, because it's balance is 0. 

#### Block B4

Now block B4 is mined, it contains these transactions:

* Coinbase
* Stake
* T4
* Refund Transaction R1
* T5
* Condensing Transaction C4

R1 vins:

* T4 contract spend vout spent with OP_TXHASH

R1: vouts:

* R1.V0: pubkeyhash script (back to the first vin of T4) of value 2

C4 vins:

* C3.V0 -- Sender2's balance
* C1.V2 -- Sender3's balance
(no coins sent with T5, so no input for it. We ignore 0-value outputs)

C4 vouts:

* C4.V0: pubkeyhash script for address A1, of value 12 (11.5 from Sender2 balance, +0.5 from withdrawal of Sender3's balance)


Now, at the end of our test, the end result is that all 3 of these contracts should now have a 0 balance and own no UTXOs. 


## Contract code

    pragma solidity ^0.4.0;
    contract Sender1 { 
        Sender2 sender2;
        Sender3 sender3;
        function Sender1() {
        }
        function setSenders(address senderx, address sendery) public{
            sender2=Sender2(senderx);
            sender3=Sender3(sendery);
        }
        function share() public payable{
            if(msg.sender != address(sender3)){
                sender2.share.value(msg.value/2);
            }
        }
        function sendAll() public payable{
            sender2.keep.value(msg.value + this.balance);
        }
        function keep() public payable{
        }
        function() payable { } //always payable
    }
    contract Sender2{ 
        Sender1 sender1;
        Sender3 sender3;
        function Sender2() {
        }
        function setSenders(address senderx, address sendery) public{
            sender1=Sender1(senderx);
            sender3=Sender3(sendery);
        }
        function share() public payable{
            sender3.share.value(msg.value/2);
        }
        function keep() public payable{
        }
        function withdrawAll() public{
            sender3.withdraw();
            msg.sender.send(this.balance);
        }
        function() payable { } //always payable
    }

    contract Sender3 { 
        Sender1 sender1;
        Sender2 sender2;
        function Sender3() {
        }
        function setSenders(address senderx, address sendery) public{
            sender1=Sender1(senderx);
            sender2=Sender2(sendery);
        }
        function share() public payable{
            sender1.share.value(msg.value/2);
            sender2.keep.value(msg.value/4);
        }
        function withdraw() public{
            msg.sender.send(this.balance);
        }
        function() payable { } //always payable
    }





