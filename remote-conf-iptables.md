
#### Jeżeli konfigurujemy iptables na odległej maszynie, warto zrobić to tak aby nie stracić z nią połączenia.
``` bash
iptables-apply [ -w "scieżka dla bezpiecznej konfiguracji" ] -c "ścieżka do skryptu wprowadzającego zmiany"
```
Po wykonaniu polecenia program sprawdzi poprawność składni
następnie wykona skrypt i poprosi użytkownika o zatwierdzenie.
``` bash
root@host:/home/darek# iptables-apply -c "/home/darek/zmiany"
Running command '/home/darek/zmiany'... done.
Can you establish NEW connections to the machine? (y/N) y
... then my job is done. See you next time.
```
Jeśli konfigurowana maszyna nie otrzyma potwierdzenie przywróci wcześniejszą konfiguracje. 
