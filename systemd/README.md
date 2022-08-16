# README

## `mochad` daemon

After installation `mochad` will have been copied to `/usr/local/bin/mochad`.

## `systemd` integration

After installation `systemd/mochad.service` will have been copied to `/etc/systemd/system/mochad.service`. Do not start it manually, do not enable it.

After installation the `91-usb-x10-controllers.rules` will have been copied to `/etc/udev/rules.d/91-usb-x10-controllers.rules`. These rules will automatically start `mochad.service` when a CM1x is plugged into a USB port.

## `init.d` integration

After installation the `91-usb-x10-controllers.rules.initd` will have been copied to `/etc/udev/rules.d/91-usb-x10-controllers.rules`. These rules will automatically run `/usr/local/bin/mochad` when a CM1x is plugged into a USB port.

Note: not tested!
