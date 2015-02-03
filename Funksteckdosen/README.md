# Funksteckdosen schalten

Anhand der Anleitung unter [http://www.raspberrypi-tutorials.de/software/funksteckdosen-mit-dem-raspberry-pi-schalten.html](http://www.raspberrypi-tutorials.de/software/funksteckdosen-mit-dem-raspberry-pi-schalten.html) kann man mit wenigen Mitteln Funksteckdosen mit dem Raspberry Pi schalten.

Damit kann man ohne Gefahr 220V Geräte schalten.

## Installation

## 433MHz Sender

Der 433MHz Sender wird an GPIO 17, GND und 5V angeschlossen.
Auf kurze Distanzen wird keine weitere Antenne benötigt.

## Software

```bash
sudo apt-get update
sudo apt-get install git-core
git clone git://git.drogon.net/wiringPi
cd wiringPi
./build
cd ..

git clone git://github.com/xkonni/raspberry-remote.git
cd raspberry-remote
make send
```

## Schalten

Funksteckdosen einschalten:

```bash
sudo ./send 10101 1 1
sudo ./send 10101 2 1
sudo ./send 10101 3 1
```

Funksteckdosen ausschalten:

```bash
sudo ./send 10101 1 0
sudo ./send 10101 2 0
sudo ./send 10101 3 0
```
