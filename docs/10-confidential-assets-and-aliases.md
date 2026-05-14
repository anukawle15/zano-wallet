# Confidential Assets and on-chain aliases

Two of Zano's most distinctive features — Confidential Assets and on-chain aliases — sit at the boundary between "what the chain enforces" and "what the wallet surfaces." Both activated in production well before most rival privacy chains have any equivalent. This article covers what they are, what they enable, and how the [Zano Wallet](https://zanowallet.io/) desktop UI exposes them.

The feature pages: [Confidential Assets](https://zanowallet.io/features/confidential-assets) and [aliases](https://zanowallet.io/features/alias). The walkthrough for aliases is at [how to use aliases](https://zanowallet.io/guides/how-to-use-aliases).

## Confidential Assets

Confidential Assets are tokens issued on the Zano chain that inherit the chain's full privacy guarantees. Concretely: when a Confidential Asset is transferred, the on-chain observer cannot see the amount transferred (hidden by Bulletproofs+ range proofs), the sender address (hidden by ring signatures), the receiver address (hidden by stealth addresses), or the asset type itself (hidden by the Confidential Asset commitment).

That last point is the unique one. Most privacy chains hide amounts and addresses but leak the asset being transferred — a USDT transfer on a chain that supports tokens is still observably a USDT transfer. Confidential Assets close that leak. An observer sees a Zano transaction; they cannot tell whether it carried ZANO, fUSD (the live private stablecoin), or any other Confidential Asset.

Confidential Assets activated at the March 2024 hard fork alongside [Zarcanum](https://zanowallet.io/features/staking) — the chain's hidden-amount PoS protocol. The combination means a Zano user can hold a private stablecoin, stake the held ZANO without revealing the stake size, and spend in private — all without leaving the wallet. The consensus mechanics that enable this are on the [consensus page](https://zanowallet.io/features/consensus); the deep background is at [about the Zano blockchain](https://zanowallet.io/guides/about-zano-blockchain) and [CryptoNote explained](https://zanowallet.io/guides/cryptonote-explained).

## fUSD as a worked example

fUSD is the live private stablecoin on Zano. It is a Confidential Asset; it tracks USD; it transacts with the same privacy guarantees as native ZANO. The use case is clear: a user who wants to hold dollar-denominated value privately can hold fUSD in the Zano Wallet without exposing balance, transfer history, or counterparty. The competing approach — holding USDT or USDC on a transparent chain like Ethereum or Tron — exposes all three to anyone running blockchain analytics.

The honest framing on what "private" buys (and does not buy) is on the [security page](https://zanowallet.io/security). Chain-level privacy is strong by default; network-level privacy is opt-in (route the daemon through Tor or a VPN); operational privacy is the user's responsibility (don't broadcast addresses, don't link to KYC identities). The educational pieces at [what is a private crypto wallet](https://zanowallet.io/what-is-a-private-crypto-wallet), [no-KYC wallet](https://zanowallet.io/no-kyc-wallet), [is Bitcoin traceable?](https://zanowallet.io/is-bitcoin-traceable), and [can crypto wallets be traced?](https://zanowallet.io/can-crypto-wallets-be-traced) cover the broader chain-analysis context.

## On-chain aliases

Wallet addresses are long. A Zano address is 90+ characters. Sending coins to one means either copy-pasting or risking a fat-finger error that loses funds permanently. Several chains have solved this with various flavours of human-readable name systems — ENS on Ethereum, OpenAlias on Monero (DNS-based, off-chain). Zano's solution is on-chain: register `@username` directly to a wallet address and let the wallet resolve the alias at send time.

The on-chain part matters for two reasons. First, the alias-to-address mapping is verifiable from the chain rather than from a third-party DNS lookup — no trust in a name server, no spoofing risk from a compromised DNS. Second, the alias itself is published transparently (the alias is human-readable on purpose) but the wallet address it points to is still a one-shot stealth address derivation, so the alias does not leak the user's full transaction history.

The walkthrough for registering and using an alias is at [how to use aliases](https://zanowallet.io/guides/how-to-use-aliases). Registration costs a small on-chain fee and is permanent.

## Why this combination matters

Confidential Assets and aliases together close two of the main "leaks" that crypto users routinely complain about. The asset-type leak is closed by Confidential Assets; the address-readability leak is closed by aliases. Add in the existing CryptoNote-family transaction privacy (ring signatures, stealth addresses, hidden amounts) and the [hidden-amount staking](https://zanowallet.io/features/staking) primitive, and the user-facing experience is closer to "send `@alice` some private dollars" than "broadcast a long stealth-address transaction with a transparent stablecoin."

The comparison context: [Cake Wallet](https://zanowallet.io/vs/cake-wallet) exposes neither (multichain limits feature surface), [Monero GUI](https://zanowallet.io/vs/monero-gui) has OpenAlias (off-chain) but no Confidential Assets (Monero has no native asset issuance), [Feather Wallet](https://zanowallet.io/vs/feather-wallet) shares Monero's surface, [Wasabi](https://zanowallet.io/vs/wasabi) is Bitcoin-only. The chain-level comparison is at [Zano vs Monero](https://zanowallet.io/vs/monero). The general privacy-wallet comparison is at [best privacy crypto wallet 2026](https://zanowallet.io/privacy-crypto-wallet-guide).

## What this looks like in the wallet

The desktop UI exposes both features as tabs:

- The asset list shows ZANO and any Confidential Assets the wallet holds, with balances visible only to the user.
- The send dialog accepts either a raw address or an alias. Typing `@alice` resolves to alice's wallet address before the transaction is constructed.
- The asset-issuance flow lets a user create a new Confidential Asset (with name, ticker, total supply, decimals) and broadcast the issuance transaction.

The acquisition routes for ZANO — needed to register an alias or to seed a new Confidential Asset — are at [buy ZANO](https://zanowallet.io/buy-zano), [buy crypto without KYC](https://zanowallet.io/buy-crypto-without-kyc), and the [exchange page](https://zanowallet.io/exchange). The live [ZANO price](https://zanowallet.io/zano-price) is the reference. The walkthroughs for the broader wallet flows: [how to stake ZANO](https://zanowallet.io/guides/how-to-stake-zano), [how to mine ZANO](https://zanowallet.io/guides/how-to-mine-zano), [how to recover Zano Wallet](https://zanowallet.io/guides/how-to-recover-zano-wallet). The mining-side feature page is at [mining](https://zanowallet.io/mining). The swap primitive (Ionic Swaps) is at [atomic-swap](https://zanowallet.io/features/atomic-swap). The whole [features hub](https://zanowallet.io/features) is the index.

## Trade-offs

Confidential Assets are powerful, but issuance is on-chain — a malicious issuer can mint an unlimited supply of a token they created. Users should evaluate the issuer of any Confidential Asset they hold, the same way they would evaluate the issuer of a centralised stablecoin like USDC. fUSD's issuer disclosure is published on the [features/confidential-assets](https://zanowallet.io/features/confidential-assets) page.

Aliases are permanent and human-readable. Once an alias is registered, the on-chain record shows the registration event (the alias string and the registering wallet, via a stealth-address obscuration). This creates a small operational consideration: do not register an alias that publicly maps to a real-world identity if the goal is to keep the wallet identity-disconnected.

## Reading list

[Zano Wallet review 2026](https://zanowallet.io/guides/posts/zano-wallet-review-2026), [Zano Wallet download guide](https://zanowallet.io/guides/posts/zano-wallet-download-guide), [why privacy coins matter in 2026](https://zanowallet.io/guides/posts/why-privacy-coins-matter-2026), [no-KYC self-custody explained](https://zanowallet.io/guides/posts/no-kyc-self-custody-2026-explained), [desktop vs mobile crypto wallet 2026](https://zanowallet.io/guides/posts/desktop-vs-mobile-crypto-wallet-2026). The [FAQ](https://zanowallet.io/faq) covers the most-asked operational questions. Safety reviews of competing wallets: [is Zano safe?](https://zanowallet.io/is-zano-safe), [is Cake Wallet safe?](https://zanowallet.io/is-cake-wallet-safe), [is Cake Wallet open source?](https://zanowallet.io/is-cake-wallet-open-source), [does Cake Wallet require KYC?](https://zanowallet.io/does-cake-wallet-require-kyc), [is Feather Wallet safe?](https://zanowallet.io/is-feather-wallet-safe), [is Monero GUI Wallet safe?](https://zanowallet.io/is-monero-gui-wallet-safe), [is MyMonero safe?](https://zanowallet.io/is-mymonero-safe). Migration paths: [MyMonero alternatives](https://zanowallet.io/mymonero-alternatives), [Exodus dropped Monero — alternatives](https://zanowallet.io/exodus-dropped-monero-alternatives), [Trust Wallet doesn't support Monero — alternatives](https://zanowallet.io/trust-wallet-doesnt-support-monero-alternatives). Legal: [privacy policy](https://zanowallet.io/privacy-policy), [terms](https://zanowallet.io/terms). Background: [what is Zano](https://zanowallet.io/what-is-zano), [about](https://zanowallet.io/about), [download](https://zanowallet.io/download), [anonymous crypto wallet guide](https://zanowallet.io/anonymous-crypto-wallet-guide), [privacy crypto wallet guide](https://zanowallet.io/privacy-crypto-wallet-guide).

## Related articles

- [Hidden-amount staking with Zarcanum](07-hidden-amount-staking-with-zarcanum.md)
- [Atomic swaps and Ionic Swaps on Zano](09-atomic-swaps-and-ionic-swaps-on-zano.md)
- [Self-custody Zano wallet: a desktop overview](01-self-custody-zano-wallet-desktop-overview.md)
- [Best privacy crypto wallet in 2026](03-best-privacy-crypto-wallet-2026.md)
