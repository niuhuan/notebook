```python
import network
import json

nic = None


def set_wlan():
    with open('/sd/wlan.config.json', 'r') as config_file:
        config = json.load(config_file)
    global nic
    nic = network.WLAN(network.STA_IF)
    nic.active(True)
    nic.connect(config['ssid'], config['password'])
