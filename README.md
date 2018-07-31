# Python utility for running a BLE UART using NRF51/52 dongle

## Introduction

## Dependencies
* [pc-ble-driver-py](https://github.com/NordicSemiconductor/pc-ble-driver-py)
* [Python](https://www.python.org/) (>= 2.7 && <= 3.0)

###pc_ble_driver_py modification
In order for this code to work for me I had to modify ble_adapter.py slightly

Running ble_adapter.py as distributed, I recieve the following AttributeError:

```Traceback (most recent call last):
  File "nrf_uart_GS.py", line 238, in <module>
    main(serial_port)
  File "nrf_uart_GS.py", line 181, in main
    conn_handle = collector.connect_and_discover()
  File "nrf_uart_GS.py", line 113, in connect_and_discover
    self.adapter.enable_notification(new_conn, NUS_TX_UUID)
  File "/media/data/anaconda3/envs/Py2/lib/python2.7/site-packages/pc_ble_driver_py/ble_driver.py", line 124, in wrapper
    err_code = wrapped(*args, **kwargs)
  File "/media/data/anaconda3/envs/Py2/lib/python2.7/site-packages/pc_ble_driver_py/ble_adapter.py", line 238, in enable_notification
    handle = self.db_conns[conn_handle].get_cccd_handle(uuid)
  File "/media/data/anaconda3/envs/Py2/lib/python2.7/site-packages/pc_ble_driver_py/ble_adapter.py", line 73, in get_cccd_handle
    if (c.uuid.value == uuid.value) and (c.uuid.baseitype == uuid.base.type):
AttributeError: 'BLEUUID' object has no attribute 'baseitype'
```
Modifying line 73 in ble_adapter.py from
```if (c.uuid.value == uuid.value) and (c.uuid.baseitype == uuid.base.type):```
to
```if (c.uuid.value == uuid.value):``
results in successful execution for me.

## Credits/Resources
This code was based off of an example (nus_collector.py) posted by Jørgen Holmefjord in [this](https://devzone.nordicsemi.com/f/nordic-q-a/29848/how-to-use-pca10040-as-dongle-scanner) post on Nordic Semiconductor DevZone.

pc-ble-driver-py driver.open() errors resolved with [this](https://devzone.nordicsemi.com/f/nordic-q-a/14849/pc-ble-driver-py-driver-open-fails) post.

##Testing
Tested with the following relevant versions:

* Python 2.7.15
* /pc-ble-driver-master/hex/sd_api_v2/connectivity_2.0.1_115k2_with_s130_2.0.1.hex progammed to central NRF51-DONGLE interacting with Python Code


