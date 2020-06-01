
### Tworzymy plik haseł
``` bash
sudo htpasswd -c <PLIK HASEL> <NAZWA UZYTKOWNIKA>
```
### Do pliku virtualhost'a dodajemy:
``` bash
<Directory <SCIEZKA DO !!!! F O L D E R U !!!!>>
	AuthType Basic
	AuthName "WERYFIKACJA"
	AuthBasicProvider file
	AuthUserFile "<SCIEZKA DO PLIKU HASEL>"
	Require valid-user
</Directory>
```
### Uruchom ponownie apacha:		
``` bash 
systemctl restart apache2
