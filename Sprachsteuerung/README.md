# Sprachsteuerung mit Jasper

Es wird Jasper anhand folgender Anleitung [http://jasperproject.github.io/documentation/installation/](http://jasperproject.github.io/documentation/installation/) eingerichtet.

Benötigt wird ein USB Mikrofon, etwas von einer WebCam.

## Installation

```bash
sudo apt-get install vim git-core python-dev python-pip bison libasound2-dev libportaudio-dev python-pyaudio --yes
sudo sed -i.bak 's/options snd-usb-audio index=-2/options snd-usb-audio index=0/' /etc/modprobe.d/alsa-base.conf
sudo alsa force-reload
```

## Testaufnahme

```bash
arecord temp.wav
aplay -D hw:1,0 temp.wav
```

## Installation Jasper

```bash
echo 'export LD_LIBRARY_PATH="/usr/local/lib"' >>~/.bash_profile
git clone https://github.com/jasperproject/jasper-client.git jasper
sudo pip install --upgrade setuptools
sudo pip install -r jasper/client/requirements.txt
chmod +x jasper/jasper.py
```

## Installation PocketSphinx

```bash
wget http://downloads.sourceforge.net/project/cmusphinx/sphinxbase/0.8/sphinxbase-0.8.tar.gz
tar -zxvf sphinxbase-0.8.tar.gz
cd sphinxbase-0.8/
./configure --enable-fixed
make
sudo make install
wget http://downloads.sourceforge.net/project/cmusphinx/pocketsphinx/0.8/pocketsphinx-0.8.tar.gz
tar -zxvf pocketsphinx-0.8.tar.gz
cd ../pocketsphinx-0.8/
./configure
make
sudo make install
cd ..
```

## Installation CMUCLMTK

```bash
sudo apt-get install subversion autoconf libtool automake gfortran g++ --yes
svn co https://svn.code.sf.net/p/cmusphinx/code/trunk/cmuclmtk/
cd cmuclmtk/
sudo ./autogen.sh && sudo make && sudo make install
cd ..
```

## Installation Phonetisaurus

```bash
sudo su -c "echo 'deb http://ftp.debian.org/debian experimental main contrib non-free' > /etc/apt/sources.list.d/experimental.list"
sudo apt-get update
sudo apt-get -t experimental install phonetisaurus m2m-aligner mitlm
```
