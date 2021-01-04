---
title: Settlement Report
layout: tutorial

summary: >
    A guide for Partners to integrate directly to the Settlement Report API.

navigation:
  header: na.tocs.na_nav_header
  footer: na.tocs.na_nav_footer
  toc: false
  header_active: Guides
---

# Settlement Report

## Report Overview

As a Bambora Merchant, the Settlement Report provides an overview of
transactions and upcoming settlement disbursement. You will be able to view
pending settlement, settlement amount, when to expect it, and what fees were
removed. With the settlement data, you can spend less time on manual tasks and
reconciliation.

As a Bambora Partner, you can help your customers reconcile by
displaying settlement data within your software by using the Settlement Report.
It allows you to display to your customers their pending settlement, settlement
amount, when they can expect it, and what fees were removed. With the settlement
data, your customers can spend less time on manual tasks and reconciliation.

## Report Columns

The response will be a JSON-formatted response which will consist of a collection
of `CardSettlementRecord` records.  The fields of each `CardSettlementRecord` are
summarized in the table below.

| Field | Description |
| ------ | ----------------- |
| `merchant_id` | The id of the merchant |
| `transactions_date` | The day the transactions settled |
| `currency` | The currency (CAD, USD, etc) of the settlement |
| `settlement_net_amount` | Total amount (net) of the settlement |
| `settlement_state` | The current state (scheduled, approved, etc) of the settlement |
| `settlement_date` | The day funds are expected to be settled |
| `approved_transaction_count` | The total count of transactions that were approved |
| `declined_transaction_count` | The total count of transactions that were declined |
| `sale_amount_total` | Total amount of sales of the settlement |
| `returned_amount_total` | Total amount of returns of the settlement |
| `chargebacks_count` | Total count of chargebacks |
| `chargebacks_amount_total` | Total amount (ie monetary total) of chargebacks |
| `card_transaction_approved_rate` | Rate for approved transactions |
| `card_transaction_declined_rate` | Rate for declined transactions |
| `card_discount_rate` | Card discount rate |
| `gst_tax_rate` | GST rate applied to fees |
| `approved_transaction_fee_total` | Total fees for approved transactions of the settlement |
| `declined_transaction_fee_total` | Total fees for declined transactions of the settlement |
| `discount_rate_fee_total` | Total of the per-transaction fees |
| `chargeback_fee_total` | Total of the chargeback fees |
| `gst_tax_fee_total` | The total GST applied to transaction fees |
| `reserves_held` | The amount currently held in a Bambora reserve account |
| `reserves_released` | Amount released from the reserve account during the current statement period |
| `reserves_forward` | Total amount that Bambora held in a reserve account as of the previous statement period |

More detailed information about these fields can be found in our API specification
which can be found at: <https://dev.na.bambora.com/docs/references/payment_APIs/v1-0-5/#smooth-scroll-top>

## Getting Started

### Environments

There are two environments where you can interact with the Settlement Report:
Sandbox and Production. Sandbox (as you might guess) is for testing your integration
and production is for retrieving real settlement data associated with you and your
partner's accounts.

Both environments are accessed via the same URL
(<https://api.na.bambora.com/v1/reports/settlement>), which environment is
accessed is determined by the API key used in the request for authentication.

#### Sandbox

For sandbox you will need a developer test account, follow the instructions at
<https://dev.na.bambora.com/docs/guides/merchant_quickstart/> for setting one
up.  Use the merchant ID and reporting API access passcode for this test account
to interact with the sandbox environment.  See the "Authentication" section below
for more details on how to generate the access passcode to use in the request
to the Settlement Report.

#### Production

For production, you simply need use your merchant ID and reporting API access
code from your actual merchant account.  See the "Authentication" section below
for more details on how to generate the access passcode to use in the request
to the Settlement Report API.

## API Requests

### Request Headers

Requests to the settlement endpoint should include two request headers:
a `Content-type` header with the value `application/json` and a `Authorization`
header which is outlined in the next section.

### Authentication

All requests to the Settlement Report require authentication in the form of an
`Authorization` header.  This header contains the value `Passcode <YOUR API
ACCESS PASSCODE>`.

`<YOUR API ACCESS PASSCODE>` is created by filling out the form at
<https://dev.na.bambora.com/docs/forms/encode_api_passcode/> For the "Passcode"
field, make sure it is the Reporting API Access Passcode (this can be found in
the [Member Area](https://web.na.bambora.com) under **administration**,
**account setttings**, and then **order settings**).

### Parameters

The Settlement report accepts two parameters to control the data returned by
the endpoint: `start_date` and `end_date`, both of which are supplied as query
string parameters on the HTTP `GET` request.

```no-highlight
Note: Both of these values are required on all requests to the Settlement Report API
```

Both parameters accept a date, and will restrict settlement records to the date
range specified (inclusive).  More details are in the formal specification at
<https://dev.na.bambora.com/docs/references/payment_APIs/v1-0-5/#smooth-scroll-top>

### Example

An example request is illustrated below.

#### Request

```shell
curl --location --request GET 'https://api.na.bambora.com/v1/reports/settlement?start_date=2020-12-01&end_date=2021-01-31' \
--header 'Authorization: Passcode <YOUR API ACCESS PASSCODE>' \
--header 'Content-Type: application/json'
```

Replace `<YOUR API ACCESS PASSCODE>` with your API passcode.

#### Response

```javascript
{
    "data": [
        [
            {
                "merchant_id": 123456789,
                "currency": "USD",
                "transactions_date": "2021-01-10",
                "settlement_net_amount": 5213.45,
                "settlement_state": "Approved",
                "settlement_date": "2021-01-10",
                "approved_transaction_count": 10,
                "declined_transaction_count": 3,
                "sale_amount_total": 6123.00,
                "returned_amount_total": 0.00,
                "chargebacks_count": 5,
                "chargebacks_amount_total": 10.00,
                "card_transaction_approved_rate": 0.00,
                "card_transaction_declined_rate": 0.00,
                "card_discount_rate": 0.00,
                "gst_tax_rate": 0.00,
                "approved_transaction_fee_total": 0.00,
                "declined_transaction_fee_total": 0.00,
                "discount_rate_fee_total": 0.00,
                "chargeback_fee_total": 0.00,
                "gst_tax_fee_total": 0.00,
                "reserves_held": 311.15,
                "reserves_released": 0.00,
                "reserves_forward": 0.00
            }
        ]
    ]
}
```
