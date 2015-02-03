# WiFi Hotspot

Aus der Anleitung [https://learn.adafruit.com/setting-up-a-raspberry-pi-as-a-wifi-access-point/overview](https://learn.adafruit.com/setting-up-a-raspberry-pi-as-a-wifi-access-point/overview) wird aus dem Raspberry Pi ein WiFi Hotspot.

Benötigte Teile: Ein Edimax WiFi USB Dongle. Vor dem Booten einfach den USB Dongle einstecken.

## Test

```bash
ifconfig -a
```

Zeigt das Gerät **wlan0** an.

## Installation

```bash
sudo apt-get update
sudo apt-get install hostapd isc-dhcp-server
```

## DHCP Server einrichten

```bash
sudo sed -i.bak 's/option domain-name/# option domain-name/' /etc/dhcp/dhcpd.conf
sudo sed -i.bak 's/#authoritative;/authoritative;/ /etc/dhcp/dhcpd.conf
echo "
subnet 192.168.42.0 netmask 255.255.255.0 {
	range 192.168.42.10 192.168.42.50;
	option broadcast-address 192.168.42.255;
	option routers 192.168.42.1;
	default-lease-time 600;
	max-lease-time 7200;
	option domain-name "local";
	option domain-name-servers 8.8.8.8, 8.8.4.4;
}" | sudo tee -a /etc/dhcp/dhcpd.conf
```

```bash
sudo sed -i.bak 's/INTERFACES=""/INTERFACES="wlan0"/' /etc/default/isc-dhcp-server
```

## Statische IP für wlan0

```bash
sudo ifdown wlan0
echo "iface wlan0 inet static
  address 192.168.42.1
  netmask 255.255.255.0" | sudo tee -a /etc/network/interfaces
sudo ifconfig wlan0 192.168.42.1
```

## Access Point einrichten

```bash
echo "interface=wlan0
driver=rtl871xdrv
ssid=Pi_AP
hw_mode=g
channel=6
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=Raspberry
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP" | sudo tee -a /etc/hostapd/hostapd.conf
sudo sed -i.bak 's,#DAEMON_CONF="",DAEMON_CONF="/etc/hostapd/hostapd.conf",' /etc/default/hostapd
```

## NAT konfigurieren

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
echo "up iptables-restore < /etc/iptables.ipv4.nat" | sudo tee -a /etc/network/interfaces
```

## Update hostapd

```bash
wget http://adafruit-download.s3.amazonaws.com/adafruit_hostapd_14128.zip
unzip adafruit_hostapd_14128.zip
sudo mv /usr/sbin/hostapd /usr/sbin/hostapd.ORIG
sudo mv hostapd /usr/sbin
sudo chmod 755 /usr/sbin/hostapd
```

## Erster Test

```bash
sudo /usr/sbin/hostapd /etc/hostapd/hostapd.conf
```

# Automatik

```bash
sudo service hostapd start
sudo service isc-dhcp-server start
sudo update-rc.d hostapd enable
sudo update-rc.d isc-dhcp-server enable
```
