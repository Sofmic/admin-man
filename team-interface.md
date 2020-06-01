### CENTOS
#### Przed użycie należy dostosować nazwy interfejsów i połączeń do własnych potrzeb
``` bash
nmcli con add type team con-name team0 ifname bond0 config '{"device": "team0","runner": {"name": "activebackup"}}'
nmcli con add type team-slave con-name team0-port1 ifname ens33 master bond0
nmcli con add type team-slave con-name team0-port2 ifname ens36 master bond0
```  
#### Jeżeli system pokaże za dużo połączeń należy usunąć odpowiednie pliki konfiguracyjne z /etc/sysconfig/network-scripts/...


### DEBIAN 
``` bash
nm-connection-editor lub nmtui
```
##### W przypadku takiego błędu:
``` bash
(nm-connection-editor:4387): Gtk-WARNING **: 17:49:28.930: cannot open display: :0
``` 
##### Wykonujemy:
``` bash
xhost +SI:localuser:<nazwa uzytkownika>
```
