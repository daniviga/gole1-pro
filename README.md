# README

## Fix the rotation sensor orientation

Add `/etc/udev/hwdb.d/61-sensor-local.hwdb` from this repo, then

```bash
sudo systemd-hwdb update
sudo udevadm trigger -v -p DEVNAME=/dev/iio:device0
```

## Better Wi-Fi driver

Use the `rtw` driver: https://github.com/lwfinger/rtw88

Place `./etc/modprobe.d/rtw88-blacklist.conf` and `./etc/modprobe.d/rtw.conf` into `/etc/modprobe.d/`. 


## Disable disk space warning on /boot/efi

### Local user
```bash
gsettings set org.gnome.settings-daemon.plugins.housekeeping ignore-paths "['/boot/efi']"
```

### GDM user
```bash
sudo -u gdm dbus-launch gsettings set org.gnome.settings-daemon.plugins.housekeeping ignore-paths "['/boot/efi']"
```

## Enable fractional scaling in Gnome
```bash
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"
```

## Recommend Gnome extensions

- **Enhanced OSK** by cass00 ([Github](https://github.com/cass00/enhanced-osk-gnome-ext)) ([Gnome](https://extensions.gnome.org/extension/6595/enhanced-osk/))
- **Touch X** by neuromorph ([GitHub](https://github.com/neuromorph/touchx)) ([Gnome](https://extensions.gnome.org/extension/6156/touch-x/))

## Install 'phosh' on Fedora

```bash
sudo dnf install -y phosh
```

### Custom scaling factor in phosh

Add the folowing lines to `/etc/phosh/phoc.ini`:

```ini
[output:DSI-1]
scale = 1.50
```
