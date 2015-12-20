Particularités des tours
========================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Tour-Specifiques"></span>

Ce chapitre va regrouper les informations spécifiques aux tours, il est encore en cours de rédaction.

Mode tour
---------

Si l’interface graphique Axis est utilisée et que la machine est un tour, pour qu’Axis représente les axes et la position de l’outil correctement il conviendra d'éditer le fichier *ini* et de modifier la section \[DISPLAY\] comme ceci:

    [DISPLAY]

    # Pour indiquer à Axis que la machine est un tour (lathe).
    LATHE = TRUE

Le mode *tour* dans Axis ne fixe pas le plan par défaut comme étant G18 (XZ). Il est nécessaire de l’ajouter dans le préambule de tout programmer G-code ou, encore mieux, de l’ajouter directement dans le fichier *ini* comme cela:

    [RS274NGC]

    # G-code modaux pour initialiser l'interpréteur
    RS274NGC_STARTUP_CODE = G18 G20 G90

Fichier d’outils
----------------

La table d’outils est un fichier texte qui contient les informations de chaque outil. Ce fichier se trouve dans le même répertoire que le fichier *ini*, il est appelé *tool.tbl* par défaut. Les outils peuvent être dans un changeur d’outils ou simplement changés manuellement. Le fichier peut être édité avec un éditeur de texte ou être mis à jour en utilisant G10 L1, L10, L11. Axis peut aussi lancer l'éditeur de texte du système en y chargeant la table, l’opérateur peut ainsi intervenir directement sur les valeurs des outils. Le nombre maximum d’outils dans la table d’outils est de 56. Les numéros d’outil et de poches peuvent aller jusqu'à 99999.

Les versions antérieures de Machinekit avaient deux différents formats de table d’outils pour les fraiseuses et les tours, mais depuis la version 2.4.x, un format de table d’outil unique est utilisé. Il faut juste ignorer les parties de la table d’outils qui ne concernent pas la machine. Plus d’informations [ici sur le format de la table d’outils](#sec:Tool-Table-Format).

Orientations des outils de tournage
-----------------------------------

La figure suivante montre les angles d’orientations des outils de tour ainsi que les informations sur l’angle frontal de l’arête de coupe (FRONTANGLE) et l’angle arrière de l’arête de coupe (BACKANGLE). Les positions vont croissantes dans le sens horaire par rapport à une ligne parallèle à l’axe Z, le zéro étant côté Z+.

![](images/tool_positions_fr.png)

Figure 1. Orientations des outils de tournage

Les images ci-dessous montrent la représentation qu’Axis donne des orientations de l’outil, en se référant à la figure ci-dessus.

Outil dans les positions 1, 2, 3 et 4<span id="fig:Outil-Positions-1-2-3-4"></span>

![](images/tool_pos_1.png)

![](images/tool_pos_2.png)

![](images/tool_pos_3.png)

![](images/tool_pos_4.png)

Outil dans les positions 5, 6, 7 et 8<span id="fig:Outil-Positions-5-6-7-8"></span>

![](images/tool_pos_5.png)

![](images/tool_pos_6.png)

![](images/tool_pos_7.png)

![](images/tool_pos_8.png)

Correction d’outil
------------------

Quand AXIS est utilisé sur un tour, il est possible de corriger l’outil sur les axes X et Z. Les corrections sont alors introduites directement dans la table d’outils en utilisant le bouton *Toucher* et sa fenêtre de dialogue.

### Offset d’outil en X

L’offset X pour chaque outil correspond à un décalage de l’axe de la broche.

Une méthode consiste à prendre un outil de tournage standard et usiner un diamètre. Mesurer exactement ce diamètre puis, sans toucher à l’axe X, dans la fenêtre qui s’ouvre, après un appui sur le bouton *Toucher*, saisir le diamètre mesuré, ou le rayon si c’est le mode en cours. Ensuite, à l’aide d’encre à tracer ou d’un marqueur, recouvrir une zone sur la pièce, faire tangenter l’outil à cet endroit pour qu’il touche juste la surface encrée, ajuster alors l’offset X au diamètre mesuré de la pièce en utilisant le bouton *Toucher*. S’assurer que le rayon de bec est bien défini dans la table d’outils, pour que le point contrôlé soit correct. Le *Toucher* ajoute automatiquement un G43, de sorte que l’offset s’applique immédiatement à l’outil courant.

### Séquence typique de correction d’outil en X:

1.  Prise d’origine machine de chacun des axes, si ce n’est pas déjà fait.

2.  Déclarer l’outil avec *M6 Tn* dans lequel *n* est le numéro de l’outil courant, présent en table d’outils.

3.  Sélectionner l’axe X dans la fenêtre de l’onglet *Contrôle manuel (F3)*.

4.  Déplacer l’axe X sur une position connue ou prendre une passe de test puis mesurer le diamètre obtenu.

5.  Cliquer le bouton *Toucher* et choisir l’option *Table d’outils*, ce qui entrera la correction directement dans la table d’outil.

6.  Recommencer la même séquence pour corriger l’axe Z.

Remarque: si le mode rayon est le mode courant, il faut évidemment entrer le rayon et non pas le diamètre.

### Offset d’outil en Z

L’offset de l’axe Z peut être un peu déroutant au premier abord car il est composé de deux éléments. Le premier est l’offset de la table d’outils, le second est l’offset des coordonnées machine. Nous allons d’abord examiner l’offset de la table d’outils. Une méthode consiste à utiliser un point fixe sur le tour et à ajuster l’offset Z de tous les outils à partir de ce point fixe. Certains utilisent le nez de broche ou la face du mandrin. Cela donne la possibilité de changer d’outil et d’ajuster son offset Z, sans avoir à réinitialiser tous les outils.

### Séquence typique de correction d’outil en Z:

1.  Prise d’origine machine de tous les axes, si ce n’est pas déjà fait.

2.  S’assurer qu’aucune compensation n’est activée pour le système de coordonnées courant.

3.  Déclarer l’outil avec *M6 Tn* dans lequel *n* est le numéro de l’outil courant, présent en table d’outils.

4.  Sélectionner l’axe Z dans la fenêtre de l’onglet *contrôle manuel (F3)*.

5.  Placer un cylindre dans le mandrin.

6.  Faire tangenter l’outil contre la face du cylindre.

7.  Cliquer le bouton *Toucher* puis choisir *Table d’outils* et saisir la position à 0.0.

8.  Répéter l’opération pour chaque outil, en utilisant le même cylindre.

Maintenant, tous les outils sont compensés à la même distance d’une position standard. Si un outil doit être changé, par exemple par un foret il suffira de répéter la séquence précédente pour qu’il soit synchronisé avec l’offset Z du reste des outils. Certains outils pourraient nécessiter un peu de réflexion pour déterminer le point contrôlé par rapport au point de *Toucher*. Par exemple, un outil de tronçonnage de 3.17mm d'épaisseur qui est touché sur le côté gauche, alors que l’opérateur veut Z0 sur le côté droit, il lui faudra alors saisir 3.17 dans la fenêtre du *Toucher*.

### Machine avec tous les outils compensés

Une fois que tous les outils ont leurs offsets renseignés dans la table d’outils, il est possible d’utiliser n’importe quel outil présent en table d’outils pour ajuster le décalage du système de coordonnées machine.

### Séquence typique de décalage du système de coordonnées:

1.  Prise d’origine machine de tous les axes, si ce n’est pas déjà fait.

2.  Déclarer l’outil avec *M6 Tn* dans lequel *n* est le numéro de l’outil courant, présent en table d’outils.

3.  Envoyer un G43 pour que l’offset de l’outil soit activé. (voir ci-dessous)

4.  Tangenter l’outil contre la pièce et fixer l’offset machine Z.

Ne pas oublier d’envoyer le G43 sur l’outil avant de définir le décalage du système de coordonnées machine, les résultats ne seraient pas ceux attendus… puisque la compensation de l’outil serait ajoutée à l’offset courant lorsque l’outil sera utilisé dans le programme.

Mouvements avec broche synchronisée
-----------------------------------

Sur un tour, les mouvements avec broche synchronisée nécessitent un signal de retour entre la broche et Machinekit. Généralement, c’est un codeur en quadrature qui fourni ce retour. Le manuel de l’intégrateur donne des explications sur l’utilisation des codeurs de broche.

Filetage

Le cycle de filetage préprogrammé G76 est utilisé, tant en filetage intérieur qu’en filetage extérieur, voir [la section G76](#sec:G76-Filetage).

Vitesse de coupe à surface constante

La vitesse de coupe à surface constante utilise l’origine machine X modifiée par l’offset d’outil X, pour calculer la vitesse de rotation de la broche en tr/mn. La vitesse de coupe à surface constante permet de suivre les changements d’offset de l’outil. L’emplacement de l’origine machine de l’axe X doit être sur l’axe de rotation et doit se faire avec l’outil de référence (celui qui a l’offset à zéro).

Avance par tour

L’avance par tour déplace l’axe Z de la valeur de F à chaque tour. Ce n’est pas destiné au filetage pour lequel il faut utiliser G76. D’autres informations sont dans la section sur [G95](#sec:G93-G94-G95-Modes).

Arcs
----

Le calcul des arcs peut être un exercice assez compliqué, même sur un tour, sans considérer les modes rayon et diamètre, ni l’orientation du système de coordonnées machine. Ce qui suit s’applique à des arcs au format centre. Sur un tour, il faut inclure G18 dans le préambule du programme G-code pour remplacer le G17 par défaut, le fait d'être en mode tour dans Axis ne suffit pas. Les arcs en G18, plan XZ utilisent les offsets pour I (l’axe X) et K (l’axe Z).

### Les arcs et la cinématique du tour

Le tour classique a la broche à gauche de l’opérateur et l’outil entre l’opérateur et le centre de rotation du mandrin. C’est un agencement avec un axe Y(+) imaginaire pointant vers le sol.

Ce qui suit est valable pour ce type d’agencement:

-   Le côté positif de l’axe Z pointe vers la droite, en s'éloignant de la broche.

-   Le côté positif de l’axe X pointe vers l’opérateur, quand il est du côté de l’opérateur par rapport au centre de rotation, ses valeur sont positives.

Certains tours ont l’outil du côté arrière et un axe Y(+) imaginaire pointant vers le haut.

Les directions des arcs G2/G3 sont basées sur l’axe autour duquel ils tournent. Dans le cas des tours, il s’agit de l’axe imaginaire Y. Si l’axe Y(+) pointe vers le sol, il faut regarder vers le haut pour que l’arc paraisse aller dans la bonne direction. Alors qu’en regardant depuis le dessus il faut inverser les G2/G3 pour que l’arc semble aller dans la bonne direction.

### Mode rayon et mode diamètre

Lors du calcul des arcs en mode rayon, il suffi de se rappeler la direction de rotation telle qu’elle s’applique à ce tour.

Lors du calcul des arcs en mode diamètre, X est le diamètre, l’offset X (I) est le rayon, même en mode diamètre G7.

Parcours d’outil
----------------

### Point contrôlé

Le point contrôlé pour l’outil, suit la trajectoire programmée. Le point contrôlé est l’intersection entre deux lignes parallèles aux axes X et Z, tangentes au rayon de bec de l’outil, définies en faisant tangenter l’outil en X puis en Z. En cylindrage ou en dressage de face sur une pièce, la trajectoire de coupe et l’arête de coupe de l’outil suivent le même parcours. Lors du tournage d’un rayon ou d’un angle, l’arête de coupe de l’outil ne suit pas la trajectoire programmée, sauf si la compensation d’outil est activée. Dans la figure suivante, on voit bien que le point contrôlé n’est pas sur l’arête de coupe de l’outil comme on pourrait le supposer.

![](images/control_point.png)

Figure 2. Point contrôlé

### Tourner les angles sans compensation d’outil

Maintenant imaginons de programmer une rampe sans compensation d’outil. La trajectoire programmée est représentée sur la figure suivante. Comme on peut le voir, la trajectoire programmée et la trajectoire de coupe souhaitée sont identiques uniquement si les mouvements de tournage suivent les axes X et Z.

![](images/ramp_entry.png)

Figure 3. Tournage en rampe

Le point contrôlé progresse en suivant la trajectoire programmée mais l’arête de coupe ne suit pas cette trajectoire comme c’est visible sur la figure suivante. Pour résoudre ce problème, il est nécessaire d’activer la compensation d’outil et d’ajuster la trajectoire programmée pour compenser le rayon de bec de l’outil.

![](images/ramp_cut.png)

Figure 4. Trajectoire en rampe

Dans l’exemple ci-dessus, pour suivre la rampe programmée et obtenir la bonne trajectoire, il suffi de décaler la trajectoire de la rampe vers la gauche, de la valeur d’un rayon de bec.

### Tournage avec rayon extérieur

Dans cet exemple nous allons examiner ce qui se passe durant le tournage d’un rayon extérieur sans compensation de rayon de bec. Sur la figure suivante on voit l’outil tourner un diamètre extérieur sur la pièce. Le point contrôlé de l’outil suit bien la trajectoire programmée, l’outil touche le diamètre extérieur de la pièce.

![](images/radius_1.png)

Figure 5. Tournage du diamètre

Sur la figure suivante, on voit que quand l’outil approche la fin la pièce, le point contrôlé continue de suivre la trajectoire alors que l’arête de coupe a déjà quitté la matière et coupe en l’air. On voit aussi que malgré qu’un rayon a été programmé, la pièce conserve son angle d’extrémité.

![](images/radius_2.png)

Figure 6. Tournage du rayon

Maintenant, comme on le voit, le point contrôlé suit bien la trajectoire programmée mais l’arête de coupe est en dehors de la matière.

![](images/radius_3.png)

Figure 7. Tournage du rayon

Sur la figure finale, on voit que l’arête de coupe a terminé le dressage de la face mais en laissant un coin carré à la place du beau rayon attendu. Noter aussi que, pour la même raison, pour ne pas laisser de téton au centre de la pièce lors du dressage de sa face, il convient de dépasser le centre de rotation de la valeur d’un rayon de bec de l’outil.

![](images/radius_4.png)

Figure 8. Dressage de la face

### Utiliser la compensation d’outil

-   Quand la compensation d’outil est utilisée sur un tour, penser à l’arête de coupe de l’outil comme étant celle d’un outil rond.

-   Quand la compensation d’outil est utilisée, la trajectoire doit être suffisamment large pour qu’un outil rond n’interfère pas avec la pièce à la ligne suivante.

-   Pour tourner des lignes droites sur un tour, il est préférable de ne pas utiliser la compensation d’outil. Par exemple pour aléser un trou avec une barre d’alésage un peu grosse, la place pourrait manquer pour dégager l’outil et faire le mouvement de sortie.

-   Le mouvement d’entrée dans un arc avec la compensation d’outil, est important pour obtenir des résultats corrects.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


