---
description: Proxmox Linux Virtual Machine nice-to-knows
---

# Proxmox Linux VM

## Disk Management

### Hoe een vm disk te resizen met Proxmox

1. Schakel VM uit
2. Resize disk (Proxmox - Hardware settings)
3. Import GParted ISO file
4. Duw zeer snel op [ESC] (Ja, echt snel!)
5. Resize de partitie (rechter-muisklik -> resize)
6. Duw opt vingske :)
7. Open SSH terminal
8. Check de disksize met commando: ``df -h``
9. Oh nee, nog niet veranderd :(
10. Geef volgend commando in en bevestig met PW: ``sudo /sbin/lvresize -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv``
11. Geef volgend commando in: ``sudo /sbin/resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv``
12. Indien geen fouten, check disksize nogmaals met: ``df -h``
13. Jippie!

## Netwerk

### Ubuntu Static IP geven

**Netplan config aanpassen**

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