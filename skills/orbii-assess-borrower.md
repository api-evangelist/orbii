---
name: Assess an SME borrower from bank statements
description: Onboard a company, upload its bank-statement PDFs, run Orbii's extraction pipeline, and read back the AI-derived KPIs, risk assessment and lending band.
api: openapi/orbii-uae-openapi-original.json
region: UAE
base_url: https://api.orbii.ai
operations:
  - POST /add-company
  - POST /pdf-upload
  - POST /start-pdf-extraction
  - GET /Check-for-new-data-received
  - GET /client-kpis/customer/{customer_id}
  - GET /predictive-kpis/customer/{customer_id}
  - GET /risk_assessment/customer/{customer_id}
  - GET /band_classification/customer/{customer_id}
---

# Assess an SME borrower from bank statements

Use Orbii's credit-intelligence API to turn a borrower's raw bank statements into
KPIs, a risk assessment, and a lending band.

## Auth & conventions
- Orbii declares no OAuth2/apiKey scheme in its OpenAPI. Credentials are passed as
  query parameters (`user`, `password`) and a `client` parameter scopes the lender.
  Never log these query strings. See `authentication/orbii-authentication.yml`.
- Errors return a flat `{"error": "<message>"}` body (not RFC 9457). See
  `errors/orbii-problem-types.yml`.
- Bodies for uploads use `multipart/form-data`.

## Steps
1. **Register the company** — `POST /add-company` with the company's details to
   create the borrower record.
2. **Upload statements** — `POST /pdf-upload` (multipart) with the borrower's
   bank-statement PDFs.
3. **Trigger extraction** — `POST /start-pdf-extraction` to run the parsing,
   enrichment and categorisation pipeline over the uploaded PDFs.
4. **Wait for data** — poll `GET /Check-for-new-data-received` until the new
   statements are processed.
5. **Read KPIs** — `GET /client-kpis/customer/{customer_id}` and
   `GET /predictive-kpis/customer/{customer_id}` for current and forward-looking
   financial metrics.
6. **Read the decision inputs** — `GET /risk_assessment/customer/{customer_id}`
   and `GET /band_classification/customer/{customer_id}` to get the risk profile
   and lending band used to size an offer.

## Notes
- KSA and Oman expose the same flow on `https://api.sa.orbii.ai` and
  `https://api.om.orbii.ai` respectively.
- Writes are not documented as idempotent; a `409 Conflict` indicates the company
  or resource already exists — fetch and reconcile before retrying.
