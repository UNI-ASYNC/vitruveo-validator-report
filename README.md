# Vitruveo V2: validator and node report

**Research date:** 24 November 2026

**Scope:** Public Vitruveo, Vertical Foundation, Pretrend and TechBubble source
code and documentation. This is a snapshot of published material, not
independent confirmation of the live network or its deployed contracts.

## Executive summary

Vitruveo is an EVM-compatible blockchain with a validator network and
additional services built around it. **Validator, HOST processor, bridge
notary, Pretrend notary and oracle are different jobs.** The same operator may
perform more than one, but the roles are not interchangeable.

The published validator program describes three node tiers:

- **Basic:** chain validation and the published Basic rewards.
- **Super:** validation, plus eligibility for HOST processing and bridge notary work.
- **Hyper:** Super eligibility, plus eligibility for Pretrend notary work.

**Eligibility does not prove appointment, operation of a service, or additional
fees.** Pretrend's documentation describes its oracle and notary services as
operated by Pretrend; it does not establish that Hyper-node operators already
run them.

## Hardware comparison: XRPL, XRPL EVM and Vitruveo

A separate infrastructure comparison covers node roles, color coding, published hardware guidance, CPU/RAM/storage charts, archive-versus-validator distinctions, and proposed Vitruveo planning targets.

**[Read the full hardware comparison](hardware-comparison.md)**

The comparison distinguishes published recommendations from local experiments and provisional planning targets.

## Production readiness and open verification questions

A companion inventory identifies areas where the public sources reviewed did not establish a complete answer, or where further testing is needed. **Unverified items are not claims that Vitruveo lacks those capabilities or practices.**

**[Read the production-readiness and unknowns inventory](production-readiness-and-unknowns.md)**

## 1. The Vitruveo chain

Vitruveo describes itself as an EVM-compatible layer-one blockchain:
Ethereum-style wallets and smart contracts can interact with its own network.

| Item | Published information |
|---|---|
| Chain ID | **1490** |
| Native currency | **VTRU**, used for transaction gas |
| Published block time | **5 seconds** |
| RPC | `https://rpc.vitruveo.ai` |
| Explorer | `https://explorer.vitruveo.ai` |

The protocol repository contains a **Clique proof-of-authority** consensus
implementation. Its presence does not by itself prove the live chain's current
consensus configuration or validator membership. The Foundation says
validators process transactions and secure consensus. Sources: [network
configuration](https://github.com/Vitruveo/vitruveo-website/blob/1028a96b982a581ca11cb8ff8ad8f6f9cf56bd59/vitruveo/lib/psc-constants.js),
[Clique implementation](https://github.com/Vitruveo/vitruveo-protocol/blob/670a2a1edaaa6e83e845947cc96ac564fca39f0e/consensus/clique/clique.go),
[Foundation overview](https://www.verticalfoundation.net/).

### Validator duty

A chain validator runs infrastructure participating in the network, keeps it
synchronized and, when authorized and scheduled, signs or produces blocks. This
is the **chain-level job**. It is separate from checking a bridge transfer,
settling a prediction market or delivering an HTTP request.

The Foundation says it oversees validator onboarding and health, and reports
reducing the planned validator set from **380 to 100**. It also says the
protocol footprint was reduced so validators can run on an AWS `t3.small`;
that is a published claim, not a complete hardware or capacity specification.
[Source](https://www.verticalfoundation.net/news/).

## 2. Validator node levels

These are the **published program terms**, not a guarantee of live rewards or
returns. [Source: validator benefits](https://github.com/Vitruveo/vf-website/blob/b46f8f51a2de77a92b1534957838142ba4ac1b8b/app/validators/NodeBenefits.tsx).

| Feature | Basic | Super | Hyper |
|---|---:|---:|---:|
| Published node price | 100 USDT | 200 USDT | 3,000 USDT |
| Published block reward | 1 VTRU per block mined | 2 VTRU per block mined | 2 VTRU per block mined |
| Published five-year maximum | 315,360 VTRU | 630,720 VTRU | 630,720 VTRU |
| VIBE shares | 10 | 10 | 10 |
| Eligible for HOST processor role | Not listed | Yes | Yes |
| Eligible for bridge notary role | Not listed | Yes | Yes |
| Eligible for Pretrend notary role | Not listed | Not listed | Yes |
| Two-hour installation support | Optional, +100 USDT | Optional, +100 USDT | Included |

The Foundation says rewards and eligibility apply while a node is **active**,
meaning fully paid under the described purchase contract. Its calculator says
actual rewards vary with uptime and rotation; token-price examples are
hypothetical. Rewards are stated **per block mined**, not per every block
produced by the network, and are not guaranteed income. [Source:
calculator](https://github.com/Vitruveo/vf-website/blob/b46f8f51a2de77a92b1534957838142ba4ac1b8b/app/validators/NodeCalculator.tsx).

### Legacy validators

The benefits page also lists a **Legacy** category for original validators:
10,000 VTRU, with specified unlocking periods, and 131,500 Pretrend VIP units.
It is presented separately from the Basic/Super/Hyper offer and should not be
assumed to be an additional benefit for every new node.
[Source](https://github.com/Vitruveo/vf-website/blob/b46f8f51a2de77a92b1534957838142ba4ac1b8b/app/validators/NodeBenefits.tsx).

### Registration and payment

The published validator UI guide describes **100 numbered slots**, with slots
1-10 initially reserved. It describes one node record per wallet, installment
payments, and a node becoming active once fully paid. The chosen tier and
support option cannot be changed after claiming. Prices are described as
changeable on-chain. [Source: validator UI guide](https://github.com/Vitruveo/vf-website/blob/b46f8f51a2de77a92b1534957838142ba4ac1b8b/VALIDATORS_UI.md).

**Chain distinction:** the purchase/registration interface operates on **BNB
Smart Chain, chain ID 56**, using USDT. The Vitruveo blockchain uses **chain ID
1490**. Paying for a slot is not by itself proof that infrastructure is online,
authorized to sign blocks, or appointed to a service role.

## 3. Roles and duties

| Role | Main job | Where it operates | Published tier eligibility |
|---|---|---|---|
| **Validator** | Participate in chain consensus and block production | Vitruveo network | Basic, Super, Hyper |
| **HOST request processor** | Deliver an authorized contract's signed HTTP webhook | Validator infrastructure and external HTTP endpoint | Super, Hyper |
| **Bridge notary** | Sign or attest to bridge-related actions or receipts | Bridge service across chains | Super, Hyper |
| **Pretrend oracle** | Collect and publish external time-series data for markets | Off-chain TrendPulse service | No validator-tier eligibility specified |
| **Pretrend notary** | Freeze market parameters, resolve or void eligible markets, and record results | Off-chain service interacting with Vitruveo contracts | Hyper is listed as eligible |
| **Trend precompile** | Perform deterministic trend calculations | Native code inside the Vitruveo protocol | Not a node tier or human role |

### HOST request processor

**HOST** means **HTTP Outbound Service Trigger**. It lets an authorized smart
contract initiate an outbound webhook to an external application, such as an
AI agent or business system.

The documented access chain is:

1. A registry administrator registers the validator node, its public key and owner.
2. The validator owner authorizes a developer.
3. The developer authorizes a deployed smart contract.
4. The contract creates a request and calls HOST.
5. The designated validator processes and signs the outbound request.

[Source: HOST primer](https://github.com/Vitruveo/vitruveo-website/blob/1028a96b982a581ca11cb8ff8ad8f6f9cf56bd59/app/%28site%29/host-primer/page.jsx).

**Implementation detail:** the reviewed protocol code emits a request log
during EVM execution. A node-level watcher subsequently reads successful
canonical-block receipts and dispatches the webhook outside the EVM. The code
includes retries, but delivery is not guaranteed: it skips old blocks, uses
in-memory deduplication, and a webhook already sent cannot be recalled if its
block is later reorganized out. A valid signature authenticates the signed
payload; it does not prove that the external service acted correctly.
[Sources: HOST implementation](https://github.com/Vitruveo/vitruveo-protocol/blob/670a2a1edaaa6e83e845947cc96ac564fca39f0e/core/vm/precompile_host.go),
[webhook watcher](https://github.com/Vitruveo/vitruveo-protocol/blob/670a2a1edaaa6e83e845947cc96ac564fca39f0e/eth/host_webhook.go).

This is more specific than website wording suggesting that the HTTP request
itself happens during transaction execution. The code reviewed sends it after
the relevant block is written.

### Bridge notary

A bridge moves or represents assets between chains. A **bridge notary** is a
service role associated with signing bridge actions or receipts; it is not the
same duty as signing a Vitruveo block.

Pretrend's documentation says its notary signs USDT bridge receipts and
describes an escrow/refund process before a receipt is signed. The Foundation's
node benefits list Super and Hyper as eligible for bridge-notary work. The
public materials reviewed do **not** establish the complete signing threshold,
operator selection process, compensation or deployment status for independent
node operators. [Source: Pretrend protocol](https://github.com/Vitruveo/pretrend-app/blob/95a0a694200bb6a35804d9cbaf5362dd915badd0/app/protocol/page.tsx).

### Oracle: TrendPulse

An **oracle** supplies information from outside a blockchain. Pretrend
describes **TrendPulse** as an off-chain service that records feeds, publishes
time-series data and serves live trend statistics.

For example, the published market configuration identifies pump.fun buy-volume
data and BTC/USDT data as feeds. The oracle's data is an input to a market; the
validator network does not independently discover whether external data is
accurate merely by processing a transaction.
[Sources: Pretrend architecture](https://github.com/Vitruveo/pretrend-app/blob/95a0a694200bb6a35804d9cbaf5362dd915badd0/app/protocol/page.tsx),
[market configuration](https://github.com/Vitruveo/pretrend-app/blob/95a0a694200bb6a35804d9cbaf5362dd915badd0/app/config/markets.js).

Pretrend states that **TrendPulse and the notary are operated by Pretrend**.
Its deterministic computation can be rerun from the published series, but that
does not eliminate trust in the collection and availability of underlying data.

### Pretrend notary

The Pretrend notary is the **market-settlement service**, distinct from the
oracle that collects the feed. According to Pretrend's documentation, it:

1. Freezes outcome-bucket boundaries when a market opens.
2. At close, fits the trend using recorded points and signs and records the result.
3. Resolves eligible markets or voids them when stated data requirements are not met.
4. Also signs USDT bridge receipts in the described Pretrend architecture.

Pretrend says markets are voided when the feed fails, fewer than a quarter of
expected points are recorded, or no result is recorded within 24 hours, with
buy amounts refunded. The status UI monitors issues including missing notary
permission, low VTRU gas and overdue settlements.
[Sources: protocol](https://github.com/Vitruveo/pretrend-app/blob/95a0a694200bb6a35804d9cbaf5362dd915badd0/app/protocol/page.tsx),
[notary status](https://github.com/Vitruveo/pretrend-app/blob/95a0a694200bb6a35804d9cbaf5362dd915badd0/app/trendpulse/_components/notary-status.jsx).

The Foundation lists **Hyper** as eligible for Pretrend notary work, but the
reviewed documentation does not explain how Hyper operators are selected,
onboarded or paid for that duty.

## 4. How the pieces fit together

A simplified Pretrend example:

```text
External data source
       |
TrendPulse oracle records the data
       |
Pretrend notary sets market boundaries and submits a result
       |
Vitruveo contracts process settlement
       |
Validators process and secure the chain transactions
```

For a separate outbound integration, a contract can request:

```text
Authorized contract -> HOST request -> designated validator processor
                    -> signed HTTP webhook -> external application
```

Pretrend's current protocol page says its market settlement happens **on
Vitruveo with no cross-chain step**. A broader Foundation overview also
describes oracle results being pushed to other chains via HOST; that should not
be assumed to describe every current Pretrend settlement.
[Sources: Pretrend](https://github.com/Vitruveo/pretrend-app/blob/95a0a694200bb6a35804d9cbaf5362dd915badd0/app/protocol/page.tsx),
[Foundation](https://www.verticalfoundation.net/).

## 5. TechBubble repository findings

The publicly accessible TechBubble repositories examined did not add further
details about validator tiers, HOST duties or notary operations. TechBubble's
former `pretrend-website` repository redirects to Vitruveo's Pretrend app,
already included above. The
[Hashdle documentation](https://github.com/techbubble/hashdle/blob/main/README.md)
independently identifies Vitruveo as its blockchain and confirms chain ID
1490, but adds no node-duty information.

An older [chain metadata file](https://github.com/techbubble/chains/blob/master/_data/chains/eip155-1490.json)
also confirms the chain ID but uses `.xyz` endpoints rather than the `.ai`
endpoints in the newer Vitruveo documentation. Public repository access does
not establish what may exist in inaccessible repositories.

## 6. Questions requiring confirmation

Before relying on this report for a purchase or operating commitment, confirm:

- The live validator count, consensus configuration, appointment and removal process.
- Whether Super/Hyper service eligibility is operational, and how operators are selected.
- Separate fees, rewards, uptime obligations and penalties for HOST, bridge and Pretrend notary work.
- Bridge signing thresholds, key custody, safeguards and recovery procedures.
- Whether the published rewards and prices are deployed and unchanged.
- Complete hardware, bandwidth, security, backup and maintenance requirements for each role.

**Bottom line:** Basic, Super and Hyper are program tiers; HOST processing and
notary work are additional service roles; and TrendPulse is an oracle service,
not another name for a validator node.
