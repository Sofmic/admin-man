
### USB formatujemy pod FAT
``` bash
umount /dev/<URZĄDZENIE>
mkfs.fat /dev/<URZĄDZENIE>
```
#### Jeżeli nasz obraz przekracza 4GB wtedy tworzymy na nim 2 partycje jedną niewielką FAT a drugą NTFS
#### Partycje dla ułatwienia warto na spokojnie zrobić w gparted  lub bez GUI:
``` bash
fdisk /dev/<URZĄDZENIE>

mkfs.fat /dev/<MNIEJSZA PARTYCJA>
mkfs.ntfs /dev/<WIĘKSZA PARTYCJA>

```
### Tworzymy folder dla pliku ISO i urządzenia
``` bash
mkdir /mnt/iso
mkdir /mnt/usb
```
### Montujemy plik oraz urządzenie (komenda:lsblk)
``` bash
mount -o loop <PLIK>.iso /mnt/iso
mount /dev/<WIĘKSZA PARTYCJA> /mnt/usb
```
#
### Kopiujemy zawartość z pliku do usb
``` bash
cp -R /mnt/iso/* /mnt/usb/
```
