# README

# phosh

sudo dnf install -y phosh

# Rotation sensor

sudo systemd-hwdb update
sudo udevadm trigger -v -p DEVNAME=/dev/iio:device0
