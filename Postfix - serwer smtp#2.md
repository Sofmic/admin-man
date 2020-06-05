### Szyfrowanie w postfix
Jeżeli mamy własną domene no to najlepiej użyć Let's encrypt.
``` bash
certbot certonly --standalone -uir --hsts --agree-tos -n -m <MAIL> -d <ADRES MX>
```  
**W przypadku własnego CA tworzymy klucz, plik żądania i generujemy certyfikat:**
``` bash
openssl genpkey -algorithm RSA -out <KLUCZ>.key -pkeyopt rsa_keygen_bits:2048
openssl req -new -sha256 -key <KLUCZ>.KEY -out <NAZWA PLIKU>.csr
cd /etc/ssl/CA
openssl ca -config openssl.cnf -out <ŚĆIEŻKA DLA CERTYFIKATU> -infiles <PLIK CSR>
``` 
**Edytujemy plik `main.cf`**
``` bash
smtp_tls_key_file = <klucz>
smtp_tls_cert_file = <wygenerowany certyfikat>
smtp_tls_CAfile = <główny certyfikat>
smtp_tls_security_level = may
smtp_tls_note_starttls_offer = yes
smtp_tls_mandatory_protocols=!SSLv2,!SSLv3
smtp_tls_protocols=!SSLv2,!SSLv3

smtpd_tls_key_file = <klucz>
smtpd_tls_cert_file = <certyfikat>
smtpd_tls_CAfile = <główny certyfikat>
smtpd_tls_security_level = may
smtpd_tls_auth_only = yes
smtpd_tls_mandatory_protocols=!SSLv2,!SSLv3
smtpd_tls_protocols=!SSLv2,!SSLv3
```
**Sprawdzanie działania:**
``` bash
nc localhost 25
220 Wyspa karakana
starttls
220 2.0.0 Ready to start TLS
```

