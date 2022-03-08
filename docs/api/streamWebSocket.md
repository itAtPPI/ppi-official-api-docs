# Stream Librería Python

``` Python title="Realtime Subscription to Market Data"
# Realtime subscription to market data
        def onconnect():
            try:
                print("Connected to realtime")
                ppi.realtime.subscribe_to_element(Instrument("GGAL", "ACCIONES", "A-48HS"))
                ppi.realtime.subscribe_to_element(Instrument("AAPL", "CEDEARS", "A-48HS"))
                ppi.realtime.subscribe_to_element(Instrument("AL30", "BONOS", "INMEDIATA"))
                ppi.realtime.subscribe_to_element(Instrument("AL30D", "BONOS", "INMEDIATA"))
                ppi.realtime.subscribe_to_element(Instrument("DLR/MAR22", "FUTUROS", "INMEDIATA"))
            except Exception as error:
                traceback.print_exc()

        def ondisconnect():
            try:
                print("Disconnected from realtime")
            except Exception as error:
                traceback.print_exc()
```

``` Python title="Realtime Broadcast Market Data"
# Realtime broadcast market data
        def onmarketdata(data):
            try:
                msg = json.loads(data)
                if msg["Trade"]:
                    print("%s [%s-%s] Price %.2f Volume %.2f" % (
                        msg['Date'], msg['Ticker'], msg['Settlement'], msg['Price'], msg['VolumeAmount']))
                else:
                    print(
                        "%s [%s-%s] Offers: %.2f-%.2f Opening: %.2f MaxDay: %.2f MinDay: %.2f Accumulated Volume %.2f" %
                        (
                            msg['Date'], msg['Ticker'], msg['Settlement'],
                            msg['Bids'][0]['Price'], msg['Offers'][0]['Price'],
                            msg['OpeningPrice'], msg['MaxDay'], msg['MinDay'], msg['VolumeTotalAmount']))
            except Exception as error:
                traceback.print_exc()

        loop = asyncio.get_event_loop()
        loop.run_until_complete(ppi.realtime.connect_websocket(onconnect, ondisconnect, onmarketdata))
        loop.run_forever()
    except Exception as message:
        print(message)
```
 