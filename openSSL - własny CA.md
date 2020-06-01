#### Generowanie klucza prywatnego
``` bash
openssl genpkey -algorithm RSA -out <KLUCZ>.key -pkeyopt rsa_keygen_bits:2048
```
#### Generowanie pliku CSR (żądania)
``` bash
openssl req -new -sha256 -key <KLUCZ>.KEY -out <NAZWA PLIKU>.csr
```
#### Własny urząd CA
``` bash
mkdir /etc/ssl/CA
mkdir /etc/ssl/CA/{private,newcerts,certs,crl}
chown -R root:root /etc/ssl/CA
chmod 0700 /etc/ssl/CA/private
echo '01' | tee /etc/ssl/CA/serial
touch /etc/ssl/CA/index.txt
cd /etc/ssl/CA
cp ../openssl.cnf ./  
```
#### Zmień w openssl.cnf (plik konfiguracyjny)
``` bash
[ CA_default ]

dir		= .
```
#### Tworzenie klucza oraz certyfikatu CA
``` bash
openssl req -new -x509 -extensions v3_ca -newkey rsa:4096 -keyout private/cakey.pem -out cacert.pem -days 3650
```
#### Podpisywanie certyfikatu
``` bash
openssl ca -config <PLIK KONFIGURACYJNY > -out <SCIEZKA DLA CERTYFIKATU> -infiles <PLIK ŻĄDANIA>
```
