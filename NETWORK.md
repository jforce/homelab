# Network

## Local Network

|  Network Config             | Address        |
|-----------------------------|----------------|
| Router                      | 192.168.1.1    |
| Network                     | 192.168.1.0    |
| Broadcast                   | 192.168.1.255  |
| Subnet                      | 255.255.255.0  |
| CIDR                        | /24            |
| DHCP start                  | 192.168.1.100  |
| DHCP end                    | 192.168.1.254  |
| Domain                      | gohyang.net    |

## OpenShift

SNO assisted/agent based installer using a custom iso.

### OCP Network config

| OpenShift Network Config    | Address        |
|-----------------------------|----------------|
| Machine networks CIDR       | 192.168.1.0/24 |
| Server/API/Ingress          | 192.168.1.173  |
| Cluster network CIDR	      | 10.128.0.0/14  |
| Cluster network host prefix | 23             |
| Service network CIDR        | 172.30.0.0/16  |
