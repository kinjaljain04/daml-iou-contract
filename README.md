# daml-iou-contract

A Daml IOU (I Owe You) smart contract built on Canton Network as part of my learning journey toward contributing to [CompliDaml](https://github.com/Pallavi-M05/CompliDaml).

[![Daml](https://img.shields.io/badge/Daml-3.4.0-blue)](https://docs.digitalasset.com/build/3.4/)
[![Canton](https://img.shields.io/badge/Canton-Network-orange)](https://canton.network)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## What this is

A simple debt agreement. Alice owes Bob 100 USD. Bob can transfer that right to Carol. Alice repays to settle and close the contract.

## What I learned

### `template` — a contract blueprint
Like a class in Python. Defines the shape (fields) and rules (choices) of a contract.

### `signatory` — whose signature creates the contract
Without a signatory, a contract cannot exist. This is what CompliDaml rule **CB-002** checks.

### `observer` — who can see but not change
Canton's privacy model: data is only visible to signatories and observers of that specific contract. Citi cannot see a Goldman ↔ JPMorgan IOU even on the same network.

### `choice` + `controller` — who can act
A choice is an action. `controller` specifies exactly who can trigger it. The Daml compiler enforces this — no code path exists to bypass it.

### `Archive` — why CB-001 matters
Without Archive, a contract can never be terminated. This violates GDPR Art. 17 (right to erasure). **CB-001** fires HIGH when Archive is missing.

### `ensure` — preventing invalid contracts
`ensure amount > 0.0` means a negative debt is rejected at creation time, not at runtime.

### `submitMustFail` — testing authorization
Proves that unauthorized actions are correctly blocked. Bob cannot repay Alice's debt.

## CompliDaml rules this contract teaches

| Rule | Checks for | This contract |
|------|-----------|---------------|
| CB-001 | Missing Archive choice | ✅ Archive present |
| CB-002 | Missing signatory | ✅ `signatory issuer` |
| CB-003 | Missing ensure clause | ✅ `ensure amount > 0.0` |
| CB-005 | Choice with no controller | ✅ All choices have controllers |

## How to run
```bash
curl https://get.digitalasset.com/install/install.sh | sh
dpm build
dpm test
```

## My Canton learning path

| Repo | Focus | CompliDaml rules |
|------|-------|-----------------|
| **daml-iou-contract** (this) | Basics: signatory, observer, choices | CB-001, CB-002, CB-003, CB-005 |
| daml-kyc-record (next) | Privacy, PII exposure, GDPR patterns | GDPR-001, GDPR-002 |
| daml-trade-audit | Immutable audit records, MiFID II | MIFID-001, MIFID-002 |
| canton-complidaml-validation | All 3 contracts audited, before/after | All frameworks |
