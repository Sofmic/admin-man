
### Instalacja narzędzi
``` bash
aptitude install lvm2
```
### Tworzenie fizycznego woluminu 
``` bash
vpcreate <dysk> ...
```
### Tworzenie grupy woluminów
``` bash
vgcreate <NAZWA GRUPY (DOWOLNA)> <URZĄDZENIA BĘDĄCE POD LVM>
```
### Dodawanie dysku do grupy
``` bash
vgextend <nazwa grupy> <dysk>
```
### Tworzenie woluminu logicznego
``` bash
lvcreate --name <NAZWA DLA WOLUMINU LOGICZNEGO> --size <ROZMIAR> <NAZWA GRUPY WOLUMINÓW>
```
### Podglad
``` bash
lvs
```
### Formatowanie partycji i można ją montować
``` bash
mkfs.<nazwa fs> /dev/mapper/<partycja>
```
### Rozszerzanie partycji jeśli jest miejsce o 10GB
``` bash
lvextend --size +10g --resizefs /dev/mapper/<partycja>
```
