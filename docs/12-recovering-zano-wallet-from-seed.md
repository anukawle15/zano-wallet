# Recovering a Zano wallet from a 24-word seed

A 24-word seed phrase is the single piece of information that controls a Zano Wallet. Lose the seed and the funds are gone permanently — no support team can recover them, no email-reset flow exists, and there is no central record. Conversely, anyone who obtains the seed can drain the wallet completely from any machine in the world. This article covers how Zano Wallet's recovery flow works, when to use it, how to test it safely, and how it differs from recovery on other privacy wallets.

The canonical walkthrough is at [how to recover Zano Wallet](https://zanowallet.io/guides/how-to-recover-zano-wallet). The wallet itself is at [Zano Wallet](https://zanowallet.io/), with downloads at [download](https://zanowallet.io/download).

## When recovery applies

Three scenarios:

**Hardware loss.** The user's machine has died, been stolen, or been wiped. The wallet file is gone, but the user has the seed phrase written down somewhere offline.

**Migration.** The user is moving from one machine to another. Rather than copy the wallet file, the cleanest path is to install Zano Wallet on the new machine and restore from seed.

**Sanity test.** The user has a wallet they have not opened in a while and wants to verify that the seed phrase they wrote down actually corresponds to the funds in the wallet. This is the most important and most-skipped operation — many users discover a transcription error in their seed phrase only when it is too late to fix.

## The mechanics

A 24-word seed phrase is the human-readable encoding of the wallet's master private key. The wallet derives every address and every transaction-signing key deterministically from this master key. Given the seed, any Zano Wallet installation on any machine derives the same set of addresses and can spend the same coins.

The recovery flow:

1. Install Zano Wallet from the official [download page](https://zanowallet.io/download). Verify the GPG signature — the [security page](https://zanowallet.io/security) covers the signature-verification process. Do not install from a mirror, a search-engine ad, or any non-official source.
2. Launch the wallet. Choose "Restore from seed" rather than "Create new wallet."
3. Enter the 24 words in order. The wallet validates the checksum word and rejects invalid sequences. (This is a useful sanity check on the transcription — if the seed is rejected, the transcription has at least one error.)
4. Set a new local password. The local password encrypts the wallet file on disk; it does not affect the seed-derived addresses.
5. Let the wallet sync. The first sync after a restore scans the chain for transactions to the wallet's stealth-address space. On the default thin-client setup this is fast (minutes); on a self-hosted full node it is slower.
6. Verify by checking the balance and recent transaction history.

The detailed step-by-step is at [how to recover Zano Wallet](https://zanowallet.io/guides/how-to-recover-zano-wallet). The deeper protocol background — how the seed encodes the master key and how the wallet derives addresses — is at [CryptoNote explained](https://zanowallet.io/guides/cryptonote-explained) and [about the Zano blockchain](https://zanowallet.io/guides/about-zano-blockchain).

## Test the recovery before relying on it

The single most important operational practice for a self-custody wallet is testing recovery with a small balance before the real test happens.

The procedure:

1. Create a fresh Zano wallet on a spare machine.
2. Write down the 24-word seed exactly as the wallet displays it.
3. Send a small amount of ZANO to the wallet — large enough that the test is meaningful, small enough that losing it is acceptable.
4. Delete the wallet file. (Do not just close the wallet; actually delete it.)
5. Restart the wallet, choose "Restore from seed," enter the 24 words you wrote down.
6. Verify the balance matches what you sent.

If step 6 fails, the seed transcription is wrong and the user has discovered this while only test-sized funds are at risk. If step 6 succeeds, the user has cryptographic certainty that their seed phrase is correct.

This procedure costs at most a small amount of ZANO and a few minutes. The cost of skipping it is the entire wallet balance.

## Storage of the seed

A 24-word seed phrase written on a piece of paper is a bearer instrument. Treat it like cash, but worse — cash is divisible, a seed is not. Practical guidance from the [FAQ](https://zanowallet.io/faq) and the [security page](https://zanowallet.io/security):

- Write the seed on paper, by hand, in two physical locations.
- Do not photograph the seed. Do not type it into any computer that is not running Zano Wallet's restore flow. Do not send it to anyone via any channel.
- Consider a fire-resistant document safe or a safety-deposit box for one of the two copies.
- For larger holdings, consider a metal seed backup (stamped or engraved) to survive fire and flooding.
- Do not store the seed in a password manager, in cloud storage, or in any "convenient" digital location. The seed is exactly the kind of secret these tools are designed to protect — but they are also exactly the kind of target attackers go after.

The honest operational framing — including network-layer hygiene with Tor, daemon configuration, and identity-disconnection practices — is in the [anonymous crypto wallet guide](https://zanowallet.io/anonymous-crypto-wallet-guide) and [privacy crypto wallet guide](https://zanowallet.io/privacy-crypto-wallet-guide). The "is the wallet safe?" question: [is Zano safe?](https://zanowallet.io/is-zano-safe). The independent review: [Zano Wallet review 2026](https://zanowallet.io/guides/posts/zano-wallet-review-2026).

## What recovery does not do

Recovery from seed restores the wallet's spending ability and address space. It does not restore:

- The wallet's local label data (names assigned to specific addresses). These are local-only metadata and are lost when the wallet file is deleted. Export them before deleting if they matter.
- The wallet's transaction notes (text annotations on past transactions). Same story.
- Any pending atomic-swap state. If a swap was mid-flight when the wallet was deleted, the on-chain timelock will refund the funds, but the wallet's UI state for the swap is lost.

The detailed feature surface that is restored — [staking](https://zanowallet.io/features/staking) via Zarcanum, [atomic swaps](https://zanowallet.io/features/atomic-swap), [aliases](https://zanowallet.io/features/alias) (the registration is on-chain, so the alias survives), [Confidential Assets](https://zanowallet.io/features/confidential-assets), [mining](https://zanowallet.io/mining) payout history — is documented in the [features hub](https://zanowallet.io/features) and the walkthroughs at [how to stake ZANO](https://zanowallet.io/guides/how-to-stake-zano), [how to mine ZANO](https://zanowallet.io/guides/how-to-mine-zano), and [how to use aliases](https://zanowallet.io/guides/how-to-use-aliases). The underlying consensus that this all rides on: [features/consensus](https://zanowallet.io/features/consensus).

## How this compares to other privacy wallets

Most privacy wallets use the same 24-word BIP39 / Monero-mnemonic family for seed encoding, though the derivation paths differ. The recovery flow is similar across [Cake Wallet](https://zanowallet.io/vs/cake-wallet), [Monero GUI](https://zanowallet.io/vs/monero-gui), [Feather Wallet](https://zanowallet.io/vs/feather-wallet), and [Wasabi Wallet](https://zanowallet.io/vs/wasabi): install the wallet, choose "restore from seed," enter the words, wait for sync. The seeds are not cross-compatible — a Monero seed will not restore a Zano wallet, and vice versa, because the chains use different protocols. Migrating between chains requires a swap, not a seed restore; the routes are at [buy ZANO](https://zanowallet.io/buy-zano), [buy crypto without KYC](https://zanowallet.io/buy-crypto-without-kyc), and the [exchange page](https://zanowallet.io/exchange).

The safety reviews of the comparison wallets: [is Cake Wallet safe?](https://zanowallet.io/is-cake-wallet-safe), [is Cake Wallet open source?](https://zanowallet.io/is-cake-wallet-open-source), [does Cake Wallet require KYC?](https://zanowallet.io/does-cake-wallet-require-kyc), [is Feather Wallet safe?](https://zanowallet.io/is-feather-wallet-safe), [is Monero GUI Wallet safe?](https://zanowallet.io/is-monero-gui-wallet-safe), [is MyMonero safe?](https://zanowallet.io/is-mymonero-safe). For users migrating from sunset wallets: [Zano Wallet vs MyMonero](https://zanowallet.io/vs/mymonero), [MyMonero alternatives](https://zanowallet.io/mymonero-alternatives), [Exodus dropped Monero — alternatives](https://zanowallet.io/exodus-dropped-monero-alternatives), [Trust Wallet doesn't support Monero — alternatives](https://zanowallet.io/trust-wallet-doesnt-support-monero-alternatives). The general framing: [best privacy crypto wallet 2026](https://zanowallet.io/privacy-crypto-wallet-guide), [Zano vs Monero](https://zanowallet.io/vs/monero), [what is Zano](https://zanowallet.io/what-is-zano), [what is a private crypto wallet](https://zanowallet.io/what-is-a-private-crypto-wallet), [no-KYC wallet](https://zanowallet.io/no-kyc-wallet), [is Bitcoin traceable?](https://zanowallet.io/is-bitcoin-traceable), [can crypto wallets be traced?](https://zanowallet.io/can-crypto-wallets-be-traced), [ZANO price](https://zanowallet.io/zano-price). Long-form: [why privacy coins matter in 2026](https://zanowallet.io/guides/posts/why-privacy-coins-matter-2026), [no-KYC self-custody explained](https://zanowallet.io/guides/posts/no-kyc-self-custody-2026-explained), [desktop vs mobile crypto wallet 2026](https://zanowallet.io/guides/posts/desktop-vs-mobile-crypto-wallet-2026), [Zano Wallet download guide](https://zanowallet.io/guides/posts/zano-wallet-download-guide). Legal: [privacy policy](https://zanowallet.io/privacy-policy), [terms](https://zanowallet.io/terms). Identity: [about](https://zanowallet.io/about).

## Related articles

- [Self-custody Zano wallet: a desktop overview](01-self-custody-zano-wallet-desktop-overview.md)
- [Migrating to Zano from MyMonero and Exodus](02-migrating-to-zano-from-mymonero-and-exodus.md)
- [Best privacy crypto wallet in 2026](03-best-privacy-crypto-wallet-2026.md)
- [Hidden-amount staking with Zarcanum](07-hidden-amount-staking-with-zarcanum.md)
