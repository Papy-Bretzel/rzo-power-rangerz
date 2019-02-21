# TP Sécurité - Audit et pentest
__git repo :__ [Megalol](https://github.com/Papy-Bretzel/rzo-power-rangerz/)

## Etape 1 - Challenge
> Quel merveilleux métier pourriez-vous exercer à l'issue de votre cursus ingésup ? (Indice: bestial)

### Périmètre
* `www.ynov.com`
* `extranet.ynov.com` (nécessite authentification)

## Etape 2 - Audit
### Audit - Metasploit (Linux)
#### Présentation

Metasploit est une suite d'outils permettant d'automatiser différents types d'attaques. `Metasploitable` est une distribution linux où un grand nombre de vulnérabilités ont été volontairement introduites dans le but de permettre à des étudiants de se former sur les différentes attaques et moyens de s'en défendre.

Dans notre cas, la machine `metasploitable` est accessible à l'adresse `192.168.1.107`.

#### Démarche
* On lance un nmap pour repérer les services en écoute :
    ![img](./metasploitable-nmap.png)
* On repère que les services Telnet et FTP, entre autres, sont en écoute

#### Failles et Exploits
##### Telnet
* On tente de se connecter en Telnet sur `192.168.1.107:23`
* Les identifiants par défaut de `metasploitable` nous sont gentiment affichés. On se connecte avec ces credentials.
* On regade si l'utilisateur a des droits `sudo`, et on remarque qu'il peut passer n'importe quelle commande sous l'identité de `root`.
* On prend l'identité de root grâce à la commande `su`
Screen shot :
![img](./metasploit-telnet.png)

##### VSFTPD
`VSFTPD` est un serveur FTP utilisant le port 21. La version disponible sur la distribution `metasploitable` comporte une backdoor directement dans le code source. En effet, celle-ci permet une connexion à partir du moment où le login finit la chaîne `:)` (un smiley).
Cette fonctionnalité est documentée sur le site de Metasploit :


> On port 21, Metasploitable2 runs vsftpd, a popular FTP server. This particular version contains a backdoor that was slipped into the source code by an unknown intruder. The backdoor was quickly identified and removed, but not before quite a few people downloaded it. If a username is sent that ends in the sequence :) [ a happy face ], the backdoored version will open a listening shell on port 6200. We can demonstrate this with telnet or use the Metasploit Framework module to automatically exploit it.



### Audit - Windows (XP)
Ou la version la plus trouée de tous les temps, indétrônée depuis la fin de son support en avril 2014.


### Audit - Web service (HTTP 1.1)


##### SSH - command : hydra
hydra -l ynov -P /usr/share/wordlists/metasploit/unix_passwords.txt -t 8 ssh://192.168.1.1

    -Login trouver : ynov\123456
