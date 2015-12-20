La cinématique dans Machinekit
==============================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Cinematique"></span>

Introduction
------------

Habituellement quand nous parlons de machines CNC, nous pensons à des machines programmées pour effectuer certains mouvements et effectuer diverses tâches. Pour avoir une représentation unifiée dans l’espace de ces machines, nous la faisons correspondre à la vision humaine de l’espace en 3D, la plupart des machines (sinon toutes) utilisent un système de coordonnées courant, le système Cartésien.

Le système de coordonnées Cartésiennes est composé de 3 axes (X, Y, Z) chacun perpendiculaire aux 2 autres. <span class="footnote">
\[Le mot *axes* est aussi communément (et incorrectement) utilisé à propos des machines CNC, il fait référence aux directions des mouvements de la machine.\]
</span>

Quand nous parlons d’un programme G-code (RS274/NGC) nous parlons d’un certain nombre de commandes (G0, G1, etc.) qui ont comme paramètres (X- Y- Z-). Ces positions se référent exactement à des positions Cartésiennes. Une partie du contrôleur de mouvements de Machinekit est responsable de la translation entre ces positions et les positions correspondantes de la cinématique de la machine<span class="footnote">
\[Cinématique: une fonction à deux voies pour transformer un espace Cartésien en espace à articulations\]
</span>.

### Les articulations par rapport aux axes

Une articulation, pour une machine CNC est un des degrés physiques de liberté de la machine. Elle peut être linéaire (vis à billes) ou rotative (table tournante, articulations d’un bras robotisé). Il peut y avoir n’importe quel nombre d’articulations sur une machine. Par exemple, un robot classique dispose de 6 articulations et une fraiseuse classique n’en a que 3.

Sur certaines machines, les articulations sont placées de manière à correspondre aux axes cinématiques (articulation 0 le long de l’axe X, articulation 1 le long de l’axe Y et articulation 2 le long de l’axe Z), ces machines sont appelées machines Cartésiennes (ou encore machines à cinématiques triviales . Ce sont les machines les plus courantes parmi les machines-outils mais elles ne sont pas courantes dans d’autres domaines comme les machines de soudage (ex: robots de soudage de type puma).

Cinématiques triviales
----------------------

Comme nous l’avons vu, il y a un groupe de machines sur lesquelles chacun des axes est placé le long d’un des axes Cartésiens. Sur ces machines le passage, du plan de l’espace Cartésien (le programme G-code) au plan de l’espace articulation (l’actuateur actuel de la machine), est trivial. C’est un simple plan 1:1:

    pos->tran.x = joints[0];
    pos->tran.y = joints[1];
    pos->tran.z = joints[2];
    pos->a = joints[3];
    pos->b = joints[4];
    pos->c = joints[5];

Dans l’extrait de code ci-dessus, nous pouvons voir comment le plan est fait: la position X est identique avec la articulation 0, Y avec la articulation 1 etc. Nous nous référons dans ce cas à une cinématique directe (une transformation avant), tandis que dans l’extrait de code suivant il est fait référence à une cinématique inverse (ou une transformation inverse):

    joints[0] = pos->tran.x;
    joints[1] = pos->tran.y;
    joints[2] = pos->tran.z;
    joints[3] = pos->a;
    joints[4] = pos->b;
    joints[5] = pos->c;

Comme on peut le voir, c’est assez simple de faire la transformation d’une machine à cinématique banale (ou Cartésienne). Cela devient un peu plus compliqué si il manque un axe à la machine.<span class="footnote">
\[Si la machine (par exemple un tour) est montée avec seulement les axes X, Z et A et que le fichier d’init de Machinekit contient uniquement la définition de ces 3 articulations, alors l’assertion précédente est fausse. Parce-que nous avons actuellement (joint0=x, joint1=Z, joint2=A) ce qui suppose que joint1=Y. Pour faire en sorte que cela fonctionne dans Machinekit il suffit de définir tous les axes (XYZA), Machinekit utilisera alors une simple boucle dans HAL pour l’axe Y inutilisé.\]
</span><span class="footnote">
\[Une autre façon de le faire fonctionner, est de changer le code correspondant et recompiler le logiciel.\]
</span>

Cinématiques non triviales
--------------------------

Il peut y avoir un certain nombre de types de configurations de machine (robots: puma, scara; hexapodes etc.) Chacun d’eux est mis en place en utilisant des articulations linéaires et rotatives. Ces articulations ne correspondent pas habituellement avec les coordonnées Cartésiennes, cela nécessite une fonction cinématique qui fasse la conversion (en fait 2 fonctions: fonction en avant et inverse de la cinématique).

Pour illustrer ce qui précède, nous analyserons une simple cinématique appelée bipode (une version simplifiée du tripode, qui est déjà une version simplifiée de l’hexapode).

![](images/bipod.png)

Figure 1. Définir un bipode<span id="cap:Bipod-setup"></span>

Le bipode dont nous parlons est un appareil, composé de deux moteurs placés sur un mur, à cet appareil un mobile est suspendu par des fils. Les articulations dans ce cas sont les distances entre le mobile et les moteurs de l’appareil (nommées AD et BD sur la figure ci-dessus.

La position des moteurs est fixée par convention. Le moteur A est en (0,0), qui signifie que sa coordonnée X est 0 et sa coordonnée Y également 0. Le moteur B est placé en (Bx, 0), se qui veut dire que sa coordonnée X est Bx.

Notre pointe mobile se trouvera au point D défini par les distances AD et BD, et par les coordonnées Cartésiennes Dx, Dy.

La tâche de la cinématique consistera à transformer les longueurs des articulations en (AD, BD) en coordonnées Cartésiennes (Dx, Dy) et vice-versa.

### Transformation avant<span id="sec:Forward-transformation"></span>

Pour effectuer la transformation de l’espace articulation en espace Cartésien nous allons utiliser quelques règles de trigonomètrie (le triangle rectangle déterminé par les points (0,0), (Dx,0), (Dx,Dy) et le triangle rectangle (Dx,0), (Bx,0) et (Dx,Dy).

Nous pouvons voir aisément que **AD<sup>2</sup>=x<sup>2</sup>+y<sup>2</sup>**, de même que **BD<sup>2</sup>=(Bx-x)<sup>2</sup>+y<sup>2</sup>**.

Si nous soustrayons l’un de l’autre nous aurons: **AD<sup>2</sup>-BD<sup>2</sup>=x<sup>2</sup>+y<sup>2</sup>-x<sup>2</sup>+2\*x\*Bx-Bx<sup>2</sup>-y<sup>2</sup>**

et par conséquent: **x=(AD<sup>2</sup>-BD<sup>2</sup>+Bx<sup>2</sup>)/(2\*Bx)**

De là nous calculons: **y=sqrt(AD<sup>2</sup>-x<sup>2</sup>)**

Noter que le calcul inclus la racine carrée de la différence, mais qu’il n’en résulte pas un nombre réel. Si il n’y a aucune coordonnée Cartésienne pour la position de cette articulation, alors la position est dite singulière. Dans ce cas, la cinématique inverse retourne -1.

Traduction en code:

    double AD2 = joints[0] * joints[0];
    double BD2 = joints[1] * joints[1];
    double x = (AD2 - BD2 + Bx * Bx) / (2 * Bx);
    double y2 = AD2 - x * x;
    if(y2 < 0) return -1;
    pos->tran.x = x;
    pos->tran.y = sqrt(y2);
    return 0;

### Transformation inverse<span id="sec:Inverse-transformation"></span>

La cinématique inverse est beaucoup plus simple dans notre exemple, de sorte que nous pouvons l'écrire directement:

**AD=sqrt(x<sup>2</sup>+y<sup>2</sup>)**

**BD=sqrt((Bx-x)<sup>2</sup>+y<sup>2</sup>)**

ou traduite en code:

    double x2 = pos->tran.x * pos->tran.x;
    double y2 = pos->tran.y * pos->tran.y;
    joints[0] = sqrt(x2 + y2);
    joints[1] = sqrt((Bx - pos->tran.x)*(Bx - pos->tran.x) + y2);
    return 0;

Détails d’implémentation
------------------------

Un module cinématique est implémenté comme un composant de HAL, et il est permis d’exporter ses pins et ses paramètres. Il consiste en quelques fonctions “C” (par opposition au fonctions de HAL):

 int kinematicsForward(const double \*joint, EmcPose \*world, const KINEMATICS\_FORWARD\_FLAGS \*fflags, KINEMATICS\_INVERSE\_FLAGS \*iflags)   
Implémente [la fonction cinématique avant](#sec:Forward-transformation).

 int kinematicsInverse(const EmcPose \* world, double \*joints, const KINEMATICS\_INVERSE\_FLAGS \*iflags, KINEMATICS\_FORWARD\_FLAGS \*fflags)   
Implémente [la fonction cinématique inverse](#sec:Inverse-transformation).

 KINEMATICS\_TYPE kinematicsType(void)\_   
Retourne l’identificateur de type de la cinématique, typiquement *KINEMATICS\_BOTH*.

 int kinematicsHome(EmcPose \*world, double \*joint, KINEMATICS\_FORWARD\_FLAGS \*fflags, KINEMATICS\_INVERSE\_FLAGS \*iflags)   
La fonction prise d’origine de la cinématique ajuste tous ses arguments à leur propre valeur à une position d’origine connue. Quand elle est appelée, cette position doit être ajustée, quand elle est connue, comme valeurs initiales, par exemple depuis un fichier INI. Si la prise d’origine de la cinématique peut accepter des points de départ arbitraires, ces valeurs initiales doivent être utilisées.

 int rtapi\_app\_main(void)
 void rtapi\_app\_exit(void)   
Il s’agit des fonctions standards d’installation et de la désinstallation des modules RTAPI.

Quand ils sont contenus dans un seul fichier source, les modules de la cinématique peuvent être compilés et installés par *comp*. Voir la manpage *comp(1)* pour d’autres informations.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


