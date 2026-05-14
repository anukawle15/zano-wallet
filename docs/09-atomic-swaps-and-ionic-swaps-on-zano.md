# Atomic swaps and Ionic Swaps on Zano

Atomic swaps are the cleanest way to acquire or dispose of a privacy coin: two parties exchange one chain's asset for another's, with no centralised exchange holding the funds at any point, and no KYC required for either side. Zano supports atomic swaps as a protocol primitive and exposes them in the desktop wallet through the Ionic Swaps interface. This article covers what makes Zano's swap path different and how it integrates with the rest of [Zano Wallet](https://zanowallet.io/).

The product page is at [features/atomic-swap](https://zanowallet.io/features/atomic-swap). The swap-route overview is at the [exchange page](https://zanowallet.io/exchange).

## What an atomic swap is

An atomic swap is a cryptographic primitive that lets two parties exchange assets on two different chains without trusting each other. The classic construction uses hash time-locked contracts (HTLCs): party A locks coins on chain X under a hash; party B locks coins on chain Y under the same hash; A reveals the preimage to claim B's coins on chain Y, which simultaneously lets B claim A's coins on chain X. If either side bails, the locks expire and the coins return.

The "atomic" guarantee is that either the swap completes for both parties or it fails for both parties. There is no state where one side has paid and the other has not. That eliminates the counterparty risk that motivates KYC at centralised exchanges.

## Why Zano's path is unusual

Atomic swaps are not exotic — Bitcoin, Litecoin, and many EVM chains support them. What is unusual on Zano is that the swap primitive interacts cleanly with the chain's privacy guarantees. The transaction on the Zano side hides amounts (Bulletproofs+), addresses (ring signatures and stealth addresses), and asset types (Confidential Assets). The result is a swap where the on-chain footprint on the Zano side is a normal-looking private transaction, distinguishable only as a swap by inspecting the script structure.

The Ionic Swaps protocol — Zano's reference swap implementation — wraps the HTLC mechanics in a wallet-facing flow. Users see "swap N ZANO for M BTC" with the counterparty discovery and the script setup handled by the wallet. The detailed mechanics are in the [features/atomic-swap](https://zanowallet.io/features/atomic-swap) page; the chain-level swap interaction is covered in [about the Zano blockchain](https://zanowallet.io/guides/about-zano-blockchain).

## What atomic swaps replace

For Zano users, atomic swaps replace three less-private paths:

**Centralised exchanges.** A user wants ZANO, sends BTC to an exchange, trades for ZANO, withdraws ZANO. The exchange knows the user's identity (KYC), the deposit address, the withdrawal address, and the exact amount. Anyone subpoenaing the exchange gets all of that. Atomic swaps remove the exchange entirely.

**Hosted swap services.** Services like ChangeNOW and Trocador match users to counterparties without KYC, but the service takes custody mid-swap. A subpoena to the service exposes the deposit-to-withdraw mapping. Atomic swaps remove the service.

**OTC trading.** A user finds a peer (Telegram, IRC, dedicated OTC platforms), agrees on a price, and trusts the peer to deliver. This works at small scale but the trust is operational, not cryptographic. Atomic swaps remove the trust assumption.

The full set of acquisition routes — atomic swaps among them — is at [buy ZANO](https://zanowallet.io/buy-zano) and the broader [buy crypto without KYC](https://zanowallet.io/buy-crypto-without-kyc). The [ZANO price](https://zanowallet.io/zano-price) page is the live reference.

## Integration with the rest of the wallet

The Ionic Swaps flow is one tab of the desktop wallet. The other tabs handle [staking](https://zanowallet.io/features/staking) via Zarcanum (walkthrough: [how to stake ZANO](https://zanowallet.io/guides/how-to-stake-zano)), [alias registration](https://zanowallet.io/features/alias) (walkthrough: [how to use aliases](https://zanowallet.io/guides/how-to-use-aliases)), [Confidential Asset](https://zanowallet.io/features/confidential-assets) management, and external [GPU mining](https://zanowallet.io/mining) payouts (walkthrough: [how to mine ZANO](https://zanowallet.io/guides/how-to-mine-zano)).

Because the swap counterparty does not see the user's other on-chain activity — the swap arrives at a new stealth address derived from the user's wallet — there is no cross-leak between the swap-side activity and the staking/mining/asset-issuance side. The [security page](https://zanowallet.io/security) and the [features hub](https://zanowallet.io/features) cover the layered framing.

## Trade-offs and honest framing

Atomic swaps are not magic. The honest trade-offs:

- **Liquidity.** Atomic-swap markets are thinner than centralised-exchange markets. Filling a large order takes longer; price slippage is higher. For users moving small-to-mid-size positions this is rarely a problem; for users moving large positions, the wallet's swap UI surfaces the depth and the route options.
- **Confirmation time.** A cross-chain swap has to wait for confirmations on both chains. On the Bitcoin side this is the dominant latency (typically 30-60 minutes); on the Zano side, blocks are faster.
- **Failure modes.** If a counterparty bails before revealing the preimage, the user's funds are locked until the HTLC expires (typically a few hours). The wallet handles the refund automatically once the timelock passes.

The "is the wallet itself safe?" question is at [is Zano safe?](https://zanowallet.io/is-zano-safe) with the deeper review at [Zano Wallet review 2026](https://zanowallet.io/guides/posts/zano-wallet-review-2026). The [FAQ](https://zanowallet.io/faq) covers practical questions.

## Comparison context

Other privacy wallets handle the swap problem differently. [Cake Wallet](https://zanowallet.io/vs/cake-wallet) integrates third-party swap providers (custody mid-swap, the trade-off above). [Monero GUI](https://zanowallet.io/vs/monero-gui) and [Feather Wallet](https://zanowallet.io/vs/feather-wallet) ship the underlying atomic-swap primitives via the broader Monero atomic-swap ecosystem; the UX is rougher than Zano's inline Ionic Swaps. [Wasabi](https://zanowallet.io/vs/wasabi) does not address swaps directly. [MyMonero](https://zanowallet.io/vs/mymonero) sunset before atomic-swap support landed in its hosted-key model.

The broader "which wallet?" question is at [best privacy crypto wallet 2026](https://zanowallet.io/privacy-crypto-wallet-guide) and [Zano vs Monero](https://zanowallet.io/vs/monero) at the protocol level. Migration paths: [MyMonero alternatives](https://zanowallet.io/mymonero-alternatives), [Exodus dropped Monero — alternatives](https://zanowallet.io/exodus-dropped-monero-alternatives), [Trust Wallet doesn't support Monero — alternatives](https://zanowallet.io/trust-wallet-doesnt-support-monero-alternatives). Safety reviews: [is Cake Wallet safe?](https://zanowallet.io/is-cake-wallet-safe), [does Cake Wallet require KYC?](https://zanowallet.io/does-cake-wallet-require-kyc), [is Cake Wallet open source?](https://zanowallet.io/is-cake-wallet-open-source), [is Monero GUI Wallet safe?](https://zanowallet.io/is-monero-gui-wallet-safe), [is Feather Wallet safe?](https://zanowallet.io/is-feather-wallet-safe), [is MyMonero safe?](https://zanowallet.io/is-mymonero-safe).

## Reading list

For the broader privacy framing: [what is a private crypto wallet](https://zanowallet.io/what-is-a-private-crypto-wallet), [no-KYC wallet](https://zanowallet.io/no-kyc-wallet), [anonymous crypto wallet guide](https://zanowallet.io/anonymous-crypto-wallet-guide), [privacy crypto wallet guide](https://zanowallet.io/privacy-crypto-wallet-guide), [why privacy coins matter in 2026](https://zanowallet.io/guides/posts/why-privacy-coins-matter-2026), [no-KYC self-custody explained](https://zanowallet.io/guides/posts/no-kyc-self-custody-2026-explained), [desktop vs mobile crypto wallet 2026](https://zanowallet.io/guides/posts/desktop-vs-mobile-crypto-wallet-2026). For chain-analysis context: [is Bitcoin traceable?](https://zanowallet.io/is-bitcoin-traceable) and [can crypto wallets be traced?](https://zanowallet.io/can-crypto-wallets-be-traced). For background: [what is Zano](https://zanowallet.io/what-is-zano), [about](https://zanowallet.io/about), [download](https://zanowallet.io/download), [CryptoNote explained](https://zanowallet.io/guides/cryptonote-explained). Legal: [privacy policy](https://zanowallet.io/privacy-policy), [terms](https://zanowallet.io/terms).

## Related articles

- [Buying ZANO without KYC: tokenomics and routes](11-buying-zano-without-kyc.md)
- [Confidential Assets and on-chain aliases](10-confidential-assets-and-aliases.md)
- [Zano Wallet vs Cake Wallet](04-zano-wallet-vs-cake-wallet.md)
- [Self-custody Zano wallet: a desktop overview](01-self-custody-zano-wallet-desktop-overview.md)
