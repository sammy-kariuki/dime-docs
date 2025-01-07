![Logo](https://dime.co.ke/images/logo.png)

# Dime Loans

This is an API for Dime partners to be able to facilitate Dime Loans on their platforms.

## Supported Features

- Authorization
- Fetching all Partner loans
- Fetching a single customer's loans
- Fetching loan products
- Posting borrowed loan

### In Progress

- Checking a corporate's status on Dime loans
- Fetching a customer's profile on Dime loans
- Fetching a single loan's details
- Triggering payment of a loan

## Authorization

Authentication / Authorization gives you access to endpoints / associated associated with your Dime Loans partner
account. You can get an access token by making a POST call.

### Request Parameters

The following parameters are expected:

| Field           | Description                                                       | Type   |
|-----------------|-------------------------------------------------------------------|--------|
| consumer_key    | An API key used to identify a customer                            | String |
| consumer_secret | A "password" used along with the `consumer_key` for authorization | String |

Sample implementation using Curl

```
curl --location 'https://api.dimeapp.co.ke/api/partner/token/' \
--header 'Content-Type: application/json' \
--data '{
    "consumer_key": "87_2pRSh]4%j}3xF:CxQ8em20;Wk3nP}yfq?bRpe",
    "consumer_secret": "j2_8Yfb6E5RZsg2+HAn5"
}'
```

### Response parameters

| Field   | Description                      | Type   |
|---------|----------------------------------|--------|
| token   | Access token to access resources | String |
| expires | Token expiry time in seconds     | String |

Below is a sample response

```json
{
  "token": "ZTJmMDg4NTNmOTM4ZTM0Y2FkMDc3N2Y4OTJhMzVk",
  "expires": "1659083476"
}
```

## Customer Registration

Customer registration is the endpoint for registering individual customers

### Request Parameters

The following parameters are expected:

| Field           | Description                                                                    | Type   |
|-----------------|--------------------------------------------------------------------------------|--------|
| identity_type   | Defines the type of identitification for the customer (National ID / Passport) | String |
| identity_number | The customer's ID number                                                       | String |
| first_name      | The customer's first name                                                      | String |
| last_name       | Customer's last name                                                           | String |
| email           | Customer's email address                                                       | String |
| phone_number    | The customer's mobile number                                                   | String |
| borrower_type   | Type of borrower the customer will belong to e.g. Secure, Weighted, Checkoff   | String |
| gender          | Customer gender (Male / Female)                                                | String |
| corporate       | The name of the corporate a customer belongs to e.g. Dime                      | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/loan-application/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "identity_type": "National ID",
    "identity_number": "12345678",
    "first_name": "John",
    "last_name": "Doe",
    "email": "johndoe@gmail.com",
    "corporate": "Dime Consultants Limited",
    "phone_number": "0711223344",
    "borrower_type": "Weighted",
    "gender": "Male"
}'
```

### Response parameters

| Field | Description                                                                 | Type   |
|-------|-----------------------------------------------------------------------------|--------|
| code  | Results code for either failed or successful. `200.001` means its a success | String |

### Error Codes

| Error   | Description             | Type   |
|---------|-------------------------|--------|
| 800.001 | API configuraiton error | String |
| 500.001 | Generic error           | String |

Below is a sample response

```json
{
  "code": "200.001"
}
```

## Fetch Partner Loans

Fetching partner loans allows you to view all loans that have been processed on Dime Loans. You can then implement a
server side DT.

### Request Parameters

The following parameters are expected:

| Field  | Description                                              | Type   |
|--------|----------------------------------------------------------|--------|
| length | The number of transactions to be returned                | String |
| start  | Where to start on the list of transactions to be fetched | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/all-loans/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "length": 25,
    "filter": '',
    "start": 0
}'
```

### Response parameters

| Field | Description                                                                 | Type   |
|-------|-----------------------------------------------------------------------------|--------|
| code  | Results code for either failed or successful. `200.001` means its a success | String |
| data  | A list of transactions                                                      | String |

Fields to be returned will be

```
"id", "customer_first_name", "customer_phone_number", "product_name", "interest_rate", "receipt", "receiver", "loan_limit", "amount", "fees_payable", "balance", "due_date", "status", "date_modified", "date_created"
```

Searchable fields are as follows:

```
"id", "customer_first_name", "customer_phone_number", "product_name", "receipt", "receiver", "fees_payable", "status"
```

Below is a sample response

```json
{
  "code": "200.001",
  "data": []
}
```

## Fetch A Customer's Loans

Fetching a customer's loans allows a customer to view all their active and completed loans on the Partner's app.

### Request Parameters

The following parameters are expected:

| Field  | Description                                              | Type   |
|--------|----------------------------------------------------------|--------|
| length | The number of transactions to be returned                | String |
| start  | Where to start on the list of transactions to be fetched | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/customer-loans/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "phone_number": "254700000000"
}'
```

### Response parameters

| Field | Description                                                                 | Type   |
|-------|-----------------------------------------------------------------------------|--------|
| code  | Results code for either failed or successful. `200.001` means its a success | String |
| data  | A list of transactions                                                      | String |

Fields to be returned will be:

```
'amount', 'balance', 'state__name', 'date_created'
```

Below is a sample response

```json
{
  "code": "200.001",
  "data": []
}
```

## Fetch Loan Products

Fetching loan products allows the customer to select the product that they will be borrowing against, on the Partner's
app.

### Request Parameters

The following parameters are expected:

| Field        | Description                                                 | Type   |
|--------------|-------------------------------------------------------------|--------|
| phone_number | This is the phone number belonging the transacting customer | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/loan-products/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "phone_number": "254700000000"
}'
```

### Response parameters

| Field        | Description                                           | Type   |
|--------------|-------------------------------------------------------|--------|
| phone_number | This is the phone number for the transacting customer | String |

Fields to be returned will be:

```
'name', 'interest_rate', 'product_id'
```

Below is a sample response

```json
{
  "code": "200.001",
  "data": []
}
```

## Loan Application

Borrow loan is the main endpoint for posting a loan request from a customer

### Request Parameters

The following parameters are expected:

| Field           | Description                                                    | Type   |
|-----------------|----------------------------------------------------------------|--------|
| phone_number    | This is the phone number belonging to the transacting customer | String |
| amount          | The amount that the customer wants to borrow                   | String |
| loan_product_id | The loan product that the customer is borrowing against        | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/loan-application/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "phone_number": "254700000000"
    "amount": "500"
    "loan_product_id": "600dbc16-31ee-4980-a1ae-79efd7f7d286"
}'
```

### Response parameters

| Field | Description                                                                 | Type   |
|-------|-----------------------------------------------------------------------------|--------|
| code  | Results code for either failed or successful. `200.001` means its a success | String |

### Error Codes

| Error   | Description                           | Type   |
|---------|---------------------------------------|--------|
| 404.002 | Customer not found                    | String |
| 400.004 | Account is frozen                     | String |
| 500.010 | Multiple active loans are not allowed | String |
| 400.020 | Invalid amount                        | String |
| 400.001 | Invalid disbursement destination.     | String |
| 400.017 | Loan limit exceeded.                  | String |
| 403.001 | General failure.                      | String |

Below is a sample response

```json
{
  "code": "200.001"
}
```

## Pay Loan

Pay loan is the main endpoint for triggering a loan payment request from a customer

### Request Parameters

The following parameters are expected:

| Field        | Description                                                    | Type   |
|--------------|----------------------------------------------------------------|--------|
| phone_number | This is the phone number belonging to the transacting customer | String |
| amount       | The amount that the customer wants to borrow                   | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/pay-loan/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "phone_number": "254700000000"
    "amount": "500"
}'
```

### Response parameters

| Field | Description                                                                 | Type   |
|-------|-----------------------------------------------------------------------------|--------|
| code  | Results code for either failed or successful. `200.001` means its a success | String |

Below is a sample response

```json
{
  "code": "200.001"
}
```

## Customer Profile / Details

Customer Profile / Details is the main endpoint for fetching a customer's details

### Request Parameters

The following parameters are expected:

| Field        | Description                                                    | Type   |
|--------------|----------------------------------------------------------------|--------|
| phone_number | This is the phone number belonging to the transacting customer | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/customer-details/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "phone_number": "254700000000"
}'
```

### Response parameters

| Field | Description                                                                 | Type   |
|-------|-----------------------------------------------------------------------------|--------|
| code  | Results code for either failed or successful. `200.001` means its a success | String |

Below is a sample response

```json
{
  "code": "200.001",
  "data": {
    "first_name": "First",
    "last_name": "Last",
    "other_name": "Other",
    "loan_limit": "1000",
    "identity_number": "12345678",
    "phone_number": "254700xxx000",
    "date_of_birth": "01/20/1990",
    "email": "example@email.com",
    "partner": "Partner Mae",
    "checkoff_organization": "Employer",
    "status": "Active"
  }
}
```

## Customer Wallet Payin

Customer Wallet paying transaction can be done using the following procedure

### Request Parameters

The following parameters are expected:

| Field                | Description                                                           | Type   |
|----------------------|-----------------------------------------------------------------------|--------|
| payee                | This is the phone number belonging to the transacting payee           | String |
| originator_reference | This is the originator's reference to enable them to identify results | String |
| customer_identifier  | This is the customer's unique identifier                              | String |
| amount               | This is the amount being transacted                                   | String |
| results_url          | A valid url where the transaction results will be sent                | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/payin/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "payee": "254700000000"
    "originator_reference": "ADM134114"
    "customer_identifier": "31e7ea6d-e684-410b-9c6e-c84e3dde41e4"
    "amount": "1000",
    "results_url": "https://yourdomain.com/payincallback",
}'
```

### Response parameters

| Field       | Description                                                                 | Type   |
|-------------|-----------------------------------------------------------------------------|--------|
| code        | Results code for either failed or successful. `200.001` means its a success | String |
| reference   | A unique reference for the transaction                                      | String |
| description | A description of the results. Whether the transaction is successful.        | String |

Below is a sample response

```json
{
  "code": "200.001",
  "reference": "DM-12344",
  "description": "Success. Request accepted for processing"
}
```

### Callback / Results Response parameters

| Field                | Description                                                             | Type   |
|----------------------|-------------------------------------------------------------------------|--------|
| code                 | Results code for either failed or successful. 0 means its a success     | String |
| description          | A description of the results. Whether the transaction is successful.    | String |
| originator_reference | The unique reference provided in the initiation of the request          | String |
| transaction_id       | The dime transaction id or reference                                    | String |
| amount               | The amount transacted                                                   | String |
| receipt              | The final network receipt                                               | String |
| completion_date      | A date and time stamp indicating the when the transaction was completed | String |

Below is a sample response

```json
{
  "code": 0,
  "description": "The service request is processed successfully.",
  "originator_reference": "REF12344",
  "transaction_id": "DM-12344",
  "amount": 1000,
  "receipt": "ABQ997D0YDG",
  "completion_date": "2024-12-04 01:49:14.046590+00:00"
}
```

## Customer Wallet Payout

Customer Wallet paying transaction can be done using the following procedure

### Request Parameters

The following parameters are expected:

| Field                | Description                                                           | Type   |
|----------------------|-----------------------------------------------------------------------|--------|
| recipient            | This is the phone number belonging to the transacting recipient       | String |
| originator_reference | This is the originator's reference to enable them to identify results | String |
| customer_identifier  | This is the customer's unique identifier                              | String |
| amount               | This is the amount being transacted                                   | String |
| results_url          | A valid url where the transaction results will be sent                | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/payout/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "recipient": "254700000000"
    "originator_reference": "ADM134114"
    "customer_identifier": "31e7ea6d-e684-410b-9c6e-c84e3dde41e4"
    "amount": "1000",
    "results_url": "https://yourdomain.com/payoutcallback",
}'
```

### Response parameters

| Field       | Description                                                                 | Type   |
|-------------|-----------------------------------------------------------------------------|--------|
| code        | Results code for either failed or successful. `200.001` means its a success | String |
| reference   | A unique reference for the transaction                                      | String |
| description | A description of the results. Whether the transaction is successful.        | String |

Below is a sample response

```json
{
  "code": "200.001",
  "reference": "DM-12344",
  "description": "Success. Request accepted for processing"
}
```

### Callback / Results Response parameters

| Field                | Description                                                             | Type   |
|----------------------|-------------------------------------------------------------------------|--------|
| code                 | Results code for either failed or successful. 0 means its a success     | String |
| description          | A description of the results. Whether the transaction is successful.    | String |
| originator_reference | The unique reference provided in the initiation of the request          | String |
| transaction_id       | The dime transaction id or reference                                    | String |
| amount               | The amount transacted                                                   | String |
| receipt              | The final network receipt                                               | String |
| completion_date      | A date and time stamp indicating the when the transaction was completed | String |

Below is a sample response

```json
{
  "code": 0,
  "description": "The service request is processed successfully.",
  "originator_reference": "REF12344",
  "transaction_id": "DM-12344",
  "amount": 1000,
  "receipt": "ABQ997D0YDG",
  "completion_date": "2024-12-04 01:49:14.046590+00:00"
}
```

## Customer Wallet Balance

Customer Wallet balance can be retrieved as follows

### Request Parameters

The following parameters are expected:

| Field               | Description                              | Type   |
|---------------------|------------------------------------------|--------|
| customer_identifier | This is the customer's unique identifier | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/wallet-balance/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "customer_identifier": "31e7ea6d-e684-410b-9c6e-c84e3dde41e4"
}'
```

### Response parameters

| Field   | Description                                                                 | Type   |
|---------|-----------------------------------------------------------------------------|--------|
| code    | Results code for either failed or successful. `200.001` means its a success | String |
| balance | The customer account's available balance                                    | String |

Below is a sample response

```json
{
  "code": "200.001",
  "balance": "10.00"
}
```

## Transaction Status

A single transaction's status can be retrieved as follows

### Request Parameters

The following parameters are expected:

| Field     | Description                                                    | Type   |
|-----------|----------------------------------------------------------------|--------|
| reference | This is the transaction's unique reference as returned by Dime | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/transaction-status/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "reference": "DM-12344"
}'
```

### Response parameters

| Field                | Description                                                             | Type   |
|----------------------|-------------------------------------------------------------------------|--------|
| code                 | Results code for either failed or successful. 0 means its a success     | String |
| description          | A description of the results. Whether the transaction is successful.    | String |
| originator_reference | The unique reference provided in the initiation of the request          | String |
| transaction_id       | The dime transaction id or reference                                    | String |
| amount               | The amount transacted                                                   | String |
| receipt              | The final network receipt                                               | String |
| completion_date      | A date and time stamp indicating the when the transaction was completed | String |

Below is a sample response

```json
{
  "code": 0,
  "description": "The service request is processed successfully.",
  "originator_reference": "REF12344",
  "transaction_id": "DM-12344",
  "amount": 1000,
  "receipt": "ABQ997D0YDG",
  "completion_date": "2024-12-04 01:49:14.046590+00:00"
}
```

## Pull Transactions

Pull a customer's transactions as follows

### Request Parameters

The following parameters are expected:

| Field               | Description                                                                                                                                             | Type   |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|--------|
| customer_identifier | This is the customer's unique identifier as returned by Dime                                                                                            | String |
| start_date          | Lower limit time                                                                                                                                        | String |
| end_date            | Upper limit time                                                                                                                                        | String |
| trans_type          | Customer's wallet transaction types: Payin or Payout                                                                                                    | String |
| offset_value        | Starts from 0. The service uses offset as opposed to page numbers. The OFF SET value allows you to specify from which row to start from retrieving data | String |

Sample implementation using Curl

```curl
curl --location 'https://api.dimeapp.co.ke/api/partner/pull-transactions/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "customer_identifier": "31e7ea6d-e684-410b-9c6e-c84e3dde41e4"
    "start_date": ""2024-10-19 11:41:27","
    "end_date": ""2024-10-19 11:50:27","
    "trans_type": "Payin"
    "offset_value": 0
}'
```

### Response parameters

| Field | Description                                                               | Type   |
|-------|---------------------------------------------------------------------------|--------|
| code  | Results code for either failed or successful. 200.001 means its a success | String |
| data  | A list of the transactions.                                               | String |

Below is a sample response

```json
{
  "code": "200.001",
  "data": [
    {
      "reference": "DM-12344",
      "phone_number": "254700000000",
      "transaction_date": "2020-08-05T10:13:00Z",
      "recipient": "72200000 - John Doe",
      "originator_reference": "UAT2",
      "trans_type": "Payin",
      "amount": "876.00"
    }
  ]
}
```