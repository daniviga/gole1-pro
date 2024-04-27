# README

## Fix the rotation sensor orientation

Add `/etc/udev/hwdb.d/61-sensor-local.hwdb` from this repo, then

```bash
sudo systemd-hwdb update
sudo udevadm trigger -v -p DEVNAME=/dev/iio:device0
```

## Fix internal speaker sound

With the default configuration no sound is emitted by the internal speakers (headphones are working just fine).
To make the internal speakers working, copy `./lib/firmware/hda-jack-retask.fw` to `/lib/firmware/hda-jack-retask.fw`, `./etc/modprobe.d/hda-jack-retask.conf` to `/etc/modprobe.d/hda-jack-retask.conf` and reboot.

These files are generated using `hdajackretask` from `alsa-tools`.

![Screenshot from 2024-01-06 00-46-00](https://github.com/daniviga/gole1-pro/assets/1818657/f03ac212-b787-40ff-80cf-aa5191e418c9)

Note: when [upgrading to Fedora 40](https://discussion.fedoraproject.org/t/f40-regression-internal-audio-output-device-not-found/109495) you need to cleanup the local `wireplumber` dir: 

```bash
rm -rf ~/.local/state/wireplumber 
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
