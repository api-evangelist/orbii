---
name: Size an invoice-financing loan
description: Upload a borrower's invoice extract, run Orbii's invoice-extraction pipeline, and read back invoice KPIs, risk assessment, band classification and the suggested loan and allocation.
api: openapi/orbii-uae-openapi-original.json
region: UAE
base_url: https://api.orbii.ai
operations:
  - POST /invoice-extract-upload
  - POST /start-invoice-extraction
  - GET /invoice-kpis/customer/{customer_id}
  - GET /invoice-risk_assessment/customer/{customer_id}
  - GET /invoice-band_classification/customer/{customer_id}
  - GET /invoice-suggested-loan/customer/{customer_id}
  - GET /invoice-suggested-loan-allocation/customer/{customer_id}
  - GET /invoice-suggested-loan-summary/customer/{customer_id}
---

# Size an invoice-financing loan

Turn a borrower's invoice history into an invoice-based credit recommendation.

## Auth & conventions
- Query-parameter credentials (`user`, `password`, `client`); never log them. See
  `authentication/orbii-authentication.yml`.
- Flat `{"error": "<message>"}` error envelope; see `errors/orbii-problem-types.yml`.
- Uploads use `multipart/form-data`.

## Steps
1. **Upload the invoice extract** — `POST /invoice-extract-upload` with the
   borrower's Excel invoice extract.
2. **Run extraction** — `POST /start-invoice-extraction` to process the invoices.
3. **Read invoice KPIs** — `GET /invoice-kpis/customer/{customer_id}` for the
   invoice-derived financial metrics.
4. **Read risk & band** — `GET /invoice-risk_assessment/customer/{customer_id}`
   and `GET /invoice-band_classification/customer/{customer_id}`.
5. **Get the recommendation** — `GET /invoice-suggested-loan/customer/{customer_id}`
   for the suggested loan, then
   `GET /invoice-suggested-loan-allocation/customer/{customer_id}` and
   `GET /invoice-suggested-loan-summary/customer/{customer_id}` for the per-company
   allocation and summary.

## Notes
- Company-level variants (`/invoice-company-kpis/...`,
  `/invoice-company-risk_assessment/...`,
  `/invoice-company-band_classification/...`) break the same signals down by the
  counterparties on the invoices.
