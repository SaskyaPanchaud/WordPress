# Stage WordPress@EPFL

## Première partie 

WordPress est un CMS* (Content Management System) gratuit (hormis certains thèmes ou plug-ins (= features spécifiques)), libre et open-source. Il a été créé par Michel Valdrighi et Matt Mullenweg en 2003.

43,2 % des sites web dans le monde utilisent Wordpress. Les autres CMS les plus utilisés sont Wix, Joomla et Drupal.

Différences entre *WordPress.com* et *WordPress.org* :

- com = COMMENT : fournit un hébérgement pour l'application (x-AMP) -> p. ex : Infomaniak
- org = QUOI : fournit le code source de l'application (maintien et mises à jour par la communauté)

*Un CMS est un logiciel qui aide les utilisateurs à créer, gérer et modifier le contenu d'un site web sans avoir besoin de connaissances techniques (possible sans codage).

## Deuxième partie

### Outils utilisés par WordPress

- PHP = Hypertext Preprocessor : langage back-end utilisant HTML et SQL (langage de l'application WordPress)
- Apache : serveur web (écoute les requêtes émises par les navigateurs, cherche la page demandée et la renvoie)
- MySQL : serveur de base de données relationnelles

### Procédure d'installation

J'ai suivi cette [procédure](https://ubuntu.com/tutorials/install-and-configure-wordpress#1-overview) pour installer WordPress.

Résumé de la procédure :
1) installation des dépendances (Apache et PHP)
2) création d'un répertoire */srv/www* (srv = services pour héberger les fichiers des sites web et www = protocole (https ou http), changement des droits sur ce répertoire pour autoriser l'écriture par *www-data* (=user par défaut) et téléchargement de WordPress dans ce répertoire en utilisant `curl`
3) création du site sur Apache, activation du site et mise à jour du serveur (voir */etc/apache2/sites-available/wordpress.conf*) :
   ```
   <VirtualHost *:80>
     DocumentRoot /srv/www/wordpress
     <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
   </VirtualHost>
   ```
4) création de la base de données et d'un utilisateur via `sudo mysql -u root` (user = saskya et mdp = wordpress) et activation de la base de données
6) configuration de WordPress pour utiliser la base de données dans */srv/www/wordpress/wp-config.php*
7) installation de WordPress et accessible [ici](http://localhost/wp-admin/index.php) avec les identifiants suivants : saskya / pr1s7C0h_o$e

## Troisième partie

### Procédure d’installation de WordPress sur une VM distante

**TODO**

# Remarques

## Aspects théoriques

- L / M / W / X -AMP : Linux / Mac / Windows / Cross-Plateform - Apache, MySQL et PHP : ce sont différents types de *Web application stack* (compilation de logiciels utilisés ensemble pour créer des sites / applications web). Les fondements de cette pile sont un système d'opération, un serveur web, une base de données et un interpréteur de scripts. Pour plus d'informations, aller [ici](https://www.geeksforgeeks.org/what-is-the-difference-between-lamp-stack-mamp-stack-and-wamp-stack/).
- Base de données non-relationnelle est constituée de fichiers plats (= fichiers au format texte tels que JSON ou XML) et base de données relationnelle est constituée de tables et est accessible avec langage SQL.
  | Base de données | Relationnelle | Non-relationnelle |
  |:---------------:|:-------------:|:-----------------:|
  | MariaDB         | x             |                   |
  | MongoDB         |               | x                 |
  | MSSQL           | x             |                   |
  | MySQL           | x             |                   |
  | Oracle          | x             |                   |
  | PostgreSQL      | x             |                   |
- WordPress :
  - post = blog (date, auteur et commentaires) et chronologie -> utilisé pour actualités
  - page = site "statique" et mise en pages différente -> majorité des sites
- Site web créé peut être accessible par tout le monde si 1) machine hébergeant le site est accessible (ip publique, même réseau, ...) et 2) si site indexé par un moteur de recherche ou si on connait l'URL
- WordPress chiffre les mots de passe en MD5 mais MySQL accepte d'autres méthodes

## Aspects pratiques
 
### Réinitialiser un mot de passe via la base de données

Si mot de passe simple (p. ex. : f71dbe52628a3f83a77ab494817525c6) alors facilement craquable [depuis là](https://10015.io/tools/md5-encrypt-decrypt) et si mot de passe complexe (p. ex. : $P$B21fpGTnSjxOr.PdpKNG1Gzj/iq0KP1), impossible de le retrouver.

1) Ouvrir un terminal, taper `mysql -u wordpress -p` et entrer le mot de passe de la base de données (*wordpress*).
2) `SHOW TABLES FROM wordpress;` pour accéder à toutes les tables de la base de données *wordpress*.
3) `SELECT * FROM wordpress.wp_users;`pour accéder aux utilisateurs.
4) `UPDATE wordpress.wp_users SET user_pass = MD5('pr1s7C0h_o$e') WHERE ID = 1;` pour modifier le mot de passe.
5) Se connecter à [WordPress](http://localhost/wp-login.php) avec les nouveaux identifiants -> saskya et pr1s7C0h_o$e.

### Utiliser l'interface web de phpmyadmin

Suivre cette [documentation](https://ubuntu.com/server/docs/how-to-install-and-configure-phpmyadmin) et se connecter avec wordpress + wordpress [ici](http://localhost/phpmyadmin/index.php).

# Questions

Données dans mysql stockées où (chemin sur mon ordi = ?) ?
