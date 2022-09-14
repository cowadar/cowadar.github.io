# Ubuntu - Snippets

## Beschrijving

Deze snippets werken in Ubuntu of andere [debian](../debian.md)-gebaseerde [operating systems](../../operating_systems.md).

## Show last 5 modified files

```bash
ls -lht | head -6
```

where:

`-l` outputs in a list format

`-h` makes output human readable (i.e. file sizes appear in kb, mb, etc.)

`-t` sorts output by placing most recently modified file first

`head -6` will show 5 files because ls prints the block size in the first line of output.

I think this is a slightly more elegant and possibly more useful approach.

Example output:

```bash
total 26960312 -rw-r--r--@ 1 user staff 1.2K 11 Jan 11:22 phone2.7.py -rw-r--r--@ 1 user staff 2.7M 10 Jan 15:26 03-cookies-1.pdf -rw-r--r--@ 1 user staff 9.2M 9 Jan 16:21 Wk1_sem.pdf -rw-r--r--@ 1 user staff 502K 8 Jan 10:20 lab-01.pdf -rw-rw-rw-@ 1 user staff 2.0M 5 Jan 22:06 0410-1.wmv
```

## Set static IP

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

```yaml
# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:                #pas dit aan
      dhcp4: no                #yes/no
      dhcp6: no                #yes/no
      addresses: [192.168.1.100/24]    #pas dit aan
      gateway4: 192.168.1.1        #pas dit aan
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
```

```bash
sudo netplan apply
```

## Upgrade Ubuntu 20.04 to 22.04

```bash
sudo apt update & sudo apt upgrade -y
sudo do-release-upgrade
```

## Sluit een process op een gegeven poort

```bash
sudo kill -9 `sudo lsof -t -i:9001`
```
