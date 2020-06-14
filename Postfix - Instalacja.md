### Podstawy postfix

*Postfix ma 2 główne pliki konfiguracyjne:*
* **`main.cf`** - składa się z wpisów typu "opcja = wartość"
* **`master.cf`** - konfiguracja procesów 

**Instalacja**:   
``` bash
aptitude install postfix -y
```
**Na start:**     
W pliku /etc/postfix/main.conf ustawiamy (zmień opcje w <>):
``` bash
myhostname = <nazwa internetowa komputera, najcześciej domena>
mydomain = firma.com
mynetworks = <dopisujemy po spacji swoją sieć>
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
``` bash
vim /var/log/mail.log
```
Możemy ją także przeczytać:
``` bash
vim /var/mail/*
```
