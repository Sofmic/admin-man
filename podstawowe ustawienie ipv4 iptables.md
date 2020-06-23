### Propozycja ustawien na początek w iptables

" Jeżeli pracujesz zdalnie użyj iptables-apply "

``` bash
*filter
:INPUT DROP [0:0]
:OUTPUT DROP [0:0]
:FORWARD DROP [0:0]

# akceptuj juz nawiazane polaczenia
-I INPUT 1 -m state --state RELATED,ESTABLISHED -j ACCEPT
-I OUTPUT 1 -m state --state RELATED,ESTABLISHED -j ACCEPT

# interfejs loopback
-A INPUT -i lo -j ACCEPT
-A OUTPUT -o lo -j ACCEPT

# wychodzace req dhcp
-A OUTPUT -p udp --dport 67:68 --sport 67:68 -j ACCEPT

# nowe polaczenia ssh
-A INPUT -p tcp -m state --state NEW --dport 22 -j ACCEPT

# zapytania dns
-A OUTPUT -p udp --dport 53 -j ACCEPT

# ping
-A OUTPUT -p icmp -j ACCEPT

# zapytania ntp
-A OUTPUT -p udp --dport 123 --sport 123 -j ACCEPT

# polaczenie http, https
-A OUTPUT -p tcp -m state --state NEW --dport 80 -j ACCEPT
-A OUTPUT -p tcp -m state --state NEW --dport 443 -j ACCEPT

COMMIT
```
