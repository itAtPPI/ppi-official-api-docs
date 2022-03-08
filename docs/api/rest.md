# REST API

!!! note ""

    __Current API version: 1.0__


## ACCOUNT
### Login

Tries to log in with the given credentials. Returns a session token which is needed to use the API.

``` title="Method: POST"
/api/{v}/Account/Login
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "user": "string",
  "password": "string"
}
```

``` JSON title="Response Body - 200 Success"
[
  {
    "creationDate": "2022-01-18T18:25:36.269Z",
    "expirationDate": "2022-01-18T18:25:36.269Z",
    "accessToken": "string",
    "expires": 0,
    "refreshToken": "string",
    "tokenType": "string"
  }
]
```


### Refresh Token
Tries to refresh the current session. Returns a new session token which is needed to continue using the API.

``` title="Method: POST"
/api/{v}/Account/RefreshToken
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "refreshToken": "string"
}
```

``` JSON title="Response Body - 200 Success"
[
  {
    "creationDate": "2022-01-18T19:47:00.054Z",
    "expirationDate": "2022-01-18T19:47:00.054Z",
    "accessToken": "string",
    "expires": 0,
    "refreshToken": "string",
    "tokenType": "string"
  }
]
```



### Accounts
Retrieves all the available accounts and their officer for the current session.

``` title="Method: GET"
/api/{v}/Account/Accounts
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |


``` JSON title="Response Body - 200 Success"
[
  {
    "accountNumber": "string",
    "name": "string",
    "officer": {
      "name": "string",
      "eMail": "string",
      "phone": "string"
    }
  }
]
```


### Bank Accounts
Retrieves all the available bank accounts for the given account.

``` title="Method: GET"
/api/{v}/Account/BankAccounts
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| accountNumber      | string  |  required  | Account number to retrieve bank accounts | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
  {
    "bankName": "string",
    "bankAccountNumber": "string",
    "bankIdentifier": "string",
    "currency": "string",
    "taxHolderIdentifier": "string"
  }
]
```


### Available Balance
Retrieves cash balance available for trading for the given account.

``` title="Method: GET"
/api/{v}/Account/AvailableBalance
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| accountNumber      | string  | required   | Account number to retrieve availability | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
  {
    "name": "string",
    "simbol": "string",
    "amount": 0,
    "settlement": "string"
  }
]
```


### Balances and Positions
Retrieves account balance and positions for the given account.

``` title="Method: GET"
/api/{v}/Account/BalancesAndPositions
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| accountNumber      | string  | required   | Account number to retrieve balance and position | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
  {
    "currency": "string",
    "availability": [
      {
        "name": "string",
        "simbol": "string",
        "amount": 0,
        "settlement": "string"
      }
    ]
  }
]
```


### Account Movements
Retrieves movements for the given account between the specified dates.

``` title="Method: GET"
/api/{v}/Account/Movements
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| accountNumber      | string  |  required   | Account number to retrieve movements | Query Params |
| dateFrom       | string     ($date-time)  | optional   | Start date to filter the retrieve movements   | Query Params |
| dateTo       | string ($date-time)  | optional | Finish date to filter the retrieve movements | Query Params |
| ticker       | string  | optional | Ticker to filter the retrieve movements |  Query Params|
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
  {
    "agreementDate": "2022-01-19T14:32:51.776Z",
    "settlementDate": "2022-01-19T14:32:51.776Z",
    "currency": "string",
    "amount": 0,
    "price": 0,
    "description": "string",
    "ticker": "string",
    "quantity": 0,
    "balance": 0
  }
]
```
## CONFIGURATION
### Instrument Types
Retrieves a list of available instrument types.

``` title="Method: GET"
/api/{v}/Configuration/InstrumentTypes
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
    "BONOS",
    "LETRAS",
    "NOBAC",
    "LEBAC",
    "ON",
    "FCI",
    "CAUCIONES",
    "ACCIONES",
    "ETF",
    "CEDEARS",
    "OPCIONES",
    "FUTUROS",
    "ACCIONES-USA",
    "FCI-EXTERIOR"
]
```
### Markets
Retrieves a list of available markets.

``` title="Method: GET"
/api/{v}/Configuration/Markets
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
    "ROFEX",
    "OTC",
    "NYSE",
    "BYMA"
]
```
### Settlements
Retrieves a list of available settlements.

``` title="Method: GET"
/api/{v}/Configuration/Settlements
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
    "INMEDIATA",
    "A-24HS",
    "A-48HS",
    "A-72HS"
]
```
### Quantity Types
Retrieves a list of available quantity types.

``` title="Method: GET"
/api/{v}/Configuration/QuantityTypes
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
    "DINERO",
    "PAPELES",
    "CANTIDAD-TOTAL"
]
```
### Operation Terms
Retrieves a list of available operation terms.

``` title="Method: GET"
/api/{v}/Configuration/OperationTerms
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
    "POR-EL-DÍA",
    "HASTA-SU-EJECUCIÓN",
    "VÁLIDA-HASTA-EL",
    "72-HS"
]
```

### Operation Types
Retrieves a list of available operation types.

``` title="Method: GET"
/api/{v}/Configuration/OperationTypes
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
    "PRECIO-DE-MERCADO",
    "PRECIO-LIMITE"
]
```

### Operations
Retrieves a list of available operations.

``` title="Method: GET"
/api/{v}/Configuration/Operations
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
    "COMPRA",
    "VENTA",
    "SUSCRIPCIÓN-FCI",
    "RESCATE-FCI",
    "COLOCAR-CAUCIÓN"
]
```
## MARKET DATA
### Search Instrument
Search for items matching a given filter.

``` title="Method: GET"
/api/{v}/MarketData/SearchInstrument
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "ticker": "string",
  "name": "string",
  "market": "string",
  "type": "string"
}
```

``` JSON title="Response Body - 200 Success"
[
    {
        "ticker": "string",
        "description": "string",
        "currency": "string",
        "type": "string",
        "market": "string"
    }, ...
]
```

### Historical Market Data
Search for historical market data.

``` title="Method: GET"
/api/{v}/MarketData/Search
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "ticker": "string",
  "type": "string",
  "dateFrom": "2022-01-19T16:05:20.623Z",
  "dateTo": "2022-01-19T16:05:20.623Z",
  "settlement": "string"
}
```

``` JSON title="Response Body - 200 Success"
[
    {
        "date": "string",
        "price": integer,
        "volume": integer,
        "openingPrice": integer,
        "max": integer,
        "min": integer
    }, ...
]
```


### Current Market Data
Search for current market data.

``` title="Method: GET"
/api/{v}/MarketData/Current
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "ticker": "string",
  "type": "string",
  "settlement": "string"
}
```

``` JSON title="Response Body - 200 Success"
{
    "date": "string",
    "price": integer,
    "volume": integer,
    "openingPrice": integer,
    "max": integer,
    "min": integer
}
```


### Intraday Market Data
Search for intraday market data.

``` title="Method: GET"
/api/{v}/MarketData/Intraday
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "ticker": "string",
  "type": "string",
  "settlement": "string"
}
```

``` JSON title="Response Body - 200 Success"
[
    {
        "date": "string",
        "price": integer,
        "volume": integer
    }, ...
]
```

### Bids and Offers on Instruments
Search for the instrument current book of bids and offers.

``` title="Method: GET"
/api/{v}/MarketData/Book
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "ticker": "string",
  "type": "string",
  "settlement": "string"
}
```

``` JSON title="Response Body - 200 Success"
{
  "date": "2022-02-24T12:31:04.581Z",
  "offers": [
    {
      "position": integer,
      "price": integer,
      "quantity": integer
    }
  ],
  "bids": [
    {
      "position": integer,
      "price": integer,
      "quantity": integer
    }
  ]
}
```


























## ORDERS
### Orders
Retrieves all the filter and active orders for the given account.

``` title="Method: GET"
/api/{v}/Order/Orders
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| accountNumber      | string  |  required  | Account number | Query Params |
| dateFrom       | string ($date-time)  | optional   | Date from (Default last month) | Query Params |
| dateTo       | string ($date-time)  | optional | Date to (Default current date)  | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
  {
    "id": 0,
    "instrumentType": "string",
    "operation": "string",
    "ticker": "string",
    "status": "string",
    "date": "2022-01-31T19:28:19.627Z",
    "settlement": "string",
    "quantity": 0,
    "orderType": "string",
    "operationType": "string",
    "operationMaxDate": "2022-01-31T19:28:19.627Z",
    "price": 0,
    "currency": "string",
    "amount": 0,
    "externalID": "string"
  }
]
```

### Order Detail
Retrieves the information for the order.

``` title="Method: GET"
/api/{v}/Order/Detail
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "accountNumber": "string",
  "orderID": 0
}
```

``` JSON title="Response Body - 200 Success"
{
  "id": 0,
  "instrumentType": "string",
  "operation": "string",
  "ticker": "string",
  "status": "string",
  "date": "2022-01-31T19:39:06.888Z",
  "settlement": "string",
  "quantity": 0,
  "orderType": "string",
  "operationType": "string",
  "operationMaxDate": "2022-01-31T19:39:06.888Z",
  "price": 0,
  "currency": "string",
  "amount": 0,
  "externalID": "string"
}
```

### Budget
Retrieves a budget for a new order.

``` title="Method: POST"
/api/{v}/Order/Budget
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "accountNumber": "string",
  "quantity": 0,
  "price": 0,
  "ticker": "string",
  "instrumentType": "string",
  "quantityType": "string",
  "operationTerm": "string",
  "operationMaxDate": "2022-01-31T19:41:14.973Z",
  "operation": "string",
  "settlement": "string",
  "operationType": "string",
  "disclaimers": [
    {
      "code": "string",
      "description": "string",
      "mandatory": true,
      "accepted": true
    }
  ],
  "externalID": "string"
}
```

``` JSON title="Response Body - 200 Success"
{
  "id": 0,
  "instrumentType": "string",
  "operation": "string",
  "ticker": "string",
  "status": "string",
  "date": "2022-01-31T19:41:14.986Z",
  "settlement": "string",
  "quantity": 0,
  "orderType": "string",
  "operationType": "string",
  "operationMaxDate": "2022-01-31T19:41:14.986Z",
  "price": 0,
  "currency": "string",
  "amount": 0,
  "disclaimers": [
    {
      "code": "string",
      "description": "string",
      "mandatory": true,
      "accepted": true
    }
  ]
}
```
### New Order
Confirm the creation for a new order.

``` title="Method: POST"
/api/{v}/Order/Confirm
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "accountNumber": "string",
  "quantity": 0,
  "price": 0,
  "ticker": "string",
  "instrumentType": "string",
  "quantityType": "string",
  "operationTerm": "string",
  "operationMaxDate": "2022-01-31T19:55:43.160Z",
  "operation": "string",
  "settlement": "string",
  "operationType": "string",
  "disclaimers": [
    {
      "code": "string",
      "description": "string",
      "mandatory": true,
      "accepted": true
    }
  ],
  "externalID": "string"
}
```

``` JSON title="Response Body - 200 Success"
{
  "id": 0,
  "instrumentType": "string",
  "operation": "string",
  "ticker": "string",
  "status": "string",
  "date": "2022-02-24T11:59:18.708Z",
  "settlement": "string",
  "quantity": 0,
  "orderType": "string",
  "operationType": "string",
  "operationMaxDate": "2022-02-24T11:59:18.708Z",
  "price": 0,
  "currency": "string",
  "amount": 0,
  "externalID": "string"
}
```

### Cancel an Order
Request the cancel for an order.

``` title="Method: POST"
/api/{v}/Order/Cancel
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Request Body"
{
  "accountNumber": "string",
  "orderID": 0,
  "externalID": "string"
}
```

``` JSON title="Response Body - 200 Success"
{
  "id": 0,
  "instrumentType": "string",
  "operation": "string",
  "ticker": "string",
  "status": "string",
  "date": "2022-01-31T19:58:00.749Z",
  "settlement": "string",
  "quantity": 0,
  "orderType": "string",
  "operationType": "string",
  "operationMaxDate": "2022-01-31T19:58:00.749Z",
  "price": 0,
  "currency": "string",
  "amount": 0
}
```

### Mass Cancel of Orders
Request the cancel for all alive orders for the given account.

``` title="Method: POST"
/api/{v}/Order/MassCancel
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| accountNumber   | string  | required | Account number | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
string
```