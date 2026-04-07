# ExoHash Protocol

Cosmos L1 blockchain for provably fair gaming. Unbiasable BLS threshold randomness, permissionless USDC bankrolls, on-chain WASM game engines.

## Status

The protocol source code will be published at mainnet launch. Until then, the full development toolkit is available for game and UI developers.

## Architecture

- **BLS Threshold Beacon** — validators run Distributed Key Generation every epoch. Each block, they produce a unique, deterministic, unbiasable 32-byte random seed via threshold signature aggregation.
- **WASM Game Engines** — game logic compiled to WebAssembly, deployed on-chain with one transaction. The chain executes games deterministically using beacon randomness.
- **Permissionless Bankrolls** — anyone creates a USDC liquidity pool, attaches games, and earns house edge. LPs deposit and withdraw freely.
- **Dual Token Economy** — EXOH for governance and staking (earns 70% of protocol fees). USDC for gaming and LP deposits. GAS for transaction fees.

## For Developers

Start building games and UIs now with the DevKit:

```bash
git clone https://github.com/exohash-labs/exohash-devkit
cd exohash-devkit
go run ./cmd/mock-bff &
go run ./cmd/bot-runner &
cd demo && npm install && npm run dev
```

Three commands. Working local casino with dice, crash, mines, and live bots.

**[exohash-devkit](https://github.com/exohash-labs/exohash-devkit)** — chain simulator, 3 reference games, React UI hooks, bot framework, and full developer documentation.

## Links

- [exohash.io](https://exohash.io)
- [DevKit](https://github.com/exohash-labs/exohash-devkit)
- [Discord](https://t.co/0bYTwICtzp)
- [FAQ](https://exohash.io/faq)

## License

The protocol will be released under a source-available license at mainnet.
