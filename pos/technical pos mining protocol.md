# Technical PoS Mining Protocol

This should cover the technical details of what goes into a PoSv3 block when it is mined. This includes specs for the stake modifier and kernel hash, as well as difficulty adjustments

Note in blackcoin code what we refer to as "stake modifier" is actually "stakeModifierv2", and the same for the PoS kernel

This should be the definitive rules and protocols of PoSv3.

## Environment

Mainnet for difficulty test, other tests are compatible with regtest or testnet

## Test Steps

1. Mine all the PoW blocks
2. Mine the first PoS block (B) with 1 coin immediately after the last PoW block (A)
3. Dump block B's info
4. Mine another PoS block (C) with 2 coins
5. Dump block C's info

## Expected Results

Variables:

* txPrev.nTime - This is the timestamp of the transaction which is referenced by the first vin of the staking transaction
* prevout.hash -  This is the hash of the transaction which is referenced by the first vin of the staking transaction
* prevout.n - This is the vout number which is spent by the first vin of the staking transaction
* nTimeTx - This is the staking transaction's timestamp (which must be equal to the blocktime of this block)
* previousBlockHash - The previous block's hash
* previousStakeModifier - The computed stake modifier for the previous block
* stakeVout.script - The 2nd vout's script in the staking transaction
* coinbaseFlags - The first vin's scriptSig of the first transaction in the block
* coinbase - The first transaction

Rules:

* nTimeTx must match block time exactly
* The staking transaction must be the 2nd transaction in the block (immediately after coinbase)
* stakeVout.script must either be a standard pubkey script, or it's first opcode must be OP_RETURN with a single pushed value that is a valid compressed or uncompressed public key. OP_RETURN with no values pushed afterwards is invalid, as is any other script type including pubkeyhash and P2SH. However, OP_RETURN with more than 1 value pushed is valid, but the values after the public key are not used for consensus in any way.
* Block signature is a script and must be "LowS", meaning that it must be reduced as far as possible and not include extra data or opcodes
* coinbaseFlags must contain the block height in it's scriptSig
* coinbase must have only 1 vin, and 1 empty vout with 0 coins.
* The staking transaction can not contain more than 100 vins
* The first vout of the staking transaction must output 0 coins and have an empty script
* A staking transaction is defined as a transaction that contains at least 1 non-null vin, at least 2 vouts, with the first vout being empty. 
* No other staking transaction can exist in the block

Note that the stake modifier is not signed or hashed as part of the block header. It is displayed in the RPC interface as if it is, but it is not serialized into the block data that is sent over the P2P network. It is computed when adding a new block to the block index in the wallet. 

Block A's info should be calculated as so:

stake modifier: sha256(blockHash << previousStakeModifier)
difficulty: appropriate PoW difficulty
signature: 0 or not displayed
block hash: sha256(nVersion << hashPrevblock << hashMerkleRoot << nTime << nBits << nNonce)
coinbaseFlags: 0 or empty


Block B's info should be capable of being manually calculated as so: 

stake modifier: sha256(prevout.hash << previousStakeModifier
kernel hash: sha256(previousStakeModifier << txPrev.nTime << prevout.hash << prevout.n << nTimeTx)
difficulty: minimum PoS difficulty
reward: 4 coins
block hash: sha256(nVersion << hashPrevblock << hashMerkleRoot << nTime << nBits << nNonce)
signature: a valid signature of the block hash matching the public key in stakeVout.script
coinbaseFlags: block height

Block C's PoS info should be calculated as so:

stake modifier: sha256(prevout.hash << previousStakeModifier)
kernel hash: sha256(previousStakeModifier << txPrev.nTime << prevout.hash << prevout.n << nTimeTx)
difficulty: half of the minimum PoS difficulty
reward: 4 coins
block hash: sha256(nVersion << hashPrevblock << hashMerkleRoot << nTime << nBits << nNonce)
signature: a valid signature of the block hash matching the public key in stakeVout.script
coinbaseFlags: block height


Notes:

* The genesis block's stake modifier is 0.
* Block hash may be slightly different with needed EVM support fields

## References

kernel.cpp in blackcoin
