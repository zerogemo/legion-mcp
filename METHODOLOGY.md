# Runtime probe methodology

## Purpose

The LEGION MCP Contract Observatory tests only schema promises that can be evaluated precisely. It does not infer a contract violation from a tool error, a changed response body, or acceptance of an unknown field when the schema is open.

## Admission gates

A target is eligible only when all applicable checks pass:

1. A signed LEGION TLOG record binds the endpoint to the tool list and selected input schema.
2. The publisher's documentation supports the access mode and read-only nature of the selected endpoint/tool. A self-declared `readOnlyHint` is an additional runtime guard, not sufficient proof on its own.
3. The runtime endpoint is public/free/unauthenticated, or the publisher has explicitly authorised an agreed test environment.
4. The current live tool-list and selected tool-schema fingerprints exactly match the chosen signed TLOG snapshot. A mismatch aborts the probe and is reported as stale/unmatched evidence.
5. The schema contains at least one required field. Closed schemas are especially useful where `additionalProperties:false` is explicit.

Private, paid, authenticated, destructive, state-changing and production-sensitive tools are not tested without explicit authorisation and a safe staging scope.

## Test sequence

For each selected tool:

1. MCP `initialize` and `tools/list`; compare complete and per-tool fingerprints.
2. One nominal warm-up call, excluded from scoring.
3. Alternate missing-required and nominal controls.
4. Alternate unknown-extra-field and nominal controls.
5. Two missing-required calls, two unknown-extra calls, and four nominal controls are scored.
6. At least one second separates HTTP POST requests.

No tool output body is retained. Reports retain request/response SHA-256 digests, case labels, status/error metadata, timing, schema descriptor, TLOG record, STH and inclusion path.

## Verdicts

- `CONFORMS`: all repeated nominal controls passed; required-field omission was rejected with field-specific validation evidence; extra fields behaved according to the schema.
- `DEVIATION`: a required field was accepted when omitted, or an extra property was accepted when `additionalProperties:false`.
- `STRICTER_THAN_SCHEMA`: an extra property was consistently rejected even though the schema permits additional properties. This is not automatically a defect; it is a schema/implementation mismatch.
- `INCONCLUSIVE`: required validation is ambiguous, controls are unstable, or nominal calls fail.
- `STALE_SCHEMA` / `NOT_TESTED`: evidence is outdated or the endpoint was excluded.

When `additionalProperties` is absent from a JSON Schema object, additional properties are permitted by default. Acceptance of unknown keys is therefore not a deviation.

## Cryptographic evidence

The report is signed with a dedicated Ed25519 `probe` subkey registered in the LEGION TLOG keyring and endorsed by the pinned root key. The report contains the selected signed TLOG leaf, leaf signature, signed tree head and RFC 6962 inclusion path.

The standalone verifier checks the keyring delegation chain, probe-key purpose, report signature, TLOG signer authorization, record signature, STH signature, Merkle inclusion, schema hashes, test order, request pacing and derived verdict.

The current published report is not Bitcoin-anchored. Never describe it as Bitcoin timestamped.

## Right of reply

No adverse public deviation is to be escalated without sending the relevant evidence to the publisher and allowing a 72-hour correction/right-of-reply window. A reply is recorded as supplied context; it does not silently overwrite the signed observation. Corrections require a new signed observation.

## Public evidence

- Matrix: https://legion-api.com/tlog/contract-matrix/
- Offline kit: https://legion-api.com/tlog/contract-matrix-kit-20261126.tar.gz
- Root pin: `a7c6a35b1c2c81d589a30c2ad628679bdea9da4139d49aa3c42cdde6e1c65036`
