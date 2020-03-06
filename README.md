# Cloner les dépôts
Le dépôt principal se trouve sur `https://github.com/LucasOMS/interface-repartie` et pointe vers chaque module. Vous pouvez néanmoins cloner chaque module indépendamment :

Projet complet : `git clone https://github.com/LucasOMS/interface-repartie InterfaceRepartieEnqueteStade; cd InterfaceRepartieEnqueteStade`

Serveur : `git clone https://github.com/LucasOMS/interface-repartie-server-side.git Serveur; cd Serveur`

Table : `git clone https://github.com/LucasOMS/interface-repartie-client-side-table.git Table; cd Table`

Casque VR : `git clone https://github.com/LucasOMS/interface-repartie-client-side-vr.git Casque; cd Casque`

Tablette : `git clone https://github.com/LucasOMS/interface-repartie-client-side-tablette.git Tablette; cd Tablette`


# Déploiement du projet
Nous considèrerons la racine du dossier `InterfaceRepartieEnqueteStade` comme la base de tous les chemins de fichiers. _(voir Cloner les dépôts > Projet complet)_

```cd InterfaceRepartieEnqueteStade```

## Serveur
Pour déployer le serveur, rendez vous dans le dossier server-side.

```cd server-side```

### Dépendances

Afin d'être déployé, le serveur nécessite une version de node installée sur le PC.

Installez les dépendances avec la commande ```npm install```

### Lancer le serveur

Pour démarrer le serveur une fois les dépendances installées, vous pouvez utiliser la commande ```npm start```

_Note : pour lancer le serveur en mode développement (redémarrage du serveur à chaque modification) vous pouvez utiliser la commande ```npm run start:dev```_

Le serveur est accessible à l'adresse `http://localhost:4444`, les sockets peuvent se connecter au serveur à l'adresse `http://localhost:10000` 

### Récupérer l'addresse distante du serveur
Afin d'accéder au serveur à distance (avec le casque, la table ou la tablette) il est nécessaire d'avoir son adresse sur le réseau local. Pour cela, utilisez la commande (bash)

```ifconfig | sed -En 's/127.0.0.1//;s/.*inet (addr:)?(([0-9]*\.){3}[0-9]*).*/\2/p'```

Vous obtenez alors l'adresse locale de chaque carte réseau de votre ordinateur. Vous devez utiliser l'adresse de la carte qui vous connecte au réseau _(carte wifi si vous êtes en wifi par exemple)_

## Table

Rendez vous dans le code source de l'interface de la table avec ```cd table```

### Dépendances
L'interface de la table nécessite une version de node installé sur le PC.

Installez les dépendances avec la commande ```npm install```
#### Sur la table
Sur la table, il est necessaire de lancer le logiciel TUIOTable
#### Sur un ordinateur
Sur ordinateur, vous pouvez interagir avec un simulateur, pour cela, il faut lancer TUIOClient qui permet d'activer le pont entre le simulateur et l'interface (via des sockets sur le port 9000).
Pour lancer TUIOClient, vous pouvez utiliser le script suivant :
```
git clone https://github.com/AtelierIHMTable/TUIOClient.git TUIOClient
cd TUIOClient
npm install
npm run develop
```

Vous pouvez alors utiliser le simulateur pour interagir, celui-ci a besoin d'une JRE pour fonctionner.

Vous pouvez télécharger TUIO Simulator sur `http://prdownloads.sourceforge.net/reactivision/TUIO_Simulator-1.4.zip?download`.
Vous pouvez ensuite extraire l'archive et lancer TUIOSimulator.jar en double cliquant dessus (si le programme associé aux .jar par défaut est java). 

### Configuration
#### Définir la connexion au serveur
Afin que la table se connecte à une adresse distante ou à une adresse locale, rendez vous dans le fichier ``table/src/utils/constants.js``.

· Pour un déploiement avec un serveur en local, assurez-vous que le `currentProfile` soit égal à `NETWORK_PROFILES.LOCAL` (ligne 94)

· Pour un déploiement avec un serveur distant, changez l'adresse à la ligne 95 par la votre, et changez `currentProfile` pour qu'il soit égal à `NETWORK_PROFILES.PROD` (ligne 94 et 95) 
 
#### Profil de code
Le code permet d'utiliser un profil de développement qui permet d'afficher les interactions tangibles et tactiles sur l'interface, notamment utile lors de l'utilisation du simulateur. Pour l'activer rendez vous dans le fichier ``table/src/index.js`` à la ligne 46 et utilisez PROFILES.LOCAL.

Pour désactiver l'affichage des interactions, utilisez le profil `PROFILES.PROD` à la place.

### Démarrer l'interface
Cloner le code source de l'interface de la table
```
git clone https://github.com/LucasOMS/interface-repartie-client-side-table.git TableEnqueteAuStade
cd TableEnqueteAuStade
```

Afin que l'interaction sur la table foncitonne correctement, il faut que l'interface soit en plein écran. Avec Google Chrome ou Firefox, vous pouvez activer/désactiver le mode plein écran avec la touche F11.

#### Sur la table
Si le serveur est également hébergé sur la table, vous pouvez utiliser `NETWORK_PROFILE.LOCAL`, sinon il faut changer pour `NETWORK_PROFILE.PROD` et définir l'adresse du serveur. _(voir Configuration > Définir la connexion au serveur)_ 

vous pouvez démarrer l'interface sur la table avec le script suivant
```
npm install
npm start
```

L'interface est disponible à l'adresse `http://localhost:3000`

#### Sur la table hébergée sur un ordinateur
Afin d'accéder à l'interface hebergée sur un ordinateur depuis la table, il faut utiliser le profil NETWORK_PROFILE.PROD et définir l'adresse du serveur. _(voir Configuration > Définir la connexion au serveur)_

Pour permettre à des connexions distantes d'accéder à l'interface, il faut changer les configs de webpack. Rendez vous dans le fichier `src/webpack.common.js`, remplacez la ligne 42 par `host: 'votreIp'`.

Vous pouvez désormais lancer l'application avec les lignes de commandes suivantes :

```
npm install
npm start
```
L'interface est disponible à l'adresse `http://votreIp:3000`

#### Sur un ordinateur
Si le serveur est également hébergé sur l'ordinateur, vous pouvez utiliser `NETWORK_PROFILE.LOCAL`, sinon il faut changer pour `NETWORK_PROFILE.PROD` et définir l'adresse du serveur. _(voir Configuration > Définir la connexion au serveur)_ 

Vous pouvez lancer l'interface avec les lignes de commandes suivantes :

```
npm install
npm start
```
L'interface est disponible à l'adresse `http://localhost:3000`

Vous aurez besoin de TUIOSimulator pour interagir avec l'interface. 

Afin d'avoir la bonne résolution sur l'interface pour que celle-ci soit utilisable, vous pouvez utiliser les outils de développement (F12) puis utiliser Ctrl + maj + M afin d'activer la vue responsive. La table a une résolution de 1920×1080 que vous pouvez définir dans la barre supérieure de l'interface de développement.

## Casque VR
Rendez vous dans le code source de l'interface du casque avec ```cd vr```

## Tablette
Rendez vous dans le code source de l'interface de la tablette avec ```cd tablette```

### Configurer l'adresse IP du serveur

Aller au fichier `lib/router.dart`.
Modifier la ligne 110 pour y écrire l'adresse du serveur.

### Lancer l'application sur simulateur sur macOS

Lancer les commandes 

```
open -a Simulator
flutter run
```

L'accès aux capteurs dépends des simulateurs et peut ne pas etre implémenté.

### Lancer l'application sur un autre dispolitif

Le lancement d'une application flutter dépends de l'OS de l'ordinateur, du choix d'utiliser un simulateur ou non, et de l'OS de la tablette cible.
Il est recommandé de lancer l'application sur des dispositifs iOS qui garantissent un accès aux capteurs utilisés.
Voir la documentation de flutter pour le lancement d'applications selon l'ordinateur et le dispositif cible.
https://flutter.dev/docs/get-started/install

