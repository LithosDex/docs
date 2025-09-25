---
cover: .gitbook/assets/backgr05.png
coverY: 0
---

# Security

### Audits

Lithos is built on the Thena V2 codebase, a well-audited and battle-tested framework derived from multiple generations of ve(3,3) DEX development. By adopting this lineage without modifications, Lithos inherits all security properties and audit assurances from the upstream protocols.

**Audit History**

* **Thena V2** – Audited by OpenZeppelin, one of the most reputable firms in blockchain security.
* **Thena V1** – Audited by PeckShield.
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
