### Najpierw zaktualizuj repozytorium wpisami na stronie producenta
https://downloads.mariadb.org/mariadb/repositories/#distro=Debian&distro_release=buster--buster&mirror=rackspace&version=10.4
#### [ INSTALACJA ]
``` bash
aptitude install mariadb-server
mysql_secure_installation
```
#### [ KONFIGURACJA ]
#### logowanie:	
``` bash
	mysql -u root -p
```
#### Podstawowe operacje na bazie:
```bash
CREATE DATABASE <NAZWA BAZY>;
DROP DATABASE <NAZWA BAZY>;
CREATE USER `<UZYTKOWNIK>`@`<HOST>` identified by `<HASLO>`;
? GRANT;  (Wyświetlanie pomocy dla polecenia)
GRANT ALL ON *.* TO `USER`@`HOST`;
FLUSH PRIVILEGES; (przeładowanie uprawnień)
```
####  Tworzenie i przywracanie kopii wszystkich baz danych
``` bash
mysqldump -u root -p --all-database > nazwa-pliku
mysql -u root -p < nazwa-pliku
```
