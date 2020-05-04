## Tarte Tatin

On commence par la commande `file` pour nous donner plus de détails sur l'exécutable.

`file TarteTatin`

```
TarteTatin: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=847e4e7a7a412e8815994c665c5208c71b0f1836, not stripped
```

Le `strings TarteTatin` ne nous donne pas d'informations utiles.

`chmod +x TarteTatin` pour pouvoir l'exécuter puis un `./TarteTatin` pour le lancer, on entre n'importe quoi, le programme se ferme.

On ouvre le fichier avec `radare2` en mode écriture:

`radare2 -w TarteTatin`

```
[0x00000670]> aa
[x] Analyze all flags starting with sym. and entry0 (aa)
[0x00000670]> fs symbols
[0x00000670]> f
0x000005f8 23 sym._init
0x00000670 42 entry0
0x00000670 43 sym._start
0x000007a4 145 main
```

On voit la fonction `main`

`[0x00000670]> pdf@main`

```
0x000007f9      751f           jne 0x81a
```

Cette ligne est intéressante, puisque c'est le seul jump de la fonction.

On teste alors de changer l'instruction `jne` par un `je` (l'inverse) pour effectuer un jump à l'adresse `0x0000081a`

```
0x00000670]> wa je 0x0000081a @0x000007f9
Written 2 byte(s) (je 0x0000081a) = wx 741f
[0x00000670]> exit
```

On exécute simplement le fichier avec un mot de passe random

```
echo 123 | ./TarteTatin
Well done! The flag is: FCSC{83f41431c111xxxxx}
```
