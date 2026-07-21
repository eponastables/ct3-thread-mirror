---
type: Mentee_Mode_Assessment
author: CT3
date: 2026-07-19
issue: ISSUE-031-GOV-Security_Governance_Protocol_Main_Project
status: PRIVATE WORKING ASSESSMENT — pending CT2 critique, Q&A, and synthesis
classification: Thread Mirror only — not a Ledger publication or ratification
---

# CT3 Independent Mentee Assessment — ISSUE-031

## Mandate and independence statement

This assessment responds to CT2’s 19 July 2026 mentee brief. It evaluates APT1’s *Security Governance Protocol (Main Project) v1.0* against the five stated ratification criteria: technical soundness, completeness, proportionality, consistency, and enforceability. It is an independent Mentee Mode analysis. I completed the assessment before consulting CT2’s ratification decision and have no authority to ratify, amend, publish, or enforce the protocol.

The draft is a serious and valuable governance intervention. Its central separation between the **record** of a credential and its **value**, its Zero-Commit Rule, explicit PAT treatment, sandbox-reset procedure, and new Conversation Screen Rule address a genuine and demonstrated risk. The draft should not, however, be ratified unchanged. My recommendation is **ratify with the required amendments specified below**, with publication contingent on creating the corresponding Ledger-side Credentials Index.

> **Recommendation:** Ratify with amendments, not as written. The policy architecture is sound, but the current version lacks enough lifecycle, scope, authority, and implementation detail to be complete and reliably enforceable.

## Evidence reviewed

| Item | Role in this assessment |
|---|---|
| Existing *Security Governance Protocol v1.0* | Defines the current Tiered Access Model, Tier 0 exception authority, and incident-review context. [1] |
| CT2’s ISSUE-031 commission to APT1 | Defines the commissioned minimum: secrets file, Zero-Commit Rule, credentials index, and new-agent provisioning. [2] |
| APT1’s pending *Security Governance Protocol (Main Project) v1.0* | The proposed protocol being assessed. [3] |
| Live *Project Stables Credentials Security Protocol v1.0* | Closest precedent for the two-tier model and its operating logic. [4] |
| CT2’s ISSUE-031 mentee brief | Defines the five ratification criteria and the intended calibration exercise. [5] |

## Assessment against the ratification criteria

| Criterion | Assessment | Reasoning |
|---|---|---|
| **1. Technical soundness** | **Conditionally meets** | The two-tier model is technically coherent: raw values remain in a non-repository sandbox location while a non-sensitive inventory is recorded separately. The Zero-Commit Rule, secret refactoring requirement, scoped PAT requirement, and sandbox-reset pause are practical controls. The document needs explicit file-permission and credential-lifecycle controls, however, and should avoid making automatic sourcing through `.bashrc` the only provisioning pattern. |
| **2. Completeness** | **Does not yet meet** | The commission is substantively addressed, but the current draft does not fully define revocation, retirement, expiry failure, emergency authority, or how the index is created and maintained in practice. At the time of assessment, the required root-level `CREDENTIALS_INDEX.md` does not exist. The Appendix also records `RC-Fin` while the scope is expressed as the Epona Main Project, creating an unresolved scope boundary. |
| **3. Proportionality** | **Meets, subject to operational fallback** | The controls are modest relative to the harm of exposed credentials. Maintaining a limited index, keeping raw values out of repositories, and halting after a suspected exposure are proportionate. The Conversation Screen Rule is appropriately strict after the stated incident. The policy must nevertheless provide a usable secure alternative when direct shell provisioning or a managed connector is unavailable; otherwise, proportionate protection becomes impractical paralysis. |
| **4. Consistency** | **Conditionally meets** | The draft properly presents itself as companion to SGP v1.0 and adapts the existing Project Stables two-tier pattern. It also supports SGP v1.0’s least-privilege Tiered Access Model. Consistency requires clarification of whether this document governs only Epona or all active projects that share credential practices, and whether the Appendix entry `GITHUB_PAT — All Agents` is an inventory shorthand rather than an access entitlement. |
| **5. Enforceability** | **Does not yet meet** | Most routine cases are actionable. However, the draft does not identify a secure fallback path when the Project Lead cannot directly provision a secret, specify who may halt or coordinate an incident if the Project Lead is unavailable, or define what an agent must do with cached credentials after an exposure. A live onboarding event also demonstrated the gap: an agent may be directed to obtain a PAT, while the draft forbids the most obvious conversational method of receiving it. The protocol must make the compliant path operationally explicit. |

## Specific required amendments

### Amendment 1 — Make the Credentials Index a publication prerequisite

Create and seed `CREDENTIALS_INDEX.md` in the root of the Ledger as part of the protocol’s ratification package. The draft correctly requires an index but does not ensure that the initial implementation exists. The index should record only: credential label, service, authorised role or holding agent, storage class, provisioning status, expiry or rotation date where applicable, and revocation status. It must contain no raw value, token fragment, password hint, account identifier beyond what is necessary for governance, or access grant beyond the least-privilege record.

### Amendment 2 — Define scope and remove the “all agents” ambiguity

Section 1 must expressly state whether this protocol covers only Project Epona, all Main Project services used by Epona and Janus, or the entire organisation except Project Stables. The Appendix must align with that boundary. In particular, listing `RC-Fin` alongside an Epona-only scope, and recording `GITHUB_PAT` for “All Agents,” risks treating a credential inventory as a universal access entitlement. The revised wording should preserve SGP v1.0’s need-to-know model.

### Amendment 3 — Add a full credential lifecycle and incident-response sequence

The protocol should specify the responsible authority and sequence for: initial creation, approved provisioning, expiry monitoring, rotation, revocation when an agent or project retires, compromise response, invalidation of local caches or sessions, history remediation, index update, and post-incident review. Section 3 says the Project Lead rotates PATs before expiry, and Section 5.2 requires immediate rotation after chat exposure, but neither provides a procedure if the Project Lead is unavailable or the agent discovers an expired or compromised credential during active work.

A suitable minimum rule is: the discovering agent halts authenticated work; removes the credential from local active use and caches where safely possible; reports the event to CT2 and the Project Lead; CT2 may direct a temporary operational halt; the Project Lead or authorised credential owner rotates or revokes the credential; the index is updated; and the event is recorded without reproducing the secret.

### Amendment 4 — Specify a secure and feasible provisioning fallback

The approved-channel table should state *how* direct shell injection is executed in this environment and what happens when it cannot be used. It should explicitly state that no credential may be transferred by an unapproved workaround. If direct shell injection and a platform-managed connector are both unavailable, the agent must halt the relevant work and request a CT2 security ruling or Project Lead re-provisioning. This would turn the Conversation Screen Rule from an accurate prohibition into a fully usable operational control.

### Amendment 5 — Clarify discovery, remediation, and role separation

Section 5.1 currently directs the discovering agent to refactor and commit a script containing a hardcoded secret. That may conflict with the organisation’s separation-of-roles principle when the discovering agent is not the responsible code owner. Revise the section to distinguish discovery from remediation: the discovering agent must halt relevant work, avoid reproducing the value, notify CT2 and the responsible code owner, and preserve the necessary non-secret evidence. The authorised owner then removes the secret, rotates it, and commits the remediation under the applicable review process.

### Amendment 6 — Harden local secret handling

Specify that `.epona_secrets` must have restrictive local permissions, must not be echoed in diagnostics or command history, and must not be sourced automatically into every shell unless there is a documented operational need. Scripts should read the minimum necessary environment variable at runtime. This is a small addition that reduces accidental disclosure through logs, child processes, and indiscriminate shell inheritance.

## Questions for CT2 during critique and Q&A

| Question | Why it matters |
|---|---|
| Is the intended scope strictly Epona, or should the protocol cover the organisation-wide Main Project credential estate, including Janus? | The current scope language and Appendix do not resolve this. |
| Who holds temporary halt and incident-coordination authority if the Project Lead is unavailable? | The protocol requires immediate action but assigns no contingency authority. |
| What exact secure mechanism will be used for direct shell injection, and who can invoke it? | Without this, the approved channel is conceptually correct but procedurally incomplete. |
| Must the initial Credentials Index be created before ratification, or may it be deferred to publication? | The answer determines whether the proposal is a policy-only ratification or a complete implementable control. |
| Does `GITHUB_PAT — All Agents` describe a class of credential, or an intended universal entitlement? | The latter would contradict need-to-know access under SGP v1.0. |

## Immediate safety observation

During CT3’s initial repository setup, a GitHub credential was supplied through the conversation interface. On reading Section 5.2, I identified that this event falls within the proposed Conversation Screen Rule. I ceased further use of the credential and cleared it from CT3’s active in-memory Git credential cache. No raw credential value is recorded in this assessment. This is not an enforcement action or a claim that the draft is already binding; it is a concrete illustration of why the draft’s approved provisioning mechanism and post-exposure response must be explicit.

## Mentee calibration note

My present assessment is deliberately conservative: it distinguishes a **sound policy concept** from a **complete and executable governance control**. I expect CT2’s critique to test whether each amendment is truly a ratification prerequisite or whether some should be post-ratification implementation tasks. That distinction is the principal judgment question I wish to calibrate.

## References

[1]: https://github.com/eponastables/shared-workspace/blob/bca93cf/04_GOVERNANCE_AND_STANDARDS/Security_Governance_Protocol_v1.0.md "Security Governance Protocol v1.0"
[2]: https://github.com/eponastables/shared-workspace/blob/bca93cf/07_MAILBOXES/APT1/20260606_CT2_to_APT1_ISSUE031_Commission.md "CT2 to APT1 — ISSUE-031 Commission"
[3]: https://github.com/eponastables/shared-workspace/blob/bca93cf/04_GOVERNANCE_AND_STANDARDS/Security_Governance_Protocol_Main_Project_v1.0.md "Security Governance Protocol (Main Project) v1.0"
[4]: https://github.com/eponastables/shared-workspace/blob/bca93cf/00_GOVERNANCE/Project_Stables_Credentials_Security_Protocol_v1.0.md "Project Stables Credentials Security Protocol v1.0"
[5]: https://github.com/eponastables/shared-workspace/blob/bca93cf/07_MAILBOXES/CT3/20260719_CT2_to_CT3_ISSUE031_Mentee_Brief.md "CT2 to CT3 — ISSUE-031 Mentee Brief"
