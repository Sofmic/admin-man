### Uwierzytelnianie w postfix
[SASL](https://pl.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) czyli prosty sposób na implementacje uwierzytelniania.
Do jego wdrożenia użyje serwera Dovecot.
``` bash
aptitude install dovecot-core -y
```
 ### Edytujemy /etc/dovecot/conf.d/10-master.conf
 ``` bash
service auth {
  unix_listener /var/spool/postfix/private/auth {
    mode = 0660
    # Assuming the default Postfix user and group
    user = postfix
    group = postfix        
  }

auth_mechanisms = plain login
```
### Edytuj /etc/postfix/main.cf
``` bash
postconf -e "smtpd_sasl_type = dovecot"
postconf -e "smtpd_sasl_path = private/auth"
postconf -e "smtpd_sasl_auth_enable = yes"
postconf -e "smtpd_relay_restrictions = permit_mynetworks, permit_sasl_authenticated, reject_unauth_destination"

```
<br /> 

**Kwestie poświadczeń zmienimy w:** 

``` bash
/etc/dovecot/conf.d/auth-system.conf.ext
```
**Domyślnie użytkownicy są brani z pliku passwd:**
``` bash
userdb {
  driver = passwd
}
```
**a hasła względem modułu pam:**
``` bash
passdb {
  driver = pam
}
```
**Testowanie:**
``` bash
openssl s_client -connect <ADRES MX>:25 -starttls smtp <<koniec
ehlo ja.com

250-firma.com
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-AUTH PLAIN LOGIN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-DSN
250-SMTPUTF8
250 CHUNKING

AUTH LOGIN
334 VXNlcm5hbWU6
ZGFyZWs=
334 UGFzc3dvcmQ6
aGFzbG9kYXJrYQ==
235 2.7.0 Authentication successful
```
Najpierw używam openssl s_client
następnie wpisuje AUTH LOGIN po czym w base64 podaje nazwę użytkownika i hasło.
