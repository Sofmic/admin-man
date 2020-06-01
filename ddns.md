### Generowanie klucza
``` bash
dnssec-keygen -a HMAC-MD5 -b 512 -r /dev/urandom -n USER ddns
```
### Do nowego pliku ddns.key wklej wartość klucza z *.private w pole \<KEY>
```bash
key ddns {    

        algorithm HMAC-MD5.SIG-ALG.REG.INT;
        secret "\<KEY>";
};
```
### Szybka instalacja pliku (dostosuj scieżkę)
```bash
install -o root -g bind -m 0640 ddns.key /etc/bind/ddns.key
install -o root -g root -m 0640 ddns.key /etc/dhcp/ddns.key
```
### Dodaj na początku /etc/bind/named.conf.local
``` bash
include "/etc/bind/ddns.key";
```
### A do obu stref w tym samym pliku dopisz
``` bash
notify no;  ### Tylko jeśli używasz prywatnych adresów
allow-update { key ddns; };
```

############################################################
### Propozycja konfiguracji /etc/dhcp/dhcpd.conf
``` bash
authoritative;
option domain-name "<NAZWA DOMENY>";
option domain-name-servers <NAZWA HOSTA Z DNS>;

ddns-updates on;
ddns-update-style interim;
ignore client-updates;
update-static-leases on;

default-lease-time 600;
max-lease-time 7200;
log-facility local7;


include "/etc/dhcp/ddns.key";

zone <NAZWA DOMENY>. {
  primary 127.0.0.1;
  key ddns;
}

zone 1.168.192.in-addr.arpa. {
  primary 127.0.0.1;
  key ddns;
}


subnet 192.168.1.0 netmask 255.255.255.0 {
        range 192.168.1.1 192.168.2.255;
        option routers 192.168.1.103;
}

```
############################################################
### Zrestartuj dhcp i bind9
``` bash
systemctl restart isc-dhcp-server
systemctl restart bind9
```
