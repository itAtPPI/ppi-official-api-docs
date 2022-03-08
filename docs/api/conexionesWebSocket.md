# Conexiones Librería Python

``` Python title="Necessary imports"
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

``` Python title="Sandbox environment"

# Change sandbox variable to False to connect to production environment
ppi = PPI(sandbox=False)
```

``` Python title="Login"
# Change login credential to connect to the API
        ppi.account.login('<user key>', '<user secret>')
```

``` Python title="Bank Account Information"
# Getting bank account information
        print("\nGetting bank account information of %s" % account_number)
        bank_accounts = ppi.account.get_bank_accounts(account_number)
        for bank_account in bank_accounts:
            print(bank_account)
```

``` Python title="Available Balance"
# Getting available balance
        print("\nGetting available balance of %s" % account_number)
        balances = ppi.account.get_available_balance(account_number)
        for balance in balances:
            print("Currency %s - Settlement %s - Amount %s %s" % (
                balance['name'], balance['settlement'], balance['symbol'], balance['amount']))
```

``` Python title="Balances and positions"
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

``` Python title="Movements"
# Getting movements
        print("\nGetting movements of %s" % account_number)
        movements = ppi.account.get_movements(AccountMovements(account_number, "2021-12-01", "2021-12-31", None))
        for mov in movements:
            print("%s %s - Currency %s Amount %s " % (
                mov['settlementDate'], mov['description'], mov['currency'], mov['amount']))
```

``` Python title="Instrument Types"
# Getting instrument types
        print("\nGetting instrument types")
        instruments = ppi.configuration.get_instrument_types()
        for item in instruments:
            print(item)
```

``` Python title="Markets"
# Getting markets
        print("\nGetting markets")
        markets = ppi.configuration.get_markets()
        for item in markets:
            print(item)
```

``` Python title="Settlements"
# Getting settlements
        print("\nGetting settlements")
        settlements = ppi.configuration.get_settlements()
        for item in settlements:
            print(item)
```

``` Python title="Quantity Types"
# Getting quantity types
        print("\nGetting quantity types")
        quantity_types = ppi.configuration.get_quantity_types()
        for item in quantity_types:
            print(item)
```

``` Python title="Operation Terms"
# Getting operation terms
        print("\nGetting operation terms")
        operation_terms = ppi.configuration.get_operation_terms()
        for item in operation_terms:
            print(item)
```

``` Python title="Operation Types"
# Getting operation types
        print("\nGetting operation types")
        operation_types = ppi.configuration.get_operation_types()
        for item in operation_types:
            print(item)
```

``` Python title="Operations"
# Getting operations
        print("\nGetting operations")
        operations = ppi.configuration.get_operations()
        for item in operations:
            print(item)
```

``` Python title="Search Instrument"
# Search Instrument
        print("\nSearching instruments")
        instruments = ppi.marketdata.search_instrument(SearchInstrument("GGAL", "", "Byma", "Acciones"))
        for ins in instruments:
            print(ins)
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

``` Python title="Search Current Market Data"
# Search Current MarketData
        print("\nSearching Current MarketData")
        current_market_data = ppi.marketdata.current(SearchMarketData("GGAL", "Acciones", "A-48HS"))
        print(current_market_data)
```

``` Python title="Search Intraday Market Data"
# Search Intraday MarketData
        print("\nSearching Intraday MarketData")
        intraday_market_data = ppi.marketdata.intraday(SearchMarketData("GGAL", "Acciones", "A-48HS"))
        for intra in intraday_market_data:
            print(intra)
```

``` Python title="Orders"
# Get orders
        print("\nGet orders")
        orders = ppi.orders.get_orders(
            OrdersFilter(from_date=datetime.today() + timedelta(days=-100), to_date=datetime.today(),
                         account_number=account_number))
        for order in orders:
            print(order)
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

``` Python title="Detail of an Order"
''' Uncomment to get the detail of an order
        # Get order detail
        print("\nGet order detail")
        detail = ppi.orders.get_order_detail(Order(order_id, account_number, None))
        print(detail)
        '''
```

``` Python title="Execute Cancellation of an Order"
''' Uncomment to execute cancellation of an order
        # Cancel order
        print("\nCancel order")
        cancel = ppi.orders.cancel_order(Order(order_id, account_number, None))
        print(cancel)
        '''
```

``` Python title="Execute Mass Cancellation of Orders"
''' Uncomment to execute mass cancellation of orders
        # Cancel all active orders
        print("\nMass Cancel")
        cancels = ppi.orders.mass_cancel_order(account_number)
        print(cancels)
        '''
```
