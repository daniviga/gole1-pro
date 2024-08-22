# README

A collection of goodies and hints to get an almost full Linux support on the GOLE1 PRO Mini Touch PC.

![gole1-min](https://github.com/user-attachments/assets/4efc2ae4-36f1-4ca4-8122-9398293c46f0)


I am currently running the following setup:

```
Operating System: Fedora Linux 40
KDE Plasma Version: 6.1.4
KDE Frameworks Version: 6.5.0
Qt Version: 6.7.2
Kernel Version: 6.10.5-200.fc40.x86_64 (64-bit)
Graphics Platform: Wayland
Processors: 4 × Intel® Celeron® J4125 CPU @ 2.00GHz
Memory: 7.6 GiB of RAM
Graphics Processor: Mesa Intel® UHD Graphics 600
```

However, stuff mentioned here is applicable to any recent Linux distribution.


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

An handy script to compile the module when needed is:

```bash
#!/bin/bash

make clean
make
sudo make install
sudo modprobe rtw_8821ce
```

## Enable support of Wayland OSK in Google Chrome and Chromium

To be able to use a Wayland based OSK (provided by either Gnome or KDE) in Google Chrome or Chromium, you have to pass the following parameters to the executable:

```bash
--ozone-platform=wayland --enable-wayland-ime --wayland-text-input-version=3
```

To make the change permanent:

```bash
cp /usr/share/applications/google-chrome.desktop ~/.local/share/applications
sed -i 's/google-chrome-stable/google-chrome-stable --ozone-platform=wayland --enable-wayland-ime --wayland-text-input-version=3/g' ~/.local/share/applications/google-chrome.desktop
```

Tested with `Google Chrome Version 127.0.6533.119 (Official Build) (64-bit)`

## KDE

As writing (Fedora 40), KDE with Plasma 6 seems to be the best option to run a DE on the Gole 1 Mini (or in general on any touch based device), since no third-party (and not always well maintained) extensions are required.

It also provides, out of the box, support for any fractional display scaling factor.

## Gnome
### Disable disk space warning on /boot/efi in Gnome

#### Local user
```bash
gsettings set org.gnome.settings-daemon.plugins.housekeeping ignore-paths "['/boot/efi']"
```

#### GDM user
```bash
sudo -u gdm dbus-launch gsettings set org.gnome.settings-daemon.plugins.housekeeping ignore-paths "['/boot/efi']"
```

### Enable fractional scaling in Gnome
```bash
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"
```

### Recommend Gnome extensions

- **Enhanced OSK** by cass00 ([Github](https://github.com/cass00/enhanced-osk-gnome-ext)) ([Gnome](https://extensions.gnome.org/extension/6595/enhanced-osk/)). **Not compatible with Gnome 46**
- **GJS OSK** by Vishram1123 ([Github](https://github.com/Vishram1123/gjs-osk)) ([Gnome](https://extensions.gnome.org/extension/5949/gjs-osk/))
- **Touch X** by neuromorph ([GitHub](https://github.com/neuromorph/touchx)) ([Gnome](https://extensions.gnome.org/extension/6156/touch-x/))

## Phosh
### Install 'phosh' on Fedora

```bash
sudo dnf install -y phosh
```

### Custom scaling factor in phosh

Add the folowing lines to `/etc/phosh/phoc.ini`:

```ini
[output:DSI-1]
scale = 1.50
```
