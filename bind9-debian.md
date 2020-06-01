# Jako parametr należy podać nazwe domeny np: ./bind9-debian dom.com

# Uwagi:
# 
# Linie oznaczone    #ADDRESS    wymagają dostosowania adresu.
# /etc nie sprawdzi się jako folder dla stref podpisanych gdyż bind nie ma pozwolenia na dodawanie czegokolwiek do /etc/...


###aptitude install bind9 bind9utils bind9-doc dnsutils -y && echo "[ Zainstalowano ]"
mkdir -p /var/cache/bind/master
dnssec-keygen -b 2048 -K /var/cache/bind/ $1
dnssec-keygen -b 2048 -f ksk -K /var/cache/bind/ $1
chown -R bind:bind /var/cache/bind
chmod -R 700 /var/cache/bind

############################################################
if [ ! -e /etc/bind/named.conf.options.backup ]
then
  cp /etc/bind/named.conf.options /etc/bind/named.conf.options.backup
fi
echo """options {
        directory \"/var/cache/bind\";
        auth-nxdomain no;    # conform to RFC1035
        listen-on port 53 { localhost; 192.168.1.0/24; };             #ADDRESS
        allow-query { localhost; 192.168.1.0/24; };                   #ADDRESS
        forwarders { 8.8.8.8; };                                      #ADDRESS
        key-directory \"var/cache/bind\";
        listen-on-v6 { any; };
        recursion yes;
        };""" > /etc/bind/named.conf.options

############################################################
if [ ! -e /etc/bind/named.conf.local.backup ]
then
  cp /etc/bind/named.conf.local /etc/bind/named.conf.local.backup
fi
echo """zone \"$1\" {
        type master;
        file \"/var/cache/bind/master/forward.$1.db\";
        key-directory \"/var/cache/bind\";
	auto-dnssec maintain;
	inline-signing yes;
 };

zone \"1.168.192.in-addr.arpa\" {                                    #ADDRESS
       type master;
       file \"/var/cache/bind/master/reverse.$1.db\";
       key-directory \"/var/cache/bind\";
       auto-dnssec maintain;
       inline-signing yes;
 };"""  > /etc/bind/named.conf.local
 
 ############################################################
 if [ ! -e /var/cache/bind/master/forward.$1.db ]
then
 echo """\$TTL 604800
@       IN      SOA     ns1.$1. mail.$1. (
                              6         ; Serial
                         604820         ; Refresh
                          86600         ; Retry
                        2419600         ; Expire
                         604600 )       ; Negative Cache TTL

@	IN      NS      ns1.$1.
@ IN      A       192.168.1.103                                    ;DDRESS
ns1 IN       A      192.168.1.103                                  ;ADDRESS

$1. IN  MX  10  mail.$1.

www     IN       A       192.168.1.103                             ;ADDRESS
mail    IN       A       192.168.1.103                             ;ADDRESS

ftp     IN      CNAME    $1.""" > /var/cache/bind/master/forward.$1.db
else
  echo "Plik strefy wyszukiwania do przodu ISTNIEJE!!! [ /var/cache/bind/master/forward.$1.db ]"
fi

############################################################
 if [ ! -e /var/cache/bind/master/reverse.$1.db ]
then
 echo """\$TTL 604800
@       IN      SOA     $1. mail.$1. (
                             21         ; Serial
                         604820         ; Refresh
                          864500        ; Retry
                        2419270         ; Expire
                         604880 )       ; Negative Cache TTL

@       IN      NS      ns1.$1.
ns1 IN      A       192.168.1.103

103      IN      PTR     ns1.$1.                                   ;ADDRESS
103      IN      PTR     www.$1.                                   ;ADDRESS
103      IN      PTR     mail.$1.                                  ;ADDRESS
 """ > /var/cache/bind/master/reverse.$1.db
else
  echo "Plik strefy wyszukiwania wstecz ISTNIEJE!!! [ /var/cache/bind/master/reverse.$1.db ]"
fi

rndc reload
rndc reconfig
rndc sign $1
rndc signing -list $1

