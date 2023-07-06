# ✨ So you want to run an audit

This `README.md` contains a set of checklists for our audit collaboration.

Your audit will use two repos: 
- **an _audit_ repo** (this one), which is used for scoping your audit and for providing information to wardens
- **a _findings_ repo**, where issues are submitted (shared with you after the audit) 

Ultimately, when we launch the audit, this repo will be made public and will contain the smart contracts to be reviewed and all the information needed for audit participants. The findings repo will be made public after the audit report is published and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the audit sponsor (⭐️)**.

---

# Audit setup

## 🐺 C4: Set up repos
- [ ] Create a new private repo named `YYYY-MM-sponsorname` using this repo as a template.
- [ ] Rename this repo to reflect audit date (if applicable)
- [ ] Rename auditt H1 below
- [ ] Update pot sizes
- [ ] Fill in start and end times in audit bullets below
- [ ] Add link to submission form in audit details below
- [ ] Add the information from the scoping form to the "Scoping Details" section at the bottom of this readme.
- [ ] Add matching info to the Code4rena site
- [ ] Add sponsor to this private repo with 'maintain' level access.
- [ ] Send the sponsor contact the url for this repo to follow the instructions below and add contracts here. 
- [ ] Delete this checklist.

# Repo setup

## ⭐️ Sponsor: Add code to this repo

- [ ] Create a PR to this repo with the below changes:
- [ ] Provide a self-contained repository with working commands that will build (at least) all in-scope contracts, and commands that will run tests producing gas reports for the relevant contracts.
- [ ] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 24 hours prior to audit start time.**
- [ ] Be prepared for a 🚨code freeze🚨 for the duration of the audit — important because it establishes a level playing field. We want to ensure everyone's looking at the same code, no matter when they look during the audit. (Note: this includes your own repo, since a PR can leak alpha to our wardens!)


---

## ⭐️ Sponsor: Edit this README

Under "SPONSORS ADD INFO HERE" heading below, include the following:

- [ ] Modify the bottom of this `README.md` file to describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. ([Here's a well-constructed example.](https://github.com/code-423n4/2022-08-foundation#readme))
  - [ ] When linking, please provide all links as full absolute links versus relative links
  - [ ] All information should be provided in markdown format (HTML does not render on Code4rena.com)
- [ ] Under the "Scope" heading, provide the name of each contract and:
  - [ ] source lines of code (excluding blank lines and comments) in each
  - [ ] external contracts called in each
  - [ ] libraries used in each
- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Does the token conform to the ERC-20 standard? In what specific ways does it differ?
- [ ] Describe anything else that adds any special logic that makes your approach unique
- [ ] Identify any areas of specific concern in reviewing the code
- [ ] Review the Gas award pool amount. This can be adjusted up or down, based on your preference - just flag it for Code4rena staff so we can update the pool totals across all comms channels. 
- [ ] Optional / nice to have: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] See also: [this checklist in Notion](https://code4rena.notion.site/Key-info-for-Code4rena-sponsors-f60764c4c4574bbf8e7a6dbd72cc49b4#0cafa01e6201462e9f78677a39e09746)
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# PoolTogether audit details
- Total Prize Pool: $121,650 USDC
  - HM awards: $61,875 USDC
  - Analysis awards: $3,750 USDC
  - QA awards: $1,875 USDC
  - Bot Race awards: $5,625 USDC
  - Gas awards: $1,875 USDC
  - Judge awards: $13,000 USDC
  - Lookout awards: $6,000 USDC
  - Scout awards: $500 USDC
  - Mitigation Review: $27,150 USDC (*Opportunity goes to top 3 certified wardens based on placement in this audit.*)
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2023-07-pooltogether/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts July 7, 2023 20:00 UTC
- Ends July 14, 2023 20:00 UTC

## Automated Findings / Publicly Known Issues

Automated findings output for the audit can be found [here](add link to report) within 24 hours of audit opening.

*Note for C4 wardens: Anything included in the automated findings output is considered a publicly known issue and is ineligible for awards.*

# Overview

PoolTogether is a prize savings protocol. Yield on deposits is awarded periodically as random prizes. PoolTogether is a gamification layer that can be added to any yield-bearing asset. We like to call them prize-wrapped assets.

PoolTogether V5 is a major leap forward for the protocol. The system is:

- Fully autonomous. There are no admin controls; prize sizes and counts adapt automatically.
- Automated. All external functions are incentivized so that the protocol continues to run in perpetuity.
- Permissionless. Anyone can add new assets or yield sources to the protocol by adding new vaults.

You can read more about the protocol design in our [official documentation](https://dev.pooltogether.com/protocol/next/design/)

# Scope

| Contract | SLOC | Purpose | Libraries used |  
| ----------- | ----------- | ----------- | ----------- |
| [claimer/src/lib/LinearVRGDALib.sol](./claimer) | 51 | Variable-Rate Gradual Dutch Auction Math | [`prb-math`](https://github.com/PaulRBerg/prb-math), [`solmate`](https://github.com/transmissions11/solmate) |
| [claimer/src/Claimer.sol](./claimer) | 86 | Incentivizes prize claims for a Prize Pool | [`prb-math`](https://github.com/PaulRBerg/prb-math), [`openzeppelin`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [prize-pool/src/abstract/TieredLiquidityDistributor.sol](./prize-pool) | 753 | Distributes liquidity as tiered prizes | [`prb-math`](https://github.com/PaulRBerg/prb-math) |
| [prize-pool/src/libraries/DrawAccumulatorLib.sol](./prize-pool) | 286 | Distributes liquidity over future draws using exponential smoothing | [`prb-math`](https://github.com/PaulRBerg/prb-math), [`ring-buffer-lib`](https://github.com/generationsoftware/ring-buffer-lib) |
| [prize-pool/src/libraries/TierCalculationLib.sol](./prize-pool) | 81 | Computes tier odds, prize counts, and winners | [`prb-math`](https://github.com/PaulRBerg/prb-math) |
| [prize-pool/src/libraries/UD34x4.sol](./prize-pool) | 35 | Provides an unsigned 34x4 precision fixed point decimal | [`prb-math`](https://github.com/PaulRBerg/prb-math) |
| [prize-pool/src/PrizePool.sol](./prize-pool) | 528 | Distributes liquidity as tiered prizes | [`prb-math`](https://github.com/PaulRBerg/prb-math), [`openzeppelin`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [twab-controller/src/libraries/ObservationLib.sol](./twab-controller) | 45 | Tracks a ring buffer of balance observations and provides a fast binary search algorithm to access them | [`ring-buffer-lib`](https://github.com/generationsoftware/ring-buffer-lib) |
| [twab-controller/src/libraries/OverflowSafeComparatorLib.sol](./twab-controller) | 21 | Provides an overflow-safe means of comparing two 32-bit timestamps |  |
| [twab-controller/src/libraries/TwabLib.sol](./twab-controller) | 442 | Provides an accounts struct and logic to mint, burn and transfer balance and measure twabs. | [`ring-buffer-lib`](https://github.com/generationsoftware/ring-buffer-lib) |
| [twab-controller/src/TwabController.sol](./twab-controller) | 393 | Allows contracts to track balances and look up the time-weighted average balance of an account | |
| [vault/src/interfaces/IVaultHooks.sol](./vault) | 20 | Provides a standard interface with which to attach hooks to vaults | |
| [vault/src/Vault.sol](./vault) | 540 | An ERC-4626 compatible vault that uses a Twab Controller to track balances. The contract is bound to an ERC-4626 yield source and exposes yield to a liquidator. | [`openzeppelin`](https://github.com/OpenZeppelin/openzeppelin-contracts), [`owner-manager-contracts`](https://github.com/pooltogether/owner-manager-contracts) |
| [vault/src/VaultFactory.sol](./vault) | 43 | Provides a mean to easily create new Vaults and find them | [`openzeppelin`](https://github.com/OpenZeppelin/openzeppelin-contracts) |

## Out of Scope

1. The yield from each Vault is converted to POOL by the Liquidator. The Liquidator has not been fully finalized, so that contract is out of scope. You can assume that the Liquidator will also be a singleton contract per-chain.
2. The Draw Auction is the mechanism that pushes new draws to the Prize Pool. It leverages the same RNG source as previous versions of PoolTogether, and incentivizes anyone to trigger the transactions using a reverse dutch auction. The contracts are not complete, but you can assume the auction will be a singleton contract on each chain, as with the Liquidator.  The Draw Auction will have the privileged ability to withdraw reserve from the Prize Pool.

# Additional Context

## Novel Mechanisms

There are a few important mathematical models used in the codebase that are important to understand:

- The Claimer uses a Variable-Rate Gradual Dutch Auction to price the claiming fees. We have tuned the algorithm to be a reverse dutch auction. You can read more about the variable rate dutch auction in the original [Paradigm article](https://www.paradigm.xyz/2022/08/vrgda). We've made the price decay greater than one to have the price increase.
- The Prize Pool receives liquidity from Vaults and distributes the liquidity across future draws with what is essentially a low-pass filter. You can read more about it in [our docs](https://dev.pooltogether.com/protocol/next/design/prize-pool#exponential-weighted-average)
- The Prize Pool is distributing prizes by generating pseudo-random numbers for each vault / account / prize combination. The user 'wins' if the PRN fits within a numeric range whose bounds are commensurate with the vault's portion of the total prize liquidity for the draw, and the given tier's odds.

## Concerns

Below we highlight protocol invariants or holistic concerns.

### Prize Liquidity

We want to make sure the prize pool doesn't award too much prize liquidity. The expectation is that the statistical model is tuned such that the rate of prizes being awarded matches the available prize liquidity for the draw.

### Prize Pool Accounting

The prize pool is a complex contract, and one of our biggest concerns is that it is tracking prize liquidity precisely. The balance of prize tokens held by the contract must always be equal to the sum of the available tier liquidity and the reserve. When contributing liquidity the prize pool will temporarily hold a balance greater than the accounted balance, but otherwise the two should match.

### Twab Manipulation

The Twab Controller should accurately measure a user's average balance held during a draw. However, we have reduced the granularity of measurements to save gas. Is it possible for a user to manipulate the twab to improve their odds of winning? For example: the user should never be able to alter their average balance in the past.

### Vault Security

The Vaults hold user funds, so it's critical that there can be no loss of funds. The liquidator is able to liquidate the accrued yield on the vault, so it's a second possible ingress point for an attacker. We want to make sure that the liquidator can't access more than the yield, and that users can't access each other's principal.

## Scoping Details 

**Repositories**

- [claimer](https://github.com/GenerationSoftware/pt-v5-claimer)
- [draw-auction](https://github.com/GenerationSoftware/pt-v5-draw-auction)
- [prize-pool](https://github.com/GenerationSoftware/pt-v5-prize-pool)
- [twab-controller](https://github.com/GenerationSoftware/pt-v5-twab-controller)
- [vault](https://github.com/GenerationSoftware/pt-v5-vault)

```
- How many contracts are in scope?:   14
- Total SLoC for these contracts?:  3335
- How many external imports are there?:  4
- How many separate interfaces and struct definitions are there for the contracts within scope?:  ~20
- Does most of your code generally use composition or inheritance?:  Composition 
- How many external calls?:   12
- What is the overall line coverage percentage provided by your tests?:  ~99.9%
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?:   False
- Please describe required context:   N/A
- Does it use an oracle?:  No
- Does the token conform to the ERC20 standard?:  Yes
- Are there any novel or unique curve logic or mathematical models?: True
- Does it use a timelock function?:  No
- Is it an NFT?: No
- Does it have an AMM?:  No
- Is it a fork of a popular project?:  No 
- Does it use rollups?:
- Is it multi-chain?:
- Does it use a side-chain?: No
```

# Tests

Within each git submodule, you can run `forge test` to run all tests. You can run `forge coverage` to see the coverage report.

Note: the Prize Pool must be compiled with `--optimize --via-ir` in order to fit within contract size limits.
