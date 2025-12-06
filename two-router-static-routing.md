# Static Routing Between Two Routers
## Topology
Two routers connected via Serial0/0, each serving a different LAN:
- **R1 LAN**: 10.1.1.0/24
- **R2 LAN**: 10.2.1.0/24
- **WAN link**: 192.168.1.0/24
PCs on each LAN use their respective router interfaces as gateways.

<img width="1133" height="562" alt="image" src="https://github.com/user-attachments/assets/5bdab337-e9fb-40db-959e-883af1f8b185" />

## Configure
### R1 Configuration
1. Configure Serial0/0
```telnet
R1#config t
R1(config)#int s0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#no shutdown
R1(config-if)#exit
R1(config)#
```

2. Configure FastEthernet0/0
```telnet
R1(config)#int f0/0
R1(config-if)#ip address 10.1.1.1 255.255.255.0
R1(config-if)#no shut
R1(config-if)#exit
```

3. Verify Interfaces
```telnet
R1#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.1.1.1        YES manual up                    up      
Serial0/0                  192.168.1.1     YES manual up                    up      
FastEthernet0/1            unassigned      YES unset  administratively down down    
```

4. Add Static Route to R2 LAN
```telnet
R1(config)#ip route 10.2.1.0 255.255.255.0 192.168.1.2
```
4. Verify Routing Table
```telnet
R1#show ip route
     10.0.0.0/24 is subnetted, 2 subnets
S       10.2.1.0 [1/0] via 192.168.1.2
C       10.1.1.0 is directly connected, FastEthernet0/0
C    192.168.1.0/24 is directly connected, Serial0/0
```

### R2 Configuration
1. Configure Serial0/0
```telnet
R1#config t
R1(config)#int s0/0
R1(config-if)#ip address 192.168.1.2 255.255.255.0
R1(config-if)#no shutdown
R1(config-if)#exit
R1(config)#
```

2. Configure FastEthernet0/0
```telnet
R1(config)#int f0/0
R1(config-if)#ip address 10.2.1.1 255.255.255.0
R1(config-if)#no shut
R1(config-if)#exit
```

3. Verify routing
```telnet
R1#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.1.2.1        YES manual up                    up      
Serial0/0                  192.168.1.2     YES manual up                    up      
FastEthernet0/1            unassigned      YES unset  administratively down down    
```

4. Add Static Route to R1 LAN
```telnet
R1(config)#ip route 10.1.1.0 255.255.255.0 192.168.1.1
```
4. Verify Routing Table
```telnet
R2#show ip route
     10.0.0.0/24 is subnetted, 2 subnets
C       10.2.1.0 is directly connected, FastEthernet0/0
S       10.1.1.0 [1/0] via 192.168.1.1
C    192.168.1.0/24 is directly connected, Serial0/0
```

### PC Configurations
#### PC1 (R1 LAN)
```telnet
PC1> ip 10.1.1.2 255.255.255.0 10.1.1.1
Checking for duplicate address...
PC1 : 10.1.1.2 255.255.255.0 gateway 10.1.1.1
```

#### PC3 (R2 LAN)
```telnet
PC1> ip 10.2.1.2 255.255.255.0 10.2.1.1
Checking for duplicate address...
PC1 : 10.2.1.2 255.255.255.0 gateway 10.2.1.1
```

## Connectivity Test
### PC → Default Gateway
```telnet
PC1> ping 10.1.1.1

84 bytes from 10.1.1.1 icmp_seq=1 ttl=255 time=29.829 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=255 time=6.650 ms
84 bytes from 10.1.1.1 icmp_seq=3 ttl=255 time=26.227 ms
84 bytes from 10.1.1.1 icmp_seq=4 ttl=255 time=36.652 ms
84 bytes from 10.1.1.1 icmp_seq=5 ttl=255 time=26.711 ms
```
### Same LAN Connectivity
```telnet
PC1> ping 10.1.1.3

84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.091 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=0.239 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=0.217 ms
84 bytes from 10.1.1.3 icmp_seq=4 ttl=64 time=0.204 ms
84 bytes from 10.1.1.3 icmp_seq=5 ttl=64 time=0.382 ms
```
### Cross-Network Routing (PC1 → R2 LAN)
```telnet
PC1> ping 10.2.1.1

84 bytes from 10.2.1.1 icmp_seq=1 ttl=254 time=46.710 ms
84 bytes from 10.2.1.1 icmp_seq=2 ttl=254 time=65.759 ms
84 bytes from 10.2.1.1 icmp_seq=3 ttl=254 time=56.754 ms
84 bytes from 10.2.1.1 icmp_seq=4 ttl=254 time=66.445 ms
84 bytes from 10.2.1.1 icmp_seq=5 ttl=254 time=56.649 ms
```

