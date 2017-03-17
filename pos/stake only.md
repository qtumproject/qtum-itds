# No hash power

Check that the hash power owned by the miner doesn't affect the staking process.

## Environment

Connect to the network and have enough coins for the staking pool.

## Test Steps

1. Set-up a pool of miners that have different hardware capabilities
2. Send them the same number of coins and wait for the coins to mature
3. Let them do mining for several hours
4. Check that the reward  `qtum-cli getwalletinfo` for all miners

## Expected Results

Step 4 should result in staking capabilities that doesn't depend on the underlining hardware hashing power.

---
# No coins age

Check that the coinage is not taken into account when generating block using the PoS consensus protocol.

## Environment

Connect to the network and have enough matured coins to do staking.

## Test Steps

1. Start `qtumd` and do staking for  some time like 4 hours
2. Record the staking rate `qtum-cli getwalletinfo`
3. Stop `qtum-cli stop` and wait a day
4. Start `qtumd` and do staking for  the same amount of time
5. Record the staking rate `qtum-cli getwalletinfo`
6. Compare the staking rates

## Expected Results

Step 6 should result in staking rates that are similar.

---
# Eco friendly

Check that the staking (PoS) is not CPU intensive so use less energy then mining (PoS)

## Environment

Connect to the network and have enough matured coins to do staking.

## Test Steps

1. Start `qtumd` and do staking
2. Check the CPU usage

## Expected Results

Step 2 should result in CPU usage close to the normal use of the PC without doing staking.

