# Bluetooth Setup on headless RPI

Sources:
- [GUIDE: Volumio Bluetooth receiver](https://volumio.org/forum/volumio-bluetooth-receiver-t8937.html)
- [Headless A2DP Audio Streaming on Raspbian Stretch](https://gist.github.com/mill1000/74c7473ee3b4a5b13f6325e9994ff84c#)

Dependencies:
- [libfdk-aac.git](https://github.com/mstorsjo/fdk-aac)
- [bluez-alsa](https://github.com/Arkq/bluez-alsa)

Installation:
#### `libfdk-aac`
```
git clone https://github.com/mstorsjo/fdk-aac.git
cd fdk-aac
./configure
make -j4 
# sudo make install
```

#### `bluez-alsa`
```
git clone https://github.com/Arkq/bluez-alsa.git
cd bluez-alsa
git checkout v1.4.0
autoreconf --install
mkdir build && cd build

# if u intend to stream audio from a linux distribution 
# (e.g. using pulse audio) disable payloadcheck
../configure --enable-aac --disable-payloadcheck
make 
sudo make install
```

#### Install files

1. copy files
```
sudo cp -v bluetooth/* /etc/bluetooth/
sudo cp -v services/* /etc/systemd/system/
sudo cp -v bin/* /usr/local/bin
```

2. Edit `/lib/systemd/system/bluetooth.service` and append to `ExecStart`: `--noplugin=sap`

3. Enable systemd services
```
sudo systemctl daemon-reload
sudo systemctl restart bluetooth.service

sudo systemctl enable bluealsa.service
sudo systemctl enable bluealsa-a2dp-playback.service
sudo systemctl enable bluetooth-a2dp-agent.service

sudo systemctl start bluealsa.service
sudo systemctl start bluealsa-a2dp-playback.service
sudo systemctl start bluetooth-a2dp-agent.service
```

4. Start bluetooth: `sudo bluetoothctl`. Enter:
```
power on
discoverable on
```



