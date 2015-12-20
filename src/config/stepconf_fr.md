Assistant graphique de configuration StepConf
=============================================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Assistant-graphique-StepConf"></span>

Introduction
------------

Machinekit est capable de contrôler un large éventail de machines utilisant de nombreuses interfaces matérielles différentes.

Stepconf est un programme qui génère des fichiers de configuration Machinekit pour une classe spécifique de machine CNC: celles qui sont pilotées via un, ou plusieurs ports parallèles standards et contrôlées par des signaux de type pas/direction (step/dir).

Stepconf est installé en même temps que Machinekit et un lanceur se trouve dans le menu *Application → CNC → Machinekit StepConf*.

Stepconf place les fichiers qu’il crée dans le répertoire *~/machinekit/config* pour y stocker les paramètres de chaque configuration. Lorsque quelque chose doit être modifié, il faut choisir le fichier correspondant au nom de la configuration et portant l’extension .stepconf.

L’Assistant Stepconf a besoin, au minimum, d’une résolution de 800 x 600 pour que les boutons sur le bas des pages soient apparents.

Page d’accueil
--------------

![](images/stepconf-config_fr.png)

Figure 1. La page d’accueil de stepconf

-   *Créer une nouvelle configuration* - Créera une configuration nouvelle.

-   *Modifier une configuration déjà créée* - Modifiera une configuration existante, déjà créée avec Stepconf. Après la sélection de celle-ci un sélecteur de fichier s’ouvre pour y choisir le fichier .stepconf à modifier. Si des modifications sont faites aux fichiers .hal et .ini avec un autre éditeur, ils ne seront plus utilisables par Stepconf. Les modifications de custom.hal et de custom\_postgui.hal, par contre, sont conservées par Stepconf.

-   *Créer un lien* - Placera un lien sur le bureau, pointant sur le dossier des fichiers de configuration.

-   *Créer un lanceur* - Placera un lanceur sur le bureau pour démarrer l’application avec sa configuration.

Informations machine
--------------------

![](images/stepconf-basic_fr.png)

Figure 2. Page d’informations sur la machine

-   *Nom de la machine* - Choisir un nom pour la machine. Utiliser uniquement des lettres majuscules, minuscules, des chiffres ou "-" et "\_".

-   *Configuration des axes* - Choisir les axes correspondants à la machine: XYZ (fraiseuse 3 axes), XYZA (fraiseuse 4 axes) ou XZ (tour).

-   *Unité machine* - Choisir entre le pouce et le millimètre. Toutes les questions suivantes (telles que la longueur des courses, le pas de la vis, etc) devront obtenir des réponses dans l’unité choisie ici.

-   *Caractéristiques du pilote* - Si un des pilotes énumérés dans la liste déroulante peut être utilisé, cliquer sur son nom. Sinon, trouver les 4 valeurs de timing dans la fiche de caractéristiques fournie par le fabricant du pilote et les saisir. Si la fiche donne des valeurs en microsecondes, les multiplier par 1000. pour les convertir en nanosecondes. Par exemple, pour 4.5µs saisir 4500ns.

Une liste de certains des pilotes pas à pas les plus populaires, avec leurs valeurs caractéristiques de timing, se trouve sur le wiki de Machinekit.org à la page [Timming des pilotes](http://wiki.machinekit.org/cgi-bin/wiki.pl?Stepper_Drive_Timing).

D'éventuels traitements des signaux, une opto-isolation ou des filtres RC, peuvent imposer des contraintes de temps supplémentaires aux signaux, il convient de les ajouter à celles du pilote. La Configuration Machinekit pour Sherline est déjà réglée.

-   *Step Time* - Durée de la largeur de l’impulsion de pas à l'état *on*, en nanosecondes.

-   *Valeur Space d’un pas* - Temps entre deux impulsions de pas, en nanosecondes.

-   *Direction Hold* - Durée de maintien du signal après un changement de direction, en nanosecondes.

-   *Réglage direction* - Délai avant le changement de direction après la dernière impulsion de pas, en nanosecondes.

-   *Adresse de base du premier port parallèle* - Généralement l’adresse 0x378 est correcte pour le premier port. Le premier port a toujours ses broches 2 à 9 configurées en sortie.

-   *Adresse du second port parallèle* - Si un second port parallèle est nécessaire, entrer son adresse ici. Si ce port est intégré à la carte mère il est possible de vérifier leur ordre dans le BIOS, habituellement 0x378 0x278 0x3bc. Attention les cartes additionnelles ont d’autres adresses. Dans ce cas, la commande lspci -v dans un terminal peux aider, si le nom du chipset de la carte est connu. Plus de détails à ce sujet sont disponibles dans le manuel de l’intégrateur. Le bouton situé à droite du champs d’adresse du port, permet de choisir le sens des broches 2 à 9, soit comme étant des entrée, soit comme étant des sorties.

-   *Adresse du troisième port parallèle* - Si un troisième port parallèle est nécessaire, entrer son adresse ici. Si ce port est intégré à la carte mère il est possible de vérifier leur ordre dans le BIOS, habituellement 0x378 0x278 0x3bc. Attention les cartes additionnelles ont d’autres adresses. Dans ce cas, la commande lspci -v dans un terminal peux aider, si le nom du chipset de la carte est connu. Plus de détails à ce sujet sont disponibles dans le manuel de l’intégrateur. Le bouton situé à droite du champs d’adresse du port, permet de choisir le sens des broches 2 à 9, soit comme étant des entrée, soit comme étant des sorties.

-   *Période de base minimale* - En se basant sur les caractéristiques du pilote et sur le résultat du test de latence, Stepconf détermine automatiquement la période de base (BASE\_PERIOD) la plus petite utilisable et l’affiche ici.

-   *Fréquence maxi des pas* - Affiche la valeur calculée de la fréquence maximum des pas que la machine devrait atteindre avec les paramètres de cette configuration.

-   *Base Period Maximum Jitter* - Après un test de latence, entrer ici la valeur retournée dans la colonne "Max Jitter" et à la ligne "Base thread". Cette valeur correspond à la latence maximale du PC testé. Pour exécuter directement un test de latence cliquer sur le bouton *Test de latence*. Lire le [chapitre sur le test de latence](#cha:test-de-latence) pour tous les détails concernant ce test.

-   *Dialogue à l'écran pour le changement d’outil* - Si cette case est cochée, Machinekit va faire une pause et ouvrir un dialogue pour charger l’outil &lt;n&gt; lorsque qu’un G-Code M6 T&lt;n&gt; sera rencontré. Laisser cette case cochée sauf si le support d’un changeur d’outils automatique est prévu dans un fichier personnalisé HAL.

Options de configuration avancée
--------------------------------

![](images/stepconf-advanced_fr.png)

Figure 3. Configuration avancée

-   *Inclure l’interface Halui* - Ajoutera l’interface utilisateur Halui. Voir le manuel de l’intégrateur pour plus d’informations sur Halui.

-   *Inclure un panneau pyVCP* - Ceci ajoutera un panneau pyVCP de base, avec son fichier de configuration sur lequel il sera possible de travailler. Quelques options sont disponibles pour enrichir le panneau grâce à des cases à cocher. Voir le manuel de l’intégrateur pour plus d’information sur pyVCP.

-   *Inclure l’API ClassicLadder* - Cette option ajoutera l’automate programmable en logique à contacts ClassicLadder. Un certain nombre d’options sont disponibles pour enrichir l’API grâce à des cases à cocher. L'éditeur de programme ladder est accessible par le bouton *Editer prog. ladder* Voir le manuel de l’intégrateur pour plus d’information sur ClassicLadder.

Réglage du port parallèle
-------------------------

![](images/stepconf-pinout_fr.png)

Figure 4. Page de réglage du port parallèle

-   *Sorties (PC vers machine)* - Pour chacune des broches, choisir le signal correspondant au brochage entre le port parallèle et l’interface matérielle. Cocher la case inverser si le signal est inversé (0V pour vrai/actif, 5V pour faux/inactif).

-   *Sorties présélectionnées* - Réglage automatique des pins 2 à 9 Direction sur les pins 2, 4, 6, 8, selon le *type Sherline* Direction sur les pins 3, 5, 7, 9, selon le *type Xylotex*

-   *Entrées et sorties* - Les entrées ou les sorties non utilisées doivent être placées sur Inutilisé.

-   *Sortie arrêt d’urgence* - Sélectionnable dans la liste déroulante des sorties. La sortie d’arrêt d’urgence est utilisée pour actionner l’organe de coupure du circuit de puissance de la machine. Le contact de cet organe est câblé en série avec les contacts des boutons d’arrêt d’urgence extérieurs ainsi qu’avec tous les contacts compris dans la boucle d’arrêt d’urgence.

-   *Entrées (machine vers PC)* - Ces choix se font dans la liste déroulante des entrées.

-   *Pompe de charge* - Si la carte de contrôle accepte un signal pompe de charge, dans la liste déroulante des sorties, sélectionner *Pompe de charge* sur la sortie correspondant à l’entrée Pompe de charge de la carte de contrôle. La sortie pompe de charge sera connectée en interne par Stepconf. Le signal de pompe de charge sera d’environ la moitié de la fréquence maxi des pas affichée sur la page des informations machine.

Configuration des axes
----------------------

![](images/stepconf-axis_fr.png)

Figure 5. Page de configuration des axes

-   *Nombre de pas moteur par tour* - Nombre de pas entiers par tour de moteur. Si l’angle d’un pas en degrés est connu (par exemple, 1.8 degrés), diviser 360 par cet angle pour obtenir le nombre de pas par tour du moteur.

-   *Micropas du pilote* - Le nombre de micropas produits par le pilote. Entrer par exemple 2 pour le demi pas ou une des valeurs permise par le pilote du moteur.

-   *Dents des poulies* - Si entre le moteur et la vis un réducteur poulie/courroie est présent, entrer ici le nombre de dents de chacune des poulies. Pour un entrainement direct, entrer 1:1.

-   *Pas de la vis* - Entrer ici le pas de la vis. Si le pouce a été choisi comme unité, entrer ici le nombre de filets par pouce. Si le mm a été choisi, entrer ici le pas du filet en millimètres. Si la vis est à plusieurs filets, déterminer de combien se déplace le mobile par tour de vis et entrer cette valeur ici. Si la machine se déplace dans la mauvaise direction, entrer une valeur négative au lieu d’une positive, et vice-versa.

-   *Vitesse maximale* - Entrer ici la vitesse de déplacement maximale de l’axe, en unités par seconde.

-   *Accélération maximale* - Les valeurs correctes pour ces deux entrées ne peuvent être déterminées que par l’expérimentation. Consulter [le calcul de la vitesse](#sec:Trouver-Vitesse-Maximale) pour trouver la vitesse et [le calcul de l’accélération](#sec:Trouver-Acceleration-Maximale) pour trouver l’accélération maximale.

-   *Emplacement de l’origine machine* - Position sur laquelle la machine se place après avoir terminé la procédure de prise d’origine de cet axe. Pour les machines sans contact placé au point d’origine, c’est la position à laquelle l’opérateur place la machine en manuel, avant de presser le bouton de *POM des axes*. Si des capteurs de fin de course sont utilisés pour la prise d’origine, le point d’origine ne doit pas se trouver au même coordonnées que le capteur. Une erreur de limite simultanée à l’origine surviendrait.

-   *Course de la table* - Étendue de la course que le programme en G-code ne doit jamais dépasser. L’origine machine doit être située à l’intérieur de cette course. En particulier, avoir un point d’origine exactement égal à cette course est une configuration incorrecte.

-   *Position du contact d’origine machine* - Position à laquelle le contact d’origine machine est activé ou relâché pendant la procédure de prise d’origine machine. Ces entrées et les deux suivantes, n’apparaissent que si les contacts d’origine ont été sélectionnés dans le réglage des broches du port parallèle.

-   *Vitesse de recherche de l’origine* - Vitesse utilisée pendant le déplacement vers le contact d’origine machine. Si le contact est proche d’une limite physique de déplacement de la table, cette vitesse doit être suffisamment basse pour permettre de décélérer et de s’arrêter avant d’atteindre la butée mécanique et cela, malgré l’inertie du mobile. Si le contact est fermé par la came sur une faible longueur de déplacement (au lieu d'être fermé depuis son point de fermeture jusqu’au bout de le course), cette vitesse doit être réglée pour permettre la décélération et l’arrêt, avant que le contact ne soit dépassé et ne s’ouvre à nouveau. La prise d’origine machine doit toujours commencer du même côté du contact. Si la machine se déplace dans la mauvaise direction au début de la procédure de prise d’origine machine, rendre négative la valeur de *Vitesse de recherche de l’origine*.

-   *Dégagement du contact d’origine* - Choisir *Identique* pour que la machine reparte d’abord en arrière pour dégager le contact, puis revienne de nouveau vers lui à très petite vitesse. La seconde fois que le contact se ferme, la position de l’origine machine est acquise. Choisir *Opposition* pour que la machine reparte en arrière à très petite vitesse jusqu’au dégagement du contact. Quand le contact s’ouvre, la position de l’origine machine est acquise.

-   *Temps pour accélérer à la vitesse maxi* - Temps en secondes, calculé en fonction des paramètres renseignés précédemment.

-   *Distance pour accélérer à la vitesse maxi* - Distance en mm, calculée en fonction des paramètres renseignés précédemment.

-   *Fréquence des impulsions à la vitesse maxi* - Informations calculées sur la base des informations entrées précédemment. Il faut rechercher la plus haute fréquence des impulsions à la vitesse maxi possible, elle détermine la période de base: BASE\_PERIOD. Des valeurs supérieures à 20000Hz peuvent toutefois provoquer des ralentissements importants de l’ordinateur, voir même son blocage (La plus grande fréquence utilisable variera d’un ordinateur à un autre)

-   *Échelle de l’axe* - Le nombre qui sera utilisé dans le fichier ini \[SCALE\]. C’est le nombre de pas moteur par unité utilisateur.

-   *Test de cet axe* - Ouvre une fenêtre permettant de tester les paramètres pour chaque axe. Il est possible de modifier par expérimentation certaines données et de les reporter dans la configuration.

-   *Adresse du second port parallèle* - Si un second port parallèle est nécessaire, entrer son adresse ici. Si les ports sont intégrés à la carte mère il est possible de vérifier dans le BIOS, habituellement 0x378 0x278 0x3bc. Attention les cartes additionnelles ont d’autres adresses. Dans ce cas, la commande lspci -v dans un terminal peux aider, si le nom du chipset de la carte est connu. Plus de détails à ce sujet sont disponibles dans le manuel de l’intégrateur.

Tester cet axe
--------------

![](images/stepconf-test_fr.png)

Figure 6. Tester cet axe

Tester cet axe et un test simple pour définir les signaux de directions et de pas, ainsi que les valeurs d’accélération et de vitesse.

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
<td align="left">Pour pouvoir utiliser ce test d’axe, il sera peut-être nécessaire de valider manuellement l’axe à tester. Si le driver utilise une pompe de charge, il faudra la bi-passer pour essayer les différentes valeurs de vitesse et d’accélération.</td>
</tr>
</tbody>
</table>

Trouver la vitesse maximale
---------------------------

Commencer avec une faible valeur d’accélération (par exemple, **`2 pouces/s2`** ou **`50 mm/s2`**) et la vitesse que espérée. En utilisant les boutons de jog, positionner l’axe vers son centre. Il faut être prudent, car avec peu d’accélération, la distance d’arrêt peut être très surprenante. Après avoir évalué le déplacement possible dans chaque direction en toute sécurité, entrer une distance dans le champs *Zone de test* garder à l’esprit qu’après un décrochage, le moteur peut repartir dans la direction inattendue. Puis cliquer sur *Lancer*. La machine commencera à aller et venir le long de cet axe. Dans cet essai, il est important que la combinaison entre l’accélération et la zone de test, permette à la machine d’atteindre la vitesse sélectionnée et de s’y déplacer au moins, sur une courte distance. La formule **`d = 0.5 * v * v/a`**, donne la distance minimale requise pour atteindre la vitesse de *croisière*. Si la sécurité est garantie, pousser sur la table dans la direction inverse du mouvement pour simuler les efforts de coupe. Si la table décroche, réduire la vitesse et recommencer le test. Si la machine ne présente aucun décrochage, cliquer sur le bouton *Lancer*. L’axe revient alors à sa position de départ. Si cette position est incorrecte, c’est que l’axe a calé ou a perdu des pas au cours de l’essai. Réduire la vitesse et relancer le test. Si la machine ne se déplace pas, cale, vibre ou perd des pas, même à faible vitesse, vérifier les éléments suivants:

-   Corriger les paramètres de temps des impulsions de commande.

-   Le brochage du port et la polarité des impulsions. Les cases *Inverser*.

-   La qualité des connexions et le blindage des câbles.

-   Les problèmes mécaniques avec le moteur, l’accouplement moteur, vis, raideurs etc.

Quand la vitesse à laquelle l’axe ne perd plus de pas et à laquelle les mesures sont exactes pendant le test a été déterminée, réduire cette vitesse de 10% et l’utiliser comme vitesse maximale pour cet axe.

Trouver l’accélération maximale
-------------------------------

Avec la vitesse maximale déterminée à l'étape précédente, entrer une valeur d’accélération approximative. Procéder comme pour la vitesse, en ajustant la valeur d’accélération en plus ou en moins selon le résultat. Dans cet essai, il est important que la combinaison de l’accélération et de la zone de test permette à la machine d’atteindre la vitesse sélectionnée. Une fois que la valeur à laquelle l’axe ne perd plus de pas pendant le test a été déterminée, la réduire de 10% et l’utiliser comme accélération maximale pour cet axe.

Configuration de la broche
--------------------------

![](images/stepconf-spindle_fr.png)

Figure 7. Page configuration de la broche<span id="cap:Page-Configuration-de-la-broche"></span>

Ces options ne sont accessibles que quand *PWM broche*, *Phase A codeur broche* ou *index broche* sont configurés dans le réglage du port parallèle.

Contrôle de la vitesse de broche
--------------------------------

Si *PWM broche* apparaît dans le réglage du port parallèle, les informations suivantes doivent être renseignées:

-   *Fréquence PWM* - La fréquence porteuse du signal PWM (modulation de largeur d’impulsions) du moteur de broche. Entrer 0 pour le mode PDM (modulation de densité d’impulsions), qui est très utile pour générer une tension de consigne analogique. Se reporter à la documentation du variateur de broche pour connaître la valeur appropriée.

-   *Vitesse 1 et 2, PWM 1 et 2* - Le fichier de configuration généré utilise une simple relation linéaire pour déterminer la valeur PWM correspondant à une vitesse de rotation. Si les valeurs ne sont pas connues, elles peuvent être déterminées. Voir la section sur [la calibration de la broche](#sub:Determiner-broche-Etalonnage-broche).

Mouvement avec broche synchronisée (filetage sur tour, taraudage rigide)
------------------------------------------------------------------------

Lorsque les signaux appropriés, provenant d’un codeur de broche, sont connectés au port parallèle, Machinekit peut être utilisé pour les usinages avec broche synchronisée comme le filetage ou le taraudage rigide. Ces signaux son:

-   *Index broche* - Également appelé PPR broche, c’est une impulsion produite à chaque tour de broche.

-   *Phase A broche* - C’est une suite d’impulsions carrées générées sur la voie A du codeur pendant la rotation de la broche. Le nombre d’impulsions pour un tour correspond à la résolution du codeur.

-   *Phase B broche* (optionnelle) - C’est une seconde suite d’impulsions, générées sur la voie B du codeur et décalées par rapport à celle de la voie A. L’utilisation de ces deux signaux permet d’accroitre l’immunité au bruit et la résolution d’un facteur 4.

Si *Phase A broche* et *Index broche* apparaissent dans le réglage des broches du port, l’information suivante doit être renseignée sur la page de configuration broche:

-   *Cycles par tour* - Le nombre d’impulsions par tour sur la broche Phase A broche.

-   *La vitesse maximale en filetage* - La vitesse de broche maximale utilisée en filetage. Pour exploiter un moteur de broche rapide ou un codeur ayant une résolution élevée, une valeur basse de BASE\_PERIOD est requise.

Calibrer la broche
------------------

Entrer les valeurs suivantes dans la page de configuration de la broche:

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Vitesse 1:</th>
<th align="left">0</th>
<th align="left">PWM 1:</th>
<th align="left">0</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Vitesse 2:</p></td>
<td align="left"><p>1000</p></td>
<td align="left"><p>PWM 2:</p></td>
<td align="left"><p>1</p></td>
</tr>
</tbody>
</table>

Finir les étapes suivantes de la configuration, puis lancer Machinekit avec cette configuration. Mettre la machine en marche et aller dans l’onglet Données manuelles, démarrer le moteur de broche en entrant: M3 S100. Modifier la vitesse de broche avec différentes valeurs comme: S800. Les valeurs permises vont de 1 à 1000.

Pour deux différentes valeurs de Sxxx, mesurer la vitesse de rotation réelle de la broche en tours/mn. Enregistrer ces vitesses réelles de la broche. Relancer Stepconf. Pour les Vitesses, entrer les valeurs réelles mesurées et pour les PWM, entrer la valeur Sxxx divisée par 1000.

Parce que la plupart des interfaces ne sont pas linéaires dans leur courbe de réponse, il est préférable de:

-   S’assurer que les deux points de mesure des vitesses en tr/mn ne soient pas trop rapprochés

-   S’assurer que les deux vitesses utilisées sont dans la gamme des vitesses utilisées généralement par la machine.

Par exemple, si la broche tourne entre 0tr/mn et 8000tr/mn, mais qu’elle est utilisée généralement entre 400tr/mn et 4000tr/mn, prendre alors des valeurs qui donneront 1600tr/mn et 2800tr/mn.

Terminer la configuration
-------------------------

Cliquer *Appliquer* pour enregistrer les fichiers de configuration. Ensuite, il sera possible de relancer ce programme et ajuster les réglages entrés précédemment.

Position des fins de course sur les axes
----------------------------------------

![](images/HomeAxisTravel.png)

La course de chaque axe est bien délimitée. Les extrémités physiques d’une course sont appelées les *butées mécaniques*, position **<span class="red">(a)</span>**.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Attention
</div></td>
<td align="left"><span class="red">Si une butée mécanique venait à être dépassée, la vis ou le bâti machine seraient détériorés!</span></td>
</tr>
</tbody>
</table>

Avant la butée mécanique se trouve un contact de fin de course **<span class="green">(b)</span>**. Si ce contact est rencontré pendant les opérations normales, Machinekit coupe la puissance du moteur. La distance entre le fin de course et la butée mécanique doit être suffisante pour permettre au moteur, dont la puissance a été coupée, de s’arrêter malgré l’inertie du mobile. Ces fins de course doivent détecter le mobile sur toutes la distance d’arrêt et ne pas se réactiver à cause d’un dépassement dû à l’inertie.

Avant le contact de fin de course se trouve une limite logicielle **<span class="blue">(d)</span>**. Cette limite logicielle est introduite après la prise d’origine machine. Si une commande manuelle ou un programme G-code dépasse cette limite, ils ne seront pas exécutés. Si un mouvement en jog ou en manuel cherche à dépasser la limite logicielle, il sera interrompu sur cette limite.

Le contact d’origine machine **<span class="purple">(c)</span>** peut être positionné n’importe où, le long d’une course entre les butées mécaniques. Si aucun mécanisme externe ne désactive la puissance moteur quand un contact de limite est enfoncé, un des contacts de fin de course peut être utilisé comme contact d’origine machine.

La position zéro **<span class="orange">(e)</span>** correspond au 0 de l’axe dans le système de coordonnées pièce, après que la prise d’origine pièce de cette axe ait été faite. La position zéro doit se trouver entre les deux limites logicielles pour que l’usinage soit possible. Sur les tours, le mode vitesse à surface constante requiert que la coordonnée **X=0** corresponde au centre de rotation de la broche quand aucun correcteur d’outil n’est actif.

La position de l’origine est la position, située le long de l’axe, sur laquelle le mobile sera déplacé à la fin de la séquence de prise d’origine. Cette position doit se situer entre les limites logicielles. En particulier, la position de l’origine ne doit jamais être égale à une limite logicielle. On place habituellement cette position au point le plus facile pour réaliser le changement d’outil.

Exploitation sans fin de course
-------------------------------

Une machine peut être utilisée sans contact de fin de course. Dans ce cas, seules les limites logicielles empêcheront la machine d’atteindre les butées mécaniques. Les limites logicielles n’opèrent qu’après que la POM (prise d’origine machine) soit faite sur la machine. Puisqu’il n’y a pas de contact, la machine doit être déplacée à la main et à l’œil, à sa position d’origine avant de presser le bouton *POM des axes* ou le sous-menu *Machine → Prises d’origines machine → POM de l’axe*. L’opérateur devra cocher chacun des axes individuellement pour faire la POM de chacun d’eux.

Exploitation sans contact d’origine
-----------------------------------

Une machine peut être utilisée sans contact d’origine machine. Si la machine dispose de contacts de fin de course, mais pas de contact d’origine machine, il est préférable d’utiliser le contact de fin de course comme contact d’origine machine (exemple, choisir *Limite mini + origine X* dans le réglage du port). Si la machine ne dispose d’aucun contact, ou que le contact de fin de course n’est pas utilisable pour une autre raison, alors la prise d’origine machine peut toujours être réalisée à la main. Faire la prise d’origine à la main n’est certes pas aussi reproductible que sur des contacts, mais elle permet tout de même aux limites logicielles d'être utilisables.

Câblage des contacts de fin de course et d’origine machine
----------------------------------------------------------

Le câblage idéal des contacts externes serait une entrée par contact. Toutefois, un seul port parallèle d’ordinateur offre un total de 5 entrées, alors qu’il n’y a pas moins de 9 contacts sur une machine 3 axes. Au lieu de cela, plusieurs contacts seront câblés ensembles, selon diverse combinaisons, afin de nécessiter un plus petit nombre d’entrées.

Les figures ci-dessous montrent l’idée générale du câblage de plusieurs contacts à une seule broche d’entrée. Dans chaque cas, lorsqu’un contact est actionné, la valeur vue sur l’entrée va passer d’une logique haute à une logique basse. Cependant, Machinekit s’attend à une valeur VRAIE quand un contact est fermé, de sorte que les cases Inverser correspondantes devront être cochées sur la page de réglage du port parallèle. Une résistance de rappel est nécessaire dans le circuit pour tirer l’entrée au nivaux haut. La valeur typique pour un port parallèle est de 47K. Une bonne sécurité utilise des contacts normalement fermés sans pièce de commande souple.

![](images/switch-nc-series_fr.png)

Figure 8. Contacts normalement fermés<span id="cap:Contacts-Normalement-Fermes"></span>

Câblage de contacts NC en série (schéma simplifié)

![](images/switch-no-parallel_fr.png)

Figure 9. Contacts normalement ouverts<span id="cap:Contacts-Normalement-Ouverts"></span>

Câblage de contacts NO en parallèle (schéma simplifié)

Les combinaisons suivantes sont permises dans Stepconf:

-   Les contacts d’origine machine de tous les axes combinés.

-   Les contacts de fin de course de tous les axes combinés.

-   Les contacts de fin de course d’un seul axe combinés.

-   Les contacts de fin de course et le contact d’origine machine d’un seul axe combinés.

-   Un seul contact de fin de course et le contact d’origine machine d’un seul axe combinés.

Les deux dernières combinaisons sont également appropriées quand le type contact + origine est utilisé.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


