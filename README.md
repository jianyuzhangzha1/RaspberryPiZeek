# Raspberry Pi Zeek

## Update zhang jianyu 
 20260417 runned on trexie, compile cost about 1 hour



  > 1 Can't pass the configure, need to install the library first
  ```Bash
  sudo apt install libnode-dev  libzmq3-dev -y
  ```

  > 2 if failed when execute configure, try below script. To fetch the submodule's makefile again.
  ```Bash
  git submodule update --init --recursive
  ```

  > 3 For Raspberry Pi 4B has four core, we can set 4. But due to the heat or memory limition, set to 2 is recommended. Of course, cross compile with a powerful PC is even better.
  ```Bash
  make -j2
  ```
<br>

## Deployment of Zeek on a Raspberry Pi 4B<BR />
This is some of my work deploying Zeek NSM on a Raspberry Pi<BR />
Zeek can be found here: https://github.com/zeek<BR />

## Initial Deployment:
Install Raspbian Lite
https://www.raspberrypi.org/documentation/installation/installing-images/

## Management Configuration:
Majority of this can be completed via Raspi-config
  ```Bash
  sudo raspi-config
  ```
Change hostname
Change password
Enable SSH
Connect wifi 
  ```Bash
  sudo iwlist wlan0 scan
   ```
Expand file system

#Run Updates and Upgrades
  ```Bash
  sudo apt update && upgrade -y
   ```
  
Reboot


## Check/enable promiscuous mode
Force eth0 into promiscuous mode 
  ```Bash
  ifconfig eth0 promisc
  ```
Run netstat -i to determine if promiscuous mode has been enabled
 ```Bash
  netstat -i
  ```


## Install Pre-Reqs for Zeek
https://docs.zeek.org/en/current/install.html#

## Required Dependencies:
Install all required Zeek dependencies
  ```Bash
  sudo apt-get install cmake make gcc g++ flex bison libpcap-dev libssl-dev python3 python3-dev swig zlib1g-dev -y
  ```


  ```Bash
  sudo apt install libnode-dev  libzmq3-dev -y
  ```
  > 20260417 can't pass the configure zhang jianyu

## Optional Dependences:
Install optional dependencies
  ```Bash
	sudo apt-get install python3-git python3-semantic-version libkrb5-dev libjemalloc-dev google-perftools -y
	sudo apt-get install libgoogle-perftools4 perl curl libtcmalloc-minimal4 libgoogle-perftools-dev -y
	sudo apt-get install libtcmalloc-minimal4 -y
	sudo apt-get install libmaxminddb-dev -y 
	sudo apt-get install sendmail sendmail-cf m4 -y
  ```
  Instructions for Geo-IP: https://docs.zeek.org/en/current/frameworks/geoip.html#geolocation<BR />
  Instructions for SendMail: https://tecadmin.net/install-sendmail-on-debian-9-stretch/ 
  
## Install Zeek:
```Bash
git clone --recursive https://github.com/zeek/zeek
cd zeek
./configure --prefix=/opt/zeek --build-type=release
make
make install-aux
```
if failed when execute configure, try
```Bash
git submodule update --init --recursive
```

## Give the current user (pi) ownership of the Zeek binary
```Bash
sudo chown -R pi:pi /opt/zeek
sudo chmod 750 /opt/zeek
```
## Add the Zeek binary path to the bottom of the user profile file - PATH="/opt/zeek/bin:$PATH"
```Bash
nano ~/.profile

source ~/.profile
```
## Run Zeekctl to setup Zeek
```Bash
zeekctl
```
[zeekctl] install
[zeekctl] deploy
[zeekctl] status
[zeekctl] debug


