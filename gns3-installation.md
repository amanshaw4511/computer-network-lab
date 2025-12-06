# GNS3 Installation in Fedora

## Install gns3-server and guid
```sh
sudo dnf install gns3-server gns3-gui
```

## Install [dynamips](https://github.com/GNS3/dynamips)
Install dependecies
```sh
sudo dnf -y install elfutils-libelf-devel libuuid-devel libpcap-devel
```
Build and install
```sh
cd /tmp
git clone https://github.com/GNS3/dynamips
cd dynamips
mkdir build
cd build
cmake ..
sudo make install
which dynamips
```

## Install [vpcs](https://github.com/GNS3/vpcs)
```sh
cd /tmp
git clone https://github.com/GNS3/vpcs.git
cd vpcs/src
sh mk.sh
sudo mv vpcs /usr/bin
which vpcs
```

## Install wireshark (Optional)
```sh
sudo dnf install wireshark
```
