Petite FAQ Linux
================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:FAQ-Linux"></span>

Voici quelques commandes et techniques de base pour l’utilisateur débutant sous Linux. Beaucoup d’autres informations peuvent être trouvées sur [le site web de Machinekit](http://www.machinekit.org/) ou dans les man pages.

Login automatique
-----------------

Quand vous installez Machinekit avec le CD-Live Debian, par défaut vous devez passer par la fenêtre de connexion à chaque démarrage du PC. Pour activer le login automatique ouvrez le menu *Système → Administration → Fenêtre de connexion*. Si l’installation est récente la fenêtre de connexion peut prendre quelques secondes pour s’ouvrir. Vous devez entrer le mot de passe utilisé pour l’installation pour accéder à la fenêtre des préférences. Ouvrez alors l’onglet *Sécurité*, cochez la case *Activer les connexions automatiques* et saisissez votre nom d’utilisateur ou choisissez en un dans la liste déroulante. Vous êtes maintenant dispensé de la fenêtre de connexion.

Les Man Pages<span id="sec:Man-Pages"></span>
---------------------------------------------

Les Man pages sont des pages de manuel générées automatiquement le plus souvent. Les Man pages existent pour quasiment tous les programmes et les commandes de Linux.

Pour visualiser une man page ouvrez un terminal depuis *Applications → Accessoires → Terminal*. Par exemple si vous voulez trouver quelques choses concernant la commande *find*, tapez alors dans le terminal:

    man find

Utilisez les touches *Vers le haut* et *Vers le bas* pour faire défiler le texte et la touche **Q** pour quitter.

Lister les modules du noyau
---------------------------

En cas de problème il est parfois utile de connaître la liste des modules du noyau qui sont chargés. Ouvrez une console et tapez:

    lsmod

Si vous voulez, pour le consulter tranquillement, envoyer le résultat de la commande dans un fichier, tapez la sous cette forme:

    lsmod > mes_modules.txt

Le fichier mes\_modules.txt résultant, se trouvera alors dans votre répertoire home si c’est de là que vous avez ouvert la console.

Éditer un fichier en root<span id="sec:Editer-un-fichier-en-root"></span>
-------------------------------------------------------------------------

Éditer certains fichiers du système en root peut donner des résultats inattendus! Soyez très vigilant quand vous éditez en root, une erreur peut compromettre tout le système et l’empêcher de redémarrer. Vous pouvez ouvrir et lire de nombreux fichiers systèmes appartenant au root qui sont en mode lecture seule.

### A la ligne de commande

Ouvrir un terminal depuis *Applications → Accessoires → Terminal*.

Dans ce terminal, tapez:

    sudo gedit

Ouvrez un fichier depuis *Fichiers → Ouvrir* puis éditez le.

### En mode graphique

1.  Faites un clic droit sur le bureau et choisissez *Créer un lanceur..*.

2.  Tapez un nom tel que *éditeur*, dans la zone *Nom*.

3.  Entrez *gksudo "gnome-open %u"* dans la zone *Commande* et validez.

4.  Glissez un fichier et déposez le sur votre lanceur, il s’ouvrira alors directement dans l'éditeur.

Commandes du terminal<span id="sec:Commandes-Terminal"></span>
--------------------------------------------------------------

### Répertoire de travail

Pour afficher le chemin du répertoire courant dans le terminal tapez:

    pwd

### Changer de répertoire

Pour remonter dans le répertoire précédent, tapez dans le terminal:

    cd ..

Pour remonter de deux niveaux de répertoire, tapez dans le terminal:

    cd ../..

Pour aller directement dans le sous-répertoire machinekit/configs tapez:

    cd machinekit/configs

### Lister les fichiers du répertoire courant

Pour voir le contenu du répertoire courant tapez:

    ls

### Trouver un fichier

La commande *find* peut être un peu déroutante pour le nouvel utilisateur de Linux. La syntaxe de base est:

    find répertoire_de_départ <paramètres> <actions>

Par exemple, pour trouver tous les fichiers .ini dans votre répertoire machinekit utilisez d’abord la commande *pwd* pour trouver le répertoire courant. Ouvrez un nouveau terminal et tapez:

    pwd

il vous est retourné par exemple le résultat suivant:

    /home/robert

Avec cette information vous pouvez taper, par exemple, la commande:

    find /home/robert/machinekit -name *.ini -print

Le *-name* est le nom du fichier que vous recherchez et le *-print* indique à find d’afficher le résultat dans le terminal. Le *\*.ini* indique à find de retourner tous les fichiers portant l’extension *.ini*

### Rechercher un texte

Tapez dans un terminal:

    grep -lir "texte à rechercher" *

Pour trouver tous les fichiers contenant le texte *"texte à rechercher"* dans le répertoire courant, tous ses sous-répertoires et en ignorant la casse. Le paramètre **-l** demande de ne pas afficher les résultats normalement mais à la place, d’indiquer le nom des fichiers pour lesquels des résultats auraient été affichés. Le paramètre **-i** demande d’ignorer la casse. Le paramètre **-r** demande une recherche récursive (qui inclus tous les sous-répertoires dans la recherche). Le caractère **\*** est un jocker indiquant *tous les fichiers*.

### Messages du boot

Pour visualiser les messages du boot utilisez la commande *dmesg* depuis un terminal. Pour enregistrer ces messages dans un fichier, redirigez les avec:

    dmesg > dmesg.txt

Le contenu de ce fichier pourra alors être copié et collé à destination des personnes en ligne qui vous aideront à diagnostiquer votre problème.

Pour nettoyer le tampon des messages tapez cette commande:

    sudo dmesg -c

C’est utile avant de lancer Machinekit, pour que ne soit enregistrés que les messages relatifs au fonctionnement courant de Machinekit.

Pour trouver les adresses des ports parallèles de la machine, tapez cette commande grep pour filtrer les informations contenues dans dmesg.

    dmesg|grep parport

Problèmes matériels
-------------------

### Informations sur le matériel

Pour voir la liste du matériel installé sur les ports PCI de votre carte mère, tapez la commande suivante dans un terminal:

    lspci -v

Pour voir la liste du matériel installé sur les ports USB de votre carte mère, tapez la commande suivante dans un terminal:

    lsusb -v

### Résolution du moniteur

Lors de l’installation d’Debian les réglages du moniteur sont automatiquement détectés. Il peut arriver que la détection fonctionne mal et que la résolution ne soit que celle d’un moniteur générique en 800x600.

Pour résoudre ce cas, suivez les instructions données ici:

<https://help.ubuntu.com/community/FixVideoResolutionHowto>

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


