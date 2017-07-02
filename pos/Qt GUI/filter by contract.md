# Qt wallet filter by contract transaction

Check filter transaction for transactions table.

## Environment

Connect to the network and mine blocks

###Contract code
```
pragma solidity ^0.4.0;   
contract test{
    address[] addresses;
    function fund() payable{} 
    function payRound(){
        addresses.push(msg.sender);
        uint i;
	    for(i=0;i<addresses.length;i++){
		    addresses[i].transfer(100000000);
        }
    }
    }
```

## Test Steps  
1. Start `qtum-qt`.
2. Create test contract `createcontract  6060604052341561000c57fe5b5b6101eb8061001c6000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff168063561a57b214610046578063b60d428814610058575bfe5b341561004e57fe5b610056610062565b005b61006061016b565b005b600060008054806001018281610078919061016e565b916000526020600020900160005b33909190916101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff16021790555050600090505b600080549050811015610167576000818154811015156100e957fe5b906000526020600020900160005b9054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff166108fc6305f5e1009081150290604051809050600060405180830381858888f19350505050151561015957fe5b5b80806001019150506100cd565b5b50565b5b565b81548183558181151161019557818360005260206000209182019101610194919061019a565b5b505050565b6101bc91905b808211156101b85760008160009055506001016101a0565b5090565b905600a165627a7a723058206954f36ed078c77eab4a33db7f37ede2faa27c9794948f7fc6164c0d29b396c10029`.
3. Wait for next block to be mined.
4. Run `sendtocontract <contract address> b60d4288(Fund) 1000` and wait for next block to be mined.
5. Go to transaction page and try filtering transactions by type `Contract send` and `contract receive`.
6. Run `sendtocontract <contract address> 561a57b2 (payRound)`
7. Wait for next block to be mined.
8. Go to transaction page and try filtering transactions by type  `contract receive`.

## Expected Results

In step 5, for chosen type for filter  `Contract send` you should get list of two transactions, one for creating contract and the other for funding contract. For chosen type for filter `contract receive` you should get an empty list because you still don't have any money received from contract.  
Step 8 should result in showing one transaction from contract.

