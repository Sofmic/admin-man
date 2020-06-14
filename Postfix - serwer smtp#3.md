### Uwierzytelnianie w postfix (implementacja w dovecot)
``` bash
aptitude install dovecot-core dovecot-pop3d dovecot-imapd -y
```
 ### /etc/dovecot/conf.d/10-master.conf
 ``` bash
szukamy sekcji "service auth" a nastepnie komentarza "Postfix smtp-auth"
odblokowujemy tą klamre i dodajemy do niej:
user = postfix
group = postfix
```
### /etc/dovecot/conf.d/10-auth.conf
``` bash
auth_mechanisms = plain login
```
### Edytuj /etc/postfix/main.cf
``` bash
postconf -e "smtpd_sasl_type = dovecot"
postconf -e "smtpd_sasl_path = private/auth"
postconf -e "smtpd_sasl_auth_enable = yes"
postconf -e "broken_sasl_auth_clients = yes"
```
### Ustawianie polis serwera i zabezpieczen
``` bash
postconf -e "smtpd_sasl_security_options = noanonymous, noplaintext"
postconf -e "smtpd_sasl_tls_security_options = noanonymous"
postconf -e "smtpd_tls_auth_only = yes"
postconf -e "smtpd_relay_restrictions = permit_mynetworks, permit_sasl_authenticated, reject_unauth_destination"
postconf -e "smtpd_sender_login_maps = hash:/etc/postfix/lista_wysylajacych"
```
#### Nalezy zrobić plik "lista_wysylajacych" i wpisać do niego email oraz login wlasciciela tego adresu
touch /etc/postfix/lista_wysylajacych
np: darek@firma.com darek
``` bash
postconf -e "smtpd_recipient_restrictions = reject_sender_login_mismatch, permit_sasl_authenticated, reject_unauthenticated_sender_login_mismatch"
```
#### Według dokumentacji jest to dobre ustawienie dla źle skonfigurowanych klientów
``` bash
postconf -e "smtpd_sasl_local_domain = firma.com"
```
