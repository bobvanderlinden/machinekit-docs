L’interface graphique TkLinuxcnc
================================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:TkLinuxcnc"></span>

Introduction
------------

TkLinuxcnc est l’interface utilisateur graphique la plus populaire après Axis, c’est l’interface traditionnelle de Machinekit. Elle est écrite en Tcl et utilise le toolkit Tk pour l’affichage. Le fait d'être écrite en TCL la rend vraiment très portable (elle fonctionne sur une multitude de plateformes).

![](images/tklinuxcnc_fr.png)

Figure 1. L’affichage de TkLinuxcnc

Utiliser TkLinuxcnc
-------------------

Pour sélectionner l’interface graphique TkLinuxcnc avec Machinekit, éditer le fichier .ini et dans la section \[DISPLAY\] modifier l’affichage comme ci-dessous:

    DISPLAY = tklinuxcnc

Puis, lancer Machinekit et choisir ce fichier ini. La configuration qui se trouve dans *sim/tklinuxcnc/tklinuxcnc.ini* est déjà configurée pour utiliser TkLinuxcnc comme interface utilisateur.

Quand Machinekit est lancé avec TkLinuxcnc, une fenêtre [comme celle-ci s’affiche](#cap:affichage-TkLinuxcnc).

### Une session typique avec TkLinuxcnc

1.  Lancer Machinekit et sélectionner un fichier de configuration.

2.  Libérer l'*Arrêt d’Urgence* et mettre la machine en marche (en pressant F1 puis F2).

3.  Faire l'*Origine Machine* de chacun des axes.

4.  Charger un fichier d’usinage.

5.  Brider le brut à usiner sur la table.

6.  Faire l'*Origine Pièce* de chacun des axes, à l’aide du jog ou en introduisant une valeur de décalage d’origine après un clic droit sur le nom d’un axe.

7.  Lancer le programme.

8.  Pour refaire une autre pièce identique, reprendre à l'étape 6. Pour usiner une pièce différente, reprendre à l'étape 4. Quand c’est terminé, quitter Machinekit.

Éléments affichés par TkLinuxcnc
--------------------------------

La fenêtre TkLinuxcnc contient les éléments suivants:

-   Une barre de menu permettant diverses actions;

-   Un jeu de boutons permettant d’agir sur le mode de travail, Marche/Arrêt de la broche et autres éléments;

-   Une barre de statut pour l’affichage des différents offsets;

-   Une zone d’affichage des coordonnées;

-   Un jeu de curseurs pour contrôler la *vitesse de jog*, le *Correcteur de vitesse d’avance* et le *Correcteur de vitesse broche* qui permettent d’augmenter ou de diminuer ces vitesses ;

-   Une boîte d’entrée de données manuelles;

-   Une barre de statut affichant le bloc de programme actif, G-codes, M-codes, mots F et S;

-   Les boutons relatifs à l’interpréteur;

-   Une zone d’affichage de texte montrant le G-code du programme chargé.

### Boutons principaux

Dans la première ligne de la gauche vers la droite et cycliquement:

1.  Marche Machine: *Arrêt d’Urgence* *Arrêt d’Urgence relâché* / *Marche*

2.  Bascule gouttelettes

3.  Broche moins vite

4.  Direction de rotation de la broche *Arrêt broche* / *Broche sens horaire* / *Broche sens anti-horaire*

5.  Broche plus vite

6.  Annuler

puis dans la deuxième ligne:

1.  Mode de marche: *MANUEL* / *MDI* / *AUTO*

2.  Bascule d’arrosage

3.  Bascule du contrôle frein de broche

### Barre de statut des différents offsets

Elle affiche, l’offset de rayon de l’outil courant (sélectionné avec Txx M6), l’offset éventuel de longueur d’outil si il est actif et les offsets de travail (ajustables par un clic droit sur les coordonnées).

### Zone d’affichage des coordonnées

La partie principale affiche la position courante de l’outil. La couleur varie selon l'état de l’axe. Si l’axe n’est pas référencé il est affiché en caractères jaunes. Si il est référencé il s’affiche en vert. Si il est en erreur, TkLinuxcnc l’affiche en rouge pour montrer un défaut. (par exemple si un contact de fin de course est activé).

Pour interpréter correctement les différentes valeurs, se référer aux boutons de droite. Si la position est *Machine*, alors la valeur affichée est en coordonnées machine. Si elle est *Relative*, la valeur affichée est en coordonnées pièce. Deux autres en dessous indiquent *actuelle* ou *commandée*. Actuelle fait référence aux valeurs retournées par les codeurs (si la machine est équipée de servomoteurs) et *commandée* fait référence à la position à atteindre envoyée aux moteurs. Ces valeurs peuvent différer pour certaines raisons: Erreur de suivi, bande morte, résolution d’encodeur ou taille de pas. Par exemple, si un mouvement est commandé vers X0.08 sur une fraiseuse, mais qu’un pas moteur fait 0.03, alors la position *Commandée* sera 0.03 mais la position *Actuelle* sera soit 0.06 (2 pas) soit 0.09 (3 pas).

Deux autres boutons permettent de choisir entre la vue *Articulation* et la vue *Globale*. Cela a peu de sens avec les machines de type normal (cinématiques triviales), mais se révèle très utile sur les machines avec des cinématiques non triviales telles que les robots ou plateforme de Stewart. (Des informations plus complètes se trouvent dans le manuel de l’intégrateur).

#### Parcours d’outil

Quand la machine se déplace, elle laisse un tracé appelé parcours d’outil. La fenêtre d’affichage du parcours d’outil s’active via le menu *Vues → Parcours d’outil*.

### Contrôle en automatique

![](images/tklinuxcnc_interp_fr.png)

Figure 2. Interpréteur de TkLinuxcnc

#### Boutons de contrôle

Les boutons de contrôle de la partie inférieure de TkLinuxcnc, visibles sur l’image ci-dessus, sont utilisés pour l’exécution du programme:

-   *Ouvrir* pour charger un fichier,

-   *Lancer* pour commencer l’usinage,

-   *Pause* pour stopper temporairement l’usinage,

-   *Reprise* pour reprendre un programme mis en pause,

-   *Pas à pas* pour avancer d’une seule ligne de programme,

-   *Vérifier* pour vérifier si il contient des erreurs,

-   *Arrêt optionnel* pour basculer l’arrêt optionnel, si ce bouton est vert l’exécution du programme est stoppée quand un code M1 est rencontré.

#### Zone texte d’affichage du programme

Quand un programme est lancé, la ligne courante est affichée en surbrillance blanche. L’affichage du texte défile automatiquement pour montrer la ligne courante.

### Contrôle en manuel

#### Touches implicites

TkLinuxcnc permet les déplacements manuels de la machine. Cette action s’appelle le *jog*. Premièrement, sélectionner l’axe à déplacer en cliquant dessus. Puis, cliquer et maintenir les boutons **+** ou **-** selon la direction du mouvement souhaité. Les quatre premiers axes peuvent aussi être déplacés à l’aide des touches fléchées pour les axes X et Y, Pg.préc et Pg.suiv pour l’axe Z et les touches \[ et \] pour l’axe A.

Si *Continu* est activé, le mouvement sera continu tant que la touche sera pressée, si une valeur d’incrément est sélectionnée, le mobile se déplacera exactement de cette valeur à chaque appui sur la touche ou à chaque clic. Les valeurs disponibles sont:

    1.0000 0.1000 0.0100 0.0010 0.0001

En cliquant le bouton *Origine* ou en pressant la touche Origine, l’axe actif est référencé sur son origine machine. Selon la configuration, la valeur de l’axe peut être simplement mise à la position absolue 0.0, ou la machine peut se déplacer vers un point spécifique matérialisé par le *contact d’origine*. Voir le manuel de l’intégrateur pour plus de détails sur les prises d’origine.

En cliquant le bouton *Dépassement de limite*, la machine permet un jog temporaire pour même si l’axe à franchi une limite d’axe fixée dans le fichier .ini. Noter que si *Dépassement de limite* est activé il s’affiche en rouge.

![](images/tkemc-override-limits.png)

Figure 3. Exemple de dépassement de limite et incréments de jog

#### Le groupe de boutons *Broche*

Le bouton central du dessus sélectionne le sens de rotation de la broche: Anti-horaire, Arrêt, Horaire. Les boutons fléchés augmentent ou diminuent la vitesse de rotation. Le bouton central du dessous permet d’engager ou de relâcher le frein de broche. Selon la configuration de la machine, les items de ce groupe ne sont peut être pas tous visibles.

#### Le groupe de boutons *Arrosage*

Ces deux boutons permettent d’activer ou non les lubrifiants *Gouttelettes* et *Arrosage*. Selon la configuration de la machine, les items de ce groupe ne sont peut être pas tous visibles.

### Entrée manuelle de G-code (MDI)

L’entrée manuelle de données (aussi appelée MDI), permet d’entrer et d’exécuter des lignes de G-code, une à la fois. Quand la machine n’est pas en marche ni mise en mode MDI, l’entrée de code n’est pas possible.

![](images/tkemc-mdi.png)

Figure 4. Le champ de saisie des entrées manuelles

#### MDI:

Le mode MDI permet d’exécuter une commande en G-code en pressant la touche *Entrée*.

#### G-Codes actifs

Ce champs montre les *codes modaux* actuellement actifs dans l’interpréteur. Par exemple, **G54** indique que le système de coordonnées courant est celui de G54 et qu’il s’applique à toutes les coordonnées entrées.

### Vitesse de Jog

En déplaçant ce curseur, la vitesse de jog peut être modifiée. Le nombre indique une vitesse en unités par minute. Le champs de texte est cliquable. Un clic ouvre un dialogue permettant d’entrer un nombre.

### Correcteur de vitesse d’avance travail

En déplaçant ce curseur, la vitesse d’avance travail peut être modifiée. Par exemple, si la vitesse d’avance travail du programme est **F600** et que le curseur est placé sur 120%, alors la vitesse d’avance travail sera de 720. Le champs de texte est cliquable. Un clic ouvre un dialogue permettant d’entrer un nombre.

### Correcteur de vitesse de broche

Le fonctionnement de ce curseur est le même que celui de la vitesse d’avance, mais il contrôle la vitesse de rotation de la broche. Si le programme demande S500 (broche à 500 tr/mn) et que le curseur est placé sur 80%, alors la vitesse de broche résultante sera de 400 tr/mn. Le minimum et le maximum pour ce curseur sont définis dans le fichier ini. Par défaut le curseur est placé sur 100%. Le champs de texte est cliquable. Un clic ouvre un dialogue permettant d’entrer un nombre.

Raccourcis clavier
------------------

La plupart des actions de TkLinuxcnc peuvent être accomplies au clavier. Beaucoup des raccourcis clavier ne sont pas accessibles en mode MDI.

Les raccourcis clavier les plus fréquemment utilisés sont montrés dans la table ci-dessous.

<table>
<caption>Tableau 1. Les raccourcis clavier les plus utilisés</caption>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Touche</th>
<th align="left">Action</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>F1</p></td>
<td align="left"><p>Bascule de l’Arrêt d’Urgence</p></td>
</tr>
<tr class="even">
<td align="left"><p>F2</p></td>
<td align="left"><p>Marche/Arrêt machine</p></td>
</tr>
<tr class="odd">
<td align="left"><p>*, 1 .. 9, 0</p></td>
<td align="left"><p>Correcteur vitesse d’avance 0% à 100%</p></td>
</tr>
<tr class="even">
<td align="left"><p>X, *</p></td>
<td align="left"><p>Active le premier axe</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Y, 1</p></td>
<td align="left"><p>Active le deuxième axe</p></td>
</tr>
<tr class="even">
<td align="left"><p>Z, 2</p></td>
<td align="left"><p>Active le troisième axe</p></td>
</tr>
<tr class="odd">
<td align="left"><p>A, 3</p></td>
<td align="left"><p>Active le quatrième axe</p></td>
</tr>
<tr class="even">
<td align="left"><p>Origine</p></td>
<td align="left"><p>POM de l’axe actif</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Gauche, Droite</p></td>
<td align="left"><p>Jog du premier axe</p></td>
</tr>
<tr class="even">
<td align="left"><p>Haut, Bas</p></td>
<td align="left"><p>Jog du deuxième axe</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Pg.prec, Pg.suiv</p></td>
<td align="left"><p>Jog du troisième axe</p></td>
</tr>
<tr class="even">
<td align="left"><p>[, ]</p></td>
<td align="left"><p>Jog du quatrième axe</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Echap</p></td>
<td align="left"><p>Arrête l’exécution</p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


