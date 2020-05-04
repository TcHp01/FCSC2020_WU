## NES Forever

Lien: http://challenges2.france-cybersecurity-challenge.fr:5000/

Challenge où il suffit de lire le code source de la page, un petit `curl` suivit d'un `grep` suffit.

```
$ curl http://challenges2.france-cybersecurity-challenge.fr:5000/ | grep FCSC
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  6995  100  699  FCSC{a1cec1710b5a2423ae927a12xxxxxxx}
5    0     0  29765      0 --:--:-- --:--:-- --:--:-- 30021
```
