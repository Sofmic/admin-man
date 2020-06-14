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
**Samopodpisywalny certyfikat**
``` bash  
openssl req -nodes -newkey rsa:2048 -keyout privatekey.key -out mail.csr
   
openssl x509 -req -days 365 -in mail.csr -signkey privatekey.key -out secure.crt
   
cp {privatekey.key,secure.crt} /etc/postfix
```
**Edytujemy plik `main.cf`**
``` bash

smtpd_tls_key_file = <klucz>
smtpd_tls_cert_file = <certyfikat>
smtpd_tls_CAfile = <główny certyfikat>
smtpd_tls_security_level = may

```
**Sprawdzanie działania:**
``` bash
nc localhost 25
220 firma.com
starttls
220 2.0.0 Ready to start TLS
```

