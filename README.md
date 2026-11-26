# LEGION MCP Contract Observatory

Public evidence for selected Model Context Protocol (MCP) input contracts.

**Live matrix:** https://legion-api.com/tlog/contract-matrix/  
**Offline evidence kit:** https://legion-api.com/contract-matrix-kit-20261126-v3.tar.gz  
**Transparency log:** https://legion-api.com/tlog/

## Current snapshot

Observed on 2026-11-26. Five tool-level checks across four publisher endpoints. All five tested tools returned **CONFORMS** for the schema rules tested.

| Publisher | Tool | Required input | additionalProperties | Result |
|---|---|---|---|---|
| Outdoors | `search` | `query` | absent (open by default) | CONFORMS |
| Edgepedia | `search` | `query` | absent (open by default) | CONFORMS |
| EOSL | `search_models` | `query` | absent (open by default) | CONFORMS |
| Total CMS | `search` | `query` | `false` | CONFORMS |
| Total CMS | `fetch` | `id` | `false` | CONFORMS |

A CONFORMS result means only that the tested required-field and extra-property rules behaved as declared during this observation. It is not a security certification, penetration test, uptime guarantee, or assessment of prompt injection, authorization, privacy, data retention, or untested tools.

## Method

For each selected tool:

1. Review publisher documentation for public, free, no-auth and read-only operation. A self-declared `readOnlyHint` is not sufficient by itself.
2. Compare the live MCP `tools/list` fingerprint with the selected historical schema in the signed LEGION transparency log. Abort if the full tool-list or selected tool schema differs.
3. Send one nominal warm-up call and exclude it from scoring.
4. Alternate two missing-required tests, two unknown-extra tests and four nominal controls, at a minimum of one second between HTTP POST requests.
5. Persist SHA-256 request/response digests rather than response bodies.
6. Sign the final report using a dedicated Ed25519 `probe` subkey endorsed by the pinned LEGION root.

The current report is Ed25519-signed and its TLOG record signatures and Merkle inclusion proofs verify offline. **It is not Bitcoin-anchored.** No Bitcoin timestamp claim is made.

## Independent verification

The live matrix page links to:
- the signed JSON report;
- the root-pinned public keyring and signed tree head;
- the exact target manifest;
- a standalone Python verifier and its RFC 8785 JCS implementation;
- SHA-256 checksums.

Start at the live matrix page and follow its README. Independently compare the first public key's 32-byte raw public-key SHA-256 with the LEGION root pin displayed on the TLOG page and in the public LEGION dataset README.

Root SHA-256:

`a7c6a35b1c2c81d589a30c2ad628679bdea9da4139d49aa3c42cdde6e1c65036`

Probe subkey ID:

`k1:11ec7bbf99a487313402ebe8ea2cd64d`

Probe subkey public-key SHA-256:

`11ec7bbf99a487313402ebe8ea2cd64dbdb8da9bfdbc66d6cbd122293b4b1c39`

Canonical report SHA-256:

`255f6578b787abc335e5766ab83c48d3cea172c9aa03a1188999b2b6f01c367c`

## Verdict vocabulary

- **CONFORMS** — tested schema rules were enforced.
- **DEVIATION** — a required field was omitted or a forbidden extra property was accepted.
- **STRICTER_THAN_SCHEMA** — the server rejected a property the schema permits.
- **INCONCLUSIVE** — the controls or error evidence did not support a reliable verdict.
- **NOT_TESTED / STALE_SCHEMA** — excluded, untested, or live schema differs from the signed snapshot.

A material adverse finding will not be publicly escalated without a 72-hour publisher right-of-reply window. Corrections can be filed through the repository's publisher-correction issue template.

## Product direction

The matrix is the public data layer of a wider MCP dependency observatory: signed schema history, runtime contract checks, publisher drift feeds, integrator dependency views, and machine-readable change alerts. Paid work is scoped as evidence reports and dependency maps; it is not a certification.

## Publisher documentation used for this snapshot

- [Outdoors public MCP documentation](https://theoutdoorsapp.com/ai)
- [Edgepedia developer documentation](https://www.edgepedia.org/developers)
- [EOSL MCP endpoint](https://eosl.ai/mcp)
- [Total CMS public read-only MCP policy](https://totalcms.co/privacy)

For the complete evidence and limitations, use the [live matrix](https://legion-api.com/tlog/contract-matrix/).
