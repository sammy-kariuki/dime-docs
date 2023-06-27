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

Authentication / Authorization gives you access to endpoints / associated associated with your Dime Loans partner account. You can get an access token by making a POST call.

### Request Parameters

The following parameters are expected:

| Field           | Description                                                       | Type   |
| --------------- | ----------------------------------------------------------------- | ------ |
| consumer_key    | An API key used to identify a customer                            | String |
| consumer_secret | A "password" used along with the `consumer_key` for authorization | String |

Sample implementation using Curl

```
curl --location 'https://dimeapp.co.ke/api/partner/token/' \
--header 'Content-Type: application/json' \
--data '{
    "consumer_key": "87_2pRSh]4%j}3xF:CxQ8em20;Wk3nP}yfq?bRpe",
    "consumer_secret": "j2_8Yfb6E5RZsg2+HAn5"
}'
```

### Response parameters

| Field   | Description                      | Type   |
| ------- | -------------------------------- | ------ |
| token   | Access token to access resources | String |
| expires | Token expiry time in seconds     | String |

Below is a sample response

```json
{
  "token": "ZTJmMDg4NTNmOTM4ZTM0Y2FkMDc3N2Y4OTJhMzVk",
  "expires": "1659083476"
}
```

## Fetch Partner Loans

Fetching partner loans allows you to view all loans that have been processed on Dime Loans. You can then implement a server side DT.

### Request Parameters

The following parameters are expected:

| Field  | Description                                              | Type   |
| ------ | -------------------------------------------------------- | ------ |
| length | The number of transactions to be returned                | String |
| start  | Where to start on the list of transactions to be fetched | String |

Sample implementation using Curl

```curl
curl --location 'https://dimeapp.co.ke/api/partner/all-loans/' \
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
| ----- | --------------------------------------------------------------------------- | ------ |
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
| ------ | -------------------------------------------------------- | ------ |
| length | The number of transactions to be returned                | String |
| start  | Where to start on the list of transactions to be fetched | String |

Sample implementation using Curl

```curl
curl --location 'https://dimeapp.co.ke/api/partner/customer-loans/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "phone_number": "254700000000"
}'
```

### Response parameters

| Field | Description                                                                 | Type   |
| ----- | --------------------------------------------------------------------------- | ------ |
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

Fetching loan products allows the customer to select the product that they will be borrowing against, on the Partner's app.

### Request Parameters

The following parameters are expected:

| Field        | Description                                                 | Type   |
| ------------ | ----------------------------------------------------------- | ------ |
| phone_number | This is the phone number belonging the transacting customer | String |

Sample implementation using Curl

```curl
curl --location 'https://dimeapp.co.ke/api/partner/loan-products/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Yjc1Njk4YjEwMDNjYmVhOWZkYjU4YjViZjZmOWMx' \
--data '{
    "phone_number": "254700000000"
}'
```

### Response parameters

| Field        | Description                                           | Type   |
| ------------ | ----------------------------------------------------- | ------ |
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

## Borrow Loan

Borrow loan is the main endpoint for posting a loan request from a customer

### Request Parameters

The following parameters are expected:

| Field           | Description                                                    | Type   |
| --------------- | -------------------------------------------------------------- | ------ |
| phone_number    | This is the phone number belonging to the transacting customer | String |
| amount          | The amount that the customer wants to borrow                   | String |
| loan_product_id | The loan product that the customer is borrowing against        | String |

Sample implementation using Curl

```curl
curl --location 'https://dimeapp.co.ke/api/partner/loan-products/' \
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
| ----- | --------------------------------------------------------------------------- | ------ |
| code  | Results code for either failed or successful. `200.001` means its a success | String |

Below is a sample response

```json
{
  "code": "200.001"
}
```
