
### Zainstaluj:    
[libpam-usb_0.5.0-6_amd64.deb](https://github.com/Sofmic/admin-man/raw/master/libpam-usb_0.5.0-6_amd64.deb)     
[pamusb-common_0.5.0-6_amd64.deb](https://github.com/Sofmic/admin-man/raw/master/pamusb-common_0.5.0-6_amd64.deb)     
  
#### Podłącz USB którym chcesz się logować i wpisz:
``` bash
sudo pamusb-conf --add-device klucz
sudo pamusb-conf --add-user <NAZWA USERA>
cat /etc/pam.d/common-auth
```
#### Powinniśmy znaleść tam:
``` bash
auth	sufficient      pam_usb.so 
auth	[success=1 default=ignore]	pam_unix.so nullok_secure try_first_pass
```
