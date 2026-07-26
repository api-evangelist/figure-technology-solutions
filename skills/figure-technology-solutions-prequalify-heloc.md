---
name: Pre-qualify a HELOC lead
description: Request real-time non-licensed HELOC pre-qualification offers for an affiliate lead.
api: openapi/figure-technology-solutions-heloc-pre-qualification-openapi.yml
operations: [prequalifyHeloc, getHelocOffersV2]
---

# Pre-qualify a HELOC lead

Base URL `https://api.figure.com` (test: `https://api.test.figure.com`). This API is keyed by an
`affiliateId` supplied by Figure. Sandbox affiliateIds are published in the docs (see
`sandbox/figure-technology-solutions-sandbox.yml`): `d02bc4e9-35af-4c31-970e-e1273079ba41`
(self-attested) or `e5c722ec-eaf1-4cb1-8fcb-f2c16b31fade` (licensed partner).

## Steps

1. **Request pre-qualification** — `POST /products/heloc/pre-qualify/v1` (`prequalifyHeloc`) with the
   lead's applicant + property info and request type `OFFERS` or `PREPOP`. Response returns HELOC
   product offers in JSON.
2. **(v2 alternative)** — `POST /products/heloc/pre-qualify/v2` (`getHelocOffersV2`) for the v2 offer shape.

## Rules

- Handle `400` (malformed request — check required fields) and `408` (upstream timeout — retry).
- Use the test host + sandbox affiliateId before switching to the production affiliateId.
- Product is `Home Equity Loan`; format is JSON.
