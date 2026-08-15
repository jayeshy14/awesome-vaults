# Awesome Vaults [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated guide to onchain vaults, from your first ERC-4626 contract to production curator systems, async accounting, structured products, and the security work that keeps depositor funds safe.

A vault is a smart contract that takes a deposit, issues shares that represent a claim on a growing pool of assets, and puts that capital to work. The pattern now sits under a large share of DeFi: yield aggregators, curated lending, liquid staking and restaking wrappers, structured products, and institutional asset management all run on it. There are `awesome` lists for Solidity, for Diamonds, and for DeFi in general, but there was no single map for vaults. This is that map.

Every entry links to a primary source, an EIP page, an official doc, a protocol's own repository, or the author's own writing. Each has a one line description of what you learn and a level tag so you can read in order. A few historical protocols are included where their design still teaches something builders reinvent today, and they are labeled as such.

**Levels:** (beginner) first exposure to the idea, (intermediate) you can already read Solidity and want the mechanism, (advanced) protocol-grade architecture, accounting internals, and security.

## Contents

- [Standards and EIPs](#standards-and-eips)
- [Foundations: What Is a Vault](#foundations-what-is-a-vault)
- [Reference Implementations and Libraries](#reference-implementations-and-libraries)
- [Security: Attacks and Defenses](#security-attacks-and-defenses)
- [Testing and Formal Verification](#testing-and-formal-verification)
- [Yield Aggregator Vaults](#yield-aggregator-vaults)
- [Curated Lending Vaults](#curated-lending-vaults)
- [Curators and Curation](#curators-and-curation)
- [Institutional and Generalized Vault Frameworks](#institutional-and-generalized-vault-frameworks)
- [Structured Products and Tranched Vaults](#structured-products-and-tranched-vaults)
- [Options and Structured-Note Vaults](#options-and-structured-note-vaults)
- [Restaking and LRT Vaults](#restaking-and-lrt-vaults)
- [Credit and RWA Vaults](#credit-and-rwa-vaults)
- [Stablecoin and Synthetic-Dollar Vaults](#stablecoin-and-synthetic-dollar-vaults)
- [Liquidity Management Vaults](#liquidity-management-vaults)
- [Asynchronous and Multi-Strategy Architecture](#asynchronous-and-multi-strategy-architecture)
- [Deep-Dive Articles and Research](#deep-dive-articles-and-research)
- [Videos, Talks, and Courses](#videos-talks-and-courses)
- [Live Data, Dashboards, and Risk](#live-data-dashboards-and-risk)

## Standards and EIPs

The specifications that define what a vault is and how it must behave.

- [ERC-4626: Tokenized Vaults](https://eips.ethereum.org/EIPS/eip-4626) - The core specification defining the tokenized vault interface, the deposit, mint, withdraw, and redeem methods, the share-to-asset conversion functions, and the required rounding directions. (intermediate)
- [ERC-7540: Asynchronous ERC-4626 Tokenized Vaults](https://eips.ethereum.org/EIPS/eip-7540) - Extends ERC-4626 with request-based asynchronous deposit and redemption flows and a pending, claimable, claimed request lifecycle for vaults that cannot settle in a single transaction. (advanced)
- [ERC-7575: Multi-Asset ERC-4626 Vaults](https://eips.ethereum.org/EIPS/eip-7575) - Separates the share token from the vault entry points so a single share can be backed by multiple assets, and externalizes the ERC-20 share accounting. (advanced)
- [ERC-7535: Native Asset ERC-4626 Tokenized Vault](https://eips.ethereum.org/EIPS/eip-7535) - Adapts ERC-4626 to use native ETH as the underlying asset through the 0xEee address convention and payable deposit paths. (intermediate)
- [ERC-6909: Minimal Multi-Token Interface](https://eips.ethereum.org/EIPS/eip-6909) - A minimal multi-token standard that drops callbacks and batching from ERC-1155 and uses a combined allowance and operator permission model, increasingly used for vault share accounting. (intermediate)
- [ERC-5115: SY Token, Standardized Yield](https://eips.ethereum.org/EIPS/eip-5115) - Defines the Standardized Yield interface that wraps yield-bearing assets behind uniform deposit, redeem, and exchange-rate methods, covering AMM LP and reward-token mechanisms that ERC-4626 alone cannot represent. (advanced)
- [EIP-4626 Discussion Thread](https://ethereum-magicians.org/t/eip-4626-yield-bearing-vault-standard/7900) - The original Ethereum Magicians thread where the standard was debated, covering fee handling, rounding, fee-on-transfer tokens, and the rationale for keeping both withdraw and redeem. (advanced)
- [EIP-7540 Discussion Thread](https://ethereum-magicians.org/t/eip-7540-asynchronous-erc-4626-tokenized-vaults/16153) - The thread behind the async vault standard, documenting the design debate over request lifecycle states, operator permissions, and backward compatibility with ERC-4626. (advanced)

## Foundations: What Is a Vault

Start here if vaults are new to you. These explain the share and asset model before you touch production code.

- [ERC-4626 Tokenized Vault Standard](https://ethereum.org/developers/docs/standards/tokens/erc-4626/) - A plain-language introduction from ethereum.org explaining what a tokenized vault is and how shares represent proportional ownership of a single underlying ERC-20 asset. (beginner)
- [ERC-4626 Interface Explained](https://rareskills.io/post/erc4626) - A function-by-function walkthrough from RareSkills showing how share and asset accounting works and how deposits, redemptions, and yield accrual move the share price. (intermediate)
- [How to Use ERC-4626 with Your Smart Contract](https://www.quicknode.com/guides/ethereum-development/smart-contracts/how-to-use-erc-4626-with-your-smart-contract) - A hands-on QuickNode guide that builds a vault by inheriting an ERC-4626 base contract, deploys it to a testnet, and deposits an underlying token to mint shares. (intermediate)
- [ERC4626 Vaults: Secure Design, Risks, and Best Practices](https://speedrunethereum.com/guides/erc-4626-vaults) - A build-oriented Speedrun Ethereum guide pairing the core vault functions with the first-depositor inflation attack, reentrancy, rounding, and fuzz-testing practice. (intermediate)
- [ERC-4626: Tokenized Vaults (L2IV Research)](https://l2ivresearch.substack.com/p/erc-4626-tokenized-vaults) - An overview covering deposit and withdraw mechanics, share conversion, fee handling, EIP-2612 permits, and security lessons drawn from the Rari and Cream incidents. (intermediate)

## Reference Implementations and Libraries

Battle-tested code to read, inherit, or fork. Reading these side by side is one of the fastest ways to learn the standard.

- [OpenZeppelin ERC4626.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/ERC4626.sol) - The production ERC-4626 base contract showing how the rounding rules and the virtual assets and shares inflation-attack mitigation are implemented in Solidity. (intermediate)
- [OpenZeppelin ERC-4626 Documentation](https://docs.openzeppelin.com/contracts/5.x/erc4626) - Explains the inflation attack against empty vaults and the virtual-shares-with-decimals-offset defense, with a worked example of adding entry and exit fees while keeping preview functions accurate. (intermediate)
- [Solmate ERC4626.sol](https://github.com/transmissions11/solmate/blob/main/src/tokens/ERC4626.sol) - A minimal implementation using fixed-point math with beforeWithdraw and afterDeposit hooks, useful for reading the standard's accounting stripped to essentials. (advanced)
- [Solady ERC4626.sol](https://github.com/Vectorized/solady/blob/main/src/tokens/ERC4626.sol) - A gas-optimized ERC-4626 implementation that exposes virtual shares and a decimals offset through overridable functions. (advanced)
- [snekmate ERC-4626 (Vyper)](https://github.com/pcaversaccio/snekmate) - Pcaversaccio's audited, security-focused Vyper library, including a modern gas-efficient ERC-4626 vault with unit, property-based, and invariant tests, the canonical Vyper counterpart to the Solidity implementations. (intermediate)
- [yield-daddy](https://github.com/timeless-fi/yield-daddy) - ERC-4626 wrapper contracts and factories that adapt Aave V2 and V3, Compound, Euler, and Lido stETH positions into the vault interface. (intermediate)
- [PoolTogether V5 PrizeVault](https://github.com/pooltogether/v5-vault) - A well-audited, spec-strict ERC-4626 wrapper that routes deposits into an underlying yield source and contributes the accrued yield to a shared prize pool instead of paying it out pro rata. (intermediate)
- [ERC-7540 Reference Implementations](https://github.com/ERC4626-Alliance/ERC-7540-Reference) - Four minimal async vaults, controlled async deposit, controlled async redeem, fully async, and timelocked redeem, that show how the request lifecycle is coded over ERC-4626. (intermediate)

## Security: Attacks and Defenses

The failure modes that have drained real vaults, and the patterns that prevent them. Read this section before you ship.

- [A Novel Defense Against ERC4626 Inflation Attacks](https://www.openzeppelin.com/news/a-novel-defense-against-erc4626-inflation-attacks) - Walks through how the inflation attack works and compares router, internal-balance, and dead-shares mitigations before deriving the virtual-offset defense used in the reference implementation. (advanced)
- [Overview of the Inflation Attack](https://mixbytes.io/blog/overview-of-the-inflation-attack) - A step-by-step derivation from MixBytes of how share-price manipulation through direct token donation lets an attacker capture a later depositor's funds, with the arithmetic worked out. (intermediate)
- [Exchange Rate Manipulation in ERC4626 Vaults](https://www.euler.finance/blog/exchange-rate-manipulation-in-erc4626-vaults) - Catalogs first-deposit frontrunning along with direct, stealth, flash-loan, and debt-repayment donation variants, then weighs mitigations such as dead shares, virtual deposits, and internal balance tracking. (advanced)
- [Exploring ERC-4626: A Security Primer](https://www.zellic.io/blog/exploring-erc-4626/) - Walks through recurring pitfalls including rounding direction, fee-on-transfer tokens, decimal mismatches, and preview-versus-convert misuse, using the Rari and Cream failures as examples. (intermediate)
- [ERC-4626 Tokens in DeFi: Exchange Rate Manipulation Risks](https://www.openzeppelin.com/news/erc-4626-tokens-in-defi-exchange-rate-manipulation-risks) - Focuses on the integrator side and shows how a protocol that prices ERC-4626 shares off the vault's internal assets-per-share can be exploited even when the vault itself is compliant. (intermediate)
- [Solodit Checklist Explained: Donation Attacks](https://www.cyfrin.io/blog/solodit-checklist-explained-3-donation-attacks) - An auditor checklist entry from Cyfrin showing with code how direct token transfers manipulate balance-based accounting and which patterns prevent it. (intermediate)
- [So You Want to Use a Price Oracle](https://samczsun.com/so-you-want-to-use-a-price-oracle/) - samczsun's landmark writeup on oracle manipulation, dissecting the bZx, Harvest, and Synthetix exploits and the defenses, the foundational reference for why reading a spot price mid-transaction is dangerous. (advanced)
- [ResupplyFi Hack Analysis](https://ackee.xyz/blog/resupply-hack-analysis/) - Post-mortem from Ackee of the 2025 Resupply exploit, tracing how a donation into a nearly empty ERC-4626 vault drove the exchange rate to zero through floor division and bypassed the solvency check. (advanced)
- [StakedUSDeV2 Breaks the ERC-4626 Standard](https://github.com/code-423n4/2023-10-ethena-findings/issues/562) - A Code4rena finding on Ethena's staking vault showing how a cooldown-gated withdraw path can make a live vault non-compliant with ERC-4626, a concrete example of standard-conformance risk. (advanced)
- [ERC4626 Inflation Attack Mitigation (PR #3979)](https://github.com/OpenZeppelin/openzeppelin-contracts/pull/3979) - The pull request that added virtual shares to OpenZeppelin's ERC4626, with review discussion covering the math and trade-offs of the decimals offset. (advanced)

## Testing and Formal Verification

Prove your vault meets the spec rather than hoping it does.

- [a16z erc4626-tests](https://github.com/a16z/erc4626-tests) - A Foundry property-test suite that checks any ERC-4626 vault for round-trip behavior, balance and allowance updates, non-reverting view functions, and preview accuracy, meant to be inherited by your own test contract. (advanced)
- [crytic/properties ERC-4626 Suite](https://github.com/crytic/properties/blob/main/contracts/ERC4626/README.md) - Reusable Echidna and Medusa invariants from Trail of Bits grouped into accounting, rounding, and security property sets that a vault can fuzz against for conformance and inflation resistance. (advanced)
- [Reusable Properties for Ethereum Contracts](https://blog.trailofbits.com/2023/02/27/reusable-properties-ethereum-contracts-echidna/) - The Trail of Bits writeup explaining the reasoning behind the reusable ERC-4626 and ERC-20 invariants and how to wire them into a fuzzing harness. (intermediate)
- [How to Fuzz ERC-4626 Vaults](https://getrecon.xyz/blog/how-to-fuzz-erc4626-vaults) - A hands-on Recon guide to building an invariant-fuzzing harness for a synchronous vault, covering the properties to assert and the setup that catches accounting and rounding bugs. (intermediate)
- [How to Fuzz ERC-7540 Async Vaults](https://getrecon.xyz/blog/how-to-fuzz-erc7540-async-vaults) - The async counterpart from Recon, showing how to model the request lifecycle and settlement so a fuzzer can reach the states where async vaults break. (advanced)
- [The Recon Book](https://book.getrecon.xyz/) - A free handbook on invariant testing and fuzzing for Solidity, useful as the broader method behind the vault-specific fuzzing guides. (intermediate)
- [Is My ERC-4626 Vault Token Up to the Standard?](https://runtimeverification.com/blog/is-my-erc-4626-vault-token-up-to-the-standard) - Runtime Verification compares the a16z property suite and the ERCx service and covers round-trip properties and functional correctness for both deployed and undeployed contracts, the formal-methods complement to fuzzing. (advanced)

## Yield Aggregator Vaults

Vaults that route deposits into strategies and compound the returns. The original vault use case.

- [Yearn V3 Vaults Overview](https://docs.yearn.fi/developers/v3/overview) - Describes how Yearn V3 separates the system into allocator vaults, standalone ERC-4626 tokenized strategies, and optional periphery contracts such as accountants and debt allocators. (intermediate)
- [Yearn V3 Tokenized Strategy Specification](https://github.com/yearn/tokenized-strategy/blob/master/SPECIFICATION.md) - Documents the immutable-proxy delegatecall pattern that routes ERC-4626 and accounting logic to a shared implementation, leaving each strategy to hold only yield-source-specific code. (advanced)
- [yearn/tokenized-strategy](https://github.com/yearn/tokenized-strategy) - Source for the Yearn V3 TokenizedStrategy base and BaseStrategy that single-strategy ERC-4626 vaults delegate their standardized vault logic to. (advanced)
- [yearn-vaults-v3 Technical Specification](https://github.com/yearn/yearn-vaults-v3/blob/master/TECH_SPEC.md) - Details the Vyper multi-strategy allocator vault, its factory deployment, debt management across strategies, and the profit-reporting and loss-accounting flows. (advanced)
- [Yearn V2 Vaults Overview](https://docs.yearn.fi/getting-started/products/yvaults/v2) - Describes the earlier model where up to twenty per-vault strategies with capital limits are ordered in a withdrawal queue and harvested by keeper bots. (intermediate)
- [yearn-vaults-v2 Specification](https://github.com/yearn/yearn-vaults-v2/blob/master/SPECIFICATION.md) - Specifies V2 vault accounting, strategy debt ratios, harvest and report mechanics, and the governance, guardian, and strategist role model. (advanced)
- [Beefy Vault Contract](https://docs.beefy.finance/developer-documentation/vault-contract) - Walks through the BeefyVaultV7 contract that mints mooToken shares and routes deposited tokens into a separate, upgradeable strategy contract to isolate strategy risk. (intermediate)
- [Beefy Strategy Contract](https://docs.beefy.finance/developer-documentation/strategy-contract) - Explains the Beefy strategy contract and its harvest flow that claims farm rewards, swaps them to the underlying asset, and redeposits to auto-compound. (intermediate)
- [beefyfinance/beefy-contracts](https://github.com/beefyfinance/beefy-contracts) - Public repository of Beefy vault and strategy contracts with the deployment and testing scripts used for community-submitted auto-compounding strategies. (advanced)
- [Tokemak Autopilot](https://docs.tokemak.xyz/developer-docs/contracts-overview/autopool-eth-contracts-overview/autopilot-system-high-level-overview) - Documents how Tokemak Autopilot deploys an ERC-4626 Autopool across a fixed set of liquidity destinations and continuously rebalances deposits toward the best risk-adjusted return. (advanced)
- [Summer.fi Lazy Summer Protocol Documentation](https://docs.summer.fi/lazy-summer-protocol/lazy-summer-protocol) - Documents the Fleet vaults that deploy deposits across yield-generating Arks with keeper-driven rebalancing inside FleetCommander constraints and externally set risk parameters. (advanced)
- [OasisDEX/lazy-summer-protocol](https://github.com/OasisDEX/lazy-summer-protocol) - Solidity source for the Lazy Summer Protocol, including the FleetCommander, Ark strategy adapters, reward auctions, and the constrained rebalancer that keepers call. (advanced)
- [Idle Best Yield Architecture](https://docs.idle.finance/developers/best-yield/architecture) - Covers how the Best Yield IdleToken vault allocates a single asset across lending protocols using off-chain-computed allocations that trigger on-chain rebalances. (intermediate)
- [Idle Yield Tranches Architecture](https://docs.idle.finance/developers/yield-tranches) - Describes the IdleCDO contract that pools deposits, mints senior AA and junior BB tranche tokens, and routes funds through a strategy proxy to a downstream yield source. (advanced)
- [Sommelier Protocol V2 Contract Architecture](https://sommelier-finance.gitbook.io/sommelier-documentation/smart-contracts/protocol-v2-contract-architecture) - Explains the Cellar V2 system of ERC-4626 vaults together with the Registry and PriceRouter contracts that price multi-token positions and constrain permitted adaptors. (intermediate)
- [Sommelier Building Adaptors](https://sommelier-finance.gitbook.io/sommelier-documentation/smart-contracts/external-protocol-integration/building-adaptors) - Details how adaptor contracts let a strategist call external DeFi protocols while the registry limits which positions are allowed. (advanced)
- [Harvest Finance Vaults](https://docs.harvest.finance/how-it-works/harvest-contracts/vaults) - Introduces the model where deposits mint fToken shares whose price rises as the attached strategy harvests and reinvests rewards. (beginner)
- [Harvest Coding Strategies Guide](https://github.com/harvest-finance/harvest/blob/master/CodingStrategies.MD) - Explains the vault-callable strategy interface and the doHardWork method that invests underlying tokens, liquidates reward crops, and reinvests the proceeds. (advanced)

## Curated Lending Vaults

The curator model, where a permissioned role allocates pooled deposits across isolated lending markets under caps. One of the fastest-growing vault categories.

- [Introducing MetaMorpho: Permissionless Lending Vaults on Morpho Blue](https://morpho.org/blog/introducing-metamorpho-permissionless-lending-vaults-on-morpho-blue/) - Explains how a MetaMorpho ERC-4626 vault pools depositor liquidity and allocates it across isolated Morpho Blue markets under supply caps set by a curator. (beginner)
- [Curator (Morpho Docs)](https://docs.morpho.org/learn/concepts/curator/) - Defines what the curator role controls in a Morpho vault, which markets are enabled, exposure caps, appointing allocators, and the timelocks that gate those changes. (beginner)
- [Morpho Vault V2 (Morpho Docs)](https://docs.morpho.org/learn/concepts/vault-v2/) - Documents the second-generation vault, covering its adapter system for routing to multiple protocols, the abstract id-and-cap risk model, and the owner, curator, allocator, sentinel role separation. (intermediate)
- [Security Considerations for Vault Curators](https://docs.morpho.org/curate/concepts/security-considerations/) - Catalogues the attack surface a curator must guard against, including faulty and reverting oracles, ERC-4626 donation and inflation manipulation, and adapter-removal frontrunning. (advanced)
- [Gates in Morpho Vaults](https://docs.morpho.org/curate/concepts/gates/) - Describes the gate contracts that let a curator restrict who can deposit, receive, send, or withdraw shares to build permissioned or compliance-constrained vaults. (intermediate)
- [morpho-org/metamorpho](https://github.com/morpho-org/metamorpho) - Source for the original MetaMorpho ERC-4626 vault, showing the supply and withdraw queue logic, per-market caps, timelocked cap changes, and the role-based access modifiers. (advanced)
- [morpho-org/metamorpho-v1.1](https://github.com/morpho-org/metamorpho-v1.1) - The V1.1 fork, useful for diffing its bad-debt handling, mutable name and symbol, and deployment changes against the original implementation. (advanced)
- [Euler Vault Kit Whitepaper](https://github.com/euler-xyz/euler-vault-kit/blob/master/docs/whitepaper.md) - Describes how the Euler Vault Kit builds ERC-4626 credit vaults with borrowing and how the Ethereum Vault Connector links vaults together as collateral and liabilities. (advanced)
- [Euler Vault Kit Developer Overview](https://docs.euler.finance/developers/evk/) - Developer entry point covering the EVault contract structure, its module system, and the difference between governed and ungoverned vaults. (intermediate)
- [Introducing Euler Earn](https://euler.finance/blog/euler-earn) - Introduces an ERC-4626 meta-vault that lets a curator allocate one deposited asset across selected Euler markets or other approved ERC-4626 vaults. (intermediate)
- [euler-xyz/euler-earn](https://github.com/euler-xyz/euler-earn) - Source for Euler Earn, a MetaMorpho-v1.1 fork, showing how the supply queue, withdraw queue, and per-strategy caps adapt to allocate over generic ERC-4626 strategy vaults. (advanced)
- [Gearbox: One Pool, Many Markets](https://docs.gearbox.finance/core/one-pool-many-markets) - Documents the model where a single passive ERC-4626 liquidity pool funds multiple isolated credit markets, each capped by its own debt ceiling. (intermediate)
- [Silo Finance V2](https://github.com/silo-finance/silo-contracts-v2) - Source for a lending protocol that builds isolated markets as paired ERC-4626 vaults, isolating each collateral asset's risk while a bridge asset connects markets for shared liquidity. (advanced)
- [Sturdy V2 Documentation](https://docs.sturdy.finance/) - Documents a two-tier design where siloed lending pairs isolate collateral risk and a Yearn V3 aggregator vault allocates a single deposited asset across whitelisted silos. (advanced)

## Curators and Curation

Vaults are only as good as the people allocating them. This section covers how professional curators reason about risk, and the writeups that dissect the model's incentives and failures.

- [Gauntlet VaultBook](https://vaultbook.gauntlet.xyz/) - A live methodology hub from one of the largest curators, explaining its curation approach, risk factors, and the per-vault optimization and risk parameters it sets. (intermediate)
- [Steakhouse Financial: Risk Management Framework](https://www.steakhouse.financial/docs) - Documents Steakhouse's multilayered risk rating framework that grades collateral across asset, platform, and market layers on a letter scale. (advanced)
- [Introducing the Re7 Risk Index](https://re7research.substack.com/p/introducing-re7-risk-index) - A curator's own methodology for scoring protocols across smart-contract, governance, economic, and third-party risk to size vault positions. (intermediate)
- [Curve Market Health Scores Methodology](https://llamarisk.substack.com/p/curve-market-health-scores-methodology) - LlamaRisk explains how it computes quantitative market health scores for lending markets, including the inputs and thresholds that flag conditions needing action. (advanced)
- [Gauging Slashing Risks of Symbiotic Networks](https://www.mevcapital.com/main-blog-post/gauging-slashing-risks-of-symbiotic-networks) - MEV Capital details a weighted, multi-category scoring method for evaluating the slashing risk a restaking vault takes on when it backs a network. (advanced)
- [The Physics of On-Chain Lending (II)](https://dirtroads.substack.com/p/69-the-physics-of-on-chain-lending) - A deep analyst treatment of the curator role model, the fee economics, and the game theory of vault runs. (advanced)
- [DeFi's Black Box: How Risk and Yield Are Repackaged](https://chaoslabs.xyz/posts/defi-s-black-box-how-risk-and-yield-are-repackaged) - A risk-analyst critique of curator incentive misalignment and the structural weaknesses of the curator model. (intermediate)
- [Collapse of the DeFi Jenga: The Stream Finance Breakdown](https://reports.tiger-research.com/p/collapse-of-the-defi-jenga-the-stream-eng) - An analyst post-mortem of a real curation failure and how the losses propagated through curator-managed vaults. (intermediate)

## Institutional and Generalized Vault Frameworks

Generalized vault stacks built for professional asset managers, where a strategist executes whitelisted actions and an off-chain valuer prices the shares.

- [Se7en-Seas/boring-vault](https://github.com/Se7en-Seas/boring-vault) - Source for BoringVault, whose minimal core contract holds assets while the Teller handles deposits and withdrawals, the Accountant prices shares, and ManagerWithMerkleVerification plus DecoderAndSanitizer restrict strategist calls to merkle-whitelisted actions. (advanced)
- [Veda Architecture and Flow of Funds](https://docs.veda.tech/architecture-and-flow-of-funds) - Traces how deposits, share issuance, oracle-based pricing, strategy execution, and queued withdrawals move between the BoringVault, Teller, Accountant, Manager, and DecoderAndSanitizer modules. (advanced)
- [How DeFi Vaults Work: The Infrastructure Abstracting Onchain Yield](https://www.plasma.org/learn/how-defi-vaults-work-the-infrastructure-abstracting-onchain-yield) - An introductory explanation of the BoringVault module split and merkle-tree whitelisting for readers new to how institutional vaults restrict strategist actions. (beginner)
- [Aera BaseVault and Core Interactions](https://docs.aera.finance/basevault-and-core-interactions) - Aera V3 documentation on the BaseVault contract, covering guardian-submitted operations verified by merkle proofs, mandatory whitelisting, operation chaining, and configurable pre and post-operation hooks. (advanced)
- [aera-finance/aera-contracts-public](https://github.com/aera-finance/aera-contracts-public) - Versioned contract snapshots of Aera's SingleDepositorVault and MultiDepositorVault implementations and their guardian-based execution layer. (advanced)
- [Lagoon Vault Architecture Overview](https://docs.lagoon.finance/vault/architecture-overview) - Documentation on Lagoon's ERC-7540 request-and-settle flow, where a valuation provider posts NAV and a curator settles pending deposits and redemptions at a defined valuation point. (intermediate)
- [superform-xyz/v2-periphery](https://github.com/superform-xyz/v2-periphery) - Source for Superform v2 SuperVaults, an ERC-7540 vault with synchronous deposits and asynchronous redemptions that executes merkle-verified hook bundles through its strategy, aggregator, and escrow contracts. (advanced)
- [Fluid (Instadapp) contracts](https://github.com/instadapp/fluid-contracts-public) - Public source for Fluid, whose liquidity layer underpins smart lending and smart vaults that share a single collateral and debt accounting system across products. (advanced)
- [YelayLiteVault](https://github.com/YieldLayer/yelay-lite) - Source for a diamond-style ERC-1155 single-asset vault that routes deposits through configurable strategy queues managed by role-based operators. (advanced)
- [Index Coop index-protocol (Set Protocol V2)](https://github.com/IndexCoop/index-protocol) - A modular framework for tokenized, manager-curated asset baskets where a manager enables modules for issuance, trading, and strategy, maintained as a Set Protocol V2 fork. (advanced)
- [Enzyme (Onyx) Architecture Overview](https://docs.enzyme.finance/onyx-protocol/architecture/architecture-overview) - Architecture docs for the oldest onchain asset-management vault protocol, whose modular shares-plus-components design deliberately extends beyond ERC-4626 for custom fees, multi-asset strategies, and granular permissions. (intermediate)
- [VaultCraft V2 Safe Smart Vaults](https://docs.vaultcraft.io/products/v2-safe-smart-vaults) - Documents a vault framework built as Safe modules, letting a manager run strategies from a Safe multisig while depositors hold tokenized shares. (intermediate)
- [Concrete Vault Documentation](https://docs.concrete.xyz/Overview/welcome/) - Introduces a vault framework with bounded off-chain value updates and modular strategy routing for building managed earn products. (intermediate)
- [Upshift Documentation](https://docs.upshift.finance/) - Documentation for an institutional structured-yield vault platform that packages curated strategies into permissioned deposit products. (intermediate)

## Structured Products and Tranched Vaults

Vaults that split risk and return into distinct layers: principal and yield, or senior and junior tranches.

- [Strata Protocol Overview](https://docs.strata.markets/technical-documentation/protocol-overview) - Documents how Strata splits yield from a base asset into ERC-4626 Senior and Junior tranches, with a CDO orchestrator routing user actions and a gain-split that targets a benchmark Senior APR while Junior TVL absorbs first losses. (intermediate)
- [Strata-Markets/contracts](https://github.com/Strata-Markets/contracts) - Solidity source for Strata's tranching vaults, where the CDO orchestrator forwards deposits and withdrawals to two ERC-4626 meta vaults for the Junior and Senior tranches alongside separate Accounting, Strategy, and APR Feed contracts. (advanced)
- [Royco Dawn Documentation](https://docs.royco.org/) - Docs for Royco's tranching product, which splits a yield source into Senior, Junior, and Senior Liquidity Provider tranches so depositors choose a risk and liquidity profile. (intermediate)
- [roycoprotocol/royco-dawn](https://github.com/roycoprotocol/royco-dawn) - Source for Royco Dawn, splitting a yield source into junior and senior tranches with a Kernel and Accountant enforcing coverage ratios and a Yield Distribution Model routing senior yield to junior. (advanced)
- [Pendle Documentation: Introduction](https://docs.pendle.finance/pendle-v2/Introduction) - Explains how Pendle wraps yield-bearing tokens into SY and splits them into Principal Tokens and Yield Tokens so fixed principal and variable yield can be traded separately. (beginner)
- [Pendle AMM Mechanics](https://docs.pendle.finance/pendle-v2/ProtocolMechanics/LiquidityEngines/AMM) - Describes how the V2 AMM concentrates liquidity in a yield range that tightens toward maturity and serves both PT and YT trades from one PT/SY pool through flash swaps. (intermediate)
- [Pendle V2 AMM Whitepaper](https://github.com/pendle-finance/pendle-v2-resources/blob/main/whitepapers/V2_AMM.pdf) - Derives the time-dependent AMM invariant, Principal Token pricing, and fee model that Pendle V2 uses to price and trade yield. (advanced)
- [Yield Tokenization Protocols, How They Are Made: Pendle](https://mixbytes.io/blog/yield-tokenization-protocols-how-they-re-made-pendle) - An auditor's walkthrough from MixBytes of Pendle's SY standard, PT and YT minting, market and router contracts, AMM curve, and the oracle and ratchet protections against manipulation. (advanced)
- [Tranchess Whitepaper](https://docs.tranchess.com/whitepaper) - Documents how a single asset-tracking fund (QUEEN) splits into a low-volatility yield tranche (BISHOP) and a leveraged tranche (ROOK) that lend to and borrow from each other, with automatic rebalancing when leverage crosses thresholds. (intermediate)
- [tranchess/contract-core](https://github.com/tranchess/contract-core) - Solidity source for the Tranchess fund, implementing primary-market creation of QUEEN shares and their split into BISHOP and ROOK tranche tokens. (advanced)
- [Buttonwood Tranche](https://github.com/buttonwood-protocol/tranche) - Contracts that deposit a collateral token into a bond and mint a series of tranche tokens redeemed in a maturity waterfall, where senior tranches are repaid first and junior tranches absorb losses and capture upside. (advanced)
- [Notional V3: What Is fCash](https://docs.notional.finance/notional-v3/fcash/what-is-fcash) - Documents fCash, a zero-coupon-bond token defined by currency and maturity whose positive and negative balances represent fixed-rate lending and borrowing claims. (intermediate)
- [notional-finance/contracts-v3](https://github.com/notional-finance/contracts-v3) - Source for Notional V3, showing how fCash markets, fixed-to-variable settlement, and leveraged vault strategies are implemented. (advanced)
- [Napier: PT and YT, Tokenized Yield](https://docs.napier.finance/learn/protocols/pt-and-yt-tokenized-yield) - Introduces stripping an ERC-5115 target asset into a Principal Token that redeems 1:1 at maturity and a Yield Token that captures accrued yield, deployed in permissionless isolated markets. (beginner)
- [Spectra: Principal and Yield Token](https://docs.spectra.finance/core-concepts/principal-and-yield-token) - Explains how Spectra splits an interest-bearing token into a discounted Principal Token redeemable 1:1 at maturity and a Yield Token that accrues future yield. (intermediate)
- [perspectivefi/spectra-core](https://github.com/perspectivefi/spectra-core) - Implementation of Spectra's yield tokenization, including its EIP-5095 Principal Token, Yield Token, router, and factory contracts built with Foundry. (advanced)
- [Sense Finance: Core Concepts](https://docs.sense.finance/docs/core-concepts/) - Explains the Divider and Adapter design that strips a target asset into fixed-term Principal and Yield Tokens traded on the YieldSpace-based Sense Space AMM. (intermediate)
- [IPOR Protocol: Interest Rate Derivative](https://docs.ipor.io/ipor-derivatives/interest-rate-derivatives/interest-rate-derivative) - Documents IPOR's on-chain interest-rate swap in which payer and receiver exchange fixed and floating cash-flow streams against a liquidity-pool counterparty. (intermediate)
- [Term Finance Documentation](https://docs.term.finance/) - Describes a non-custodial fixed-rate lending protocol modeled on tri-party repo where recurring sealed-bid auctions clear borrowers and lenders at a single market rate. (intermediate)
- [BarnBridge Litepaper](https://github.com/BarnBridge/BarnBridge-Whitepaper/blob/master/Litepaper.md) - Outlines SMART Yield fixed-rate tranching of variable lending yield and SMART Alpha volatility tranching, both structured as senior and junior risk layers. (intermediate)
- [BarnBridge Docs: Junior Tranches](https://github.com/BarnBridge/barnbridge-docs/blob/master/sy-specs/junior-tranches.md) - Specifies junior-token accounting in SMART Yield, including how juniors absorb yield shortfalls below the senior guarantee and exit through maturing jBOND NFTs. (advanced)
- [SOFA.org Protocols](https://docs.sofa.org/technical-design/vault-classification.html) - Documents an onchain structured-products system that locks deposits in ERC-1155 vaults minting position tokens for capital-protected and leveraged payoffs settled at a fixed strike and expiry. (intermediate)
- [Alchemix v2 Transmuter](https://docs.alchemix.fi/alchemix-ecosystem/transmuter) - Explains the self-repaying-loan design, where collateral is deposited into yield strategies and a synthetic debt token is issued against it while the generated yield routes through a transmuter to repay the loan over time. (intermediate)
- [Saffron Finance (saffron-finance/saffron)](https://github.com/saffron-finance/saffron) - A historical but instructive monorepo whose senior and junior tranche pools route a fixed lower yield to senior providers and a variable residual to junior providers layered over Compound lending. (advanced)
- [Element Finance (delvtech/elf-contracts)](https://github.com/delvtech/elf-contracts) - A historical principal-and-yield-token design that splits a yield-bearing position into a principal token redeemable 1:1 at maturity and a separate yield token, a clean study of the zero-coupon split that predates much of the current PT/YT ecosystem. (advanced)
- [88mph (88mphapp/88mph-contracts)](https://github.com/88mphapp/88mph-contracts) - A historical fixed-rate design where the DInterest contract pools variable-yield deposits and pays each depositor a locked fixed rate, funded by selling the corresponding floating-rate bond to a counterparty. (advanced)
- [Yield Protocol v2 (yieldprotocol/vault-v2)](https://github.com/yieldprotocol/vault-v2) - A historical but rigorous fyToken design, ERC-20 zero-coupon tokens redeemable 1:1 after maturity that trade at a discount to give collateralized fixed-rate borrowing and lending. (advanced)

## Options and Structured-Note Vaults

Vaults that sell options or shape a payoff to generate premium, hedge, or underwrite risk.

- [Building Decentralized Option Vaults](https://www.paradigm.co/blog/decentralized-option-vaults-part-1) - A vendor-neutral engineering walkthrough from Paradigm of decentralized option vault design, covering covered-call and protective-put strategies and the auction and settlement flow that turns deposits into option premium. (intermediate)
- [Ribbon Finance: Theta Vault Architecture](https://docs.ribbon.finance/theta-vault/ribbon-v2) - The canonical description of the DeFi options vault pattern most later option-selling vaults copied, where a vault mints short options against collateral each week, auctions them for premium, and rolls at expiry. (intermediate)
- [ribbon-finance/ribbon-v2](https://github.com/ribbon-finance/ribbon-v2) - A historical but production-grade reference for how a weekly-roll option-selling vault is implemented, covering vault accounting, auction settlement, and Opyn otoken minting. (advanced)
- [Opyn Squeeth Monorepo](https://github.com/opynfinance/squeeth-monorepo) - Contracts for the Crab Strategy vault, a rare onchain example of an automated short-volatility vault that pairs long ETH collateral with short Squeeth power-perpetual debt and rebalances to stay delta-neutral. (advanced)
- [Squeeth Primer](https://medium.com/opyn/squeeth-primer-a-guide-to-understanding-opyns-implementation-of-squeeth-a0f5e8b95684) - Explains the Squeeth power perpetual and how the Crab vault earns funding by selling volatility while staying delta-neutral to ETH, the conceptual bridge to the contracts. (intermediate)
- [Cega Documentation](https://docs.cega.fi/) - Documents exotic structured notes built as EVM vaults, including fixed coupon notes that sell out-of-the-money puts for a fixed coupon while a knock-in barrier governs principal loss on large drawdowns. (intermediate)
- [Thetanuts: Basic Vaults](https://docs.thetanuts.finance/legacy-v3/basic-vaults) - Describes Basic Vaults that sell out-of-the-money European cash-settled options to market makers and tokenize the resulting call and put positions into transferable LP tokens. (intermediate)
- [Y2K Finance: Earthquake](https://github.com/Y2K-Finance/Earthquake) - Source for a historical two-sided depeg-insurance vault built on an ERC-4626 variant with ERC-1155 epoch receipts, where a risk side underwrites stablecoin depeg coverage and a hedge side buys it, with collateral moving to the winning side at settlement. (advanced)

## Restaking and LRT Vaults

Vaults built for restaking and liquid restaking, where deposits back external networks and take on slashing risk.

- [Symbiotic Vault (Core Concepts)](https://docs.symbiotic.fi/learn/core-concepts/vault) - Documents how a Symbiotic vault holds and delegates restaked collateral to networks, and how deposit, withdraw, and slashing accounting work across epochs. (intermediate)
- [symbioticfi/core](https://github.com/symbioticfi/core) - Source for Symbiotic's core restaking contracts, including the vault, delegator, and slasher modules that compose into a restaking market. (advanced)
- [Mellow Vault Architecture](https://docs.mellow.finance/core-vaults/architecture/vaults/vault) - Explains Mellow's modular LRT vault design, how a vault composes deposit, strategy, and validator-management modules to build a liquid restaking token. (intermediate)
- [mellow-finance/flexible-vaults](https://github.com/mellow-finance/flexible-vaults) - Source for Mellow's flexible vault framework, a modular system for assembling restaking and yield vaults from swappable components. (advanced)
- [Mellow Flexible Vaults: Architecture, Workflows, and Security Model](https://crypto.training/blog/2026-01-05-mellow-architecture-and-workflows/) - A detailed third-party walkthrough of the Flexible Vaults architecture, the deposit and withdrawal workflows, and the security model. (advanced)
- [Byzantine-Finance/byzantine-contracts](https://github.com/Byzantine-Finance/byzantine-contracts) - Source for a restaking aggregation layer that deploys strategy vaults routing deposits across EigenLayer, Symbiotic, and native staking. (advanced)

## Credit and RWA Vaults

Vaults that fund undercollateralized credit or tokenized real-world assets, usually with a senior and junior structure.

- [Maple Smart Contract Architecture](https://docs.maple.finance/technical-resources/protocol-overview/smart-contract-architecture) - Documents how Maple structures lending pools, pool delegates, loan managers, and withdrawal queues for institutional undercollateralized lending. (intermediate)
- [maple-labs/pool-v2](https://github.com/maple-labs/pool-v2) - Source for Maple's V2 pools, showing the ERC-4626 pool, pool manager, loan manager, and withdrawal-manager contracts that run a managed credit book. (advanced)
- [Huma Tranche Deposit Mechanics](https://docs.huma.finance/products/huma-institutional/tranches/deposit) - Explains how Huma splits a receivables-financing pool into a senior tranche with capped fixed yield and a junior tranche that takes first loss for the residual. (intermediate)
- [00labs/huma-contracts-v2](https://github.com/00labs/huma-contracts-v2) - Source for Huma Protocol V2, implementing tranched pools, credit lines, and the receivable-backed lending flow. (advanced)
- [OpenTrade Blockchain Protocol](https://docs.opentrade.io/developers/blockchain-protocol) - Documents a vault-based protocol for tokenized fixed-income and treasury products, covering the deposit, settlement, and redemption flow for institutional RWA yield. (intermediate)
- [Goldfinch Protocol](https://dev.goldfinch.finance/docs/reference/how-the-protocol-works) - Structures each borrower pool into a junior first-loss tranche funded by backers and a senior second-loss tranche funded by a pooled senior vault, applying repayments to the senior tranche first. (intermediate)
- [MetaStreet v2](https://github.com/metastreet-labs/metastreet-contracts-v2) - Pools lender capital into per-collection NFT lending vaults where depositors set their own price ticks, composing those ticks into senior and junior tranche exposure without an external oracle. (advanced)
- [Tinlake (Centrifuge legacy)](https://github.com/centrifuge/tinlake) - Centrifuge's historical V1 securitization contracts that pool NFT-collateralized real-world assets and issue a senior DROP tranche protected against defaults and a junior TIN tranche that takes first loss for higher yield. (advanced)

## Stablecoin and Synthetic-Dollar Vaults

Yield-bearing stablecoins and synthetic dollars implemented as vaults.

- [Sky sUSDS (SUsds.sol)](https://github.com/sky-ecosystem/sdai/blob/susds/src/SUsds.sol) - Source for the sUSDS savings token, an ERC-4626 vault that accrues the Sky Savings Rate to USDS depositors through an internal rate-per-second accumulator. (advanced)
- [Ethena StakedUSDe.sol](https://github.com/ethena-labs/bbp-public-assets/blob/main/contracts/contracts/StakedUSDe.sol) - Source for sUSDe, an ERC-4626 staking vault that distributes protocol yield to USDe stakers with a vesting mechanism and a cooldown-gated withdrawal path. (advanced)
- [Origin ARM (Automated Redemption Manager)](https://github.com/OriginProtocol/arm-oeth) - Source for Origin's ARM, a vault that provides instant redemption liquidity for a liquid staking token by holding a buffer and arbitraging the redemption queue. (advanced)
- [Resolv: Staking stUSR and wstUSR](https://docs.resolv.xyz/litepaper/using-resolv/usr/stake) - Documents how the USR synthetic dollar is staked into the yield-bearing stUSR and its wrapped ERC-4626 form wstUSR, and how insurance-pool yield is distributed. (intermediate)

## Liquidity Management Vaults

Vaults that manage concentrated liquidity positions and rebalance their price ranges.

- [Gamma Strategies Hypervisor](https://github.com/GammaStrategies/hypervisor) - A widely forked fungible-share vault that manages a concentrated Uniswap V3 liquidity position and rebalances its price ranges through a supervisor contract. (intermediate)
- [Arrakis V2 Core](https://github.com/ArrakisFinance/v2-core) - Source for Arrakis V2 vaults, which manage concentrated liquidity across multiple price ranges and expose the position as a fungible token with programmable rebalancing. (advanced)
- [Steer Protocol Documentation](https://docs.steer.finance/) - Documents a framework for automated concentrated-liquidity management vaults where off-chain strategy executors rebalance ranges within on-chain guardrails. (intermediate)

## Asynchronous and Multi-Strategy Architecture

Vaults that cannot settle atomically, where deposits and redemptions become requests fulfilled in a later epoch, and vaults that allocate across many strategies at once.

- [OpenZeppelin ERC-7540: Asynchronous Tokenized Vaults](https://docs.openzeppelin.com/community-contracts/erc7540) - Documents a modular ERC-7540 base with admin-controlled and time-delayed fulfillment strategies, covering the request lifecycle, controller and operator authorization, and preview-function security. (advanced)
- [Centrifuge Protocol Vaults Architecture](https://docs.centrifuge.io/developer/protocol/architecture/vaults/) - Explains how Centrifuge structures BaseVault, async and sync-deposit vault variants, request managers, and transfer hooks within its hub-and-spoke multi-chain design. (advanced)
- [centrifuge/protocol](https://github.com/centrifuge/protocol) - The onchain asset-management protocol combining an immutable core with modular extensions for vaults, cross-chain adapters, valuation, and balance-sheet accounting. (advanced)
- [Securing Lagoon's Asynchronous ERC-7540 Vaults from V1 to V5](https://www.nethermind.io/blog/securing-lagoons-asynchronous-erc-7540-vaults-as-the-protocol-scaled-from-v1-to-v5) - An auditor's account from Nethermind of the failure modes specific to async vaults, including pending-to-settled state transition errors, races between synchronous and asynchronous paths, and share-price manipulation. (advanced)
- [Manage Adapters (Morpho Vaults V2)](https://docs.morpho.org/curate/tutorials-v2/listing-adapters/) - Walks through registering adapters via the timelocked flow, setting caps on adapter, collateral, and market ids, and safely delisting adapters by zeroing caps first. (advanced)

## Deep-Dive Articles and Research

Long-form analysis of vault design and the economy that has grown around it.

- [The Steakhouse View on Vaults](https://kitchen.steakhouse.financial/p/the-steakhouse-view-on-vaults) - Argues that a vault should meet three properties: trustless onchain NAV accounting, a strategy that is transparent and announced in advance, and strict noncustodiality that keeps depositors in control of their withdrawals. (intermediate)
- [Curators Explained](https://morpho.org/blog/curators-explained/) - Introduces the curator role, how curators set strategy and earn management and performance fees, and four dimensions for evaluating one: track record, transparency, communication, and conflicts of interest. (beginner)
- [Morpho Vaults V2: The Latest DeFi Breakthrough](https://www.bankless.com/read/morpho-vaults-v2-defi) - Describes Vaults V2, the owner, curator, allocator, sentinel role split, the adapter layer that routes a single vault to multiple protocols, and the exposure caps and access controls. (intermediate)
- [DeFi Curators in 2025: Navigating Chaos, Building Resilience](https://chorus.one/reports-research/defi-curators-in-2025-navigating-chaos-building-resilience) - Traces curator TVL growth and analyzes how the 2025 Balancer exploit and Stream Finance collapse propagated through curator-managed vaults. (intermediate)
- [The Vault Economy](https://sentora.com/research/reports/the-vault-economy) - Surveys vault types from single-protocol earn vaults to multi-strategy cross-chain deployments and presents a risk taxonomy that treats collateral selection as the central curation decision. (intermediate)
- [Institutionalizing Risk Curation in Decentralized Credit](https://arxiv.org/html/2512.11976v1) - An academic study modeling DeFi lending as a two-layer system of ERC-4626 vaults and third-party curators, measuring curator concentration and correlated tail risk across Aave, Morpho, and Euler, and proposing standardized onchain disclosures. (advanced)
- [YieldSpace: An Automated Liquidity Provider for Fixed Yield Tokens](https://yield.is/YieldSpace.pdf) - The paper deriving the YieldSpace constant-power invariant, an AMM curve whose marginal price tracks a constant interest rate to maturity so a pool can quote fixed yields on discount tokens. (advanced)

## Videos, Talks, and Courses

Watch someone build and reason through a vault.

- [ERC4626 Part 1: Tokenized Vault Explained](https://www.youtube.com/watch?v=dZFS8tTQiD4) - Walks through a minimal ERC-4626 vault in Solidity and shows how deposit, mint, withdraw, and redeem convert between assets and shares. (beginner)
- [ERC4626 Vault Smart Contract Tutorial](https://www.youtube.com/watch?v=ftfsCxG1560) - A build-along tutorial that implements a tokenized vault on the standard and adds an entry and exit fee variant with source code. (beginner)
- [Solidity Fridays with Joey Santoro: ERC 4626 Discussion](https://www.youtube.com/watch?v=L8dijE5qhTg) - A discussion with ERC-4626 co-author Joey Santoro on the motivation behind the standard and its interface design decisions. (intermediate)
- [Unchaining DeFi With ERC-4626: The Tokenized Vault Standard](https://www.youtube.com/watch?v=PSVC4GA3aVg) - Explains how ERC-4626 standardizes yield-bearing vault interfaces and what that composability enables for integrators and aggregators. (intermediate)
- [Yearn V3: Permissionless Vaults with ERC4626 and Tokenized Strategies](https://www.youtube.com/watch?v=STWqIBX9yZk) - Covers Yearn V3's modular architecture, where ERC-4626 tokenized strategies plug into multi-strategy vaults with optional periphery contracts. (advanced)
- [Securing ERC4626 Implementations](https://www.youtube.com/watch?v=5KVD7EX6HWQ) - Reviews common security pitfalls including the first-depositor inflation attack, rounding direction, and donation manipulation. (advanced)
- [Advanced Smart Contract Development With Foundry](https://updraft.cyfrin.io/courses/advanced-foundry) - A free multi-project Cyfrin course covering DeFi protocol, stablecoin, and cross-chain rebase token development with vault accounting and testing practice. (intermediate)

## Live Data, Dashboards, and Risk

Study real vaults in production, and the tools that rate their risk. Tie the numbers you see back to the accounting you have read.

- [DeFiLlama Yields](https://defillama.com/yields) - Ranks live yield-bearing pools and vaults across chains by TVL and APY, with filters for ERC-4626 vaults and a view of each pool's underlying strategy. (beginner)
- [vaults.fyi](https://app.vaults.fyi) - Aggregates ERC-4626 and curator-managed vaults across networks into one table of TVL, seven-day yield, and holder counts for side-by-side comparison. (beginner)
- [Yearn Vaults Explorer](https://yearn.fi/vaults) - Lists Yearn v2 and v3 vaults with net APY after fees, TVL, and chain, letting you tie front-end numbers back to on-chain vault accounting. (beginner)
- [Morpho App](https://app.morpho.org) - The production interface for MetaMorpho vaults, where each vault shows its allocation across Morpho Blue markets, supply caps, curator, and live rates. (beginner)
- [Morpho Data Dashboards](https://docs.morpho.org/developers/ecosystem/data-dashboards) - Morpho's documentation index of its official Dune dashboards, including vault performance and vault curators, pointing to the canonical MetaMorpho analytics. (intermediate)
- [Morpho Vaults and Curators Analysis (Dune)](https://dune.com/morpho/vaults-curators-analysis) - An official Dune dashboard breaking MetaMorpho vaults and their curators down by TVL, allocation, reallocation activity, and yield on Ethereum and Base. (intermediate)
- [Veda Dashboard (Dune)](https://dune.com/veda/veda) - A Dune dashboard tracking TVL and capital flows across Veda's BoringVault deployments. (intermediate)
- [Xerberus Documentation](https://documentation.xerberus.io/) - Documents an onchain risk-rating API that exposes per-vault and per-asset scores decomposing a vault into subscores across smart-contract, oracle, custody, and economic risk. (intermediate)
- [DD.xyz](https://www.dd.xyz/) - A vault data and risk platform from Webacy that assigns live risk scores and listing verdicts to ERC-4626 vaults across Morpho, Aave, Compound, and Yearn, alongside stablecoin peg and RWA monitoring. (intermediate)
- [DIA DeFi Vaults and Lending Map](https://www.diadata.org/map/defi-vaults-lending/) - Tracks thousands of vaults across many chains with per-vault audits, TVL, oracle and timelock configuration, and curator track records. (intermediate)
- [Dune Curated Vaults Data Catalog](https://docs.dune.com/data-catalog/curated/vaults/overview) - Documents the decoded on-chain event tables Dune exposes for Morpho, Euler v2, Aave v3, Fluid, and other vault protocols, the raw tables for writing custom vault queries. (advanced)
- [List All ERC-4626 Vaults On-Chain](https://web3-ethereum-defi.readthedocs.io/tutorials/erc-4626-vault-list.html) - A tutorial that programmatically enumerates every ERC-4626 vault across chains from on-chain data, showing how to detect and read deployed vaults directly instead of through a front-end. (advanced)

## Contributing

Contributions are welcome and held to a high bar. Read [CONTRIBUTING.md](CONTRIBUTING.md) for the format and the quality checklist before opening a pull request. In short: link to the primary source, describe what the reader learns in one neutral sentence, tag the level, and keep hype out.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

Released under [CC0 1.0](LICENSE).
