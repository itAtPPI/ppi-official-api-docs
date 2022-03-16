# Documentación Librería Python

``` Python title="Necessary Imports"
# Imports 
from ppi_client.models.account_movements import AccountMovements
from ppi_client.ppi import PPI
from ppi_client.models.orders_filter import OrdersFilter
from ppi_client.models.order_budget import OrderBudget
from ppi_client.models.order_confirm import OrderConfirm
from ppi_client.models.disclaimer import Disclaimer
from ppi_client.models.search_instrument import SearchInstrument
from ppi_client.models.search_marketdata import SearchMarketData
from ppi_client.models.search_datemarketdata import SearchDateMarketData
from ppi_client.models.order import Order
from ppi_client.models.instrument import Instrument
from datetime import datetime, timedelta
import asyncio
import json
import traceback
```

``` Python title="Sandbox Environment"

# Change sandbox variable to False to connect to production environment
ppi = PPI(sandbox=False)
```

``` Python title="Login"
# Change login credential to connect to the API
        ppi.account.login('<user key>', '<user secret>')
```

``` Python title="Account Information"
# Getting accounts information
        print("Getting accounts information")
        account_numbers = ppi.account.get_accounts()
        for account in account_numbers:
            print(account)
        account_number = account_numbers[0]['accountNumber']
```

``` JSON title="Response Account Information" 
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

``` Python title="Bank Account Information"
# Getting bank account information
        print("\nGetting bank account information of %s" % account_number)
        bank_accounts = ppi.account.get_bank_accounts(account_number)
        for bank_account in bank_accounts:
            print(bank_account)
```

``` JSON title="Response Bank Account Information" 
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

``` Python title="Available Balance"
# Getting available balance
        print("\nGetting available balance of %s" % account_number)
        balances = ppi.account.get_available_balance(account_number)
        for balance in balances:
            print("Currency %s - Settlement %s - Amount %s %s" % (
                balance['name'], balance['settlement'], balance['symbol'], balance['amount']))
```

``` JSON title="Response Available Balance" 
[
  {
    "name": "string",
    "simbol": "string",
    "amount": 0,
    "settlement": "string"
  }
]
```

``` Python title="Balances and Positions"
# Getting balance and positions
        print("\nGetting balance and positions of %s" % account_number)
        balances_positions = ppi.account.get_balance_and_positions(account_number)
        for balance in balances_positions["groupedAvailability"]:
            for currency in balance['availability']:
                print("Currency %s Settlement %s Amount %s %s" % (
                    currency['name'], currency['settlement'], currency['symbol'], currency['amount']))
        for instruments in balances_positions["groupedInstruments"]:
            print("Instrument %s " % instruments['name'])
            for instrument in instruments['instruments']:
                print("Ticker %s Price %s Amount %s" % (
                    instrument['ticker'], instrument['price'], instrument['amount']))
```

``` JSON title="Response Balances and Positions" 
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

``` Python title="Movements"
# Getting movements
        print("\nGetting movements of %s" % account_number)
        movements = ppi.account.get_movements(AccountMovements(account_number, "2021-12-01", "2021-12-31", None))
        for mov in movements:
            print("%s %s - Currency %s Amount %s " % (
                mov['settlementDate'], mov['description'], mov['currency'], mov['amount']))
```

``` JSON title="Response Movements" 
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

``` Python title="Instrument Types"
# Getting instrument types
        print("\nGetting instrument types")
        instruments = ppi.configuration.get_instrument_types()
        for item in instruments:
            print(item)
```

``` JSON title="Response Instrument Types" 
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

``` Python title="Markets"
# Getting markets
        print("\nGetting markets")
        markets = ppi.configuration.get_markets()
        for item in markets:
            print(item)
```

``` JSON title="Response Markets" 
[
    "ROFEX",
    "OTC",
    "NYSE",
    "BYMA"
]
```

``` Python title="Settlements"
# Getting settlements
        print("\nGetting settlements")
        settlements = ppi.configuration.get_settlements()
        for item in settlements:
            print(item)
```

``` JSON title="Response Settlements" 
[
    "INMEDIATA",
    "A-24HS",
    "A-48HS",
    "A-72HS"
]
```

``` Python title="Quantity Types"
# Getting quantity types
        print("\nGetting quantity types")
        quantity_types = ppi.configuration.get_quantity_types()
        for item in quantity_types:
            print(item)
```

``` JSON title="Response Quantity Types" 
[
    "DINERO",
    "PAPELES",
    "CANTIDAD-TOTAL"
]
```

``` Python title="Operation Terms"
# Getting operation terms
        print("\nGetting operation terms")
        operation_terms = ppi.configuration.get_operation_terms()
        for item in operation_terms:
            print(item)
```

``` JSON title="Response Operation Terms" 
[
    "POR-EL-DÍA",
    "HASTA-SU-EJECUCIÓN",
    "VÁLIDA-HASTA-EL",
    "72-HS"
]
```

``` Python title="Operation Types"
# Getting operation types
        print("\nGetting operation types")
        operation_types = ppi.configuration.get_operation_types()
        for item in operation_types:
            print(item)
```

``` JSON title="Response Operation Types" 
[
    "PRECIO-DE-MERCADO",
    "PRECIO-LIMITE"
]
```

``` Python title="Operations"
# Getting operations
        print("\nGetting operations")
        operations = ppi.configuration.get_operations()
        for item in operations:
            print(item)
```

``` JSON title="Response Operations" 
[
    "COMPRA",
    "VENTA",
    "SUSCRIPCIÓN-FCI",
    "RESCATE-FCI",
    "COLOCAR-CAUCIÓN"
]
```

``` Python title="Search Instrument"
# Search Instrument
        print("\nSearching instruments")
        instruments = ppi.marketdata.search_instrument(SearchInstrument("GGAL", "", "Byma", "Acciones"))
        for ins in instruments:
            print(ins)
```

``` JSON title="Response Instrument" 
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

``` Python title="Search Historical Market Data"
# Search Historic MarketData
        print("\nSearching MarketData")
        market_data = ppi.marketdata.search(SearchDateMarketData("GGAL", "Acciones", "A-48HS",
                                                                 "2021-01-01", "2021-12-31"))
        for ins in market_data:
            print("%s - %s - Volume %s - Opening %s - Min %s - Max %s" % (
                ins['date'], ins['price'], ins['volume'], ins['openingPrice'], ins['min'], ins['max']))
```

``` JSON title="Response Historical Market Data" 
[
    {
        "date": "string",
        "price": 0,
        "volume": 0,
        "openingPrice": 0,
        "max": 0,
        "min": 0
    }, ...
]
```

``` Python title="Search Current Market Data"
# Search Current MarketData
        print("\nSearching Current MarketData")
        current_market_data = ppi.marketdata.current(SearchMarketData("GGAL", "Acciones", "A-48HS"))
        print(current_market_data)
```

``` JSON title="Response Current Market Data" 
{
    "date": "string",
    "price": 0,
    "volume": 0,
    "openingPrice": 0,
    "max": 0,
    "min": 0
}
```

``` Python title="Search Current Book"
# Search Current Book
        print("\nSearching Current Book")
        current_book = ppi.marketdata.book(SearchMarketData("GGAL", "Acciones", "A-48HS"))
        print(current_book)
```

``` JSON title="Response Current Book" 
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

``` Python title="Search Intraday Market Data"
# Search Intraday MarketData
        print("\nSearching Intraday MarketData")
        intraday_market_data = ppi.marketdata.intraday(SearchMarketData("GGAL", "Acciones", "A-48HS"))
        for intra in intraday_market_data:
            print(intra)
```

``` JSON title="Response Intraday Market Data" 
[
    {
        "date": "string",
        "price": 0,
        "volume": 0
    }, ...
]
```

``` Python title="Orders"
# Get orders
        print("\nGet orders")
        orders = ppi.orders.get_orders(
            OrdersFilter(from_date=datetime.today() + timedelta(days=-10), to_date=datetime.today(),
                         account_number=account_number))
        for order in orders:
            print(order)
```

``` JSON title="Response Orders" 
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

``` Python title="Budget of an Order"
''' Uncomment to get the budget of an order
        # Get budget
        print("\nGet budget")
        budget = ppi.orders.budget(OrderBudget(account_number, 10000, 150, "GGAL", "ACCIONES", "Dinero", "PRECIO-LIMITE"
                                               , "HASTA-SU-EJECUCIÓN", None, "Compra", "INMEDIATA"))
        print(budget)
        disclaimers = budget['disclaimers']
        '''
```

``` JSON title="Response Budget of an Order" 
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

``` Python title="Create an Order"
''' Uncomment to create an order
        # Confirm budget
        print("\nConfirm budget")
        acceptedDisclaimers = []
        for disclaimer in disclaimers:
            acceptedDisclaimers.append(Disclaimer(disclaimer['code'], True))
        confirmation = ppi.orders.confirm(OrderConfirm(account_number, 10000, 150, "GGAL", "ACCIONES", "Dinero",
                                                       "PRECIO-LIMITE", "HASTA-SU-EJECUCIÓN", None, "Compra"
                                                       , "INMEDIATA", acceptedDisclaimers, None))
        print(confirmation)
        order_id = confirmation["id"]
        '''
```

``` JSON title="Response Create an Order" 
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

``` Python title="Detail of an Order"
''' Uncomment to get the detail of an order
        # Get order detail
        print("\nGet order detail")
        detail = ppi.orders.get_order_detail(Order(order_id, account_number, None))
        print(detail)
        '''
```

``` JSON title="Response Detail of an Order" 
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

``` Python title="Execute Cancellation of an Order"
''' Uncomment to execute cancellation of an order
        # Cancel order
        print("\nCancel order")
        cancel = ppi.orders.cancel_order(Order(order_id, account_number, None))
        print(cancel)
        '''
```

``` JSON title="Response Cancellation of an Order" 
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

``` Python title="Execute Mass Cancellation of Orders"
''' Uncomment to execute mass cancellation of orders
        # Cancel all active orders
        print("\nMass Cancel")
        cancels = ppi.orders.mass_cancel_order(account_number)
        print(cancels)
        '''
```

``` JSON title="Response Mass Cancellation of Orders" 
string
```

``` Python title="Realtime Subscription to Market Data / Realtime Broadcast Market Data"
''' Uncomment to use realtime market data
        # Realtime subscription to market data
        def onconnect():
            try:
                print("\nConnected to realtime")
                ppi.realtime.subscribe_to_element(Instrument("GGAL", "ACCIONES", "A-48HS"))
                ppi.realtime.subscribe_to_element(Instrument("AAPL", "CEDEARS", "A-48HS"))
                ppi.realtime.subscribe_to_element(Instrument("AL30", "BONOS", "INMEDIATA"))
                ppi.realtime.subscribe_to_element(Instrument("AL30D", "BONOS", "INMEDIATA"))
                ppi.realtime.subscribe_to_element(Instrument("DLR/MAR22", "FUTUROS", "INMEDIATA"))
            except Exception as error:
                traceback.print_exc()

        def ondisconnect():
            try:
                print("\nDisconnected from realtime")
            except Exception as error:
                traceback.print_exc()

        # Realtime broadcast market data
        def onmarketdata(data):
            try:
                msg = json.loads(data)
                if msg["Trade"]:
                    print("%s [%s-%s] Price %.2f Volume %.2f" % (
                        msg['Date'], msg['Ticker'], msg['Settlement'], msg['Price'], msg['VolumeAmount']))
                else:
                    if len(msg['Bids']) > 0:
                        bid = msg['Bids'][0]['Price']
                    else:
                        bid = 0

                    if len(msg['Offers']) > 0:
                        offer = msg['Offers'][0]['Price']
                    else:
                        offer = 0

                    print(
                        "%s [%s-%s] Offers: %.2f-%.2f Opening: %.2f MaxDay: %.2f MinDay: %.2f Accumulated Volume %.2f" %
                        (
                            msg['Date'], msg['Ticker'], msg['Settlement'], bid, offer,
                            msg['OpeningPrice'], msg['MaxDay'], msg['MinDay'], msg['VolumeTotalAmount']))
            except Exception as error:
                traceback.print_exc()

        ppi.realtime.connect_to_market_data(onconnect, ondisconnect, onmarketdata)
        '''        
```

``` JSON title="Response Books" 
{
        "Ticker": "string", 
        "Price": 0.0, 
        "VolumeAmount": 0.0, 
        "VolumeCurrency": 0.0, 
        "Date": "2022-03-14T16:52:51.87-03:00", 
        "Type": "string", 
        "Settlement": "string", 
        "VarDay": 0.0, 
        "Offers": [
                        {
                                "Price": 0.0, 
                                "Quantity": 0.0, 
                                "Position": 0
                        },
                        {
                                "Price": 0.0, 
                                "Quantity": 0.0, 
                                "Position": 0
                        }, ...
                ], 
        "Bids": [
                        {
                                "Price": 0.0, 
                                "Quantity": 0.0, 
                                "Position": 0
                        },
                        {
                                "Price": 0.0, 
                                "Quantity": 0.0, 
                                "Position": 0
                        }, ...
                ], 
        "Trade": False, 
        "OpeningPrice": 0.0, 
        "MaxDay": 0.0, 
        "MinDay": 0.0, 
        "VolumeTotalAmount": 0
}

```

``` JSON title="Response Market Data" 
{
        "Ticker": "string", 
        "Price": 0.0, 
        "VolumeAmount": 0.0, 
        "VolumeCurrency": 0.0, 
        "Date": "2022-03-14T16:52:51.87-03:00", 
        "Type": "string", 
        "Settlement": "string", 
        "VarDay": 0.0, 
        "Offers": [], 
        "Bids": [], 
        "Trade": True, 
        "OpeningPrice": 0.0, 
        "MaxDay": 0.0, 
        "MinDay": 0.0, 
        "VolumeTotalAmount": 0
}

```
