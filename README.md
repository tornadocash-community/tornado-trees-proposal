# Tornado.cash trees governance proposal [![Build Status](https://github.com/tornadocash/tornado-trees-proposal/workflows/build/badge.svg)](https://github.com/tornadocash/tornado-trees-proposal/actions)

This repo deploys governance proposal for [TornadoTrees](https://github.com/tornadocash/tornado-trees) update. It significantly reduces the cost of updating tornado merkle trees by offloading onchain updates to zkSNARKs.

## Dependencies

1. node 12
2. yarn

## Init

```bash
$ yarn
$ cp .env.example .env
$ vi .env
```

## testing

```bash
$ yarn changeTreeHeight
$ npx hardhat test
```

## mainnet instructions

### #before proposal creation

1. have you added new instances on the UI?
2. make sure you have set all the ens names for the instances

### #proposal creation

1. go to tornado-tress repo
1. `yarn changeTreeHeight 8`
1. `docker build . -t tornadocash/tornado-trees` you will need 50GB RAM
1. `docker run --rm -it --name tornadoTrees tornadocash/tornado-trees bash` just leave it and go to the next steps
1. `d cp tornadoTrees:/app/artifacts/circuits/* backup`
1. send the backup folder to telegram

1. go to proposal repo
1. add the new instances to the proposal contract
1. `d cp tornadoTrees:/app/artifacts/circuits/BatchTreeUpdateVerifier.sol snarks`
1. `npx hardhat searchParams` and use the output in deployMainnet script
1. `yarn deployMainnet`
1. test the proposal on fork
1. verify on etherscan
1. create proposal on forum to ask community to create the governance proposal

### #during voting

1. make sure you have at least 10 ETH for the root-updater

### #after the proposal execution

#### UI (make it as PR)

1. change tornadoProxy and tornadoTrees addresses
1. turn on estimateGas on deposits
1. use cache events for mining
1. set notification on index page. "Mining is temporally unavailable"

#### root updater

1. go to root-updater repo
1. checkout migration branch
1. edit `.env`
1. run `generateCacheEvents` (uncomment last lines there 1 by 1)
1. docker build . -t tornadocash/tornado-root-updater
1. run both

- `docker run --rm -e MIGRATION_TYPE=deposit tornadocash/tornado-root-updater`
- `docker run --rm -e MIGRATION_TYPE=withdrawal tornadocash/tornado-root-updater`

1. after finish, create the `deposits.json` and `withdrawals.json` cache using `allEvents` from `events.js` and move it to the `snark` branch
1. create the final release on github
