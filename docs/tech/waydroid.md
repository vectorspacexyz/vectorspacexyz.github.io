---
title: Waydroid
comments: true
---

###  Installation
```sh
sudo pacman -S linux-xanmod-anbox linux-xanmod-anbox-headers
```

```sh
sudo rm -rf /var/lib/waydroid /home/.waydroid ~/waydroid ~/.share/waydroid ~/.local/share/applications/*aydroid* ~/.local/share/waydroid
sudo pacman -S waydroid
sudo waydroid init -s GAPPS

# waydroid-extras: https://aur.archlinux.org/waydroid-script-git.git
# For amd use libndk
sudo waydroid-extras install libndk widevine magisk

sudo systemctl enable waydroid-container
sudo systemctl start waydroid-container

# Using cage since I use xorg.
cage waydroid show-full-ui
```
### Google Play certification
[https://docs.waydro.id/faq/google-play-certification](https://docs.waydro.id/faq/google-play-certification)

## Shadowfight Arena 4
### Setup udev waydroid to detect Joystick

```sh
waydroid prop set persist.waydroid.udev true
waydroid prop set persist.waydroid.uevent true
```

### Emulate Joystick
```sh
git clone https://github.com/iosonofabio/virtual_gamepad

# For script to work load uinput
sudo modprobe uinput
# Load module on boot
sudo echo uinput >> /etc/modules-load.d/uinput.conf

# To use script without sudo:
sudo groupadd uinput
sudo usermod -aG uinput "$USER"
sudo chmod g+rw /dev/uinput
sudo chgrp uinput /dev/uinput
```

### Play

```sh
cage waydroid show-full-ui
python virtual_gamepad/virtual_gamepad.py
```

### virtual_gamepad.py with hjkl bindings

```python
from collections import defaultdict
import pynput
import uinput
import time

events = (
    uinput.BTN_A,
    uinput.BTN_B,
    uinput.BTN_X,
    uinput.BTN_Y,
    uinput.BTN_TL,
    uinput.BTN_TR,
    uinput.BTN_THUMBL,
    uinput.BTN_THUMBR,
    uinput.ABS_X + (0, 255, 0, 0),
    uinput.ABS_Y + (0, 255, 0, 0),
)
device = uinput.Device(
    events,
    vendor=0x045e,
    product=0x028e,
    version=0x110,
    name="Microsoft X-Box 360 pad",
)

# Center joystick
# syn=False to emit an "atomic" (128, 128) event.
device.emit(uinput.ABS_X, 128, syn=False)
device.emit(uinput.ABS_Y, 128)

keymap = {
    'right': 'l',
    'left': 'h',
    'up': 'k',
    'down': 'j',
    'kick': 'd',
    'fight': 'f',
    'shadow': 's',
    'range': 'a',
    'jumpleft': 'i',
    'jumpright': 'o',
    'rollleft': 'n',
    'rollleft2': 'b',
    'rollright': 'm',
    'range': 'a',
    'confirm': '0',
    '?': '-',
    'requests/cancel': '9',
    'zoomout': '8',
    'back': 'p',
}
keys = list(keymap.values())

def find_key(key):
    #if key == keyboard.Key.esc:
    #    return False  # stop listener
    try:
        k = key.char  # single-char keys
    except:
        k = key.name  # other keys
    if k not in keys:
        return True

    return k

    #print('Key pressed: ' + k)
    #return False  # stop listener; remove this if want more keys 


def on_press(key):
    k = find_key(key)
    if k is True:
        return True

    if k == keymap['kick']:
        device.emit(uinput.BTN_A, 1)
    elif k == keymap['fight']:
        device.emit(uinput.BTN_B, 1)
    elif k == keymap['shadow']:
        device.emit(uinput.BTN_X, 1)
    elif k == keymap['range']:
        device.emit(uinput.BTN_Y, 1)
    elif k == keymap['confirm']:
        device.emit(uinput.BTN_TL, 1)
    elif k == keymap['?']:
        device.emit(uinput.BTN_TR, 1)
    elif k == keymap['requests/cancel']:
        device.emit(uinput.BTN_THUMBL, 1)
    elif k == keymap['zoomout']:
        device.emit(uinput.BTN_THUMBR, 1)
    elif k == keymap['up']:
        device.emit(uinput.ABS_Y, 0)                    # Zero Y
    elif k == keymap['jumpleft']:
        device.emit(uinput.ABS_Y, 0)                    # Zero Y
        device.emit(uinput.ABS_X, 0)                    # Zero X
    elif k == keymap['jumpright']:
        device.emit(uinput.ABS_Y, 0)                    # Zero Y
        device.emit(uinput.ABS_X, 255)                    # Zero X
    elif k == keymap['down']:
        device.emit(uinput.ABS_Y, 255)                  # Max Y
    elif k == keymap['rollleft']:
        device.emit(uinput.ABS_Y, 255)                  # Max Y
        device.emit(uinput.ABS_X, 0)                    # Zero X
    elif k == keymap['rollleft2']:
        device.emit(uinput.ABS_Y, 255)                  # Max Y
        device.emit(uinput.ABS_X, 0)                    # Zero X
    elif k == keymap['rollright']:
        device.emit(uinput.ABS_Y, 255)                  # Max Y
        device.emit(uinput.ABS_X, 255)                  # Max X
    elif k == keymap['left']:
        device.emit(uinput.ABS_X, 0)                    # Zero X
    elif k == keymap['right']:
        device.emit(uinput.ABS_X, 255)                  # Max X

    return True


def on_release(key):
    k = find_key(key)
    if k is True:
        return True

    if k == keymap['kick']:
        device.emit(uinput.BTN_A, 0)
    elif k == keymap['fight']:
        device.emit(uinput.BTN_B, 0)
    elif k == keymap['shadow']:
        device.emit(uinput.BTN_X, 0)
    elif k == keymap['range']:
        device.emit(uinput.BTN_Y, 0)
    elif k == keymap['confirm']:
        device.emit(uinput.BTN_TL, 0)
    elif k == keymap['?']:
        device.emit(uinput.BTN_TR, 0)
    elif k == keymap['requests/cancel']:
        device.emit(uinput.BTN_THUMBL, 0)
    elif k == keymap['zoomout']:
        device.emit(uinput.BTN_THUMBR, 0)
    elif k == keymap['up']:
        device.emit(uinput.ABS_Y, 128)                # Center Y
    elif k == keymap['jumpleft']:
        device.emit(uinput.ABS_Y, 128)                    # Zero Y
        device.emit(uinput.ABS_X, 128)                    # Zero X
    elif k == keymap['jumpright']:
        device.emit(uinput.ABS_Y, 128)                    # Zero Y
        device.emit(uinput.ABS_X, 128)                    # Zero X
    elif k == keymap['rollright']:
        device.emit(uinput.ABS_Y, 128)                    # Zero Y
        device.emit(uinput.ABS_X, 128)                    # Zero X
    elif k == keymap['rollleft']:
        device.emit(uinput.ABS_Y, 128)                    # Zero Y
        device.emit(uinput.ABS_X, 128)                    # Zero X
    elif k == keymap['rollleft2']:
        device.emit(uinput.ABS_Y, 128)                    # Zero Y
        device.emit(uinput.ABS_X, 128)                    # Zero X
    elif k == keymap['down']:
        device.emit(uinput.ABS_Y, 128)                # Center Y
    elif k == keymap['left']:
        device.emit(uinput.ABS_X, 128)                # Center Y
    elif k == keymap['right']:
        device.emit(uinput.ABS_X, 128)                # Center Y

    #time.sleep(.02)    # Poll every 20ms (otherwise CPU load gets too high)

    return True


if True:
    listener = pynput.keyboard.Listener(
        on_press=on_press,
        on_release=on_release,
        )
    listener.start()  # start to listen on a separate thread
    listener.join()  # remove if main thread is polling self.keys

```
