# Homelab

This repository will give an overview of my homelab journey.

## Initial considerations

I wanted a small, silent, efficient but powerful home server and was considering the below options:

**Hardware:**
- Something like [STH’s TinyMiniMicro series](https://www.servethehome.com/introducing-project-tinyminimicro-home-lab-revolution) and buy half a dozen 1L mini pcs.
- Dell/HP server tower server from [Bargain Hardware](https://www.bargainhardware.co.uk).
- Raspberry Pi ['cluster'](https://www.tomshardware.com/news/raspberry-pi-4-cluster-in-a-bag).
- Custom server type miniITX system using something like this [asrock rack board](https://www.asrockrack.com/general/productdetail.asp?Model=X570D4i-2t#Specifications).
- Nuc type [Microcluster](https://www.tranquil-it.co.uk/microclusters) like [Canonicals cloud in a box](https://arstechnica.com/information-technology/2014/06/hands-on-with-canonicals-orange-box-and-a-peek-into-cloud-nirvana).
- Custom built tower gambling with an [AliExpress AMD EPYC](https://www.aliexpress.com/w/wholesale-amd-epyc.html).
- [AsRock DeskMeet X300](https://www.asrock.com/nettop/AMD/DeskMeet%20X300%20Series/index.asp).

**Software**
- VMware
  - Mimics the vast majority of customers environments I work with, but kinda boring/uninteresting.
- Standalone all-in-one OpenStack / DevStack
  - I could see this being kinda fun but even though I do have some basic OSP experience I do risk spending too much time on the OSP side of things.
- SNO with KubeVirt and Hypershift
  - Future looking, fairly interesting, but will require some outside supporting services (DNS etc).
- Proxmox
  - No experience with this, but seems fairly simple.

In the end I opted to go for the fun DIY route and go with a small custom build tower with some dubious AliExpress parts and go for bare metal SNO OCP. However, I really was tempted by the nuc type cluster and go [full DIY](https://www.reddit.com/r/homelab/comments/r46ita/nuc_cluster_is_up_and_running_info_in_comment/) after coming accross [this](https://blog.fosketts.net/2020/09/10/introducing-rabbit-i-bought-a-cloud/)! In terms of software though, I think for my purpose I'd get better bang for my buck and remove any virtulization layer and go for running containers on bare metal.

## Parts list

I bought the parts from a mix of AliExpress, eBay, Scan  and Amazon. I was able to source everything with my budget of £1k through timing and bargaining.

| Item        | Component                                                                      |
|-------------|--------------------------------------------------------------------------------|
| CPU         | AMD EPYC 7551P CPU (32 cores, 64 threads)                                      |
| Motherboard | Supermicro H11SSL-i motherboard                                                |
| Memory      | Samsung 32gb 2133MHz DDR4 ECC RAM - x8 (256gb)                                 |
| PSU         | Super Flower LEADEX III HG 850w Gold PSU                                       |
| SSD         | Crucial P5 Plus 2TB M.2 PCIe Gen4 NVMe                                         |
| SSD         | Samsung SSD PM1735 HHHL PCIe card 1.6TB NVME SSD                               |
| GPU         | GIGABYTE GT 1030 Silent Low Profile 2GB Graphics Card                          |
| Cooler      | Noctua NH-U14S TR4-SP3 for AMD sTRX4/TR4/SP3 (140mm, Brown) Noctua NF-A15 PWM  |
| Fan         | Noctua NF-A15 PWM, Premium Quiet Fan, 4-Pin (140mm, Brown)                     |
| Case        | Raijintek Thetis Aluminium ATX case                                            |

## Build issues:

* **Networking** - IPMI was coming up on a static IP address on another subnet.
  - Resolved by removing the existing static IP config for the IPMI in the BIOS.
* **BMC** - Admin password had been changed to what was on the motherboard.
  - Resolved by loading supermicro’s [ipmicfg](https://www.supermicro.com/en/solutions/management-software/ipmi-utilities) utility onto a USB and booting into UEFI shell tool to add/reset users/passwords.
* **SSD**  - M.2 SSD was not being recognised.
  - Resolved by a BIOS setting to change pci/m.2 device compatibility from being 'Vendor Defined' to 'AMI Native Support.
* **Memory** - DIMMH1 was not recognising the installed memory module.
  - Resolved by [removing a case standoff](https://www.bleepingcomputer.com/forums/t/710239/supermicro-mbd-h11ssl-and-amd-epyc-7402-insane-memory-pmu-training-error-problem/) that was touching the underside of the board; only an issue due to using a server motherboard in a consumer case.
* **Fans** - Supermicro fans were ramping up and down though using consumer grade fans.
  - Resolved by setting the fan speed to max; it’s still pretty much dead silent thanks to the Noctuas and their anti-vibration mounts. See [Supermicro fan speed ramps up and down](https://youtu.be/Qez9F4uhsw4?si=WQ60yUGlw3q9aG0e).
* **Disks** - SNO requires two disks if wanting to use kubevirt.
  - Resolved by adding a second disk.
* **BMC Licence** - "Not licensed to perform this request. The following licenses SUM DCMS OOB were needed".
  - Resolved by [Reverse Engineering Supermicro IPMI](https://peterkleissner.com/2018/05/27/reverse-engineering-supermicro-ipmi/).

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

### Network config

| OpenShift Network Config    | Address        |
|-----------------------------|----------------|
| Machine networks CIDR       | 192.168.1.0/24 |
| Server/API/Ingress          | 192.168.1.173  |
| Cluster network CIDR	      | 10.128.0.0/14  |
| Cluster network host prefix | 23             |
| Service network CIDR        | 172.30.0.0/16  |
