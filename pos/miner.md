# PoW Miner

Check the configuration parameter for starting the PoW miner. Story *QTUMCORE-30*.

## Environment

Connect to the network and mine blocks.

## Test Steps

1.  Change in `qtum.conf` add config `gen=1`
2.  Start `qtumd`
3.  Stop `qtum-cli stop`
4.  Change in `qtum.conf` add config `gen=0`
5.  Start `qtumd`

## Expected Results

1. When step 1 is done, step 2 should result in high CPU usage and blocks will be mined.
2. When stem 4 is done, step 5 should result in normal CPU usage and blocks will not be mined.

---
# PoS Miner

Check the configuration parameter for starting the PoS miner. Story *QTUMCORE-30*.

## Environment

Connect to the network and mine blocks. Wait until the consensus protocol is switched to PoS.

## Test Steps

1.  Change in `qtum.conf` add config `staking=1`
2.  Start `qtumd`
3.  Stop `qtum-cli stop`
4.  Change in `qtum.conf` add config `staking=0`
5.  Start `qtumd`

## Expected Results

1. When step 1 is done, step 2 should result in normal CPU usage and blocks will be mined.  
2. When stem 4 is done, step 5 should result in normal CPU usage and blocks will not be mined.
