# Ejemplo Runner Python

Antes de ejecutar el archivo de ejemploRunner.py se debe haber realizado previamente la instalación python (recomendamos tener la versión 3.10.2 de Python) y de la librería ppi-client.
Para ejecutar el archivo de ejemplo, debe abrirse una nueva consola y ejecutar el siguiente comando:

``` Python title="Comando CMD"
python ejemploRunner.py
```
Si querés descargar el ejemplo de runner completo ingresá a nuestro repositorio de [__GitHub__](https://github.com/itAtPPI).

``` Python title="ejemploRunner.py"
from ppi_client.api.constants import ACCOUNTDATA_TYPE_ACCOUNT_NOTIFICATION, ACCOUNTDATA_TYPE_PUSH_NOTIFICATION, \
    ACCOUNTDATA_TYPE_ORDER_NOTIFICATION
from ppi_client.models.account_movements import AccountMovements
from ppi_client.models.order import Order
from ppi_client.ppi import PPI
from ppi_client.models.disclaimer import Disclaimer
from ppi_client.models.instrument import Instrument
from datetime import datetime, timedelta
import asyncio
import json
import traceback
import os
​
# Change sandbox variable to False to connect to production environment
ppi = PPI(sandbox=False)
​
​
def main():
    try:
        # If you signed up after 16/6/22 and have "Key Publica" and "Key Privada"  credentials, please use this.
        # Change login credential to connect to the API
        ppi.account.login_api('<key publica>', '<key privada>')
        # This method will be deprecated
        # Change login credential to connect to the API
        # ppi.account.login('<user key>', '<user secret>')
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
        def onconnect_marketdata():
            try:
                print("\nConnected to realtime market data")
                ppi.realtime.subscribe_to_element(Instrument("GGAL", "ACCIONES", "A-48HS"))
                ppi.realtime.subscribe_to_element(Instrument("AAPL", "CEDEARS", "A-48HS"))
                ppi.realtime.subscribe_to_element(Instrument("AL30", "BONOS", "INMEDIATA"))
                ppi.realtime.subscribe_to_element(Instrument("AL30D", "BONOS", "INMEDIATA"))
                ppi.realtime.subscribe_to_element(Instrument("DLR/MAR22", "FUTUROS", "INMEDIATA"))
            except Exception as error:
                traceback.print_exc()
​
        def ondisconnect_marketdata():
            try:
                print("\nDisconnected from realtime market data")
            except Exception as error:
                traceback.print_exc()
​
        # Realtime MarketData
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
​
                    if len(msg['Offers']) > 0:
                        offer = msg['Offers'][0]['Price']
                    else:
                        offer = 0
​
                    print(
                        "%s [%s-%s] Offers: %.2f-%.2f Opening: %.2f MaxDay: %.2f MinDay: %.2f Accumulated Volume %.2f" %
                        (
                            msg['Date'], msg['Ticker'], msg['Settlement'], bid, offer,
                            msg['OpeningPrice'], msg['MaxDay'], msg['MinDay'], msg['VolumeTotalAmount']))
            except Exception as error:
                print(datetime.now())
                traceback.print_exc()
                
        ppi.realtime.connect_to_market_data(onconnect_marketdata, ondisconnect_marketdata, onmarketdata)
​
        # Starts connections to real time: for example to account or market data
        ppi.realtime.start_connections()
​
    except Exception as message:
        print(datetime.now())
        print(message)
​
​
if __name__ == '__main__':
    main()      
```


 