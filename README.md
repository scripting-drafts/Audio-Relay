# HFP-Relay
HFP Client to stream Android call audio

sudo apt-get install pi-bluetooth ofono ninja python3.9 python3.9-pip
sudo rm /usr/bin/python
sudo ln -s python3.9 /usr/bin/python
sudo pip3 install meson

In /etc/dbus-1/system.d/ofono.conf modify:
  <policy context="default">
    <deny send_destination="org.ofono"/>
  </policy>
To:
  <policy context="default">
    <allow send_destination="org.ofono"/>
  </policy>
  
git clone git://anongit.freedesktop.org/pulseaudio/pulseaudio

In src/modules/bluetooth/backend-native.c and src/modules/bluetooth/backend-ofono.c change (if possible):
*imtu = 48;
To:
*imtu = 60;

sudo apt-get build-dep pulseaudio
cd pulseaudio
sudo meson build
sudo ninja -C build
sudo ninja -C build install
sudo ldconfig

Enable developer sources in the /etc/apt/sources.list file by commenting out the lines starting with deb-src.

Optionally limit profiles to HFP onlyu123 /etc/pulse/default.pa by adding:
.ifexists module-bluetooth-discover.so
load-module module-bluetooth-discover headset=ofono
.endif
