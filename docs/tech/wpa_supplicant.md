---
title: wpa_supplicant
---
```
# wpa_supplicant -B -i wlp3s0 -c /etc/wpa_supplicant/wpa_supplicant.conf
```

/etc/wpa_supplicant/wpa_supplicant.conf:

```
ctrl_interface=/run/wpa_supplicant
update_config=1
```

/etc/systemd/system/wpa_supplicant.service

```
[Unit]
Description=WPA supplicant daemon (interface-specific version)
Requires=sys-subsystem-net-devices-wlp3s0.device
After=sys-subsystem-net-devices-wlp3s0.device
Before=network.target
Wants=network.target

# NetworkManager users will probably want the dbus version instead.

[Service]
Type=simple
ExecStart=/usr/bin/wpa_supplicant -c /etc/wpa_supplicant/wpa_supplicant.conf -i wlp3s0

[Install]
WantedBy=multi-user.target
```
