# Figure Technology Solutions

Reno, Nevada financial-technology company (founded 2018) building blockchain-based platforms for lending, capital markets, and asset management. Its public Partner APIs originate and manage Home Equity Line of Credit (HELOC) loans, and Figure Connect is its blockchain-based loan marketplace.

- Website: https://www.figure.com/
- Developer docs: https://docs.figure.com/
- GitHub: https://github.com/FigureTechnologies
- Status: https://status.figure.com/

## APIs

- **HELOC Inquiries** (OpenAPI 3.1.0) — full inquiry lifecycle (start, property match, income/SSN, liens, offers, documents).
- **HELOC Pre-Qualification** (OpenAPI 3.0.1) — real-time non-licensed lead pre-qualification and offer retrieval.
- **Portfolio Manager** (OpenAPI 3.0.1) — reporting/download over owned and pledged loan pools.

Auth is an `apikey` header (plus required `User-Agent`); PII payloads are JWE-encrypted (RSA-OAEP-256 + A256GCM). Webhooks cover inquiry and application status events.

Backed by: dcm-ventures.
