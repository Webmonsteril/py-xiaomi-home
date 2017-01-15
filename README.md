xiaomi-home
==================

Pythonic bindings for Xiaomi Smart Home suite


Basic Usage
==================


Discover gateways and subdevices by sending `read` commands.

```python
from mihome.base import XiaomiConnection
from mihome.gateway import Gateway

conn = XiaomiConnection()
gateway_data = conn.whois()

gateway = Gateway(
	connection=conn,
	sid=gateway_data['sid'],
	ip=gateway_data['ip'],
	port=gateway_data['port']
)
gateway.register_subdevices()
```

Listen to any devices events

```python
def switch_cb(item):
	print item

for switch in gateway.connected_devices['switch']:
	switch.listen(callback=switch_cb)
```

Save all devices to config

```python
from mihome.config_manager import YamlConfig

config = YamlConfig([gateway])
config.save()
```

Load config to get all devices
This saves you time from doing discovery everytime you
start up the code

```python
from mihome.config_manager import YamlConfig

config = YamlConfig()
gateways = config.load(conn, Gateway)
for gateway in gateways:
	gateway.register_subdevices()
	print gateway.connected_devices
```