## SuSHi

###### Données:

```
Adresse : 		challenges2.france-cybersecurity-challenge.fr
Port : 			6000
Utilisateur : 	ctf
Mot de passe : 	ctf
```

Il suffit simplement de se connecter en `ssh`.

Pour cela:

`ssh ctf@challenges2.france-cybersecurity-challenge.fr -p 6000`

`ssh` : binaire permettant d'établir la connexion

`ctf` : nom de l'utilisateur de la machine

`challenges2.france-cybersecurity-challenge.fr` : serveur distant

`-p 6000` : spécifie le port utilisé (par default 22 est utilisé pour le `ssh`)

Lors de la première connexion à l'host, vous devriez voir apparaitre une question:

`Are you sure you want to continue connecting (yes/no/[fingerprint])?`

Il suffit de taper `yes` pour que le mot de passe de l'utilisateur vous soit demandé.

Entrez `ctf` comme mot de passe (les caractères ne sont pas visible pour la sécurité)

et `Entrée` pour se connecter.

 ```
 __    __            _                 __           _     _   ___ 
/ / /\ \ \__ _ _ __ | |_      __ _    / _\_   _ ___| |__ (_) / _ \
\ \/  \/ / _` | '_ \| __|    / _` |   \ \| | | / __| '_ \| | \// /
 \  /\  / (_| | | | | |_    | (_| |   _\ \ |_| \__ \ | | | |   \/ 
  \/  \/ \__,_|_| |_|\__|    \__,_|   \__/\__,_|___/_| |_|_|   () 
ctf@SuSHi:~$ ls -la
total 24
drwxr-xr-x 1 ctf-admin ctf 4096 Apr 25 10:39 .
drwxr-xr-x 1 ctf-admin ctf 4096 Apr 25 10:38 ..
-rw-r--r-- 1 ctf-admin ctf  220 May 15  2017 .bash_logout
-rw-r--r-- 1 ctf-admin ctf 3526 May 15  2017 .bashrc
-r--r--r-- 1 ctf-admin ctf   71 Apr 25 10:38 .flag
-rw-r--r-- 1 ctf-admin ctf  675 May 15  2017 .
ctf@SuSHi:~$ cat .flag 
FCSC{ca10e42620c4e3bxxxxxxxx}
ctf@SuSHi:~$
 ```

`ls -la` permet de LiSter tout les fichiers cachés avec leurs informations.

`cat .flag` sert à afficher le contenu du fichier `.flag` (grâce au point au début d'un fichier sur linux le fichier n'est pas visible directement)

