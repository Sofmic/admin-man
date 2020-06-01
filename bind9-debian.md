# Uwagi:
 Dostosuj adresy do własnych potrzeb
 Nie używaj folderu /etc do przechowywania plików stref ani kluczy
```bash
aptitude install bind9 bind9utils bind9-doc dnsutils -y 
mkdir -p /var/cache/bind/master
dnssec-keygen -b 2048 -K /var/cache/bind/ <NAZWA DOMENY>
dnssec-keygen -b 2048 -f ksk -K /var/cache/bind/ <NAZWA DOMENY>
chown -R bind:bind /var/cache/bind
chmod -R 700 /var/cache/bind
```
############################################################
### /etc/bind/named.conf.options
```bash
options {
        directory "/var/cache/bind";
        auth-nxdomain no;
        listen-on port 53 { localhost; 192.168.1.0/24; };
        allow-query { localhost; 192.168.1.0/24; };
        forwarders { 8.8.8.8; };
        key-directory "var/cache/bind";
        listen-on-v6 { any; };
        recursion yes;
        };
```
############################################################
### /etc/bind/named.conf.local
```bash
zone "<NAZWA DOMENY>" {
        type master;
        file "/var/cache/bind/master/<NAZWA DOMENY>";
        key-directory "/var/cache/bind";
	auto-dnssec maintain;
	inline-signing yes;
 };

zone "<ORIGIN DLA REVERSE>.in-addr.arpa" {
       type master;
       file "/var/cache/bind/master/<NAZWA DOMENY>.rev";
       key-directory "/var/cache/bind";
       auto-dnssec maintain;
       inline-signing yes;
 };
```
 
 ############################################################
#### /var/cache/bind/master/\<NAZWA DOMENY>
 ``` bash
 $ORIGIN <NAZWA DOMENY>.
 $TTL 604800
@       IN      SOA     ns1.<NAZWA DOMENY>. mail.<NAZWA DOMENY>. (
                              6         ; Serial
                         604820         ; Refresh
                          86600         ; Retry
                        2419600         ; Expire
                         604600 )       ; Negative Cache TTL

 	IN      NS      ns1.<NAZWA DOMENY>.
 	IN	MX  10  mail.<NAZWA DOMENY>.
@ 	IN      A       192.168.1.103
ns1 	IN      A	192.168.1.103
mail    IN      A       192.168.1.103
``` 
############################################################
#### /var/cache/bind/master/\<NAZWA DOMENY>.rev

``` bash
$ORIGIN 1.168.192.in-addr.arpa.
$TTL 604800
@       IN      SOA     <NAZWA DOMENY>. mail.<NAZWA DOMENY>. (
                             21         ; Serial
                         604820         ; Refresh
                          864500        ; Retry
                        2419270         ; Expire
                         604880 )       ; Negative Cache TTL

 	IN      NS      ns1.<NAZWA DOMENY>.
103	IN	PTR	<NAZWA DOMENY>.
103     IN      PTR     ns1.<NAZWA DOMENY>.
103     IN      PTR     mail.<NAZWA DOMENY>.
 ```
############################################################
#### Podpisywanie rekordów
 ``` bash
rndc reload
rndc reconfig
rndc sign <NAZWA DOMENY>
rndc signing -list <NAZWA DOMENY>
```
