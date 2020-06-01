1. Zainstaluj:
              [libpam-usb_0.5.0-6_amd64.deb](https://github.com/Sofmic/admin-man/raw/master/libpam-usb_0.5.0-6_amd64.deb)
              [pamusb-common_0.5.0-6_amd64.deb](https://github.com/Sofmic/admin-man/raw/master/pamusb-common_0.5.0-6_amd64.deb)
  
2. Podłącz USB którym chcesz się logować

3. sudo pamusb-conf --add-device klucz

4. sudo pamusb-conf --add-user < NAZWA USERA >

5. cat /etc/pam.d/common-auth

Powinniśmy znaleść tam:
```
auth	sufficient      pam_usb.so 
auth	[success=1 default=ignore]	pam_unix.so nullok_secure try_first_pass
```
