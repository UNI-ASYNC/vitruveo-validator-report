# Production Readiness — Verification Gaps and Open Questions

**Research date:** 24 November 2026  
**Purpose:** Companion inventory to the [validator and node report](README.md) and [hardware comparison](hardware-comparison.md).

## Scope and interpretation

This document records areas where the public sources reviewed for this report did not provide enough evidence to establish a complete answer, or where additional testing would be needed. **An item marked “not established” is not a claim that Vitruveo lacks the capability or that its operators do not perform the activity.** It means the capability, practice, requirement, or result was not independently established from the cited/reviewed material for this report.

The report is based on public documentation, source code that was reviewed, and limited pilot experiments. It is not a production audit, penetration test, live validator fleet inspection, or confirmation of private operator procedures.

### Status definitions

| Status | Meaning |
|---|---|
| **Documented** | Public material explicitly describes the item; this is not necessarily independent validation. |
| **Partially documented** | Some relevant details are available, but they do not establish the full picture. |
| **Not established in reviewed sources** | Research did not establish an answer from the public material reviewed. This does not mean the item is absent. |
| **Not tested** | The question requires a test or observation that has not been completed as part of this research. |

## Open verification inventory

### 1. Live network topology and validator distribution
**Status: Not established in reviewed sources**

The public material describes validator onboarding, validator responsibilities, and a validator network. This review has not independently established a current, complete inventory of active validators, their locations, hosting providers, network connections, or distribution of operators.

Questions to verify:
- Current active validator count and how that count is defined.
- Whether validators are independently operated or share ownership/operator/provider infrastructure.
- Geographic and hosting-provider distribution, and concentration risks.
- Validator selection, rotation, authorization, removal, and replacement procedures.
- Which information is public, and which is intentionally private for security reasons.

The Foundation's public overview describes its role in validator onboarding and network health; that overview is not a live topology measurement.

### 2. Validator lifecycle and operating procedures
**Status: Partially documented**

Public information describes validator tiers, onboarding, and roles. A complete independently verified operator runbook was not established in this review.

Questions to verify:
- Exact installation/configuration procedure and supported software release/update path.
- Initial synchronization and snapshot validation procedure.
- Expected uptime, monitoring, maintenance windows, and alerting.
- Restart, upgrade, rollback, and version compatibility procedures.
- Signing/block-production duties and any rotation or scheduling details.
- Operator support and escalation paths.

### 3. Host security, access controls, and key custody
**Status: Partially documented; deployment-level configuration not tested**

The HOST Primer describes a three-layer authorization chain (validator, approved developer, whitelisted application) and an encryption scheme for sensitive webhook values (ECDH with secp256k1 and AES-256-GCM). This documents specific HOST protections; it does not establish the security posture of every validator host or all signing keys.

Questions to verify:
- Validator signing-key creation, storage, access restrictions, backup, rotation, and recovery.
- Whether keys are held on the host, in a hardware device, or in another signing arrangement.
- Recommended OS hardening, patch cadence, firewall rules, SSH controls, and privileged-user practices.
- Which RPC endpoints/methods should be exposed, and how public access is restricted or authenticated.
- Separation between validator signing keys and service credentials (HOST, bridge, notary, monitoring).

### 4. Network connectivity and RPC exposure
**Status: Not established in reviewed sources**

A complete role-specific network/port matrix and verified firewall baseline were not established in this review.

Questions to verify:
- Required inbound/outbound ports and peer-discovery behavior for each node role.
- Which RPC interfaces are required locally, privately, or publicly.
- Recommended firewall/security-group rules and access controls.
- Peer-count expectations, peer quality, connection limits, and network troubleshooting.
- Whether sentry/proxy patterns are supported or recommended and for which use cases.

Do not treat general Geth defaults or another chain's port/firewall guidance as Vitruveo-specific requirements without verifying them against the Vitruveo client and network.

### 5. Recovery, backup, and continuity
**Status: Not established in reviewed sources; not tested**

The review did not establish a complete, published recovery plan for validator operators or independently test a full recovery of a validator host.

Questions to verify:
- What should be backed up (configuration, identity/key material, service files, monitoring, and other required data).
- Whether the chain database should be backed up, rebuilt, or restored from snapshots.
- Snapshot source, authenticity/integrity checks, age limits, and recovery procedure.
- Recovery steps after host, disk, provider, OS, or operator-account failure.
- Expected recovery time and any recovery-point expectations.
- How key restoration is protected and validated without risking duplicate or unsafe signing.

### 6. Performance and production capacity
**Status: Not tested at production scale**

The hardware comparison separates inherited Geth-style guidance and proposed sizing from the limited pilot. The pilot did not establish sustained peer connectivity, production synchronization performance, validator signing, or a long-duration production workload.

Questions/tests to verify:
- Initial sync duration and resource usage with current supported software and snapshots.
- CPU, RAM, disk latency/IOPS, bandwidth, and database growth under sustained operation.
- Stable peer count and sync lag under normal and degraded conditions.
- Block/signing reliability and missed-duty behavior where applicable.
- Restart, catch-up, and recovery performance.
- Whether measured requirements differ by validator, RPC, archive, HOST, or notary workloads.

Proposed hardware targets in the companion report are planning estimates, not official validated specifications.

### 7. Storage, snapshots, pruning, and long-term growth
**Status: Partially documented; growth not measured**

A snapshot is a point-in-time dataset, not a long-term storage forecast. The reviewed pilot snapshot size does not establish future database growth or disk requirements.

Questions to verify:
- Current supported sync modes and pruning/archive behavior in the Vitruveo fork.
- Actual database size by node mode and growth rate over time.
- Snapshot cadence, publication process, retention, provenance, and integrity verification.
- Storage expansion procedures and whether downtime is required.
- Archive/history service requirements, if different from ordinary validators.

### 8. Redundancy, resilience, and failure domains
**Status: Not established in reviewed sources**

This review has not independently established network-wide redundancy or tested failure scenarios.

Questions to verify:
- Provider, region, power, network, storage, and operator concentration.
- Failover expectations and whether a standby/replacement node can be used.
- Recovery behavior during peer loss, RPC failure, disk exhaustion, host outage, or provider outage.
- Monitoring and notification coverage for critical failure states.
- Whether and how redundancy is coordinated without creating signing/key risks.

### 9. Operating costs and service-role economics
**Status: Partially documented**

Published validator program pricing, tiers, and reward terms are covered in the main report. A complete ongoing cost model and independently confirmed service-role economics were not established.

Questions to verify:
- Compute, storage, snapshots/backup, bandwidth, monitoring, and support costs.
- Cost differences by cloud provider, region, and node mode.
- Separate HOST, bridge-notary, and Pretrend-notary fees/rewards, selection, workload, and uptime expectations.
- Maintenance labor, upgrade time, incident response, and replacement costs.
- Whether published program prices/rewards are currently deployed and applicable to an operator's actual terms.

Do not infer revenue or profitability from published maximum rewards or eligibility alone.

### 10. Service-role architecture and dependencies
**Status: Partially documented**

The main report distinguishes chain validators from HOST processing, bridge notary, TrendPulse oracle, and Pretrend notary. Some role-level descriptions are published; the full production deployment architecture and operator dependencies were not established.

Questions to verify:
- Which roles run on validator hosts versus separate machines or services.
- Whether service roles require additional software, credentials, APIs, monitoring, or network permissions.
- External feed, webhook, RPC, and other service dependencies and failure handling.
- Service selection/assignment, operational status, onboarding, compensation, and support for independent operators.
- Separation of duties, access, and failure containment between services.

### 11. Maintenance, upgrades, and incident handling
**Status: Not established in reviewed sources**

A complete operator maintenance and incident-response lifecycle was not established.

Questions to verify:
- Release announcement, supported versions, security updates, and upgrade windows.
- Rollback and compatibility policy.
- Incident severity, reporting, escalation, and post-incident review.
- Monitoring, logs, metrics, and retention recommendations.
- Operator communications and coordinated maintenance.

## Evidence and source starting points

- [Vitruveo validator/node report](README.md) — public research findings and source links already reviewed.
- [Hardware comparison](hardware-comparison.md) — published recommendations, pilot notes, and explicitly provisional planning targets.
- [Vitruveo HOST Primer](https://www.vitruveo.ai/host-primer) — public HOST authorization layers and encryption description.
- [Vitruveo Developer Documentation](https://www.vitruveo.ai/developers) — published protocol capabilities and network endpoints.
- [Vertical Foundation](https://www.verticalfoundation.net/) — public Foundation and validator-network overview.
- [Vitruveo Snapshot Portal](https://snapshot.vitruveo.ai/) — snapshot/node onboarding entry point (page was not independently retrievable in this research pass).

These sources are starting points, not proof that all relevant documentation has been reviewed or that live/private practices are absent.

## Recommended next verification steps

Prioritize evidence gathering in this order:

1. Obtain the official, current validator operator runbook and configuration requirements.
2. Verify current live validator/network topology using an authoritative source, while respecting security-sensitive details.
3. Confirm node-key and RPC security guidance for each node role.
4. Obtain snapshot integrity, restore, and host-recovery procedures; test recovery in a non-production environment.
5. Run controlled hardware qualification and record synchronization, sustained load, storage growth, peer stability, signing/block reliability, and restart recovery.
6. Confirm role assignment, eligibility versus active service, economics, and operational obligations directly from current contracts/documentation or the responsible service operator.

## Conclusion

This inventory identifies the **limits of the evidence reviewed**, not confirmed shortcomings in Vitruveo. Some questions may already be answered in internal documentation, operator guides, private procedures, deployed configuration, or live infrastructure that was outside the scope or access of this research. Items should be upgraded to documented or verified only when evidence is obtained, with the source and method recorded.
