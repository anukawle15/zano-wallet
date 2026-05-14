# Self-custody Zano wallet: a desktop overview

[Zano Wallet](https://zanowallet.io/) is independent third-party desktop wallet software for the Zano privacy blockchain. The product is published by Zano Wallet LLC (a US limited liability company) and is intentionally narrow — Zano only, desktop only, open source on every release, with no KYC, no signup, and no telemetry. This overview walks through what the wallet does, what it does not do, and where the rest of the documentation lives.

## Form factor and platform support

The wallet is a native desktop application. There is no mobile build, no browser extension, no web wallet, and no custodial fallback. Builds are published for [Windows](https://zanowallet.io/download), macOS, and Linux through the single official [download hub](https://zanowallet.io/download). The current build supports Windows 10 and later, macOS 11 Big Sur and later (both Apple Silicon and Intel), and most modern Linux distributions via AppImage and `.deb`.

If a phone is the primary device, this wallet is not the right tool — the trade-offs are written up in the [desktop vs mobile thesis](https://zanowallet.io/guides/posts/desktop-vs-mobile-crypto-wallet-2026). For a multichain user holding many tokens across many networks, Zano Wallet is also not the right tool — single-chain focus is the explicit product thesis, and Cake Wallet or another multichain wallet is the easier life. The comparisons at [Zano vs Cake Wallet](https://zanowallet.io/vs/cake-wallet), [Zano vs Wasabi](https://zanowallet.io/vs/wasabi), and [Zano vs Monero](https://zanowallet.io/vs/monero) make the trade-offs explicit.

## Self-custody and the seed phrase

Zano Wallet is non-custodial. A 24-word seed phrase is generated locally on first launch, stored locally, and never transmitted. Zano Wallet LLC has no copy of any user's seed phrase and cannot recover one if lost. The recovery flow is documented step-by-step in [how to recover Zano Wallet](https://zanowallet.io/guides/how-to-recover-zano-wallet).

Operationally, this means three things. The seed phrase is the single point of failure: anyone with the 24 words can drain the wallet, and losing the 24 words means losing access permanently. Backup is the user's responsibility. The wallet file on disk is encrypted with a local password but the password protects only against a casual file copy — the seed phrase itself is what authorises a transfer.

The honest privacy framing for a self-custody wallet is on the [security page](https://zanowallet.io/security). Chain-level privacy is strong by default; network-level (IP) privacy is opt-in; operational privacy is the user's responsibility. The [anonymous crypto wallet guide](https://zanowallet.io/anonymous-crypto-wallet-guide) and [privacy crypto wallet guide](https://zanowallet.io/privacy-crypto-wallet-guide) cover the practical setup.

## What the wallet exposes from Zano

The full feature surface of the Zano blockchain is reachable from the desktop UI:

- [Hidden-amount staking](https://zanowallet.io/features/staking) via the Zarcanum protocol. Stake amounts are concealed cryptographically, so stakers earn rewards proportional to balance and uptime without revealing how much ZANO they hold. Walkthrough: [how to stake ZANO](https://zanowallet.io/guides/how-to-stake-zano).
- [Atomic swaps](https://zanowallet.io/features/atomic-swap) via Ionic Swaps. Counterparty-custody-free swaps with Zano Trade integration patterns.
- [On-chain aliases](https://zanowallet.io/features/alias) — register a human-readable `@username` and map it to a wallet address. Setup: [how to use aliases](https://zanowallet.io/guides/how-to-use-aliases).
- [Confidential Assets](https://zanowallet.io/features/confidential-assets) — issue and transact privacy-preserving on-chain tokens that inherit the chain's hidden-amount, hidden-address, hidden-asset guarantees. fUSD is a live private stablecoin built on this primitive.
- [Hybrid PoW + PoS consensus](https://zanowallet.io/features/consensus) — alternating ProgPoWZ proof-of-work blocks and Zarcanum proof-of-stake blocks.
- [GPU mining via ProgPoWZ](https://zanowallet.io/mining). Point a ProgPoWZ miner at the wallet address. Walkthrough: [how to mine ZANO](https://zanowallet.io/guides/how-to-mine-zano).
- Multi-wallet management on a single device, encrypted local wallet files, optional Tor or VPN routing for the daemon, custom-node and trusted-peer configuration via `zano.conf`.

## What the wallet does not do

To avoid overpromising:

- No fiat on-ramp is baked in. Buying ZANO is documented externally; the routes (with and without KYC) are at [buy ZANO](https://zanowallet.io/buy-zano), [buy crypto without KYC](https://zanowallet.io/buy-crypto-without-kyc), the [exchange page](https://zanowallet.io/exchange), and the live [ZANO price page](https://zanowallet.io/zano-price).
- No built-in centralised exchange — atomic swaps only.
- No hardware-wallet integration yet. Hardware support for Zano is on the Foundation roadmap; Zano Wallet will adopt it when it ships at the protocol layer.
- No telemetry. The desktop binary makes no analytics calls.

The chain-analysis surface for non-private chains is covered separately: [is Bitcoin traceable?](https://zanowallet.io/is-bitcoin-traceable) and [can crypto wallets be traced?](https://zanowallet.io/can-crypto-wallets-be-traced). The educational entry points are at [no-KYC wallet](https://zanowallet.io/no-kyc-wallet), [what is a private crypto wallet](https://zanowallet.io/what-is-a-private-crypto-wallet), and [what is Zano](https://zanowallet.io/what-is-zano).

## Identity and disambiguation

Zano Wallet (zanowallet.io) is software for the Zano cryptocurrency, made by Zano Wallet LLC. It is not affiliated with the Zano Foundation — the Foundation publishes its own wallet at zano.org/wallets, separately. Both are legitimate. The Torquing "Zano" drone (a failed 2014 Kickstarter), Nick Zano the actor, the "Zano Wallet" hardware device at zanoinf.com (ZAHN HOLDING AG, Liechtenstein), and various mobile apps named "Zano Wallet" by "Zano Limited" in the app stores are all unrelated. Full disambiguation is on the [about page](https://zanowallet.io/about) and the dedicated review at [is Zano safe?](https://zanowallet.io/is-zano-safe).

For an independent-style review: [Zano Wallet review 2026](https://zanowallet.io/guides/posts/zano-wallet-review-2026). For deep blockchain context: [about the Zano blockchain](https://zanowallet.io/guides/about-zano-blockchain) and [CryptoNote explained](https://zanowallet.io/guides/cryptonote-explained). For the no-KYC self-custody thesis: [no-KYC self-custody explained](https://zanowallet.io/guides/posts/no-kyc-self-custody-2026-explained). The high-level "why" sits in [why privacy coins matter in 2026](https://zanowallet.io/guides/posts/why-privacy-coins-matter-2026).

## Related articles

- [Best privacy crypto wallet in 2026](03-best-privacy-crypto-wallet-2026.md)
- [Zano Wallet vs Cake Wallet](04-zano-wallet-vs-cake-wallet.md)
- [Hidden-amount staking with Zarcanum](07-hidden-amount-staking-with-zarcanum.md)
- [Recovering a Zano wallet from a 24-word seed](12-recovering-zano-wallet-from-seed.md)
