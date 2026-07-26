---
name: Run a Figure HELOC inquiry end to end
description: Start a HELOC inquiry, resolve the property, add borrower income and SSN, fetch offers, and select one.
api: openapi/figure-technology-solutions-heloc-inquiries-openapi.yml
operations: [startInquiry, fetchMatchingProperties, selectProperty, addInquirySsn, addInquiryIncome, selectInquiryOffer, fetchAppDetails]
---

# Run a Figure HELOC inquiry end to end

Base URL `https://api.figure.com` (test: `https://api.test.figure.com`). Every request
needs an `apikey` header and a `User-Agent` header. PII payloads are JWE-encrypted
(RSA-OAEP-256 + A256GCM) inside an `encrypted` field — use `POST /encryption/v1/encrypt`
(`encryptPayload`) if you cannot encrypt client-side.

## Steps

1. **Start the inquiry** — `POST /products/heloc/v1/inquiry/start` (`startInquiry`) with the
   encrypted applicant + property payload (includes `loanOriginatorUuid`). Capture `appUuid`
   from the response.
2. **Resolve the property** — if the response step is `SELECT_PROPERTY`, call
   `GET /products/heloc/v1/inquiry/{appUuid}/properties` (`fetchMatchingProperties`) and then
   `PUT /products/heloc/v1/inquiry/{appUuid}/select-property` (`selectProperty`) with the chosen `propertyId`.
3. **Add SSN when required** — on `CREDIT_MATCH_REQUIRED`, submit the encrypted SSN via
   `PUT /products/heloc/v1/inquiry/{appUuid}/add-ssn` (`addInquirySsn`).
4. **Add income when required** — on `MORE_ASSETS_REQUIRED`, call
   `PUT /products/heloc/v1/inquiry/{appUuid}/add-income` (`addInquiryIncome`).
5. **Poll status** — `GET /products/heloc/v1/details/{appUuid}` (`fetchAppDetails`) or listen for the
   inquiry webhook until `nextInquiryStepType` is `OFFERS_AVAILABLE`.
6. **Select an offer** — `PUT /products/heloc/v1/inquiry/{appUuid}/select-offer` (`selectInquiryOffer`)
   with `offerUuid` and `selectedOfferAmount`.

## Rules

- Drive the flow off `nextInquiryStepType` (SELECT_PROPERTY, CREDIT_MATCH_REQUIRED, UNFREEZE_CREDIT,
  LIEN_MATCHING_REQUIRED, MORE_ASSETS_REQUIRED, PAYOFF_REQUIRED, PROPERTY_VALUE_REQUIRED, OFFERS_AVAILABLE) —
  do not assume a fixed order.
- No idempotency key is supported; do not blind-retry writes.
- Test with the `api.test.figure.com` host and a test `loanOriginatorUuid`.
