L’interface graphique Touchy
============================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:touchy-gui"></span>

Touchy est une interface utilisateur pour Machinekit, destinée à être utilisée avec les panneaux de commande de machines. Il ne nécessite ni clavier, ni souris.

Il a été conçu pour fonctionner également sur les écrans tactiles, en combinaison avec une manivelle électronique (MPG) et des boutons et interrupteurs.

![](images/touchy_fr.png)

Figure 1. L’interface tactile Touchy

Panneau de Configuration
------------------------

### Connections avec HAL

Touchy requiert qu’un fichier nommé *touchy.hal* soit créé, dans le même répertoire de configuration que le fichier ini, pour y connecter ses contrôles. Touchy exécute les commandes de HAL dans ce fichier après qu’il a créé ses propres pins, lesquelles sont disponibles pour connexion.

Touchy dispose de plusieurs pins de sortie destinées à être connectées au contrôleur de mouvement pour gérer les manivelles de jog:

-   touchy.jog.wheel.increment, qui doit être connecté à la pin *axis.N.jog-scale* de chacun des N axes.

-   touchy.jog.wheel.N, qui doit être connecté à *axis.N.jog-enable* pour chacun des N axes.

-   En plus d'être connecté à *touchy.wheel-counts*, le compteur d’impulsions de la manivelle doit aussi être connecté à *axis.N.jog-counts* pour chacun des N axes. Si le composant de HAL *ilowpass* est utilisé pour adoucir les mouvements de jog à la manivelle, il faut l’appliquer uniquement sur *axis.N.jog-counts* et non sur *touchy.wheel-counts*.

#### Contrôles requis

-   Un bouton *Abandon* (contact momentané) connecté sur la HAL pin *touchy.abort*

-   Un bouton de *Départ cycle* (contact momentané) connecté à *touchy.cycle-start*

-   Volant/Manivelle/MPG, connecté à *touchy.wheel-counts* et à la pin de mouvement comme décrit précédemment.

-   Un bouton à bascule simple (contact à deux positions) connecté à *touchy.single-block*

#### Contrôles optionnels

-   Pour le jog continu, un interrupteur à trois positions avec retour au centre (ou deux boutons momentanés) pour chacun des axes concernés, attaché à *touchy.jog.continuous.x.negative* et à *touchy.jog.continuous.x.positive*, etc. pour les autres axes.

-   Si un bouton de genouillère est nécessaire, (pour jogger Z en haut de sa course en grande vitesse), un bouton à contact momentané sera connecté à *touchy.quill-up*.

#### Voyants de panneau optionnels

-   *touchy.jog.active* indique quand les contrôles de jog du panneau sont actifs.

-   *touchy.status-indicator* est allumé en continu quand la machine exécute un G-code et clignote quand la machine est en marche mais en pause, ou en vitesse à zéro.

### Recommandé pour toutes les configurations

-   Un bouton d’Arrêt d’Urgence (A/U) câblé physiquement, dans la chaîne d’arrêt d’urgence.

Réglages
--------

Pour utiliser Touchy, dans la section \[DISPLAY\] du fichier ini de la machine, modifier la ligne de cette manière: DISPLAY = touchy

Quand Touchy démarre pour la première fois, vérifier l’onglet *Préférences*. Dans le cas d’un écran tactile, choisir de cacher le pointeur dans les options, pour obtenir les meilleurs résultats.

La fenêtre d'état est fixée en haut, ajustée par la taille d’une police fixe. La résolution de Gnome peut affecter cela. Si le bas de l'écran est coupé, aller dans le gestionnaire de résolution de Gnome pour revenir au réglage d’origine, à 96 DPI.

### Macros

Touchy peut invoquer les macros avec mots O au travers de l’interface MDI. Pour configurer cette possibilité, dans la section \[TOUCHY\] du fichier ini, ajouter une ou plusieurs lignes avec MACRO. Chacune de ces lignes doit respecter le format suivant:

    MACRO=increment xinc yinc

Dans lequel, *increment* est le nom de la macro, laquelle accepte deux paramètres, nommés ici *xinc* et *yinc*.

Maintenant, placer la macro proprement dite dans un fichier nommé *increment.ngc*. La variable PROGRAM\_PREFIX du fichier ini pourra être utilisée pour identifier le répertoire contenant ce fichier. Il est également possible de le déclarer dans la variable SUBROUTINE\_PATH.

Elle pourrait ressembler à cela:

    O<increment> sub
    G91 G0 X#1 Y#2
    G90
    O<increment> endsub

Noter que le nom du sous-programme, le nom de la macro ainsi que le nom du fichier .ngc doivent correspondre exactement, y compris les minuscules/ majuscules des noms.

Quand la macro est invoquée en pressant le bouton *Macro* dans l’onglet MDI de Touchy, il est possible de saisir des valeurs pour xinc et yinc, lesquelles seront passées à la macro comme étant respectivement **\#1** et **\#2**. Les paramètres laissées vides sont passés comme des valeur **0**.

Si il y a plusieurs macros différentes, presser le bouton *Macro* répétitivement pour les faire défiler.

Dans notre petit exemple, si -1 est entré pour xinc puis que le *Départ cycle* est pressé, un **G0**, sera invoqué, provoquant un déplacement en vitesse rapide, vers la gauche, de une unité machine.

Cette capacité d’utilisation des macros est très utile pour le palpage de contours ou d’orifices ainsi que pour d’autres opérations simples pré-configurées, de fraisage ou de perçage, qui pourront être réalisées depuis le panneau de Touchy sans avoir, pour cela, à écrire de programme G-code.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


