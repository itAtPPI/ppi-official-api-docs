# Ejemplo Runner Python

Antes de ejecutar el archivo de ejemploRunner.py se debe haber realizado previamente la instalación python (recomendamos tener la versión 3.10.2 de Python) y de la libreria ppi-client.
Para ejecutar el archivo de ejemplo, debe abrirse una nueva consola y ejecutar el siguiente comando:

``` Python title="Comando CMD"
python ejemploRunner.py
```
Si querés descargar el ejemplo de runner completo ingresa a nuestro repositorio de [__GitHub__](https://github.com/itAtPPI).

``` Python title="ejemploRunner.py"
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
​
# Change sandbox variable to True to connect to sandbox environment
ppi = PPI(sandbox=False)
​
def main():
    try:
        # Change login credential to connect to the API
        ppi.account.login('<user key>', '<user secret>')
​
        # Getting accounts information
        print("Getting accounts information")
        account_numbers = ppi.account.get_accounts()
        for account in account_numbers:
            print(account)
        account_number = account_numbers[0]['accountNumber']
​
        # Getting available balance
        print("\nGetting available balance of %s" % account_number)
        balances = ppi.account.get_available_balance(account_number)
        for balance in balances:
            print("Currency %s - Settlement %s - Amount %s %s" % (
                balance['name'], balance['settlement'], balance['symbol'], balance['amount']))
​
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
​
        def ondisconnect():
            try:
                print("\nDisconnected from realtime")
            except Exception as error:
                traceback.print_exc()
​
        ppi.realtime.connect_to_market_data(onconnect, ondisconnect)
        
​
    except Exception as message:
        print(message)
​
if __name__ == '__main__':
    main()
        
```


 