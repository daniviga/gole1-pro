# README

# phosh

```bash
sudo dnf install -y phosh
```

# Rotation sensor

Add `/etc/udev/hwdb.d/61-sensor-local.hwdb` then

```bash
sudo systemd-hwdb update
sudo udevadm trigger -v -p DEVNAME=/dev/iio:device0
```
