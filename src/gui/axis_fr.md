L’interface graphique AXIS
==========================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Axis"></span>

Introduction
------------

AXIS est une interface utilisateur graphique pour Machinekit, il offre un aperçu permanent du parcours de l’outil. Il est écrit en Python, utilise Tk et OpenGL pour l’affichage de l’interface graphique.

![](images/axis_25_fr.png)

Figure 1. Fenêtre d’AXIS

Commencer avec AXIS
-------------------

Pour choisir AXIS comme interface graphique de Machinekit, éditez le fichier .ini et dans la section \[DISPLAY\] changez la ligne DISPLAY comme ceci:

    DISPLAY = axis

Puis, lancez Machinekit et choisissez le fichier ini. La configuration simplifiée *sim/axis.ini* est déjà configurée pour utiliser AXIS comme interface graphique.

Quand AXIS démarre, une fenêtre telle que celle de la figure [ci-dessus](#cap:Fenetre-AXIS) s’ouvre.

### Une session typique avec AXIS

1.  Lancer Machinekit et sélectionner un fichier de configuration.

2.  Relâcher le bouton d’arrêt d’urgence *A/U* et presser celui de *Marche* machine.

3.  Faire les prises d’origine machine *POM* pour chacun des axes.

4.  Charger un fichier d’usinage.

5.  Utiliser l’affichage du parcours d’outil pour vérifier que le programme est correct.

6.  Brider le brut à usiner sur la table.

7.  Faire les prises d’origine pièce *POP* de chacun des axes avec le jog et en utilisant le bouton *Toucher*.

8.  Lancer le programme.

9.  Pour usiner le même fichier une nouvelle fois, retourner à l'étape 6. Pour usiner un fichier différent, retourner à l'étape 4. Quand le travail est terminé, quitter AXIS.

Eléments de la fenêtre d’AXIS
-----------------------------

La fenêtre d’AXIS contient les éléments suivants:

-   Un espace d’affichage qui montre une pré-visualisation du fichier chargé (dans ce cas, *axis.ngc*), ainsi que la position courante du point programmé par la machine. Plus tard, cette zone affichera le parcours de l’outil déplacé par la machine, cette zone est appelée le parcours d’outil (backplot)

-   Une barre de menus, une barre d’outils, des curseurs et des onglets permettant d’effectuer différentes actions.

-   L’onglet *Contrôle manuel*, qui permet de faire des mouvements d’axe, de mettre la broche en rotation ou de l’arrêter, de mettre l’arrosage en marche ou de l’arrêter.

-   L’onglet *Données manuelles* (appelé aussi MDI), où les blocs de programme G-code peuvent être entrés et exécutés à la main, une ligne à la fois.

-   Les curseurs *Correcteurs de vitesse*, qui permettent d’augmenter ou de diminuer la vitesse de la fonction concernée.

-   Une zone de textes qui affiche le G-code du fichier chargé.

-   Une barre d'état qui affiche l'état de la machine. Dans cette capture d'écran, la machine est en marche, aucun outil n’est monté, la position affichée est *relative* à l’origine machine (par opposition à une position *absolue*) et *actuelle* (par opposition à une position *commandée*)

### Options des menus

Certaines options de menu peuvent s’afficher en grisé, c’est dépendant des options du fichier de configuration ini.

#### Menu Fichier

 Ouvrir…   
C’est une boîte de dialogue standard pour ouvrir un fichier G-code à charger dans AXIS. Si un filtre de programme a été configuré, il peut aussi être ouvert ici.

 Fichiers récents…   
Affiche la liste des fichiers ouverts récemment.

 Éditer…   
Ouvre le fichier G-code courant pour édition si un éditeur a été déclaré dans le fichier ini.

 Recharger…   
Recharge le fichier G-code courant. Si le fichier a été édité, il doit être rechargé pour que les modifications prennent effet. Si un programme a été stoppé, pour le reprendre depuis le début, le recharger. Le bouton *Recharger* a le même effet que l’option de menu.

 Enregistrer le G-code sous…   
Enregistre le fichier courant sous un nouveau nom.

 Propriétés…   
Donne la somme des mouvements en vitesse rapide et celle en vitesse travail. Ne tient pas compte des accélérations, ni des décélérations, ni des modes de trajectoire, de sorte qu’il ne donne jamais de temps inférieur au temps réel d’exécution.

 Éditer la table d’outils…   
Ouvre un dialogue permettant d'éditer les valeurs de la table d’outils.

 Recharger la table d’outils…   
Après avoir édité la table d’outil, il convient de la recharger pour que les nouvelles valeurs soient prisent en compte.

 Éditeur de Ladder…   
Si Classic Ladder a été chargé, il est possible de l'éditer ici.

 Quitter…   
Termine la session courante de Machinekit.

#### Menu Machine

 Arrêt d’Urgence F1…   
(bascule) Active/désactive l’arrêt d’urgence.

 Marche/Arrêt F2…   
(bascule) Active/désactive la puissance machine.

 Démarrer le programme…   
Lance l’exécution du programme G-code.

 Démarrer à la ligne sélectionnée…   
Prudence avec cette commande, respecter la démarche suivante: Premièrement, sélectionner à la souris, la ligne à laquelle démarrer. Déplacer ensuite manuellement, l’outil à la position de la ligne précédente puis, cette commande exécutera le reste du code.

 Pas à pas…   
Avance d’un seul pas de programme.

 Pause…   
Effectue une pause dans le programme.

 Reprise…   
Reprends la marche après une pause.

 Stopper…   
Stoppe le programme en marche.

 Arrêt sur M1…   
Si un M1 est rencontré et que cette option est cochée, l’exécution du programme s’interrompra à la ligne du M1. Presser *Reprise* pour continuer.

 Sauter les lignes avec "/"…   
Si une ligne commençant par / est rencontrée et que cette option est cochée, cette ligne sera sautée.

 Vider l’historique du MDI…   
Efface l’historique des données manuelles.

 Copier depuis l’historique du MDI…   
Copier l’historique des données manuelles dans le presse-papier.

 Coller dans l’historique du MDI…   
Coller le contenu du presse-papier dans la fenêtre d’historique des données manuelles.

 Calibration…   
Lance un assistant de réglage de PID. Cette option n’est utile qu’avec un système à servomoteurs.

 Afficher configuration de HAL…   
Ouvre une fenêtre sur la configuration de HAL depuis laquelle il est possible de visualiser tous les *Components*, *Pins*, *Parameters*, *Signals*, *Functions* et *Threads* de HAL.

 HAL Mètre…   
Ouvre une fenêtre dans laquelle il est possible de visualiser un seul *Signal, HAL Pin*, ou *Parameter* de HAL.

 HAL Scope…   
Ouvre un oscilloscope virtuel qui permet de tracer dans le temps, les valeurs de HAL.

 Afficher l'état de Machinekit…   
Ouvre une fenêtre montrant l'état de Machinekit.

 Choisir le niveau de Debug…   
Ouvre une fenêtre dans laquelle les niveaux de débogage sont visibles et certains réglables.

 Prise d’origine…   
Effectue la prise d’origine machine d’un ou de tous les axes.

 Annulation OM…   
Annule les origines d’un ou de tous les axes.

 Annulation décalage d’origine…   
Annule les décalages d’origine du système de coordonnées choisi.

 L’outil touchera la pièce…   
Lorsqu’un *Toucher* est effectué, la valeur entrée est relative au système de coordonnées pièce actuel (G5x), tel que modifié par le décalage d’axe (G92). Quand la séquence de *Toucher* est complète, la coordonnée relative pour l’axe choisi prendra la valeur entrée. (Voir aussi G10 L10)

 L’outil touchera le porte-pièce…   
Lorsqu’un *Toucher* est effectué, la valeur entrée est relative au 9ème système de coordonnées (G59.3), le décalage d’axe (G92) est ignoré. Mode destiné aux machines possédant un porte-pièce référencé à un endroit, sur lequel s’effectue le *Toucher*. Le 9ème système de coordonnées doit être ajusté pour que la pointe d’un outil de longueur nulle (le nez de broche), soit à l’origine du porte-pièce quand les coordonnées relatives sont à 0. Voir aussi [G10 L11](#sec:G10-L11).

#### Menu Vues

Tout est dans le point de vue…

Les icônes de choix du type d’affichage et du menu *Vues* d’AXIS se référent à des *Vue de dessus*, *Vue de face* et *Vue de côté*. Ces termes sont corrects si la machine CNC a un axe Z vertical, avec une valeur de Z positive en haut. C’est vrai pour les fraiseuses verticales, qui sont probablement les plus populaires, c’est également vrai pour toutes les machines d'électro-érosion et aussi les tours verticaux, sur lesquels la pièce tourne sous l’outil.

Les termes *Vue de dessus*, *Vue de face* et *Vue de côté* sont cependant source de confusion sur d’autres machines CNC, comme un tour standard, sur lequel l’axe Z est horizontal, ou sur une fraiseuse horizontale, qui a également l’axe Z horizontal, ou même un tour vertical inversé, sur lequel la pièce tourne au dessus de l’outil et qui a son axe Z positif vers le bas!

Il faut juste se rappeler que l’axe Z est toujours parallèle a la broche et plus positif en s'éloignant de celle-ci. Soyez familiarisé avec la cinématique de vos machines, vous interpréterez alors l’affichage comme il se doit.

 Vue de dessus…   
La vue de dessus (ou vue de Z) affiche l’aspect du G-code vu depuis le côté positif de l’axe Z et en regardant vers son côté négatif. Cette vue convient bien pour visualiser les axes X et Y.

 Vue de dessus basculée…   
La vue de dessus basculée (ou vue de Z basculé) affiche également l’aspect du G-code vu depuis le côté positif de l’axe Z et en regardant vers son côté négatif. Mais cette fois, les axes X et Y sont représentés pivotés de 90 degrés pour mieux occuper l’espace d’affichage. Cette vue convient bien également, pour visualiser les axes X et Y.

 Vue de côté…   
La vue de côté (ou vue de X) affiche l’aspect du G-code vu depuis le côté positif de l’axe X et en regardant vers son côté négatif. Cette vue convient pour visualiser les axes Y et Z.

 Vue de face…   
La vue de face (ou vue de Y) affiche l’aspect du G-code vu depuis le côté positif de l’axe Y et en regardant vers son côté négatif. Cette vue convient bien pour visualiser les axes X et Z.

 Vue en perspective…   
La vue en perspective (ou vue P) affiche l’aspect du G-code en regardant vers la pièce depuis un point de vue orientable, par défaut vers X+, Y-, Z+. Cette position est orientable en la sélectionnant à la souris. L’affichage est un compromis, il tente d’afficher en 3D, entre trois et neuf axes, sur un écran en deux dimensions. Il y aura donc souvent certaines caractéristiques difficiles à voir, ce qui requerra un changement de point de vue. Cette vue convient bien pour voir les trois axes à la fois.

 Affichage en pouces…   
Ajuste l'échelle d’affichage d’AXIS pour les pouces.

 Affichage en mm…   
Ajuste l'échelle d’affichage d’AXIS pour les millimètres.

 Afficher le programme…   
L’affichage à l'écran de l’aspect du G-code peut être entièrement désactivé si l’opérateur le souhaite.

 Parcours d’outil en vitesse rapide…   
L’affichage du parcours d’outil du programme G-code courant représente toujours les mouvements en vitesse travail (G1,G2,G3) en blanc. Mais l’affichage des mouvements en vitesse rapide (G0) en cyan peut être désactivé si si l’opérateur le souhaite.

 Simulation de transparence…   
Cette option rends plus lisible le tracé des parcours affichés par les programmes complexes, mais il peut rendre l’affichage plus lent.

 Parcours d’outil en temps réel…   
La surbrillance des chemins d’outils en vitesse travail (G1,G2,G3) quand l’outil se déplace peut être désactivée si l’opérateur le souhaite.

 Afficher l’outil…   
Le symbole d’un outil, représenté par un cône ou un cylindre peut être désactivé si l’opérateur le souhaite.

 Afficher les étendues…   
L’affichage des étendues du programme G-code chargé (déplacements maximum de chacun des axes), peut être désactivé si l’opérateur le souhaite.

 Afficher les offsets…   
L’emplacement de l’origine du système de coordonnées pièce (G54 à G59.3) peut être représenté par un jeu de trois lignes orthogonales, une rouge, une bleue et une verte. L’affichage de cette origine pièce (ou zéro pièce), peut être désactivé si l’opérateur le souhaite.

 Afficher les limites machine…   
Les limites maximales de déplacement machine pour chacun des axes, qui sont fixées dans le fichier ini, s’affichent comme une boîte rectangulaire en lignes pointillées rouges. Il est facile, au chargement d’un nouveau programme G-code, de voir si la pièce est contenue dans le volume représenté. Ou de vérifier de combien l'étau doit être décalé, pour que le G-code puisse être usiné sans dépasser les limites de déplacements de la machine. Cette option peut être désactivée si l’opérateur le souhaite.

 Afficher la vitesse d’avance…   
L’affichage de la vitesse peut être utile pour voir la précision avec laquelle la machine suit la vitesse commandée. Cette option peut être désactivée si l’opérateur le souhaite.

 Afficher la distance restante…   
La distance restante est une valeur très utile à suivre, au lancement d’un programme de G-code inconnu pour la première fois. En combinaison avec les curseurs des correcteurs de vitesse, des dégâts sur l’outil ou la machine peuvent être évités. Quand le programme G-code sera débogué et qu’il fonctionnera en douceur, l’affichage de la distance restante pourra être désactivée si l’opérateur le souhaite.

 Coordonnées en police large…   
Les coordonnées des axes et la vitesse d’avance, s’afficheront en police large dans la vue du parcours d’outil.

 Rafraîchir le parcours d’outil…   
Au fur et à mesure des déplacements de l’outil, les parcours s’affichent sur l'écran d’Axis en surbrillance. Avant de répéter le programme, ou pour avoir un affichage clair sur une zone intéressante, la surbrillance des parcours précédents peut être rafraîchie.

 Afficher la position commandée…   
C’est la position que Machinekit cherche à atteindre. Quand le mouvement est stoppé, c’est la position que Machinekit cherchera à maintenir.

 Afficher la position actuelle…   
La position actuelle est la position mesurée grâce aux informations issues des codeurs ou simulées par le générateur de pas. Elle peut différer légèrement de la position commandée pour diverses raisons, comme les réglages des boucles PID, les contraintes physiques ou les efforts de coupe.

 Afficher la position machine…   
C’est la position par rapport à l’origine machine, telle qu'établie par la prise d’origine machine *(POM)*.

 Afficher la position relative…   
C’est la position par rapport à l’origine pièce, telle qu'établie par la prise d’origine pièce *(POP)*. On peut aussi représenter cette position comme étant l’origine machine à laquelle on a appliqué les codes de décalages des systèmes de coordonnées G5x, G92 et G43.

#### Menu Aide

 A propos d’Axis…   
Donne la version et quelques informations relatives au copyright.

 Aide rapide…   
Affiche la liste des raccourcis clavier.

### Boutons de la barre d’outils

Signification des boutons de la fenêtre d’AXIS, de gauche à droite:

1.  <span class="image"> ![](images/tool_estop.gif) </span> *Arrêt d’urgence* (A/U) (en Anglais, E-Stop)

2.  <span class="image"> ![](images/tool_power.gif) </span> Marche/Arrêt puissance machine

3.  <span class="image"> ![](images/tool_open.gif) </span> Ouvrir un fichier

4.  <span class="image"> ![](images/tool_reload.gif) </span> Recharger le fichier courant

5.  <span class="image"> ![](images/tool_run.gif) </span> Départ cycle

6.  <span class="image"> ![](images/tool_step.gif) </span> Cycle en pas à pas

7.  <span class="image"> ![](images/tool_pause.gif) </span> Pause/Reprise

8.  <span class="image"> ![](images/tool_stop.gif) </span> Stopper l’exécution du programme

9.  <span class="image"> ![](images/tool_blockdelete.gif) </span> Sauter ou non les lignes commençant par **/**

10. <span class="image"> ![](images/tool_optpause.gif) </span> Avec ou sans pause optionnelle

11. <span class="image"> ![](images/tool_zoomin.gif) </span> Zoom plus

12. <span class="image"> ![](images/tool_zoomout.gif) </span> Zoom moins

13. <span class="image"> ![](images/tool_axis_z.gif) </span> Vue prédéfinie **Z** (vue de dessus)

14. <span class="image"> ![](images/tool_axis_z2.gif) </span> Vue prédéfinie **Z basculée**

15. <span class="image"> ![](images/tool_axis_x.gif) </span> Vue prédéfinie **X** (vue de côté)

16. <span class="image"> ![](images/tool_axis_y.gif) </span> Vue prédéfinie **Y** (vue de face)

17. <span class="image"> ![](images/tool_axis_p.gif) </span> Vue prédéfinie **P** (vue en perspective)

18. <span class="image"> ![](images/tool_rotate.gif) </span> Orienter la vue avec le bouton gauche de la souris

19. <span class="image"> ![](images/tool_clear.gif) </span> Rafraîchir le parcours d’outil

### Zones d’affichage graphique du programme

#### Affichage des coordonnées

L’affichage des coordonnées est situé en haut à gauche de l'écran graphique. Il montre les positions de la machine. A gauche du nom de l’axe, un symbole d’origine est visible si la prise d’origine de l’axe a été faite.

<span class="image"> ![](images/axis-homed.png) </span> Symbole de prise d’origine faite.

A droite du nom de l’axe, un symbole de limite est visible si l’axe est sur un de ses capteurs de limite.

<span class="image"> ![](images/axis-limit.png) </span> Symbole de limite d’axe.

Pour interpréter correctement ces valeurs, référez vous à l’indicateur *Position* de la barre d'état. Si la position est *Absolue*, alors les valeurs affichées sont exprimées en coordonnées machine. Si la position est *Relative*, alors les valeurs affichées sont exprimées en coordonnées relatives à la pièce. Quand les coordonnées affichées sont relatives, une marque d’origine de couleur cyan est visible pour représenter l’origine machine.

<span class="image"> ![](images/axis-machineorigin.png) </span> Symbole d’origine machine.

Si la position est *Commandée*, alors il s’agit de la position à atteindre. Par exemple, les coordonnées passées dans une commande **G0**. Si la position est *Actuelle*, alors il s’agit de la position à laquelle la machine vient de se déplacer. Ces valeurs peuvent varier pour certaines raisons: erreur de suivi, bande morte, résolution d’encodeur, ou taille de pas. Par exemple, si vous demandez un mouvement à X 0.08 à votre fraiseuse, mais un pas du moteur fait 0.03, alors la position *Commandée* sera de 0.08, mais la position *Actuelle* sera de 0.06 (2 pas) ou 0.09 (3 pas).

#### Vue du parcours d’outil

Quand un fichier est chargé, une vue du parcours d’outil qu’il produira est visible dans la zone graphique. Les mouvements en vitesse rapide (tels ceux produits par une commande **G0**) sont affichés en lignes pointillées vertes. Les déplacements en vitesse travail (tels ceux produits par une commande **G1**) sont affichés en lignes continues blanches. Les arrêts temporisés (tels ceux produits par la commande **G4**) sont représentés par une petite marque **X**.

Un mouvement G0 (Vitesse rapide) avant un déplacement en vitesse travail ne sera pas affiché sur l'écran des parcours d’outil. Un mouvement en vitesse rapide, après un appel d’outil T&lt;n&gt;, n’apparaîtra sur l'écran des parcours d’outil qu’après le mouvement en vitesse travail suivant. Pour contourner une de ces caractéristiques, programmer un G1 sans déplacement, juste avant le G0.

#### Étendues du programme

Les *étendues* du programme sont affichées pour chacun des axes. Aux extrémités, les coordonnées minimales et maximales sont indiquées. Au centres, la différence, entre ces deux coordonnées, est indiquée.

Quand une coordonnée dépasse la limite logicielle fixée dans le fichier .ini, la coordonnée correspondante s’affiche en rouge, entourée d’un rectangle. Dans la figure ci-dessous, la limite maximale est dépassée sur l’axe X, comme l’indique le rectangle entourant la valeur de la coordonnée. Le déplacement X minimal du programme est de -1.95, la course maximale est de 1.88 en X et le programme nécessite un déplacement en X de 3.83 pouces. Le déplacement total demandé par le programme est donc possible. Pour cela, se déplacer en jog vers la gauche puis *Toucher* à nouveau pour corriger l’origine pièce.

![](images/axis-outofrange.png)

Figure 2. Limites logicielles<span id="cap:Etendues-Depassees"></span>

#### Le cône d’outil

Si aucun outil n’est chargé, l’emplacement de la pointe de l’outil est indiqué par le *cône d’outil*. Le cône d’outil ne donne aucune indication sur la forme, la longueur, ou le rayon de l’outil.

Quand un outil est chargé, par exemple dans le MDI, avec la commande **T1 M6**, le cône d’outil passe de conique à cylindrique, il indique alors la proportion du diamètre de l’outil lu dans le fichier de la table d’outils.

#### Parcours d’outil

Quand la machine se déplace, elle laisse une trace appelée le parcours d’outil. La couleur des lignes indique le type de mouvement: jaune pour les mouvementq jog, vert clair pour les mouvements en vitesse rapide, rouge pour les mouvements en vitesse d’avance programmée et magenta pour les mouvements circulaires en vitesse d’avance programmée.

#### Interaction avec l’affichage

Par un clic gauche sur une portion du parcours d’outil, la ligne sous la souris passe en surbrillance à la fois dans le parcours d’outil et dans le texte. Un clic droit dans une zone vide enlève la surbrillance

En déplaçant la souris avec son bouton gauche appuyé, la vue est glissée sur l'écran.

En déplaçant la souris avec le bouton *Maj* enfoncé, ou en glissant avec la molette de la souris appuyée, la vue est tournée. Si une ligne du tracé est en surbrillance, elle devient le centre de rotation de la vue. Autrement, le centre de rotation est le milieu du fichier dans son ensemble.

En tournant la molette de la souris, ou en glissant la souris avec son bouton droit enfoncé, ou encore en glissant la souris avec son bouton gauche enfoncé et la touche *Ctrl* appuyée, le tracé sera zoomé en plus ou en moins.

En cliquant sur une des icônes de vue pré-définie de la barre d’outils, ou en pressant la touche **V**, cette vue est sélectionnée.

### Zone texte d’affichage du programme

Un clic gauche sur une ligne du programme passe la ligne en surbrillance à la fois dans la zone texte et dans le parcours d’outil.

Quand le programme est lancé, la ligne en cours d’exécution est en surbrillance rouge. Si aucune ligne n’est sélectionnée par l’utilisateur, le texte défile automatiquement pour toujours laisser la ligne courante visible.

![](images/axis-currentandselected_fr.png)

Figure 3. Ligne courante et ligne en surbrillance

### Contrôle manuel

Quand la machine est en marche mais qu’aucun programme n’est exécuté, les éléments graphiques de l’onglet *Contrôle manuel* peuvent être utilisés pour actionner la machine ou mettre en marche et arrêter ses différents organes.

Quand la machine n’est pas en marche, ou quand un programme est en cours d’exécution, le contrôle manuel n’est pas disponible.

Certains des éléments décrits plus bas ne sont pas disponibles sur toutes les machines. Quand AXIS détecte qu’une pin particulière n’est pas connectée dans le fichier HAL, l'élément correspondant de l’onglet *Contrôle manuel* est supprimé. Par exemple, si la pin HAL *motion.spindle-brake* n’est pas connectée, alors le bouton *Frein de broche* n’apparaîtra pas sur l'écran. Si la variable d’environnement AXIS\_NO\_AUTOCONFIGURE est mise à 1, ce comportement est désactivé et tous les boutons sont visibles.

#### Le groupe de cases et boutons *Axes*

Les cases à cocher du groupe *Axes* permettent de choisir l’axe de la machine à actionner manuellement. Cette action s’appelle le *jog*. Premièrement sélectionner l’axe à actionner en cochant sa case. Puis cliquer sur le bouton **+** ou **-** selon le sens de déplacement souhaité. Les quatre premiers axes peuvent aussi être déplacés avec les touches fléchées pour X et Y, avec les touches Page précédente et Page suivante pour (Z) et les touches \[ et \] pour A.

Si *En continu* est sélectionné, le mouvement continuera tant que la touche ou le bouton resteront appuyés. Si une autre valeur est sélectionnée, la machine se déplacera juste de la distance affichée à chaque fois que la touche ou le bouton seront appuyés. Par défaut, les valeurs disponibles sont:

    0.1000 0.0100 0.0010 0.0001

Voir le Manuel de l’intégrateur pour plus d’informations sur la configuration des incréments de jog.

#### La prise d’origine machine

Si votre machine dispose de contacts d’origine machine et a une séquence de prise d’origine définie dans le fichier ini, le bouton *POM générale* lancera cette séquence pour tous les axes, les touches *Ctrl-HOME* auront le même effet.

Si votre machine dispose de contacts d’origine mais n’a pas de séquence de prise d’origine définie dans le fichier ini, le bouton *POM générale* effectuera uniquement la prise d’origine de l’axe sélectionné. Cette procédure doit alors être réalisée, séparément pour chacun des axes.

Si votre machine ne dispose d’aucun contact d’origine défini dans la configuration, le bouton *POM générale* définira la position actuelle de l’axe comme étant la position d’origine machine et l’axe sera marqué comme ayant sa prise d’origine machine faite.

#### Toucher

Si le bouton *Toucher* ou la touche *FIN* sont appuyés, le décalage d’origine pièce de l’axe Z, sur la figure ci-dessous: P1 G54, prendra la valeur spécifiée dans le champ de la boite de dialogue. Les expressions peuvent être entrées en suivant les règles de programmation rs274ngc, sauf les variables qui ne peuvent pas être utilisées. La valeur résultante sera affichée sous le champ. Exemple, pour faire la prise d’origine pièce, on affleure l’outil sur une cale de 5mm d'épaisseur posée sur le bloc, on presse le bouton *Toucher* et on saisi 5 dans le champ de la boîte de dialogue. La pointe de l’outil sera alors référencée à 0 sur la surface du bloc.

![](images/touchoff_fr.png)

Figure 4. Fenêtre du Toucher

Voir aussi les options du menu Machine: *Toucher la pièce* et *Toucher le porte-pièce*.

#### Dépassement de limite

En appuyant sur *Dépassement de limite*, la machine sera temporairement autorisée à se déplacer au delà d’un contact de limite physique. Cette case à cocher n’est disponible que lorsqu’un fin de course est pressé. Elle est désactivée après chaque mouvement de jogging. Si l’axe est configuré avec des contacts positifs et négatifs séparés, Machinekit permettra le jogging uniquement dans le sens du dégagement. *Dépassement de limite* ne permettra pas un jogging au delà d’une limite logicielle. La seule façon de désactiver une limite logicielle sur un axe est d’annuler sa prise d’origine.

#### Le groupe *Broche*

Les boutons de la première rangée permettent de sélectionner la direction de rotation de la broche: Sens anti-horaire, Arrêt, Sens horaire. Les boutons de la rangée suivante augmentent ou diminuent la fréquence de rotation. La case à cocher de la troisième rangée permet d’engager ou de relâcher le frein de broche. Selon la configuration de votre machine, ces éléments n’apparaîtront peut être pas tous.

#### Le groupe *Arrosage*

Ces deux boutons permettent d’activer les *gouttelettes* et l'*Arrosage fluide* ou de les désactiver. Selon la configuration de votre machine, ces boutons n’apparaîtront peut être pas tous.

### Données manuelles (MDI)

L’onglet d’entrée de données manuelles (encore appelé MDI), permet d’entrer et d’exécuter manuellement et une par une, des lignes de programme en G-code. Quand la machine n’est pas en marche, ou quand un programme est en cours d’exécution, cet onglet n’est pas opérationnel.

![](images/axis-codeentry_fr.png)

Figure 5. L’onglet *Données manuelles*

 *Historique*   
Affiche les commandes précédemment tapées et au cours des session précédentes.

 *Commande MDI*   
Ce champ permet la saisie d’une ligne de commande à exécuter. La commande sera exécutée par l’appui de la touche &lt;Entrée&gt; ou un appui sur le bouton *Envoi*.

 *G-Codes actifs*   
Cette fenêtre affiche les *codes modaux* actuellement actifs dans l’interpréteur. Par exemple, **G54** indique que le système de décalage d’origine actuel est **G54** qu’il s’appliquera à toutes les coordonnées qui seront entrées.

### Correcteurs de vitesse

En déplaçant le curseur, la vitesse de déplacement programmée peut être modifiée. Par exemple, si un programme requiert une vitesse à **F600** et que le curseur est placé sur 120%, alors la vitesse résultante sera de **F720**.

### Correcteur de vitesse de broche

En déplaçant ce curseur, la vitesse programmée de la broche peut être modifiée. Par exemple, si un programme requiert une vitesse à F8000 et que le curseur est placé sur 80%, alors la fréquence de rotation résultante sera de **F6400**. Cet élément n’apparaît que si la *HAL pin* *motion.spindle-speed-out* est connectée dans le fichier .hal.

### Vitesse de Jog

En déplaçant ce curseur, la vitesse de jog peut être modifiée. Par exemple, si ce curseur est placé sur 100 mm/mn, alors un jog de 1 mm durera .6 secondes, ou 1/100 de minute. Du côté gauche du curseur (jog lent) l’espacement des valeurs est petit alors que du côté droit (jog rapide) l’espacement des valeurs est plus grand, cela permet une large étendue de vitesses de jog avec un contrôle plus fin du curseur dans les zones les plus importantes.

Sur les machines avec axes rotatifs, un second curseur de vitesse est présent. Il permet d’ajuster la vitesse de rotation des axes rotatifs (A, B et C).

### Vitesse Maxi

En déplaçant ce curseur, la vitesse maximale peut être réglée. Ceci couvre la vitesse maximale de tous les mouvements programmés, sauf les mouvements avec broche synchronisée.

Raccourcis clavier
------------------

La plupart des actions d’AXIS sont accessibles depuis le clavier. La liste complète des raccourcis clavier est disponible dans l’aide rapide d’AXIS qui s’affiche en cliquant sur Aide &gt; Aide rapide . Beaucoup de ces raccourcis sont inaccessible en mode Entrées manuelles.

Touches des correcteurs de vitesse.

-   Les touches des correcteurs de vitesse se comportent différemment en mode manuel. Les touches *12345678* sélectionneront l’axe correspondant, si il est programmé.

-   Si vous avez 3 axes, alors **\*** sélectionnera l’axe 0, 1 sélectionnera l’axe 1, et 2 sélectionnera l’axe 2.

-   Pendant l’exécution d’un programme, les touches *1234567890* fixeront la correction de vitesse travail entre 10% et 100%.

Les raccourcis clavier les plus fréquents sont visibles dans la table ci-dessous.

<table>
<caption>Tableau 1. Raccourcis clavier usuels<span id="cap:Raccourcis-clavier-usuels"></span></caption>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Touches</th>
<th align="left">Actions produites</th>
<th align="left">Mode</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>F1</p></td>
<td align="left"><p>Bascule l’arrêt d’urgence</p></td>
<td align="left"><p>Tous</p></td>
</tr>
<tr class="even">
<td align="left"><p>F2</p></td>
<td align="left"><p>Bascule le marche/arrêt machine</p></td>
<td align="left"><p>Tous</p></td>
</tr>
<tr class="odd">
<td align="left"><p>`, 1 .. 9, 0</p></td>
<td align="left"><p>Correcteurs de vitesse de 10% à 100%</p></td>
<td align="left"><p>Varie</p></td>
</tr>
<tr class="even">
<td align="left"><p>X, *</p></td>
<td align="left"><p>Active le premier axe</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Y, 1</p></td>
<td align="left"><p>Active le deuxième axe</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="even">
<td align="left"><p>Z, 2</p></td>
<td align="left"><p>Active le troisième axe</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>A, 3</p></td>
<td align="left"><p>Active le quatrième axe</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="even">
<td align="left"><p>I</p></td>
<td align="left"><p>Sélection d’incrément du jog</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>C</p></td>
<td align="left"><p>jog en mode continu</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="even">
<td align="left"><p>Ctrl+origine</p></td>
<td align="left"><p>Lance une séquence de POM</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Fin</p></td>
<td align="left"><p>Toucher: valide l’offset G54 de l’axe actif</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="even">
<td align="left"><p>Gauche, Droite</p></td>
<td align="left"><p>Jog du premier axe</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Up, Down</p></td>
<td align="left"><p>Jog du deuxième axe</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="even">
<td align="left"><p>Pg Up, Pg Dn</p></td>
<td align="left"><p>Jog du troisième axe</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[, ]</p></td>
<td align="left"><p>Jog du quatrième axe</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="even">
<td align="left"><p>O</p></td>
<td align="left"><p>Ouvrir un fichier</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Ctrl+R</p></td>
<td align="left"><p>Recharger le fichier courant</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="even">
<td align="left"><p>R</p></td>
<td align="left"><p>Exécuter le programme</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>P</p></td>
<td align="left"><p>Pause dans l’exécution du programme</p></td>
<td align="left"><p>Auto</p></td>
</tr>
<tr class="even">
<td align="left"><p>S</p></td>
<td align="left"><p>Reprise de l’exécution du programme</p></td>
<td align="left"><p>Auto</p></td>
</tr>
<tr class="odd">
<td align="left"><p>ESC</p></td>
<td align="left"><p>Stopper l’exécution</p></td>
<td align="left"><p>Auto</p></td>
</tr>
<tr class="even">
<td align="left"><p>Ctrl+K</p></td>
<td align="left"><p>Rafraichi le tracé d’outil</p></td>
<td align="left"><p>Auto/Manuel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>V</p></td>
<td align="left"><p>Défilement cyclique des vues prédéfinies</p></td>
<td align="left"><p>Auto/Manuel</p></td>
</tr>
<tr class="even">
<td align="left"><p>Maj-gauche,droite</p></td>
<td align="left"><p>Axe X vitesse rapide</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Maj-haut, bas</p></td>
<td align="left"><p>Axe Y vitesse rapide</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
<tr class="even">
<td align="left"><p>Maj-PgUp, PgDn</p></td>
<td align="left"><p>Axe Z vitesse rapide</p></td>
<td align="left"><p>Manuel</p></td>
</tr>
</tbody>
</table>

Affichage de l'état de Machinekit (Machinekittop)
-------------------------------------------------

AXIS inclut un programme appelé *machinekittop* qui affiche en détail l'état de Machinekit. Ce programme est accessible dans le menu Machine &gt; Fenêtre d'état de Machinekit.

![](images/axis-emc-status.png)

Figure 6. Fenêtre d'état de Machinekit

Le nom de chaque entrée est affiché dans la colonne de gauche. La valeur courante de chaque entrée s’affiche dans la colonne de droite. Si la valeur a changé récemment, elle s’affiche en surbrillance rouge.

Entrée de données en texte (MDI)
--------------------------------

AXIS inclut un programme appelé *mdi*, il permet d’envoyer des commandes à la session de Machinekit en cours, sous forme de lignes de texte. Vous pouvez lancer ce programme en ouvrant une console et en tapant:

    mdi /chemin/vers/machinekit.nml

En cours d’exécution il affiche le prompt: *MDI&gt;*. Quand une ligne vide est entrée, la position courante de la machine est affichée. Quand une commande est entrée, elle est passée à Machinekit qui l’exécute.

C’est un exemple de session de MDI.

    $ MDI ~/machinekit/configs/sim/emc.nml
    MDI>
    (0.0, 0.0, 0.0, 0.0, 0.0, 0.0)
    MDI> G1 F5 X1
    MDI>
    (0.5928500000000374, 0.0, 0.0, 0.0, 0.0, 0.0)
    MDI>
    (1.0000000000000639, 0.0, 0.0, 0.0, 0.0, 0.0)

axis-remote: Commande à distance de l’interface graphique d’AXIS
----------------------------------------------------------------

AXIS inclut un programme appelé *axis-remote* qui permet d’envoyer certaines commandes vers l’application AXIS fonctionnant à distance. Les commandes disponibles sont visibles en faisant: *axis-remote --help* pour vérifier qu’AXIS est en marche, inclure: (*--ping*), charger un fichier, recharger le fichier courant avec: (*--reload*) et quitter le programme AXIS avec: (*--quit*).

hal\_manualtoolchange: Dialogue de changement d’outil manuel
------------------------------------------------------------

Machinekit inclut un composant userspace HAL de appelé *hal\_manualtoolchange*, il ouvre une fenêtre d’appel d’outil visible ci-dessous, quand la commande **M6** est invoquée. Dés que le bouton *Continuer* est pressé, l’exécution du programme reprend.

Le fichier de configuration HAL *configs/sim/axis\_manualtoolchange.hal* montre les commandes HAL nécessaires pour l’utilisation de ce composant.

hal\_manualtoolchange peut être utilisé même si l’interface graphique AXIS n’est pas en service. Cette composante est particulièrement utile si vous avez des outils de pré-réglage et que vous utilisez la table d’outils.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Important
</div></td>
<td align="left">Le parcours d’outil d’un mouvement en vitesse rapide ne sera pas visible s’il suit un changement d’outil T&lt;n&gt; et ce jusqu’au prochain mouvement en vitesse travail. Ceci peut être source de confusion pour l’opérateur. Pour contourner cette particularité, ajoutez toujours un G1 sans mouvement après un M6 T&lt;n&gt;.</td>
</tr>
</tbody>
</table>

![](images/manualtoolchange_fr.png)

Figure 7. La fenêtre de changement manuel d’outil

Modules en Python
-----------------

AXIS inclut plusieurs modules en Python qui peuvent être très utiles. Pour des informations complètes sur ces modules, faites: *pydoc &lt;nom du module* ou lisez son code source. Modules inclus:

-   **Machinekit** fournit l’accès aux commandes de Machinekit, à son état et aux chaînes d’erreur

-   **gcode** fournit l’accès à l’interpréteur RS274NGC

-   **rs274** fournit des outils supplémentaires pour travailler sur les fichiers RS274NGC

-   **hal** permet la création par l’utilisateur de composants de HAL écrits en Python

-   **\_togl** fournit des éléments OpenGL utilisables dans les applications Tkinter

-   **minigl** fournit l’accès aux sous-ensembles d’OpenGL utilisés par AXIS

Pour utiliser ces modules dans vos propres scripts, assurez-vous que le répertoire où ils se trouvent est dans le chemin d’accès des modules Python. Avec une version installée de Machinekit, ça se fera automatiquement. Avec une version installée en *in-place*, ça peut être fait avec l’aide de: */scripts/rip-environment*.

Utiliser AXIS en mode tour
--------------------------

En incluant la ligne

    LATHE = 1

dans la section \[DISPLAY\] du fichier ini, AXIS sélectionnera le mode tour. L’axe Y ne s’affiche pas parmi les coordonnées, la vue est modifiée pour placer la broche à gauche, l’axe Z horizontalement avec son côté positif vers la droite **(Z+)** et l’axe X s'étendant vers le bas de l'écran **(X+)**. Plusieurs contrôles (tels que ceux des vues prédéfinies) sont supprimés. Les lectures de coordonnées pour X sont désormais en diamètre et en rayon.

La touche **V** agit alors sur le zoom pour afficher le tracé complet du fichier chargé.

En mode tour (lathe), la forme et l’orientation de l’outil chargé sont représentés.

![](images/axis-lathe-tool.png)

Figure 8. Représentation de l’outil en mode tour

Configuration avancée d’AXIS
----------------------------

Pour plus d’informations sur les paramètres du fichier ini pouvant modifier le fonctionnement d’AXIS, voir le fichier ini, sections \[DISPLAY\] et le chapitre sur la configuration dans le manuel de l’intégrateur.

### Filtres de programme

AXIS a la capacité d’envoyer des fichiers chargés à travers un *filtre de programme*. Ce filtre peut faire diverses tâches: Simple, comme s’assurer que le programme se termine bien par un ***M2*** ou complexe, comme détecter que l’entrée est une image et générer le g-code qui permettra d’usiner sa forme.

La section \[FILTER\] du fichier ini défini comment les filtres doivent agir. Premièrement, pour chaque type de fichier, écrire une ligne PROGRAM\_EXTENSION puis, spécifier le programme à exécuter pour chaque type de fichier. Ce programme reçoit comme argument le nom du fichier d’entrée, il doit produire le G-code selon le standard rs274ngc, en sortie. Les lignes de cette sortie s’affichent alors dans la zone de texte, le parcours d’outil résultant est visible dans la zone graphique, enfin il sera exécuté quand Machinekit recevra la commande *Exécuter le programme*. Les lignes suivantes fournissent la possibilité d’utiliser *image-to-gcode*, le convertisseur d’images fourni avec Machinekit:

    [FILTER]
    PROGRAM_EXTENSION = .png,.gif Greyscale Depth Image
    png = image-to-gcode
    gif = image-to-gcode

Il est également possible de spécifier un interpréteur:

    PROGRAM_EXTENSION = .py Python Script
    py = python

De cette manière, n’importe quel script Python pourra être ouvert et sa sortie traitée comme du G-code. Un autre exemple est disponible dans: */nc\_files/holecircle.py* . Ce script crée le G-code pour percer une série de trous suivant un arc de cercle.

![](images/holes.png)

Figure 9. Perçages circulaires

Si la variable d’environnement: AXIS\_PROGRESS\_BAR est active, alors les lignes seront écrites sur stderr de la forme:

    FILTER_PROGRESS=%d

AXIS fixera la barre de progression selon le pourcentage donné. Cette fonction devrait être utilisée pour un filtre qui fonctionne suffisamment longtemps.

### La base de données des ressources X

Les couleurs de la plupart des éléments de l’interface utilisateur d’AXIS peuvent être personnalisées grâce à la base de données X. Le fichier *axis\_light\_background* modifie les couleurs de la fenêtre du parcours d’outil sur le modèle *lignes noires et fond blanc*, il sert aussi de référence des éléments configurables dans l'écran graphique. L’exemple de fichier *axis\_big\_dro* évolution de la position de lecture à une police plus grande taille. Pour utiliser ces fichiers:

    xrdb -merge /usr/share/doc/machinekit/axis_light_background

    xrdb -merge /usr/share/doc/machinekit/axis_big_dro

Pour plus d’informations au sujet des éléments configurables dans les applications Tk, référez vous aux manuels de Tk.

Les bureaux graphiques modernes effectuent certains réglages dans la base de données des ressources X ces réglages peuvent affecter ceux d’AXIS, par défaut ces réglages sont ignorés. Pour que les éléments des ressources X écrasent ceux par défaut dans AXIS, il faut inclure cette ligne dans vos ressources X:

        *Axis*optionLevel: widgetDefault

ce qui entraînera la construction des options au niveau *widgetDefault*, de sorte que les ressources X (qui sont elles, au niveau *userDefault*) puissent l’emporter.

### Manivelle de jog

Pour accroître l’interaction d’AXIS avec une manivelle de jog physique, l’axe actif courant sélectionné dans l’interface graphique est aussi reporté sur une *pin HAL* avec un nom comme *axisui.jog.x*. Excepté pendant un court instant après que l’axe courant ait changé, une seule de ces pins à la fois est *TRUE*, les autres restent *FALSE*.

Après qu’AXIS ait créé ces *HAL pins*, il exécute le fichier hal déclaré avec: \[HAL\]POSTGUI\_HALFILE. Ce qui diffère de \[HAL\]HALFILE, qui lui ne s’utilise qu’une seule fois.

### ~/.axisrc

Si il existe, le contenu de: *~/.axisrc* est exécuté comme un code source Python juste avant l’ouverture de l’interface graphique d’AXIS. Les détails de ce qui peut être écrit dans .axisrc sont sujets à changement durant le cycle de développement.

Les lignes visibles ci-dessous ajoutent un Ctrl+Q comme raccourci clavier pour Quitter et activer l’option *Distance restante* par défaut.

Exemple de fichier .axisrc<span id="cap:Exemple-de-fichier-axisrc"></span>

    root_window.bind("<Control-q>", "destroy .")
    help2.append(("Control-Q", "Quit"))

### Éditeur externe

En définissant: \[DISPLAY\]EDITOR , les options de menu: *Fichier* → *Éditer* ainsi que *Fichier* → *Éditer la table d’outils*, deviennent accessibles. Deux valeurs qui fonctionnent bien: EDITOR=gedit et *EDITOR=gnome-terminal -e nano*.

### Panneau de contrôle virtuel

AXIS peut afficher un panneau de commande virtuel personnalisé dans le volet de droite. Il est possible d’y placer des boutons, des indicateurs qui afficheront des données et plus encore. Voir le manuel de l’intégrateur.

### Commentaires spéciaux<span id="sub:Commentaires-speciaux"></span>

Les commentaires spéciaux peuvent être insérés dans le fichier de G-code pour contrôler le comportement de l’affichage d’AXIS. Pour limiter l’aperçu au seul affichage du parcours d’outil, utiliser ces commentaires spéciaux. Rien ne s’affichera entre le commentaire (AXIS,hide) et le commentaire (AXIS,show) sauf le parcours d’outil. Les (AXIS,hide) et (AXIS,show) doivent être utilisés par paires avec le (AXIS, hide) en premier. Tout ce qui est après un (AXIS,stop) ne sera pas visible.

Ces commentaires sont utiles pour désencombrer l’affichage d’aperçu (Par exemple pendant le débogage d’un grand fichier G-code, on peut désactiver l' aperçu sur certaines parties qui sont déjà fonctionnelles).

-   (AXIS,hide) Arrête le parcours d’outil (à placer en premier)

-   (AXIS,show) Reprend le parcours d’outil (il faut suivre un cache)

-   (AXIS,stop) Arrête le parcours d’outil d’ici à la fin du fichier.

-   (AXIS,notify,le\_texte) Affiche *le\_texte* à l'écran, comme une info. Cet affichage peut être très utile lors du pré-affichage du parcours d’outil, quand les commentaires (debug,message) ne sont pas affichés.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


