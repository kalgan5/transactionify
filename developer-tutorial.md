# transacionify


## Overview
This guide describes how to setup Transacionify, which is a new, internal microservice. This service handles account an payment transactions.

The guide is divided into the following sections:

[[_TOC_]]

## How to clone the repository

To clone the repository locally:

```
$ git clone https://github.com/rrgarciach/transactionify.git
$ cd transactionify
```

This allows you to download the source code and to explore files such as `README.md`, `openapi.yaml`, and the `src/python` directory.

## Review README.md for initial hints

The `README.md` provides hints about the API and setup:

### API overview:

- Create a new account (lines 5-11):
```
POST /api/v1/accounts { “currency”: “USD” }
```
- Create a new payment (lines 13-23):
```
POST /api/v1/accounts/{account_id}/payments
```
- Get balance (lines 25-26):
```
GET /api/v1/accounts/{accounts/{account_id}/balance
```
- List transactions (lines 28-29):
```
GET /api/v1/accounts/{account_id}/transactions
```

### Prerequisites (lines 35-38):
-	Python 3.9+, Node.JS 18+, AWS CDK CLI.

### Setup:
-	Install Python dependencies:
```
pip install -r requirements.txt
```
- Install Node.JS dependencies:
```
npm install
```
Using Git CMD:
```
cd <REPO_PATH>\transactionify\src\python
```
Run the tests in the source coder
```
pytest
```
## How to find the API specification in openapi.json
- Paths (lines 35-303):
  - Lines 36-103: `/api/v1/accounts`
  - Lines 105-178: `/api/v1/accounts/{account_id}/payments`
  - Lines 180-218: `/api/v1/accounts/{account_id}/balance`
  - Lines 220-302: `/api/v1/accounts/{account_id}/transactions`
    
- Methods
  - Lines 106-178: `POST`
  - Lines 181-218: `GET`
    
-	Request/response schemas
  -	Lines 328-343: `Amount`
  - Lines 345-351: `Balance`
  - Lines 353-365: `AccountCreated`
  - Lines 367-379: `BalanceWithDate`
  - Lines 381-405: `Payment`
  - Lines 407-431: `Transaction`
  - Lines 433-451: `TransactionList`
    
- Error codes
  - Lines 133-148: `200`
  - Lines 149-163: `400`
  - Lines 164-176: `401`
  - Lines 177-178: `500`

## Review the source code to identify endpoints, request/response, and error handling

The endpoints and logic are in /src/python/transactionify:
- Lambda handlers: `handlers/`
- Business logic: `services/`

## Review the unit tests to identify edge cases
The tests are in `test/unit/src/python/`:
- The handlers are in `handlers/test_autorizer.py`, `handlers/test_provisioning.py`
- The testing UUIDs are in `tools/validators/test_uuid.py`
The edge cases include:
- Invalid UUID format
- Missing required fields in `POST` requests.
- Unauthorized access

## Use provided URL and API key to test live API
- The base URL is `https://gj7edrv1il.execute-api.us-east-1.amazonaws.com`
- The API key is `7a94f6af-377d-7c2d-b73c-852ff8411d90`

To test the live API, you can use the following examples:
- Create account
```
curl -X POST "https://gj7edrv1il.execute-api.us-east-1.amazonaws.com/api/v1/accounts" -H "Content-Type: application/json" -H "x-api-key: 7a94f6af-377d-7c2d-b73c-852ff8411d90" \ -d '{"currency": "USD"}'
```
- Get balance
```
curl -X GET "https://gj7edrv1il.execute-api.us-east-1.amazonaws.com/api/v1/accounts/{account_id}/balance” -H "x-api-key: 7a94f6af-377d-7c2d-b73c-852ff8411d90"
```
## Document quirks
These are the quirks identified:
- **Authentication header**: Most APIs use OAuth or Bearer tokens. This one uses a custom header `x-api-key`.
- **Content-type**: If you omit `application/json` for `POST` requests, you get `415 Unsupported Media Type`. This strict requirement is a quirk.
- **Response format**: They are always JSON, but the IDs such as `accountId` and `paymentId` are strings, even though they look numeric.
- **Error codes**: Standard HTTP codes are used, but `400 Bad Request` often lacks detailed error messages, making debugging harder.
