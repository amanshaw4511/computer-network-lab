# Setting Up DHCP on a LAN Router
<img width="589" height="478" alt="image" src="https://github.com/user-attachments/assets/f3063691-1a81-421b-8ea3-04513268e9fc" />


### Configure LAN Interface
- Configure IP/Subnet to the LAN-facing interface.
```telnet
R1(config)#int f0/0
R1(config-if)#ip address 10.1.1.1 255.255.255.0
R1(config-if)#no shut
R1(config-if)#exit
```

### Configure DHCP on Router
- Create a DHCP pool.
- Define the network.
- Set default gateway and DNS.
```telnet
R1(config)#ip dhcp pool pool1
R1(dhcp-config)#network 10.1.1.0 255.255.255.0
R1(dhcp-config)#default-router 10.1.1.1
R1(dhcp-config)#dns-server 8.8.8.8
R1(dhcp-config)#do write 
```

### Enable DHCP on PCs
- PC will request a dynamic IP using the DHCP protocol.
- PC receives the following information from the DHCP server:
  - IP address
  - subnet mask
  - default gateway
  - DNS server address

#### PC1
```telnet
PC1> ip dhcp
DDORA IP 10.1.1.2/24 GW 10.1.1.1
```

##### PC2
```telnet
PC1> ip dhcp
DDORA IP 10.1.1.3/24 GW 10.1.1.1
```

## Testing
### Check PC IP info
```telnet
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 10.1.1.1
DNS         : 8.8.8.8  
DHCP SERVER : 10.1.1.1
DHCP LEASE  : 85705, 86400/43200/75600
DOMAIN NAME : write
MAC         : 00:50:79:66:68:00
LPORT       : 10054
RHOST:PORT  : 127.0.0.1:10055
MTU         : 1500
```

### Ping the gateway
```telnet
PC1> ping 10.1.1.1

84 bytes from 10.1.1.1 icmp_seq=1 ttl=255 time=29.829 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=255 time=6.650 ms
84 bytes from 10.1.1.1 icmp_seq=3 ttl=255 time=26.227 ms
84 bytes from 10.1.1.1 icmp_seq=4 ttl=255 time=36.652 ms
84 bytes from 10.1.1.1 icmp_seq=5 ttl=255 time=26.711 ms
```

### Ping b/w PCs
```telnet
PC1> ping 10.1.1.3

84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.075 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=0.228 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=0.200 ms
84 bytes from 10.1.1.3 icmp_seq=4 ttl=64 time=0.177 ms
84 bytes from 10.1.1.3 icmp_seq=5 ttl=64 time=0.174 ms
```

