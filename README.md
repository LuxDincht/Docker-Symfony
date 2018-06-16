# Stack Développement Symfony

**Kézako ?**  
Une stack Docker pour développer sur du Symfony avec :

- Nginx
- PHP (7-fpm) -- Custom pour Symfony
- MariaDB

Des outils :

- Composer (Utilisation Docker)

## Utilisation

### Paramétrage de la Stack

Il va falloir fournir les informations que vous souhaitez donner a votre projet.  
Pour cela, saisir les informations pour votre base de données et votre serveur Nginx dans les fichier `.env` et `nginx.template`.

#### Base de données

*Les variables d'environnement peuvent différer si vous changer la base de données (dans notre cas, on utilise MariaDB).*

- **MYSQL_ROOT_PASSWORD** : Mot de passe root de la base de données.
- **MYSQL_DATABASE** : Nom de la base de donnée pour votre projet.
- **MYSQL_USER** : Nom de l'utilisateur pour votre base.
- **MYSQL_PASSWORD** : Mot de passe de l'utilisateur de votre base.

#### Nginx

- **NGINX_HOST** : Nom de domaine pour accéder à votre application.
- **NGINX_PORT** : Le port d'accès à votre application (dans Nginx, hors remapping des ports par Docker).

*Pour le moment, il faut également redonner ces informations dans le fichier `nginx.template`.*  
**TODO** : Faire en sorte que l'on puisse remplir des variables dans ce fichier à partir des variables d'environnement de `.env`
lors de la construction des containers Docker.

```nginx.template
    listen       [NGINX_PORT];
    server_name  [NGINX_HOST];
```

Exemple :
```nginx.template
    listen       80;
    server_name  dev.local;
```

#### Docker

- **REPO_DOCKER** : Nom de votre compte Docker (correspondant à votre répertoire).

### Symfony

Afin de créer votre projet, nous allons utiliser `composer` avec Docker !

Comment faire ?

1. Ouvrir un Terminal avec votre IDE favori (PhpStorm).
2. Placez vous dans le dossier `docker`.
3. Exécuter la commande `docker-compose -f cmd.yml run --rm composer create-project symfony/website-skeleton .`

Après le chargement et la configuration de votre projet, vous devriez avoir un nouveau dossier `web` au même niveau que `docker`.  
C'est dans ce dossier `web` que se trouve votre projet Web (Symfony) qui est pour le moment un squelette Symfony.

#### Composer

Vous pouvez donc continuer a utiliser `composer` avec cette ligne de commande et en y mettant les paramètres que vous voulez :
`docker-compose -f cmd.yml run --rm composer [VOS PARAMETRES]`

Pour faciliter l'utilisation, vous pouvez ajouter un alias sur votre poste pour appeler cette commande plus simplement.

### Exécution

Une fois le paramétrage fini, vous allez pouvoir lancer les containers Docker qui permettront de faire tourner votre application.

Pour cela rien de plus simple.

1. Toujours dans le Terminal et le dossier `docker`.
3. Lancez la commande `docker-compose up -d`.

*N.B : La première exécution est longue, puisque nous construisons une image PHP Custom pour pouvoir faire du Symfony.*

### Architecture

Une fois la premiere création des containers est faite, vous devriez avoir de nouveaux dossiers 
au même niveau que `docker` et `web`.  

- **db** : Si vous souhaitez remplir votre base de données à sa création, vous pouvez y stocker vos fichers `.sql` et ils seront exécuter à ce moment là.  
Pour plus d'informations, veuillez vous réferer à la [Documentation MariaDB](https://store.docker.com/images/mariadb)
- **logs** : Vos fichiers de log seront ici ! 

### Accès

Pour accéder à votre application, pensez a ajouter votre nom de domaine dans votre fichier `hosts`.  
Exemple : `127.0.0.1   dev.local`

Une fois cela fait, ouvrir son navigateur et saisir l'url `dev.local:8080`.

## Commandes

### PHPUnit

Afin d'exécuter vos Tests Unitaires, il vous suffit comme pour `composer` de lancer la commande :
```bash
docker-compose run --rm php7 bin/phpunit
```

### Symfony

```bash
docker-compose run --rm php7 bin/console [VOS_PARAMETRES]
```