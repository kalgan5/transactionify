# Internal Developer Guide: Enrolling New Users and Running Transactionify Service

## Overview
This guide is intended for LoanPro backend engineers who are responsible for managing and maintaining Transactionify.

## How to enroll new users
### Prerequisites
- Access to LoanPro internal database.
- API key for Transactionify service.
- AWS credentials for Lambda/API Gateway if needed.

### Steps
1.	Create the user record in the database:
```
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
```
2.	Activate user via internal API:
```
curl -X POST "{{base_url}}/internal/enroll" -H "x-api-key: {{api_key}}" -H "Content-Type: application/json" -d '{"userId": 123}'
```
3.	Verify the enrollment of the new user.
```
curl -X GET "{{base_url}}/internal/users/123" -H "x-api-key: {{api_key}}"
```

## Local environment setup
### Prerequisites
- Python 3.9+
- Node.JS 18+
- AWS CDK CLI installed

### Steps
```
git clone https://github.com/rrgarciach/transactionify.git
cd transactionify
pip install -r requirements.txt
npm install
npm run start:dev
pytest
```

## Deployment instructions
### Prerequisites
- AWS account with permissions for Lambda, API Gateway, and DynamoDB.
- AWS CLI configured locally.

### Steps
```
cdk bootstrap
cdk deploy
```

