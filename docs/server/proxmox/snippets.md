# Proxmox - Snippets

## Disk Management

### Ubuntu VM Disk vergroten in Proxmox

Als je in Proxmox een disk vergroot, zal deze door de OS nog niet volledig gebruikt kunnen worden.
Daarom moeten we, na de vergroting in proxmox, deze alsnog met [GParted](../software/gparted.md) en via de CLI resizen zodat de volledige grootte herkend kan worden door het [Operating System](../../operating_systems/linux/operating_systems.md).

#### 1. Virtuele machine uitzetten

Schakel de virtuele machine eerst helemaal uit.

#### 2. Harde schijf resizen

Nu kan je de harde schijf vergroten.

`Proxmox` > `Hardware Settings`

#### 3. GParted downloaden en opstarten

1. Download laatste Gparted ISO van de [GParted download website](https://gparted.org/download.php)
2. Importeer de GParted ISO file in Proxmox
3. Start de VM en laat deze starten vanaf de ISO. (Duw snel op ++esc++)

#### 4. Partitie resizen met GParted

1. Zoek de correcte partitie
2. Resize de partitie (++right-button++ -> resize)
3. Duw opt vingske 🙂

#### 5. CLI resizing

Check de huidige disksize met commando:

```bash
df -h
```

Laat Ubuntu de volledige disk gebruiken:

```bash
sudo /sbin/lvresize -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```

Gebruik resize2fs op deze disk

```bash
sudo /sbin/resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

Hierna kan je nogmaals de disksize bekijken.

```bash
df -h
```

Als er geen fouten opgetreden zijn, heb je nu net de harde schijf vergroot!
