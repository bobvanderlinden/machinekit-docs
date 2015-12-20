Mises à jour de Machinekit
==========================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

Mise à jour de 2.4.x vers 2.5.x
-------------------------------

La version 2.5.0 de Machinekit change de nom, elle passe de *EMC2* à *Machinekit*. Tous les programmes dont les noms contenaient *emc* ont été renommés pour contenir *machinekit*. Toute la documentation à été mise à jour.

De plus, le nom du paquet debian contenant le logiciel a changé. Malheureusement les mises à jour automatiques sont cassées. Pour mettre à jour depuis emc2 2.4.X vers machinekit 2.5.X, suivez ces méthodes:

### Sous Debian Lucid 10.04

Déclarer d’abord où se trouve le nouveau logiciel Machinekit, pour cela:

-   Dans le menu, cliquer sur *Système → Administration → Sources de logiciels*.

-   Sélectionner l’onglet *Autres logiciels*.

-   Sélectionner la ligne indiquant *http://www.machinekit.org/emc2 lucid base emc2.4* ou *http://www.machinekit.org/emc2 lucid base emc2.4-sim* puis cliquer le bouton *Éditer*.

-   Dans le champ *URI*, remplacer la ligne courante par *http://www.machinekit.org*

-   Dans le champ *Composants*, modifier *emc2.4* par *machinekit2.5*, ou *emc2.4-sim* par *machinekit2.5-sim*.

-   Cliquer enfin sur le bouton *Valider*.

-   De retour dans la fenêtre des sources de logiciels, onglet *Autres logiciels*, cliquer sur le bouton *Fermer*.

-   Une fenêtre surgissante informe alors que *Les informations sur les logiciels disponibles sont obsolètes*. Cliquer le bouton *Actualiser*.

-   Suivre la même procédure pour le *Code source*.

Maintenant l’ordinateur sait où trouver le nouveau logiciel, ensuite il faudra lui demander de l’installer:

-   Dans le menu, cliquer sur *Système → Administration → Gestionnaire de paquets Synaptic*

-   Dans la barre de recherche rapide, en haut, taper *machinekit*.

-   Cocher la case pour valider l’installation du nouveau paquet machinekit.

-   Cliquer sur le bouton *Appliquer* et laisser le paquet s’installer.

-   L’ancien paquet emc 2.4 sera automatiquement supprimé pour laisser place au nouveau paquet Machinekit 2.5.

### Sous Debian Hardy 8.04

Déclarer d’abord où se trouve le nouveau logiciel Machinekit, pour cela:

-   Dans le menu, cliquer sur *Système → Administration → Gestionnaire de paquets Synaptic*

-   Aller dans le menu *Configuration → Dépôts*.

-   Sélectionner l’onglet *Logiciels tiers*.

-   Sélectionner la ligne indiquant *http://machinekit.org/hardy hardy emc2.4* ou *http://machinekit.org/hardy hardy emc2.4-sim* et cliquer sur le bouton *Éditer*.

-   Dans le champs *Composants*, modifier *emc2.4* par *machinekit2.5* ou *emc2.4-sim* par *machinekit2.5-sim*.

-   Cliquer sur le bouton *Valider*.

-   De retour dans la fenêtre des sources de logiciels, cliquer sur le bouton *Fermer*.

-   De retour dans la fenêtre de Synaptic, cliquer sur le bouton *Actualiser*.

Maintenant l’ordinateur sait où trouver le nouveau logiciel, ensuite il faudra lui demander de l’installer:

-   Dans le menu, cliquer sur *Système → Administration → Gestionnaire de paquets Synaptic*, cliquer sur le bouton *Rechercher*.

-   Dans le champ du dialogue de recherche qui s’ouvre, taper *machinekit* puis cliquer sur le bouton *Rechercher*.

-   Cocher la case pour valider l’installation du paquet *machinekit*.

-   Cliquer sur le bouton *Appliquer* et laisser le nouveau paquet s’installer.

-   L’ancien paquet emc 2.4 sera automatiquement supprimé pour laisser place au nouveau paquet Machinekit 2.5.

Changement de configuration
---------------------------

Les configurations utilisateur ont migré de $HOME/emc2 vers $HOME/machinekit, il sera nécessaire de renommer l’ancien répertoire si il existe, ou de déplacer les fichiers vers ce nouvel endroit. Le watchdog de hostmod2 dans Machinekit 2.5 ne démarre qu’après que les threads de HAL soient eux-même démarrés. Cela signifie qu’il tolère désormais un délai d’attente de l’ordre de la période servo thread, au lieu de nécessiter un délai qui soit de l’ordre du temps entre le chargement du pilote et le démarrage des threads de HAL. Ce qui signifie, de l’ordre de quelques millisecondes (quelques périodes du thread servo) au lieu de plusieurs centaines de millisecondes préalablement. La valeur par défaut est descendue de 1 seconde à 5 millisecondes. Vous ne devriez donc plus avoir a ajuster le délai du watchdog, à moins que vous ne modifiez la période du threads servo.

Les anciens pilotes pour les cartes Mesa 5i20, hal\_m5i20, ont été enlevés, ils étaient obsolètes et remplacés par hostmot2 depuis 2009 (version 2.3.) Si vous utilisiez ces pilotes, vous devrez reconstruire une nouvelle configuration utilisant le pilote hostmod2. Pncconf peux vous y aider et il contient quelques exemples de configurations (hm2-servo et hm2-stepper) qui vous serviront d’exemple.

Mise à jour 2.3.x à 2.4.x
-------------------------

Les instructions suivantes s’appliquent à Debian 8.04 "Hardy Heron". Machinekit 2.4 n’est pas disponible pour des versions plus anciennes de Debian.

En raison de plusieurs incompatibilités mineures entre les versions 2.3.5 et 2.4.x, votre installation existante ne sera pas automatiquement mise à jour vers la version 2.4.x. Si vous voulez exécuter la version 2.4.x, veuillez modifier le dépôt Machinekit-2.4 en suivant ces instructions :

Lancez Système/Administration/Gestionnaire de paquets Synaptic Sélectionnez Configuration/Dépôt Dans l’onglet "Autres logiciels" vous trouverez deux lignes concernant machinekit.org.

Pour chacune:

-   Sélectionnez la ligne et cliquez sur le bouton "Editer"

-   Dans le champ composants remplacez emc2.3 par emc2.4

-   Cliquez sur "Valider"

-   Fermer la fenêtre "Source de logiciels" avec le bouton fermer

-   Cliquez sur "Recharger" dans la barre d’outils

-   Cliquez sur "Tout mettre à jour" dans la barre d’outils

Utilisateurs de carte Mesa et hostmot2 :

Si vous utilisez une carte *mesa*, trouvez le paquet hostmot2-firmware approprié à votre carte et marquez-le pour l’installation. Astuce: faites une recherche "hostmot2-firmware" dans gestionnaire de paquet Synaptic.

-   Cliquez "Appliquer"

Changements entre 2.3.x et 2.4.x
--------------------------------

Une fois que vous avez fait la mise à jour, mettez à jour les configurations personnalisées en suivant ces instructions

### Changement emc.nml (2.3.x to 2.4.x)

Pour les configurations qui ont personnalisé emc.nml, enlevez la ligne `NML_FILE = emc.nml` dans le fichier inifile. Cela forcera l’utilisation de la version la plus à jour de emc.nml.

Pour les configurations qui n’ont pas personnalisé emc.nml, un changement similaire est requis.

Un échec de cette opération provoque une erreur comme:

    libnml/buffer/physmem.cc 143: PHYSMEM_HANDLE:
    Can't write 10748 bytes at offset 60 from buffer of size 10208.

### Changements de la table d’outils (2.3.x to 2.4.x)

Le nouveau format de la table d’outil est incompatible. La documentation explique le nouveau format. La table d’outil sera automatiquement convertie dans le nouveau format.

### Images du micro logiciel hostmot2 (2.3.x to 2.4.x)

Les images du micro logiciel hostmot2 sont dorénavent dans un paquet séparé. Vous pouvez :

-   Continuer à utiliser un paquet déjà installé `emc2-firmware-mesa-*`

-   Installer les nouveaux paquets du gestionnaire de paquets Synaptic. Les nouveaux paquets sont nommés `hostmot2-firmware-*`

-   Télécharger les fichiers .tar des images du microprogramme depuis <http://emergent.unpy.net/01267622561> et les installer manuellement.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


