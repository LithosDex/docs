# Security

## Audits

* Lithos is a direct fork of Thena V2, which has been audited by [OpenZeppelin](https://blog.openzeppelin.com/retro-thena-audit).
* The original Thena V1 codebase was audited by [PeckShield](https://github.com/peckshield/publications/tree/master/audit_reports/PeckShield-Audit-Report-Thena-v1.0.pdf).
* Thena was originally adapted from Velodrome codebase, which is directly derived from the Solidly smart contracts that have been open sourced in March 2022.

**Battle-Tested Foundation**: By using the Thena V2 codebase without modifications, Lithos inherits all security properties and audit assurances from the original audited contracts.

The AMM part of Solidly has been audited by PeckShield that revealed 5 low-severity and 1 informal findings. There have been no security-related incidents involving Solidly smart contracts since their deployment on Fantom in February 2022.

The Velodrome codebase went through a security audit and a peer review as part of a Code4rena bug bounty contest. All high or medium risk issues were either resolved pre-deployment, except for one known issue (users can claim eligible rewards from ExternalBribe contracts more than once) that has been addressed via a wrapped contract solution.

**Zero Modifications**: Lithos preserves the integrity of the audited codebase by making no changes to the core smart contracts, ensuring that all security guarantees remain intact.

**Future Development**: While Lithos currently uses the proven Thena V2 codebase without modifications, we have plans to expand functionality over time. Any new features will only be deployed after extensive testing, comprehensive security audits, and approval through a public governance process to maintain our security-first approach.

---

## Multisig

All critical protocol transactions will go through a multisig with appropriate timelock mechanisms.

*Multisig details will be published upon deployment to Plasma blockchain.*

**Security Principles**:
- Multiple signers required for any critical protocol changes
- Appropriate timelock delays for community review
- Transparent governance process for all administrative actions
- Community oversight of multisig operations