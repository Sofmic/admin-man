### Podstawy postfix

*Postfix ma 2 główne pliki konfiguracyjne:*
* **`main.cf`** - składa się z wpisów typu "opcja = wartość"
* **`master.cf`** - konfiguracja procesów 

**Instalacja**:   
aptitude install postfix -y
  
**Na start:**     
W pliku /etc/postfix/main.conf ustawiamy (zmień opcje w <>):
``` bash
myhostname = <nazwa internetowa komputera, najcześciej domena>
inet_intefaces = <interfejs albo all>
inet_protocols = <all>
home_mailbox = Maildir/
mynetworks = <dopisujemy po spacji swoją sieć>
```
W folderze użytkownika tworzymy strukture i ustawiamy prawa:
``` bash
mkdir -p /home/<USER>/Maildir/{cur,new,tmp}
chown <USER>:<USER> -R /home/<USER>/Maildir/{cur,new,tmp}
chmod 0700 -R /home/<USER>/Maildir/{cur,new,tmp}
```
W tej chwili restartujemy serwer i możemy spróbować wysłać naszą pierwszą wiadomość:
``` bash
systemctl restart postfix
nc <DOMENA> 25 <<koniec
mail from:<USER1>@<DOMENA>
rcpt to:<USER2>@<DOMENA>
data
TEST POCZTY
.
quit
koniec
```
Jeżeli w logach znajdziemy "status=sent (delivered to maildir)" to oznacza że wiadomość dotarła.
Możemy ją także przeczytać:
``` bash
vim /home/<USER2>/Maildir/new/*
```
