Machinekit
==========

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:machinekit-user-introduction"></span>

Introduction
------------

Ce document est centré sur l’utilisation de Machinekit, il est plutôt destiné aux lecteurs l’ayant déjà installé et configuré. Quelques informations sur l’installation sont données dans les chapitres suivants. La documentation complète sur l’installation et la configuration se trouve dans le manuel de l’intégrateur.

Comment fonctionne Machinekit
-----------------------------

Machinekit est un peu plus que juste un autre programme de fraiseuse CNC. Il est capable de contrôler des machines-outils, des robots ou d’autres automatismes. Il est capable de contrôler des servomoteurs, des moteurs pas à pas, des relais ainsi que d’autres mécanismes relatifs aux machines-outils.

Il y a quatre principales composantes du logiciel Machinekit:

-   un contrôleur de mouvement (EMCMOT),

-   une contrôleur d’entrées/sorties discrètes (EMCIO),

-   un exécuteur des tâches qui les coordonne (EMCTASK),

-   et les interfaces utilisateur graphiques.

En outre il existe une couche appelée HAL (couche d’abstraction du matériel) qui permet la configuration de Machinekit sans avoir besoin de recompiler.

![](images/whatstep1.png)

Figure 1. Machine simple contrôlée par Machinekit

La figure ci-dessus montre un diagramme bloc représentant une machine 3 axes typique comme Machinekit les aime. Cette figure montre un système basé sur des moteurs pas à pas. Le PC, tournant sous Linux contrôle les interfaces de puissance des moteurs pas à pas en leur envoyant des signaux au travers du port parallèle. Ces signaux (impulsionnels) font que la puissance adéquate est fournie aux moteurs. Machinekit peut également contrôler des servomoteurs via une interface de puissance pour servomoteurs ou utiliser le port parallèle étendu connecté à une carte de contrôle externe. Quand nous examinerons chacun des composants qui forment un système Machinekit, nous nous référerons à cette machine typique.

Interfaces utilisateur graphiques
---------------------------------

L’interface graphique est la partie de Machinekit qui interagit avec l’opérateur de la machine. Machinekit est fourni avec plusieurs interfaces utilisateurs graphiques:

-   [*Axis*](#cha:Axis), l’interface utilisateur standard.

![](images/axis_25_fr.png)

Figure 2. L’interface graphique AXIS<span id="fig:Interface-graphique-AXIS"></span>

-   [*Touchy*](#cha:touchy-gui), une interface graphique pour écran tactile.

![](images/touchy_fr.png)

Figure 3. L’interface graphique Touchy<span id="fig:touchy-gui"></span>

-   [*NGCGUI*](#cha:ngcgui), une interface graphique gérant les sous-programmes. Elle permet très simplement de créer des programme G-code. Elle supporte surtout la concaténation de fichiers de sous-programmes, ce qui permet de construire des programmes G-code complets sans aucune programmation.

![](images/ngcgui_fr.png)

Figure 4. L’interface graphique NGCGUI intégrée dans Axis<span id="fig:ngcgui-gui"></span>

-   [*Mini*](#cha:Mini), une interface en Tcl/Tk.

![](images/mini_fr.png)

Figure 5. L’interface graphique Mini<span id="fig:Interface-graphique-Mini"></span>

-   [*TkMachinekit*](#cha:TkMachinekit), une autre interface basée sur Tcl/Tk. C’est l’interface la plus populaire après Axis

![](images/tkmachinekit_fr.png)

Figure 6. L’interface graphique tkmachinekit<span id="fig:L-interface-graphique-tkmachinekit"></span>

-   [*Keystick*](#cha:keystick-gui), une interface graphique textuelle et minimaliste, appropriée pour les installations légères (sans serveur X par exemple).

![](images/keystick.png)

Figure 7. L’interface textuelle Keystick<span id="fig:L-interface-Keystick"></span>

-   *Xemc*, un programme X-Windows

-   *halui*, une interface utilisateur basée sur HAL, qui permet de contrôler Machinekit en utilisant des boutons et des interrupteurs

-   *machinekitrsh*, une interface utilisateur basée sur telnet, qui permet d’envoyer des commandes à partir d’ordinateurs distants de celui de Machinekit

Panneaux de contrôle virtuels
-----------------------------

-   *PyVCP*, un panneau de contrôle virtuel basé sur Python, il peut être intégré dans l’interface graphique Axis ou utilisé en autonome.

-   *GladeVCP*, un panneau de contrôle virtuel basé sur Glade, il peut être intégré dans l’interface graphique Axis ou utilisé en autonome.

Langues
-------

Machinekit utilise des fichiers traduits pour les interfaces utilisateur. Il fonctionne dans plusieurs langues et démarre dans la langue de la session ouverte par l’utilisateur au démarrage du PC. Si votre langue n’a pas encore été traduite contactez un développeur sur l’IRC ou sur la mailing liste si vous pouvez aider à la traduction.

Penser comme un opérateur sur CNC<span id="sec:Penser-operateur"></span>
------------------------------------------------------------------------

Ce manuel ne prétend pas vous apprendre à utiliser un tour ou une fraiseuse. Devenir un opérateur expérimenté prends beaucoup de temps et demande beaucoup de travail. Un auteur a dit un jour, *Nous apprenons par l’expérience, si on la possède toute*. Les outils cassés, les étaux attaqués et les cicatrices sont les preuves des leçons apprises. Une belle finition, des tolérances serrées et la prudence pendant le travail sont les preuves des leçons retenues. Aucune machine, aucun programme ne peut remplacer l’expérience humaine.

Maintenant que vous commencez à travailler avec le programme Machinekit, vous devez vous placer dans la peau d’un opérateur. Vous devez être dans le rôle de quelqu’un qui a la charge d’une machine. C’est une machine qui attendra vos commandes puis qui exécutera les ordres que vous lui donnerez. Dans ces pages, nous donnerons les explications qui vous aideront à devenir un bon opérateur de CNC avec Machinekit. Vous aurez besoin de bonnes informations ici, devant vous, c’est là que les pages suivantes prendront tout leur sens.

Modes opératoires<span id="sub:Modes-operatoires"></span>
---------------------------------------------------------

Quand Machinekit fonctionne, il existe trois différents modes majeurs pour entrer des commandes. Les modes *Manuel*, *Auto* et *MDI*. Passer d’un mode à un autre marque une grande différence dans le comportement de Machinekit. Des choses spécifiques à un mode ne peuvent pas être faites dans un autre. L’opérateur peut faire une prise d’origine sur un axe en mode manuel mais pas en mode auto ou MDI. L’opérateur peut lancer l’exécution complète d’un programme de G-codes en mode auto mais pas en mode manuel ni en MDI.

En mode manuel, chaque commande est entrée séparément. En termes humains, une commande manuelle pourrait être *active l’arrosage* ou *jog l’axe X à 250 millimètres par minute*. C’est en gros, équivalent à basculer un interrupteur ou à tourner la manivelle d’un axe. Ces commandes sont normalement contrôlées en pressant un bouton de l’interface graphique avec la souris ou en maintenant appuyée une touche du clavier. En mode auto, un bouton similaire ou l’appui d’une touche peut être utilisé pour charger ou lancer l’exécution complète d’un programme de G-codes stocké dans un fichier. En mode d’entrée de données manuelles (MDI) l’opérateur peut saisir un bloc de codes est dire à la machine de l’exécuter en pressant la touche *Return* ou *Entrée* du clavier.

Certaines commandes de mouvement sont disponibles et produisent les mêmes effets dans tous les modes. Il s’agit des commandes *Abandon*, *Arrêt d’Urgence* et *Correcteur de vitesse travail* . Ces commandes se dispensent d’explications.

L’interface utilisateur graphique AXIS supprime certaines distinctions entre Auto et les autres modes en rendant automatique la disponibilité des commandes, la plupart du temps. Il rend également floue la distinction entre Manuel et MDI parce que certaines commandes manuelles comme *Toucher*, sont également implémentées en envoyant une commande MDI. Il fait cela en changeant automatiquement le mode qui est nécessaire pour l’action que l’utilisateur a demandé.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


