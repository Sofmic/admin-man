### Zainstaluj potrzebne pakiety
```bash
  apt install apache2 php-fpm php-mysql php-gd php-imap php-mbstring
```

### Włącz moduły:
``` bash
  a2enmod alias proxy proxy_fcgi
```
  
### Do pliku Virtualhost'a dodaj ten blok:
``` bash
  <FilesMatch \.php$>
    SetHandler "proxy:unix:/run/php/php7.3-fpm.sock|fcgi://localhost"
  </FilesMatch>
```
