## Flag Checker

Lien: http://challenges2.france-cybersecurity-challenge.fr:5005/

En regardant sur Chrome dans Outils de Développement > Sources, on y voit un index.wasm

`curl http://challenges2.france-cybersecurity-challenge.fr:5005/index.wasm --output index.wasm`

On récupère le fichier et on regarde ce que la commande "file" nous dit sur lui

`file index.wasm`

```
index.wasm: WebAssembly (wasm) binary module version 0x1 (MVP)
```

`strings index.wasm`

```
memory
kApq"
FE@P@x4f1g7f6ab:42`1g:f:7763133;e0e;03`6661`bee0:33fg732;b6fea44be34g0~
```

La troisième ligne fait la taille d'un flag

Une petite recherche Google nous permet de voir qu'on peut gérer ce fichier avec Chrome.

![image](https://i.imgur.com/MixirnS.png)

Nous retrouvons cette chaine de caractères à la fin du fichier wasm ouvert avec le débogueur Chrome.

Après quelques documentations sur le WebAssembly, une instruction ligne 32 nous indique que le flag doit faire 70 caractères.

`i32.const 70`

On commence donc avec le début de flag `FCSC{}` 
`python -c "print 'a'*64"` -> et on obtient 

`FCSC{aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa}`
comme premier flag.

Après avoir testé plusieurs BreakPoints sur les valeurs `i32.eqz` ou `i32.ne`

Seule `i32.ne` nous intéresse ici, ligne 48, juste au dessus d'un `br_if` (condition)

En mettant un BreakPoint sur cette instruction, on peut voir que l'ASCII de la première lettre de notre flag est comparée avec l'ASCII de la première lettre de la chaine (ici F avec E).

![image](https://i.imgur.com/bEEVkqt.png)

`stack: {0: 69, 1: 69}`

Où `0`est notre input, et `1` la correspondance.

On doit vérifier caractères par caractères dans la boucle.

Après le 6ème tour, nous avons le message d'erreur, c'est normal puisque notre flag ne contient que `FCSC{` de valide, sur le dernier BP, le programme compare `98` et `52`.

Un coup de `python -c "print chr(52)"`  pour voir que c'est `4`

On teste alors `FCSC{4aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa}`

![image](https://i.imgur.com/2yjXmuM.png)

Pourtant ça ne marche pas, `55` n'étant pas le code ASCII de `4` on teste plusieurs chiffres se rapprochant de `4`, arrivé à `7` ça marche.

On fait pareil pour plusieurs caractères du flag jusqu'à voir une similitude entre les caractères.

Déjà pour les deux `C` dans FCSC, ils possèdent le même code dans le programme, `64`.

Après plusieurs essaies, on peut petit à petit construire notre table de correspondance.

| ASCII de comparaison | Correspondance Alphanumérique |
| -------------------- | ----------------------------- |
| 48                   | 3                             |
| 49                   | 2                             |
| 50                   | 1                             |
| 51                   | 0                             |
| 52                   | 7                             |
| 53                   | 6                             |
| 54                   | 5                             |
| 55                   | 4                             |
| 58                   | 9                             |
| 59                   | 8                             |
| 64                   | C                             |
| 69                   | F                             |
| 80                   | S                             |
| 96                   | c                             |
| 97                   | b                             |
| 98                   | a                             |
| 101                  | f                             |
| 102                  | e                             |
| 103                  | d                             |

(à partir d'un seuil d'essaie on commence à déduire avec le nombre restreint de caractères non-utilisés)

L'heure de scripter:

```js
var j = {48: '3', 49: '2', 50: '1', 51: '0', 52: '7', 53: '6', 54: '5', 55: '4',
         58: '9', 59: '8', 64: 'C', 69: 'F', 80: 'S', 96: 'c', 97: 'b', 98: 'a',
         101: 'f', 102: 'e', 103: 'd', 120: '{', 126: '}' };
var t = "E@P@x4f1g7f6ab:42`1g:f:7763133;e0e;03`6661`bee0:33fg732;b6fea44be34g0~";
//TcHp
t.split('').forEach( ( x ) =>
{
	if ( j[ x.toString().charCodeAt() ] ) //if !undefined
	{
		process.stdout.write( j[ x.toString().charCodeAt() ] );
	}
	else
	{
		process.stdout.write( 'x' );
	}
} );
```

(j'ai ajouté au fur et à mesure les éléments dans le json pour aller plus vite)

Et voilà le Flag apparait, nous pouvons valider l'épreuve.

J'ai aussi trouvé plus tard la méthode la plus simple:
```python
t = "E@P@x4f1g7f6ab:42`1g:f:7763133;e0e;03`6661`bee0:33fg732;b6fea44be34g0~"

for x in t:
	print(chr(ord(x)^3), end='')
print()
```
