# Security

### Audits

Lithos is built on the Thena V2 codebase, a well-audited and battle-tested framework derived from multiple generations of ve(3,3) DEX development. By adopting this lineage without modifications, Lithos inherits all security properties and audit assurances from the upstream protocols.

**Audit History**

* **Thena V2** – Audited by OpenZeppelin ([retro audit recap](https://blog.openzeppelin.com/retro-thena-audit)).
* **Thena V1** – Audited by PeckShield ([full report PDF](https://github.com/peckshield/publications/blob/master/audit_reports/PeckShield-Audit-Report-Thena-v1.0.pdf)).
* **Velodrome** – Adapted from the Solidly codebase and reviewed through both a security audit and a Code4rena bug bounty contest.
* **Solidly AMM** – Audited by PeckShield, which identified five low-severity and one informational finding. No security incidents have occurred since its deployment on Fantom in February 2022.

**Findings and Resolutions**

* Velodrome resolved all high- and medium-risk issues pre-deployment.
* A single known issue related to ExternalBribe contracts was mitigated with a wrapped contract solution.
* No unresolved critical issues remain in the codebase lineage.

**Battle-Tested Foundation**

* Lithos uses the Thena V2 codebase without modifications to the core smart contracts.
* The unmodified codebase allows Lithos to rely directly on the assurances provided by prior audits.

**Future Development**

* Planned expansions and new features will only be deployed after:
  * Extensive internal and external testing.
  * Comprehensive third-party audits.
  * Approval through a public governance process.

Security reviews will remain a prerequisite for all new functionality.

***

### Multisig

All critical protocol transactions are protected by a multisig setup with appropriate timelock mechanisms. This structure ensures that no single actor can execute sensitive operations unilaterally.

* **Multiple Signers** – Any critical change requires signatures from multiple trusted parties.
* **Timelock Delays** – Timelocks provide the community with a review period before changes are executed.
* **Transparent Governance** – Administrative actions will be communicated openly and verifiable on-chain.
* **Community Oversight** – Multisig operations are subject to monitoring by the Lithos community.

**Deployment Details**

* Multisig configurations, including signer addresses and timelock parameters, will be published upon deployment to the Plasma blockchain.

---

### Ownership Snapshot

Governance multisig (`0x21F1…f4dBC`) now anchors protocol control: it is the `lithosMultisig`, holds all `PermissionsRegistry` roles (`GOVERNANCE`, `VOTER_ADMIN`, `GAUGE_ADMIN`, `BRIBE_ADMIN`), and owns the Pair/Gauge/Bribe factories (with `PairFactory.acceptFeeManager()` pending to self-execute).

Operations/emissions multisig (`0xbEe8…618F7`) is the `lithosTeamMultisig`, owns `RewardsDistributor`, and serves as team on `VotingEscrow`; it is queued as the new Minter "team" (`acceptTeam()` still needs to be called).

The 7-day Timelock (`0x9f7d…1794`) controls both upgrade proxies via their `ProxyAdmin` contracts, so all proxy upgrades route through timelocked governance proposals.

Emergency council multisig (`0x7716…d799`) holds the dedicated emergency key inside `PermissionsRegistry`.

Incentives flow remains consistent: `RewardsDistributor.depositor` stays the Minter proxy, giving the operations multisig oversight without disrupting emissions.

#### Multisig Addresses (PlasmaScan)

- [Governance Multisig (4/6)](https://plasmascan.to/address/0x21F1c2F66d30e22DaC1e2D509228407ccEff4dBC) — `0x21F1c2F66d30e22DaC1e2D509228407ccEff4dBC`
- [Operations Multisig (3/4)](https://plasmascan.to/address/0xbEe8e366fEeB999993841a17C1DCaaad9d4618F7) — `0xbEe8e366fEeB999993841a17C1DCaaad9d4618F7`
- [Emergency Council (2/3)](https://plasmascan.to/address/0x771675A54f18816aC9CD71b07d3d6e6Be7a9D799) — `0x771675A54f18816aC9CD71b07d3d6e6Be7a9D799`
- [Treasury Multisig (4/6)](https://plasmascan.to/address/0xe98c1e28805A06F23B41cf6d356dFC7709DB9385) — `0xe98c1e28805A06F23B41cf6d356dFC7709DB9385`
- [Timelock (7-day)](https://plasmascan.to/address/0x9f7d46cE1EA22859814e51E9D3Fe07a665f21794) — `0x9f7d46cE1EA22859814e51E9D3Fe07a665f21794`
