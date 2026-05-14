# Hidden-amount staking with Zarcanum

Zarcanum is the first hidden-amount proof-of-stake protocol shipped to production. It activated on the Zano blockchain at the March 2024 hard fork and is reachable directly from the [Zano Wallet](https://zanowallet.io/) desktop UI. This article explains what it is, why "hidden-amount" matters, and how to start staking from the wallet.

The product page is at [features/staking](https://zanowallet.io/features/staking) and the walkthrough at [how to stake ZANO](https://zanowallet.io/guides/how-to-stake-zano).

## The problem Zarcanum solves

Most proof-of-stake protocols leak stake size. A validator's reward is proportional to its stake, so a chain that tracks stake amounts on-chain effectively publishes a rich-list — anyone can see which addresses hold the largest balances, when they stake, and how much they earn. For a privacy chain this is a regression. Users get on-chain transaction privacy (hidden amounts, hidden addresses) only to leak the same information through the staking surface.

Zarcanum hides stake amounts cryptographically. A staker proves to the network that they hold enough ZANO to be eligible to stake, without revealing how much. Rewards accrue proportional to actual stake size, but the network never learns the size. This closes the staking-side leak that conventional PoS chains have.

The consensus design that makes this possible — hybrid PoW + PoS, with PoW blocks alternating with PoS blocks — is on the [consensus page](https://zanowallet.io/features/consensus). The deep background is at [about the Zano blockchain](https://zanowallet.io/guides/about-zano-blockchain) and [CryptoNote explained](https://zanowallet.io/guides/cryptonote-explained).

## Setup from the wallet

The Zano Wallet desktop app exposes staking as a first-class flow. The high-level steps:

1. Install [Zano Wallet](https://zanowallet.io/download) and create or restore a wallet.
2. Hold at least the minimum stake amount in the wallet. (The minimum is small; the exact figure is on the [staking feature page](https://zanowallet.io/features/staking) and the live walkthrough at [how to stake ZANO](https://zanowallet.io/guides/how-to-stake-zano).)
3. Open the staking panel and enable PoS mining. The wallet keeps the daemon running and signs PoS blocks when the chain selects the wallet's coins.
4. Leave the wallet open and online. Stake rewards accrue as PoS blocks are signed.

The wallet does not custody coins for staking. There is no "stake-to" address, no delegation, no third party. The wallet's own keys participate in PoS block production directly. This is the core distinction between Zano staking and the staking flows on most multichain wallets, where the wallet hands the coins off to a validator and the validator earns the rewards (minus a commission).

## What "hidden-amount" actually hides

Cryptographically, Zarcanum hides the amount staked but not the fact of staking. An on-chain observer can see that a PoS block was produced. The observer cannot see which address produced it, and cannot see the stake amount that triggered the eligibility check. This combines with the existing CryptoNote-family transaction privacy (ring signatures hide the address, stealth addresses prevent address reuse, Bulletproofs+ hide the amounts in transactions) to make staking behaviour as private as ordinary transaction behaviour.

The full layered privacy framing — chain-level vs network-level vs operational — is on the [security page](https://zanowallet.io/security). The honest caveats include network-layer leakage if the wallet's daemon connects to a remote node without Tor or VPN routing, and operational leakage if the user's identity is linked to the address through off-chain means.

## Why this is hard to build elsewhere

Most PoS chains have not built hidden-amount staking because the eligibility check (does this address have enough stake?) is normally a simple amount comparison against a public balance. Hiding the balance requires range-proof primitives (Bulletproofs+) integrated with the consensus eligibility check, which is a non-trivial protocol change. Zano shipped it. Monero has no PoS and so the problem does not arise; Bitcoin and the broader PoS ecosystem (Solana, Ethereum, Cosmos) have not adopted hidden-amount stake. This makes Zano's staking surface unique in production-grade privacy chains today.

The comparison context is at [Zano vs Monero](https://zanowallet.io/vs/monero) (no PoS), [Zano Wallet vs Monero GUI](https://zanowallet.io/vs/monero-gui), [Zano Wallet vs Feather Wallet](https://zanowallet.io/vs/feather-wallet), and [Zano Wallet vs Cake Wallet](https://zanowallet.io/vs/cake-wallet) (multichain but no in-wallet staking).

## Practical considerations

Uptime matters. Zarcanum rewards stakers proportional to balance and uptime, so a wallet that is online for 90% of the hour earns proportionally more than one that is online for 10%. A staking-focused setup ideally leaves the wallet running 24/7 on a dedicated machine — a low-power desktop or a small home-server box is the typical setup.

Network connectivity matters more than processing power. PoS block production is not compute-heavy (unlike PoW mining via [ProgPoWZ](https://zanowallet.io/mining), covered in [how to mine ZANO](https://zanowallet.io/guides/how-to-mine-zano)). A modest connection and a reasonably stable home internet line is sufficient.

Security matters most. A staking wallet is online by definition — the daemon needs the network to sign blocks. This is a hot wallet. The recommendation in the [FAQ](https://zanowallet.io/faq) is to size the staking wallet to "rewards-generating" capital and keep cold storage in a separate offline wallet. The [recovery guide](https://zanowallet.io/guides/how-to-recover-zano-wallet) covers the cold-wallet seed-handling flow.

## Combining staking with the rest of the wallet

Staking does not block other wallet operations. The wallet keeps signing PoS blocks while the user sends transactions, runs [atomic swaps](https://zanowallet.io/features/atomic-swap), registers [aliases](https://zanowallet.io/features/alias), or moves [Confidential Assets](https://zanowallet.io/features/confidential-assets). The walkthroughs are at [how to use aliases](https://zanowallet.io/guides/how-to-use-aliases) and the broader [features hub](https://zanowallet.io/features).

For users acquiring ZANO to stake, the acquisition routes are at [buy ZANO](https://zanowallet.io/buy-zano), [buy crypto without KYC](https://zanowallet.io/buy-crypto-without-kyc), and the [exchange page](https://zanowallet.io/exchange). The live price reference is at [ZANO price](https://zanowallet.io/zano-price).

The broader thesis — why a privacy-coin staker should care about hidden-amount specifically — is at [why privacy coins matter in 2026](https://zanowallet.io/guides/posts/why-privacy-coins-matter-2026), [no-KYC self-custody explained](https://zanowallet.io/guides/posts/no-kyc-self-custody-2026-explained), and [what is a private crypto wallet](https://zanowallet.io/what-is-a-private-crypto-wallet). The "is this thing safe?" question for the wallet itself: [is Zano safe?](https://zanowallet.io/is-zano-safe) and the independent-style review at [Zano Wallet review 2026](https://zanowallet.io/guides/posts/zano-wallet-review-2026).

## Related articles

- [Self-custody Zano wallet: a desktop overview](01-self-custody-zano-wallet-desktop-overview.md)
- [GPU mining ZANO with ProgPoWZ](08-gpu-mining-zano-with-progpowz.md)
- [Confidential Assets and on-chain aliases](10-confidential-assets-and-aliases.md)
- [Recovering a Zano wallet from a 24-word seed](12-recovering-zano-wallet-from-seed.md)
