# XRPL Proof of Funds Pattern

**Protocol-specific documentation for implementing ledger-native proof-of-funds attestations on the XRP Ledger.**

This repository documents how the [Ledger Proof of Funds reference pattern](https://github.com/Y3KDigital/ledger-proof-of-funds-reference) maps to XRPL primitives, constraints, and semantics.

---

## Reference Implementation

For the protocol-neutral pattern specification, see:  
**[ledger-proof-of-funds-reference](https://github.com/Y3KDigital/ledger-proof-of-funds-reference)**

This repository provides XRPL-specific context only.

---

## XRPL-Specific Design

### Transaction Type: `AccountSet`

The pattern uses `AccountSet` transactions for on-chain attestation anchoring.

**Why `AccountSet` instead of `Payment`?**

- `AccountSet` is explicitly metadata-oriented
- Does not imply value transfer or settlement activity
- Avoids semantic confusion for compliance and monitoring tools
- Clearly signals "this is auxiliary data, not payment flow"

### Memo Structure

Attestation data is encoded in transaction memos using hex-encoded JSON.

**Memo Fields:**

```json
{
  "Memo": {
    "MemoType": "<hex: proof_of_funds>",
    "MemoData": "<hex-encoded JSON>"
  }
}
```

**Decoded MemoData contains:**

```json
{
  "hash": "sha256_hash_of_snapshot",
  "ipfs_cid": "Qm...",
  "timestamp": "2026-01-29T12:00:00.000Z",
  "network": "mainnet",
  "type": "proof_of_funds"
}
```

### Memo Size Limits

- XRPL memos have size constraints (1KB typical)
- Hash + CID + metadata fit comfortably within limits
- Full snapshot stored off-chain (IPFS)

---

## Snapshot Contents

XRPL snapshots capture publicly readable account state via standard RPC calls:

### API Calls Used

```
account_info       → Native XRP balance, sequence, flags
account_lines      → Trustlines (issued currencies)
account_objects    → Escrows, payment channels, offers, NFTs
```

### Snapshot Schema

```json
{
  "address": "rN7n7otQDd6FczFgLdlqtyMVrn3HMzxp8u",
  "balances": {
    "XRP": "1000.000000"
  },
  "trustlines": [
    {
      "currency": "USD",
      "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
      "balance": "500.00",
      "limit": "1000.00"
    }
  ],
  "escrows": [],
  "paymentChannels": [],
  "offers": []
}
```

---

## Fee Considerations

### Reserve Requirements

- Base reserve: 10 XRP (per account)
- Owner reserve: 2 XRP (per object)
- Attestation does not create new objects

### Transaction Fees

- `AccountSet` fee: ~0.00001 XRP (network dependent)
- Fee is dynamic based on ledger load
- No custody or signing on user's behalf

---

## Verification on XRPL

### Step 1: Locate Transaction

Use XRPL explorers or RPC:

```bash
# Via rippled RPC
{
  "method": "tx",
  "params": [{
    "transaction": "<tx_hash>",
    "binary": false
  }]
}
```

### Step 2: Extract Memo

Decode hex memo data:

```javascript
// MemoData is hex-encoded
const decoded = Buffer.from(memoData, 'hex').toString('utf-8');
const attestation = JSON.parse(decoded);
```

### Step 3: Verify Hash

1. Retrieve snapshot from IPFS using `ipfs_cid`
2. Recompute SHA-256 hash
3. Compare to `hash` field in memo

### Step 4: Confirm Immutability

- Once validated, XRPL transactions are immutable
- Ledger index provides canonical timestamp
- No trust in operator required

---

## Protocol Compatibility

### Current

- Works on XRPL mainnet today
- No amendments required
- Uses standard `AccountSet` + memos

### Future

- **Hooks**: Could automate attestation triggers
- **AMMs**: Extend snapshot schema to include LP positions
- **NFTs**: Already supported via `account_objects`

Pattern remains valid across protocol evolution.

---

## XRPL-Specific Constraints

| Constraint       | Value                    | Impact                          |
| ---------------- | ------------------------ | ------------------------------- |
| Memo size        | ~1KB                     | Hash + CID fit comfortably      |
| Base reserve     | 10 XRP                   | Account must exist              |
| Transaction fee  | ~0.00001 XRP             | Negligible cost                 |
| Validation time  | 3-5 seconds              | Fast finality                   |
| AccountSet limit | No practical limit       | Can attest repeatedly           |
| Memo encoding    | Hex (UTF-8 → hex → memo) | Standard conversion, no loss    |

---

## Examples

See [examples/](./examples/) for:

- Complete `AccountSet` transaction payloads
- Hex-encoded memo examples
- Decoded JSON structures
- Explorer links to real attestations

---

## Security Model

### Read-Only Access

- Queries use public RPC endpoints
- No private key access required for snapshot generation
- User signs only the attestation `AccountSet`

### No Protocol Risk

- Uses existing, well-understood primitives
- Does not introduce new transaction types
- Does not modify consensus rules
- Does not create liabilities for validators

---

## References

- **Reference Pattern**: [ledger-proof-of-funds-reference](https://github.com/Y3KDigital/ledger-proof-of-funds-reference)
- **XRPL Documentation**: https://xrpl.org/docs.html
- **AccountSet Specification**: https://xrpl.org/accountset.html
- **Memo Format**: https://xrpl.org/transaction-common-fields.html#memos-field

---

## License

MIT License

This documentation is intentionally protocol-focused and implementation-agnostic. XRPL developers are encouraged to reference, fork, or adapt this pattern.
