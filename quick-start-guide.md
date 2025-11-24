# Transactionify Quick Start Guide

## Overview

The Transactionify API allows you to manage accounts and payment transactions. This guide helps you make your first successful API call.

## Prerequisites

- Base URL:
  ```
  https://gj7edrv1il.execute-api.us-east-1.amazonaws.com
  ```
- API Key:
  ```
  7a94f6af-377d-7c2d-b73c-852ff8411d90
  ```
- Tools:
  - `curl` or Postman
  - JSON support in you language of choice.

## Authentication
All requests require the following header:
```
x-api-key: YOUR_API_KEY
```

## Create your first account

You can use `POST /api/v1/accounts/` to create an account:
```
curl -X POST "https://gj7edrv1il.execute-api.us-east-1.amazonaws.com/api/v1/accounts" -H "Content-Type: application/json" -H "x-api-key: 7a94f6af-377d-7c2d-b73c-852ff8411d90" -d '{"currency": "USD"}'
```
The expected response is:
```
{
  "accountId": "12345",
  "currency": "USD",
  "balance": 0.00
}
```
If there are any questions or comments, please send an email to Jose Soto at `jsoto.soto09@gmail.com`.
