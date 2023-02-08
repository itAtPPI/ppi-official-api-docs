# REST API

!!! note ""

    __Version actual de API: 1.0__

__Swagger Sandbox__
!!! note ""

    Accede a la documetación de swagger sandbox a traves de este [__link__](https://clientapi_sandbox.portfoliopersonal.com/swagger/index.html).

__Postman Collection__
!!! note ""

    Accede a la collección de postman a traves de este [__link__](https://elements.getpostman.com/redirect?entityId=15036183-639ce544-e507-4f0f-9f36-87131e32b52b&entityType=collection). Para poder usar nuestra colección de Postman deberás crear una bifurcación o "Fork".

- Realizá un fork.
- Completá estos datos para comenzar a loguearte:
    - ApiKey 
    - ApiSecret
    - AuthorizedClient 
    - ClientKey
- El AuthorizedClient y ClientKey de Sandbox(testing) los recibis en un email. Los productivos se encuentran dentro de tu cuenta en Gestiones -> Gestión de servicio API.    
- Recordá que cada un tiempo el token se vence y debes volver a pedir un token con endpoint de Refresh token.

## ACCOUNT
### Login

Tries to log in with the given credentials. Returns a session token which is needed to use the API.

``` title="Method: POST"
/api/{v}/Account/LoginApi
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |
| ApiKey    | string  | required | Api Key for the API | Headers |
| ApiSecret    | string  | required | Api Secret for the API | Headers |


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

### Investing Profile Questions
Retrieves all investing profile questions and possible answers.

``` title="Method: GET"
/api/{v}/Account/InvestingProfileQuestions
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
    "code": "string",
    "description": "string",
    "answers": [
      {
        "code": "string",
        "description": "string"
      }
    ]
  }
]
```

### Investing P. Instrument Types
Retrieves all investing profile instrument types.

``` title="Method: GET"
/api/{v}/Account/InvestingProfileInstrumentTypes
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
  "string"
]
```

### Get Investing Profile 
Retrieves investing profile for the given account number.

``` title="Method: GET"
/api/{v}/Account/InvestingProfile
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| accountNumber      | string  |  required   | Account number to retrieve investing profile | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
{
  "date": "2022-05-06T14:25:29.920Z",
  "type": "string",
  "description": "string"
}
```

### Create Investing Profile
Creates a new investing profile for an account.

``` title="Method: POST"

​/api​/{v}​/Account​/InvestingProfile
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
  "answers": [
    {
      "questionCode": "string",
      "answerCode": "string"
    }
  ],
  "instrumentTypes": [
    "string"
  ]
}
```

``` JSON title="Response Body - 200 Success"
{
  "date": "2022-05-06T14:31:38.638Z",
  "type": "string",
  "description": "string"
}
```

### Register Bank Account
Creates a request to register a new bank account.

``` title="Method: POST"

/api/{v}/Account/BankAccount
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
  "currency": "string",
  "cbu": "string",
  "cuit": "string",
  "alias": "string",
  "bankAccountNumber": "string"
}
```

``` JSON title="Response Body - 200 Success"
string
```

### Cancel Bank Account
Creates a request to cancel a bank account.

``` title="Method: POST"

/api/{v}/Account/CancelBankAccount
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
  "cbu": "string",
  "bankAccountNumber": "string"
}
```

``` JSON title="Response Body - 200 Success"
string
```

### Register Foreing Bank A.
Creates a request to register a new foreign bank account.

``` title="Method: POST"

​/api​/{v}​/Account​/ForeignBankAccount
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| Request       | object  | required | ForeignBankAccountRequest DTO | Query |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` title="ForeignBankAccountRequest DTO Body"
{
  "accountNumber": "string",
  "cuit": "string",
  "intermediaryBank": "string",
  "intermediaryBankAccountNumber": "string",
  "swiftIntermediaryBank": "string",
  "bank": "string",
  "bankAccountNumber": "string",
  "swift": "string",
  "ffc": "string"
}
```

Request Body:

| Key       | Value  | Type  | Content-Type |
| ----------  | ------------------------------------ | ----------- | ----------- | 
| extractFile  | Extract Bank Information File (PDF) | string($binary) | multipart/form-data |


``` JSON title="Response Body - 200 Success"
string
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

### Holidays
Retrieves a list of holidays.

``` title="Method: GET"
/api/{v}/Configuration/Holidays
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| start       | string ($date-time)  | optional   | 	Start date to filter the holidays (Default this year) | Query Params |
| end       | string ($date-time)  | optional | End date to filter the holidays (Default this year)  | Query Params |
| isUSA      | boolean  |  optional  | Defines if holiday is in USA or not (Default: false) | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
  {
    "date": "2022-05-06T15:42:43.473Z",
    "description": "string",
    "isUSA": true
  }
]
```


### Local Holiday
Retrieves if actual date is a local holiday.

``` title="Method: GET"
/api/{v}/Configuration/IsLocalHoliday
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
true/false
```

### USA Holiday
Retrieves if actual date is USA holiday.

``` title="Method: GET"
/api/{v}/Configuration/IsUSAHoliday
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
true/false
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
| Ticker      | string  | required | Ticker of the instrument | Query Params |
| Name       | string  | optional | Name or description of the instrument | Query Params |
| Market       | string  | optional | Market of the instrument | Query Params |
| Type       | string  | optional | Type of the instrument | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |


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
| Ticker      | string  | required | Ticker of the instrument | Query Params |
| Type       | string  | required | Type of the instrument | Query Params |
| DateFrom       | string ($date-time)  | required | Start date to filter data | Query Params |
| DateTo       | string ($date-time) | required | End date to filter data | Query Params |
| Settlement       | string  | required | Settlement | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |


``` JSON title="Response Body - 200 Success"
[
    {
        "date": "2022-02-24T12:31:04.581Z",
        "price": 0,
        "volume": 0,
        "openingPrice": 0,
        "max": 0,
        "min": 0
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
| Ticker      | string  | required | Ticker of the instrument | Query Params |
| Type       | string  | required | Type of the instrument | Query Params |
| Settlement       | string  | required | Settlement | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |


``` JSON title="Response Body - 200 Success"
{
    "date": "2022-02-24T12:31:04.581Z",
    "price": 0,
    "volume": 0,
    "openingPrice": 0,
    "max": 0,
    "min": 0
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
| Ticker      | string  | required | Ticker of the instrument | Query Params |
| Type       | string  | required | Type of the instrument | Query Params |
| Settlement       | string  | required | Settlement | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
    {
        "date": "2022-02-24T12:31:04.581Z",
        "price": 0,
        "volume": 0
    }, ...
]
```

### Bonds calculator 
Calculate the return on your investments.

``` title="Method: GET"
/api/{v}/MarketData/Bonds/Estimate
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| Ticker      | string  | required | Ticker of the instrument | Query Params |
| Date       | string ($date-time)  | required | Date from | Query Params |
| QuantityType       | string  | required | Quantity Type | Query Params |
| Quantity       | number ($double)  | required | Quantity | Query Params |
| AmountOfMoney       | number ($double)  | required | Amount of money | Query Params |
| Price       | number ($double)  | required | Price | Query Params |
| ExchangeRate       | number ($double)  | required | Exchange rate | Query Params |
| EquityRate       | number ($double)  | required | Equity rate | Query Params |
| ExchangeRateAmortization       | number ($double)  | required | Exchange Rate Amortization  | Query Params |
| RateAdjustmentAmortization       | number ($double)  | required | Rate Adjustment Amortization | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |

``` JSON title="Response Body - 200 Success"
[
  {
    "flows": [
      {
        "cuttingDate": "2022-11-09T14:29:09.823Z",
        "residualValue": 0,
        "rent": 0,
        "amortization": 0,
        "total": 0,
        "quantity": 0,
        "ticker": "string",
        "currency": "string",
        "name": "string"
      }
    ],
    "sensitivity": [
      {
        "tir": 0,
        "price": 0,
        "parity": 0,
        "variation": 0
      }
    ],
    "tir": 0,
    "md": 0,
    "interestAccrued": 0,
    "parity": 0,
    "technicalValue": 0,
    "residualValue": 0,
    "totalRevenue": 0,
    "totalAmortization": 0,
    "total": 0,
    "currency": "string",
    "amountToInvest": 0,
    "amountToReceive": 0,
    "quantityTitles": 0,
    "currentCoupon": 0,
    "title": "string",
    "abbreviationCurrencyPay": "string",
    "abbreviationCurrencyIssuance": "string",
    "issuer": "string",
    "issueCurrency": "string",
    "amortization": "string",
    "interests": "string",
    "issueDate": "string",
    "expirationDate": "string",
    "law": "string",
    "minimalSheet": "string",
    "isin": "string"
  }
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
| Ticker      | string  | required | Ticker of the instrument | Query Params |
| Type       | string  | required | Type of the instrument | Query Params |
| Settlement       | string  | required | Settlement | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |


``` JSON title="Response Body - 200 Success"
{
  "date": "2022-02-24T12:31:04.581Z",
  "offers": [
    {
      "position": 0,
      "price": 0,
      "quantity": 0
    }
  ],
  "bids": [
    {
      "position": 0,
      "price": 0,
      "quantity": 0
    }
  ]
}
```

## ORDERS

### Active Orders
Retrieves all the active orders for the given account.

``` title="Method: GET"
/api/{v}/Order/ActiveOrders
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| accountNumber      | string  |  required  | Account number | Query Params |
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
    "date": "2022-05-06T18:06:46.549Z",
    "settlement": "string",
    "quantity": 0,
    "orderType": "string",
    "operationType": "string",
    "operationMaxDate": "2022-05-06T18:06:46.549Z",
    "price": 0,
    "currency": "string",
    "amount": 0,
    "externalID": "string"
  }
]
```

### Orders
Retrieves all the orders between two dates for the given account.

``` title="Method: GET"
/api/{v}/Order/Orders
```

Parameters:

| Name        | Type                                 | Mandatory   | Description                          | Location |
| ----------  | ------------------------------------ | ----------- | ------------------------------------ | -------- |
| accountNumber      | string  |  required  | Account number | Query Params |
| dateFrom       | string ($date-time)  | optional   | 	Start date to filter the orders | Query Params |
| dateTo       | string ($date-time)  | optional | Finish date to filter the orders  | Query Params |
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
| accountNumber      | string  |  required  | Account number | Query Params |
| orderID       | integer($int32)  | required   | 	PPI Order Id | Query Params |
| externalID       | string  | optional | Client ID for the order  | Query Params |
| v       | string  | required | API Version | Path |
| AuthorizedClient       | string  | required | Authorized Client ID for the API | Headers |
| ClientKey    | string  | required | Client Key for the API | Headers |


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
  "activationPrice": 0,
  "ticker": "string",
  "instrumentType": "string",
  "quantityType": "string",
  "operationTerm": "string",
  "operationMaxDate": "2022-05-10T16:56:29.426Z",
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
  "activationPrice": 0,
  "ticker": "string",
  "instrumentType": "string",
  "quantityType": "string",
  "operationTerm": "string",
  "operationMaxDate": "2022-05-10T16:59:37.549Z",
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

### Budget of Transfer
Budget a transfer and returns the transfer with the budget.

``` title="Method: POST"
/api/{v}/Order/BudgetTransfer
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
  "cuit": "string",
  "currency": "string",
  "cbu": "string",
  "bankAccountNumber": "string",
  "amount": 0
}
```

``` JSON title="Response Body - 200 Success"
{
  "accountNumber": "string",
  "date": "2022-05-06T18:13:16.983Z",
  "status": "string",
  "currencyDescription": "string",
  "currency": "string",
  "bankDestination": "string",
  "cbuDestination": "string",
  "bankAccountNumberDestination": "string",
  "estimatedAmount": 0,
  "estimatedTariff": 0,
  "estimatedTotal": 0,
  "productType": "string",
  "operation": "string",
  "alerts": [
    "string"
  ]
}
```

### Confirm Transfer
Confirm a transfer and returns the transfer in detail.

``` title="Method: POST"
/api/{v}/Order/ConfirmTransfer
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
  "cuit": "string",
  "currency": "string",
  "cbu": "string",
  "bankAccountNumber": "string",
  "amount": 0
}
```

``` JSON title="Response Body - 200 Success"
{
  "accountNumber": "string",
  "date": "2022-05-10T17:08:15.487Z",
  "status": "string",
  "currencyDescription": "string",
  "currency": "string",
  "bankDestination": "string",
  "cbuDestination": "string",
  "bankAccountNumberDestination": "string",
  "estimatedAmount": 0,
  "estimatedTariff": 0,
  "estimatedTotal": 0,
  "productType": "string",
  "operation": "string",
  "alerts": [
    "string"
  ]
}
```