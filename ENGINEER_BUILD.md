# Engineering Build

**Non-normative implementation examples for XRPL proof-of-funds attestations.**

---

## Status and Purpose

This directory contains **example implementations** of the proof-of-funds pattern documented in the [reference repository](https://github.com/Y3KDigital/ledger-proof-of-funds-reference).

**Important Clarifications:**

- ✅ These are **non-normative examples**
- ✅ They demonstrate **one possible implementation**
- ❌ They do **not define protocol behavior**
- ❌ They do **not imply XRPL endorsement**
- ❌ They are **not production-ready without review**

If there is any conflict between this build and the reference documentation, the reference documentation is authoritative.

---

## What This Is

This build exists to help XRPL engineers:

- **Inspect** raw transaction payloads
- **Verify** canonical snapshot generation
- **Test** memo encoding and decoding
- **Understand** fee calculations and reserves
- **Experiment** with attestation patterns

It is a **learning and inspection tool**, not a production service.

---

## What This Is Not

This build does **not**:

- Provide custody or key management
- Guarantee correctness or security for production use
- Imply support, maintenance, or SLA commitments
- Represent official XRPL tooling or documentation
- Create liabilities for XRPL validators or protocol maintainers

---

## Intended Audience

- **Protocol Engineers**: Evaluating the pattern's XRPL compatibility
- **Security Researchers**: Auditing snapshot generation and verification
- **Integration Developers**: Understanding transaction structures before building
- **Compliance Teams**: Reviewing attestation properties and limitations

**Not intended for:**

- End users seeking production services
- Developers seeking official XRPL SDK examples
- Organizations requiring production support

---

## Technical Scope

### Included

- Canonical snapshot generation (XRPL-specific)
- SHA-256 hash computation (client-side)
- AccountSet transaction structure examples
- Memo hex encoding and decoding
- Fee estimation (network-based)
- Read-only account queries (`account_info`, `account_lines`, `account_objects`)

### Excluded

- Private key handling (wallet-only)
- Transaction signing (wallet-delegated)
- Persistent storage (ephemeral by design)
- Production IPFS pinning (local node assumed)
- Error handling for production environments

---

## Security Considerations

### Assumptions

This build assumes:

- User has custody of their own keys
- Wallet software handles signing securely
- XRPL RPC endpoints are trusted for read operations
- IPFS node (if used) is locally controlled

### Out of Scope

- Multi-signature coordination
- Key recovery or backup
- Regulatory compliance validation
- Production deployment hardening

### Verification

All attestations produced by this build can be independently verified using the instructions in:

**[VERIFY.md](https://github.com/Y3KDigital/ledger-proof-of-funds-reference/blob/main/VERIFY.md)**

No trust in this build is required for verification.

---

## Usage

### Prerequisites

- Understanding of XRPL transaction structure
- Familiarity with JavaScript/TypeScript
- Local XRPL node or trusted public RPC access
- (Optional) Local IPFS node for content addressing

### Running Locally

```bash
# Clone this repository
git clone https://github.com/Y3KDigital/xrpl.git
cd xrpl/build

# Open in browser (static files)
# No build step required
```

### Inspecting Payloads

All transaction payloads are human-readable JSON.
Use browser DevTools to inspect:

- Network requests (RPC calls)
- Console logs (hash computations)
- Transaction structures (AccountSet memos)

---

## License

**Apache 2.0**

This build is provided under the Apache 2.0 license, which includes an explicit patent grant and is compatible with enterprise and institutional use.

See [LICENSE](../LICENSE) for full terms.

---

## Disclaimers

### No Warranty

This software is provided "AS IS" without warranty of any kind, express or implied.

### No Protocol Authority

This build does not define, extend, or modify XRPL protocol semantics.

### No Endorsement

Use of this build does not imply endorsement by:

- Ripple
- XRPL Foundation
- XRPL validators or node operators
- Protocol maintainers

### No Production Claims

This build is not:

- Audited for production use
- Supported with SLAs or uptime guarantees
- Guaranteed to be maintained

---

## Contributing

Contributions that improve:

- Correctness of XRPL-specific implementations
- Clarity of transaction structure examples
- Verification reproducibility

...are welcome.

Contributions that add:

- Product features
- Marketing copy
- Non-technical content

...will be declined.

**Philosophy**: Engineering builds should remain inspectable, minimal, and educational.

---

## References

- **Reference Pattern**: [ledger-proof-of-funds-reference](https://github.com/Y3KDigital/ledger-proof-of-funds-reference)
- **XRPL Protocol Documentation**: https://xrpl.org/docs.html
- **Verification Guide**: [VERIFY.md](https://github.com/Y3KDigital/ledger-proof-of-funds-reference/blob/main/VERIFY.md)

---

## Contact

For questions about:

- **The pattern**: Open issue in [ledger-proof-of-funds-reference](https://github.com/Y3KDigital/ledger-proof-of-funds-reference)
- **XRPL-specific implementation**: Open issue in this repository
- **Protocol behavior**: Consult official XRPL documentation

Do not contact XRPL Foundation or Ripple about this build. It is third-party tooling.
