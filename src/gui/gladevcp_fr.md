Création d’interfaces graphiques avec GladeVCP
==============================================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:GladeVCP"></span>

Qu’est-ce que GladeVCP?
-----------------------

GladeVCP est un composant de Machinekit qui donne la possibilité d’ajouter de nouvelles interfaces graphiques utilisateur à Machinekit telles qu’Axis ou Touchy. À la différence de PyVCP, GladeVCP n’est pas limité à l’affichage et aux réglages des pins de HAL, toutes les actions peuvent être exécutées en code Python. En fait, une interface utilisateur Machinekit complète peut être construite avec GladeVCP et Python.

GladeVCP utilise l’environnement graphique et WYSIWYG [Glade](http://glade.gnome.org/) qui simplifie l'édition et la création visuelle de panneaux esthétiquement très réussis. Il s’appuie sur les liaisons entre [PyGTK](http://www.pygtk.org/) et le riche jeu de widgets [GTK+](http://www.gtk.org/), finalement, tous peuvent être utilisés dans une application GladeVCP et pas seulement les widgets spécialisés pour interagir avec HAL et Machinekit présentés ici.

### PyVCP par rapport à GladeVCP

Tous les deux supportent la création de panneaux avec des *widgets de HAL*, des éléments utilisateur visuels tels que boutons, Leds, curseurs etc. dont les valeurs sont liées à des pins de HAL qui à leur tour, sont des interfaces pour le reste de Machinekit.

 PyVCP   
-   Jeu de widgets: utilise les widgets TkInter.

-   Cycle de création d’interfaces utilisateur:

    -   Éditer les fichiers XML

    -   Lancer

    -   Évaluer le look.

-   Pas de support pour intégrer une gestion des événements définie par l’utilisateur.

-   Pas d’interaction avec Machinekit au-delà des interactions avec les pins d’E/S de HAL supportées.

 GladeVCP   
-   Jeu de widgets: Liaison avec le jeu de widgets de [GTK+](http://www.gtk.org/).

-   Création d’interface utilisateur: utilise l’interface graphique [Glade](http://glade.gnome.org/) qui est un éditeur WYSIWYG.

-   Tout changement sur une pin de HAL peut diriger un appel vers une gestion d'événements définie en Python par l’utilisateur.

-   Tous les signaux GTK (touches/appui sur un bouton, fenêtre, E/S, timer, événements réseau) peuvent être associés avec la gestion d'événements définie en Python par l’utilisateur.

-   Interaction directe avec Machinekit: exécution de commandes, telle qu’initialiser une commande MDI pour appeler un sous-programme G-code.

-   Plusieurs panneaux GladeVCP indépendants peuvent tourner dans des onglets différents.

-   Séparation entre l’apparence de l’interface et les fonctionnalités: change d’apparence sans passer par aucun code.

Description du fonctionnement, avec un exemple de panneau
---------------------------------------------------------

Une fenêtre de panneau GladeVCP peut démarrer avec trois différentes configuration:

-   Toujours visible, intégré dans Axis, du côté droit, exactement comme un panneau PyVCP.

-   Dans un onglet dans Axis ou Touchy; dans Axis un troisième onglet sera créé à côté des deux d’origine, ils doivent être choisis explicitement.

-   Comme une fenêtre indépendante, qui peut être iconisée ou agrandie, indépendamment de la fenêtre principale.

Lancer un panneau GladeVCP simple, intégré dans Axis comme PyVCP, taper les commandes suivantes:

    $ cd configs/sim/gladevcp

    $ machinekit gladevcp_panel.ini

![](images/example-panel-small.png)

Lancer le même panneau, mais dans un onglet d’Axis avec:

    $ cd configs/sim/gladevcp

    $ machinekit gladevcp_tab.ini

![](images/example-tabbed-small.png)

Pour lancer ce même panneau comme une fenêtre autonome à côté d’Axis, démarrer Axis en arrière plan puis démarrer gladevcp de la manière suivante:

    $ cd configs/sim/gladevcp

    $ machinekit axis.ini &

    $ gladevcp -c gladevcp -u ../gladevcp/hitcounter.py -H
    ../gladevcp/manual-example.hal ../gladevcp/manual-example.ui

![](images/example-float-small.png)

Pour lancer ce panneau dans *Touchy*:

    $ cd configs/sim

    $ machinekit gladevcp_touchy.ini

![](images/touchy-tab-33.png)

<span class="comment"> Ces deux derniers exemples ne fonctionnent pas pour le moment.</span>

Fonctionnellement, ces configurations sont identiques. La seule différence porte sur l'état et la visibilité de l'écran. Puisqu’il est possible de lancer plusieurs composants GladeVCP en parallèle (avec des noms de modules de HAL différents), le mélange des configurations est également possible. Par exemple, un panneau sur le côté droit et un ou plusieurs en onglets pour des parties d’interface moins souvent utilisées.

### Description de l’exemple de panneau

Pendant qu’Axis est en marche, explorons *Afficher configuration de HAL* dans lequel nous trouvons le composant de HAL *gladevcp* et dont nous pouvons observer la valeur des pins pendant l’interaction avec les widgets du panneau. La configuration de HAL peut être trouvée dans *configs/gladevcp/manual-example.hal*.

Usage des deux cadres en partie basse. Le panneau est configuré pour que, quand l’Arrêt d’Urgence est désactivé, le cadre *Settings* s’active et mette la machine en marche, ce qui active à son tour le cadre *Commandes* du dessous. Les widgets de HAL du cadre *Settings* sont liés aux Leds et labels du cadre *Status* ainsi qu’au numéros de l’outil courant et à celui de l’outil préparé. Les utiliser pour bien voir leur effet. L’exécution des commandes *T&lt;numéro d’outil&gt;* et *M6* dans la fenêtre du MDI aura pour effet de changer les numéros de l’outil courant et de l’outil préparé dans les champs respectifs.

Les boutons du cadre *Commandes* sont des *widgets d’action MDI*. Les presser exécutera une commande MDI dans l’interpréteur. Le troisième bouton *Execute Oword subroutine* est un exemple avancé, il prends plusieurs pins de HAL du cadre *Settings* et leur passe comme paramètres, le *sous-programme Oword*. Les paramètres actuels reçus par la routine sont affichés par une commande *(DEBUG, )*. Voir *configs/gladevcp/nc\_files/oword.ngc* pour le corps du sous-programme.

Pour voir comment le panneau est intégré dans Axis, voir la déclaration de *\[DISPLAY\]GLADEVCP* dans gladevcp\_panel.ui, ainsi que les déclarations de *\[DISPLAY\]EMBED* et de *\[HAL\]POSTGUI\_HALFILE* dans *gladevcp\_tab.ini*, respectivement.

### Description de l'éditeur de Glade

L’interface utilisateur est créée avec l'éditeur graphique de Glade. Pour l’essayer il faut avoir le pré-requis nécessaire, [que glade soit installé](#gladevcp:Pre-requis). Pour éditer l’interface utilisateur, lancer la commande:

    $ glade configs/gladevcp/manual-example.ui

La zone centrale de la fenêtre montre l’apparence de l’interface en création. Tous les objets de l’interface et les objets supportés se trouvent dans la partie haute à droite de la fenêtre, où il est possible de choisir un widget spécifique (ou en cliquant sur lui au centre de la fenêtre). Les propriétés du widget choisi sont affichées et peuvent être modifiées, dans le bas à droite de la fenêtre.

Pour voir comment les commandes MDI sont passées depuis les widgets d’action MDI, explorer la liste des widgets sous *Actions* en haut à droite de la fenêtre, et dans le bas à droite de la fenêtre, sous l’onglet *Général*, les propriétés des *commandes MDI*.

### Explorer la fonction de rappel de Python

Voici comment une fonction de rappel Python est intégrée dans l’exemple:

-   Dans glade, regarder le label du widget `hits` (un widget GTK+).

-   Dans le widget `button1`, regarder dans l’onglet *Signaux* et trouver le signal *pressed* associé avec le gestionnaire *on\_button\_press*.

-   Dans ../gladevcp/hitcounter.py, regarder la méthode *on\_button\_press* et comment elle place la propriété du label dans l’objet *hits*.

C'était juste pour toucher le concept du doigt. Le mécanisme de fonction de rappel sera détaillé plus en détails dans la section [Programmation de GladeVCP](#gladevcp:GladeVCP_Programming).

Créer et intégrer une interface utilisateur Glade
-------------------------------------------------

### Pré-requis: Installation de Glade

Pour visualiser ou modifier les fichiers d’une interface Glade, Glade doit être installé. Ce n’est pas nécessaire pour seulement essayer un panneau GladeVCP. Si la commande *glade* est manquante, l’installer de la manière suivante:

    $ sudo apt-get install glade

Vérifier ensuite la version installée, qui doit être égale ou supérieure à 3.6.7:

    $ glade --version

**`glade3 3.6.7`**

### Lancer Glade pour créer une nouvelle interface utilisateur

Cette section souligne juste les étapes initiales spécifiques à Machinekit. Pour plus d’informations et un tutoriel sur Glade, voir <http://glade.gnome.org>. Certains trucs & astuces sur Glade, peuvent aussi être trouvés sur [youtube](http://www.youtube.com).

Soit modifier une interface existante en lançant `glade <fichier>.ui` ou, démarrer une nouvelle en lançant juste la commande `glade` depuis un terminal.

-   Si Machinekit n’a pas été installé depuis un paquetage, l’environnement Machinekit du shell doit être configuré avec *. &lt;machinekitdir&gt;/scripts/rip-environment*, autrement Glade ne trouverait pas les widgets spécifiques à Machinekit.

-   Quand l'éditeur demande pour enregistrer les préférences, accepter ce qui est proposé par défaut et presser *Close*.

-   Depuis les *Niveaux supérieurs* (cadre de gauche), choisir *Fenêtre* (première icône) en haut des Niveaux supérieurs, par défaut cette fenêtre sera nommée *window1*. Ne pas changer ce nom, GladeVCP lui est relié.

-   Dans le bas des onglets de gauche, dérouler *HAL Python* et *Machinekit Actions*.

-   Ajouter au nouveau cadre, un conteneur comme une boîte HAL\_Box ou une HAL\_Table depuis *HAL Python*.

-   Pointer et placer dans un conteneur d’autres éléments, comme une LED, un bouton, etc.

Le résultat pourrait ressembler à cela:

![](images/glade-manual-small.png)

Glade a tendance à écrire beaucoup de messages dans la fenêtre du terminal, la plupart peuvent être ignorés. Sélectionner *Fichier → Enregistrer sous*, donner lui un nom comme *myui.ui* et bien vérifier qu’il sera enregistré comme un fichier *GtkBuilder* (bouton radio en bas à gauche du dialogue d’enregistrement). GladeVCP peut aussi traiter correctement l’ancien format *libglade* mais il n’y a aucune raison de l’utiliser. Par convention, l’extension des fichier GtkBuilder est *.ui*.

### Tester un panneau

Vous êtes maintenant prêt à faire un essai (avec Machinekit, par exemple Axis en marche) faites:

    gladevcp myui.ui

GladeVCP crée le composant de HAL portant le nom qui a été donné au fichier, par exemple, le très original *myui.ui* dans notre cas, à moins qu’il n’ait été surchargé pat l’option `-c <nom du composant>`. Si Axis est en marche, essayer de trouver le composant dans *Afficher configuration de HAL* et inspecter ses pins.

Vous vous demandez peut être pourquoi les widgets conteneurs comme *HAL\_Hbox* ou *HAL\_Table* apparaissent grisés (inactifs). Les conteneurs HAL ont une pin de HAL associée qui est désactivée par défaut, c’est ce qui cause ce rendu grisé des widgets conteneurs inactifs. Un cas d’utilisation courante pourrait être pour associer les pins de HAL du conteneur `halui.machine.is-on` ou un des signaux `halui.mode.`, pour s’assurer que certains widgets n’apparaissent actifs que dans un certain état.

Pour activer un conteneur, exécuter la commande HAL `setp gladevcp.<nom-du-conteneur> 1`.

### Préparer le fichier de commande HAL

La voie suggérée pour lier les pins de HAL dans un panneau GladeVCP consiste à les collecter dans un fichier séparé portant l’extension `.hal`. Ce fichier est passé via l’option `POSTGUI_HALFILE=`, dans la section `[HAL]` du fichier de configuration.

ATTENTION: Ne pas ajouter le fichier de commandes HAL de GladeVCP à la section ini d’Axis `[HAL]HALFILE=`, ça n’aurait pas l’effet souhaité. Voir les sections suivantes.

### Intégration dans Axis, comme pour PyVCP

Pour placer le panneau GladeVCP dans la partie droite d’Axis, ajouter les lignes suivantes dans le fichier ini:

    [DISPLAY]
    # ajouter le panneau GladeVCP à l'emplacement de PyVCP:
    GLADEVCP= -u ../gladevcp/hitcounter.py ../gladevcp/manual-example.ui

    [HAL]
    # Les commandes HAL pour les composants GladeVCP dans un onglet, doivent être
    exécutées via POSTGUI_HALFILE
    POSTGUI_HALFILE =  ../gladevcp/manual-example.hal

    [RS274NGC]
    # les sous-programmes Oword spécifiques à gladevcp se placent ici
    SUBROUTINE_PATH = ../gladevcp/nc_files/

Le nom de composant HAL d’une application GladeVCP lancé avec l’option GLADEVCP est toujours: `gladevcp`. La ligne de commande actuellement lancée par Axis dans la configuration ci-dessous est la suivante:

    halcmd loadusr -Wn gladevcp gladevcp -c gladevcp -x {XID} <arguments pour GLADEVCP>

Ce qui veux dire que n’importe quelle option gladevcp, peut être ajoutée ici, tant qu’elle n’entre pas en collision avec les options des lignes de commande suivantes.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Note
</div></td>
<td align="left">L’option <code>[RS274NGC]SUBROUTINE_PATH=</code> est fixée seulement pour que l’exemple de panneau puisse trouver le sous-programme Oword pour le widget de commande MDI. Il n’est peut être pas nécessaire dans votre configuration.</td>
</tr>
</tbody>
</table>

### Intégration dans un nouvel onglet d’Axis, à la suite des autres

Pour cela, éditer le fichier .ini et ajouter dans les sections DISPLAY et HAL, les lignes suivantes:

    [DISPLAY]
    # ajoute le panneau GladeVCP dans un nouvel onglet:
    EMBED_TAB_NAME=GladeVCP demo
    EMBED_TAB_COMMAND=halcmd loadusr -Wn gladevcp gladevcp -c gladevcp -x {XID} -u
    ../gladevcp/hitcounter.py ../gladevcp/manual-example.ui

    [HAL]
    # commandes HAL pour le composant GladeVCP dans un onglet doit être exécuté via
    POSTGUI_HALFILE
    POSTGUI_HALFILE =  ../gladevcp/manual-example.hal

    [RS274NGC]
    # les sous-programmes Oword spécifiques à gladevcp se placent ici
    SUBROUTINE_PATH = ../gladevcp/nc_files/

Noter le *halcmd loadusr* pour charger la commande d’onglet, elle assure que *POSTGUI\_HALFILE* ne sera lancé que seulement après que le composant de HAL ne soit prêt. Dans de rares cas, une commande pourrait être lancée ici, pour utiliser un onglet sans être associée à un composant de HAL. Une telle commande pourrait être lancée sans *halcmd loadusr*, ce qui indiquerait à Axis qu’il ne doit plus attendre un composant de HAL, puisqu’il n’existe pas.

Noter que quand le nom du composant est changé dans l’exemple suivant, les noms utilisés dans `-Wn <composant>` et `-c <composant>` doivent être identiques.

Essayer en lançant Axis, il doit avoir un nouvel onglet appelé *GladeVCP demo* à droite de l’onglet de la visu. Sélectionner cet onglet, le panneau de l’exemple devrait être visible, bien intégré à Axis.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Note
</div></td>
<td align="left">Bien vérifier que le fichier de l’interface est la dernière option passée à GladeVCP dans les deux déclarations <code>GLADEVCP=</code> et <code>EMBED_TAB_COMMAND=</code>.</td>
</tr>
</tbody>
</table>

### Intégration dans Touchy

Pour ajouter un onglet GladeVCP à *Touchy*, éditer le fichier .ini comme cela:

    [DISPLAY]
    # ajoute un panneau GladeVCP dans un onglet
    EMBED_TAB_NAME=GladeVCP demo
    EMBED_TAB_COMMAND=gladevcp -c gladevcp -x {XID} -u ../gladevcp/hitcounter.py -H
    ../gladevcp/gladevcp-touchy.hal ../gladevcp/manual-example.ui

    [RS274NGC]
    # les sous-programmes Oword spécifiques à gladevcp se placent ici
    SUBROUTINE_PATH = ../gladevcp/nc_files/

Noter les différences suivantes avec la configuration de l’onglet d’Axis:

-   Le fichier de commandes HAL est légèrement modifié puisque *Touchy* n’utilise pas le composant *halui*, ses signaux ne sont donc pas disponibles et certains raccourcis ont été pris.

-   Il n’y a pas d’option *POSTGUI\_HALFILE=*, mais il est correct, de passer le fichier de commandes HAL,
     par la ligne *EMBED\_TAB\_COMMAND=*.

-   L’appel *halcmd loaduser -Wn …* n’est pas nécessaire.

Options de GladeVCP en ligne de commande
----------------------------------------

Voir également, *man gladevcp*. Ce sont les options pour cette ligne de commande:

Usage: gladevcp \[options\] myfile.ui

Options:

 -h, --help   
Affiche ce message d’aide et sort.

 -c NAME   
Fixe le nom du composant à NAME. Par défaut, le nom de base des fichiers UI

 -d   
Active la sortie débogage

 -g GEOMETRY   
Fixe la géométrie à WIDTHxHEIGHT+XOFFSET+YOFFSET. Les valeurs sont en pixels,
 XOFFSET/YOFFSET est référencé à partir du coin haut, à gauche de l'écran.
 Utilise -g WIDTHxHEIGHT pour fixer une taille ou -g +XOFFSET+YOFFSET pour fixer une position

 -H FILE   
exécute les déclarations de HAL depuis FILE, avec halcmd après que le composant soit chargé et prêt

 -m MAXIMUM   
force la fenêtre du panneau à se maximiser. Toutefois avec l’option -g geometry le panneau est déplaçable d’un moniteur à un autre en le forçant à utiliser toute l'écran

 -t THEME   
fixe le thème gtk. Par défaut, le thème système. Différents panneaux peuvent avoir différents thèmes. Un exemple de thème peut être trouvé sur le [Wiki de Machinekit](http://wiki.machinekit.org/cgi-bin/wiki.pl?GTK_Themes).

 -x XID   
Redonne un parent GladeVCP dans une fenêtre existante XID au lieu d’en créer une nouvelle au niveau supérieur

 -u FILE   
Utilise les FILE comme modules définis par l’utilisateur avec le gestionnaire

 -U USEROPT   
passe les modules python USEROPT

Références des Widgets HAL
--------------------------

GladeVcp inclus une collection de widgets Gtk qui ont des pins de HAL attachées, appelés widgets HAL, il sont destinés à contrôler, à afficher et à avoir d’autres interactions avec la couche HAL de Machinekit. Il sont destinés à être utilisés avec les interfaces créées par l'éditeur de Glade. Avec une installation correcte, les widgets HAL devraient être visibles, dans l'éditeur Glade, dans le groupe des Widgets *HAL Python*. Beaucoup de champs spécifiques à HAL dans l’onglet *Général* affichent une infobulle au survol de la souris.

Il y a deux variantes de signaux de HAL, bits et nombres. Les signaux bits sont les on/off. Les nombres peuvent être des "float", des "s32" ou des "u32". Pour plus d’informations sur les types de données de HAL, voir le manuel de HAL. Les widgets GladeVcp peuvent soit, afficher la valeur d’un signal avec un widget d’indication, soit, modifier la valeur d’un signal avec un widget de contrôle. Ainsi, il existe quatre classes de widgets gladvcp qui peuvent être connectés à un signal de HAL. Une autre classe de widgets d’aide permettent d’organiser et d'étiqueter les panneaux.

-   Widgets d’indications "bit" signals: [Led HAL](#gladevcp:HAL_LED)

-   Widgets de contrôle "bit" signals: [HAL Bouton](#gladevcp:HAL_Button), [HAL Bouton radio](#gladevcp:HAL_Button), [HAL Case à cocher](#gladevcp:HAL_Button)

-   Widgets d’indications "nombre" signals: [\[gladevcp:HAL\_Label\]](#gladevcp:HAL_Label), [HAL Barre de progression](#gladevcp:HAL_ProgressBar), [HAL HBar](#gladevcp:HAL_HBar), [HAL VBar](#gladevcp:HAL_HBar), [HAL Indicateur](#gladevcp:HAL_Meter)

-   Widgets de contrôle "nombre" signals: [boîte d’incrément](#gladevcp:HAL_SpinButton), [HAL HScale](#gladevcp:HAL_HScale), [HAL VScale](#gladevcp:HAL_HScale)

-   widgets d’aide: [HAL Table](#gladevcp:HAL_HBox), [HAL HBox](#gladevcp:HAL_HBox)

-   Tracé du parcours d’outil: [HAL Gremlin](#gladevcp:HAL_Gremlin)

Les widgets HAL héritent des méthodes, propriétés et signaux des widgets Gtk sous-jacents, il est donc utile de consulter le site du [GTK+](http://www.gtk.org/) ainsi que la documentation pour les liaisons avec [PyGTK](http://www.pygtk.org/).

### Nommage des Widgets HAL et de leurs pins

La plupart des widgets HAL on une simple pin de HAL associée et portant le même nom que le widget (glade: Général→Nom).

Les exceptions à cette règle sont actuellement:

-   *HAL\_Spinbutton* et *HAL\_ComboBox*, qui ont deux pins: une pin
     `<nomwidget>-f` (float) et une pin `<nomwidget>-s` (s32)

-   *HAL\_ProgressBar*, qui a une pin d’entrée `<nomwidget>-value`, et une pin d’entrée `<nomwidget>-scale`.

### Donner des valeurs aux Widgets HAL et à leurs pins

En règle générale, si une valeur doit être attribuée à la sortie d’un widget HAL depuis un code Python, le faire en appelant le *setter* Gtk sous-jacent (par exemple `set_active()`, `set_value()`), ne pas essayer de donner directement la valeur à la pin associée par un `halcomp[nompin] = value`, parce-que le widget ne verra jamais le changement!.

Il pourrait être tentant de *fixer une pin d’entrée de widget HAL* par programme. Noter que cela va à l’encontre du but premier d’une pin d’entrée. Elle devrait être attachée à un autre composant de HAL et réagir au signal qu’il génère. Bien qu’aucune protection, empêchant d'écrire sur les pins d’entrée HAL Python, ne soit présente actuellement, cela n’aurait aucun sens. Il faut utiliser `setp nompin valeur` dans un fichier Hal associé, pour les essais.

Il est par contre, parfaitement autorisé de mettre une valeur sur une pin de sortie de Hal avec `halcomp[nompin] = valeur` à condition que cette pin ne soit pas déjà associée avec un autre widget, ce qui aurait pu être créé par la méthode
 `hal_glib.GPin(halcomp.newpin(<nom>,<type>,<direction>)`. Voir la [programmation de GladeVCP](#gladevcp:GladeVCP_Programming) pour d’autres exemples.

### Le signal *hal-pin-changed*

La programmation événementielle signifie que l’interface graphique indique au code quand "quelque chose se produit", grâce à une fonction de rappel, comme quand un bouton est pressé, la sortie du widget HAL (ceux qui affichent la valeur des pins de HAL) comme une LED, une barre, une VBar, un indicateur à aiguille etc, supportent le signal *hal-pin-changed* qui peut provoquer une fonction de rappel dans le code Python quand une pin de HAL change de valeur. Cela veut dire qu’il n’est plus nécessaire d’interroger en permanence les pins de HAL dans le code pour connaitre les changements, les widgets font ça en arrière plan et le font savoir.

Voici un exemple montrant comment régler un signal `hal-pin-changed` pour une Hal Led, dans l'éditeur de Glade:

![](images/hal-pin-change-66.png)

L’exemple dans `configs/gladevcp/examples/complex` montre comment c’est géré en Python.

### Les boutons (HAL Button)

Ce groupe de widgets est dérivé de divers boutons Gtk, ce sont les widgets HAL\_Button, HAL\_ToggleButton, HAL\_RadioButton et CheckButton. Tous ont une seule pin de sortie BIT portant un nom identique au widget. Les boutons n’ont pas d’autres propriétés additionnelles, contrairement à leurs classes de base Gtk.

-   HAL\_Button: Action instantanée, ne retient pas l'état. Signal important: `pressed`.

-   HAL\_ToggleButton, HAL\_CheckButton: Retiennent l'état on/off. Signal important: `toggled`.

-   HAL\_RadioButton: Un parmi un groupe. Signal important: `toggled` (par bouton).

-   Importantes méthodes communes: `set_active()`, `get_active()`

-   Importantes propriétés: `label`, `image`

<span class="comment"> .Boutons</span>

Case à cocher: <span class="image"> ![](images/checkbutton.png) </span>

Boutons radio: <span class="image"> ![](images/radiobutton.png) </span>

Bouton à bascule: <span class="image"> ![](images/button.png) </span>

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Astuce
</div></td>
<td align="left"><div class="paragraph">
<p>Définir les groupes de boutons radio dans Glade:</p>
</div>
<div class="ulist">
<ul>
<li><p>Décider du bouton actif par défaut</p></li>
<li><p>Dans les boutons radio, <em>Général→Groupe</em> sélectionner le nom du bouton actif par défaut dans le dialogue <em>Choisir un Bouton radio pour ce projet</em>.</p></li>
</ul>
</div>
<div class="paragraph">
<p>Voir <code>configs/gladevcp/by-widget/radiobutton</code> pour une application GladeVCP avec un fichier d’interface utilisateur, pour travailler sur les boutons radio.</p>
</div></td>
</tr>
</tbody>
</table>

### Les échelles (Scales)

HAL\_HScale et HAL\_VScale sont respectivement dérivées de GtkHScale et GtkVScale. Elles ont une pin de sortie FLOAT portant le même nom que le widget. Les échelles n’ont pas de propriété additionnelle.

Pour créer une échelle fonctionnelle dans Glade, ajouter un *Ajustement* (Général→Ajustement→Nouveau ou existant) et éditer l’objet ajustement. Il défini les valeurs défaut/min/max/incrément. Fixer la *Sensibilité de l’incrément* de l’ajustement sur automatique pour éviter les warnings.

Exemple d'échelle (HAL\_hscale): <span class="image"> ![](images/hscale.png) </span>

### La boîte d’incrément (SpinButton)

La boîte d’incrément de HAL est dérivée de GtkSpinButton, elle a deux pins de sortie:

 &lt;nomwidget&gt;-f   
out FLOAT pin

 &lt;nomwidget&gt;-s   
out S32 pin

Pour être fonctionnelle, Spinbutton doit avoir une valeur d’ajustement comme l'échelle, vue précédemment.

Exemple de boîte d’incrément: <span class="image"> ![](images/spinbutton.png) </span>

### Les labels

Le Label HAL est un simple widget basé sur GtkLabel qui représente la valeur d’une pin de HAL dans un format défini par l’utilisateur.

 HAL pin type   
Les pins de HAL sont des types (0:S32, 1:float ou 2:U32), voir aussi l’infobulle d’info sur *Général → HAL pin type*, (noter que c’est différent de PyVCP qui lui, a trois widgets label, un pour chaque type).

 text template   
Détermine le texte à afficher, une chaine au format Python pour convertir la valeur de la pin en texte. Par défauts, à `%s` (les valeurs sont converties par la fonction str()), mais peut contenir n’importe quel argument légal pour la méthode format() de Python. Exemple: `Distance: %.03f` va afficher le texte et la valeur de la pin avec 3 digits fractionnaires remplis avec des zéros pour une pin FLOAT.

### Les conteneurs: HAL\_HBox et HAL\_Table

Comparés à leurs contreparties Gtk ils ont une pin d’entrée BIT qui contrôle si les enfants des widgets sont sensitifs ou non. Si la pin est basse, alors les widgets enfants sont inactifs, ce qui est le comportement par défaut.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Astuce
</div></td>
<td align="left">Si vous trouvez que certaines parties de votre application GladeVCP sont <em>grisées</em> (insensible), vérifiez que les pins d’un conteneur ne soient pas inutilisées.</td>
</tr>
</tbody>
</table>

### Les Leds

La Led hal simule un vrai indicateur à Led. Elle a une seule pin d’entrée BIT qui contrôle son état: ON ou OFF. Les Leds ont quelques propriétés pour contrôler leur aspect:

 on\_color   
Une chaine définissant la couleur ON de la Led. Peut être tout nom valide de gtk.gdk.Color. Ne fonctionne pas sous Debian 8.04.

 off\_color   
Un chaine définissant la couleur OFF de la Led. Peut être tout nom valide de gtk.gdk.Color ou la valeur spéciale *dark*. *dark* signifie que la couleur OFF sera fixée à 0.4 valeur de la couleur ON. Ne fonctionne pas sous Debian 8.04.

 pick\_color\_on, pick\_color\_off   
Couleurs pour les états ON et OFF peuvent être représentées par une chaine comme *\#RRRRGGGGBBBB*. Ces propriétés optionnelles ont la précédence sur *on\_color* et *off\_color*.

 led\_size   
Rayon de la Led (pour une Led carrée, 1/2 côté)

 led\_shape   
Forme de la Led Shape. Les valeurs permises sont 0 pour ronde, 1 pour ovale et 2 pour carrée.

 led\_blink\_rate   
Si utilisée et que la Led est ON, alors la Led clignotera. La fréquence du clignotement est égal à la valeur de "led\_blink\_rate", spécifiée en millisecondes.

Comme un widget d’entrée, la Led aussi supporte le `hal-pin-changed signal`. Si vous voulez avoir une notification dans votre code quand les pins des Leds HAL ont changé d'état, alors connectez ce signal au gestionnaire, par exemple `on_led_pin_changed` et passez ce qui suit au gestionnaire:

    def on_led_pin_changed(self,hal_led,data=None):
        print "on_led_pin_changed() - HAL pin value:",hal_led.hal_pin.get()

Ce code sera appelé à chaque front du signal et également au démarrage du programme pour reporter la valeur courante.

Exemple de Leds: <span class="image"> ![](images/leds.png) </span>

### La barre de progression (ProgressBar)

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Note
</div></td>
<td align="left">Ce widget pourrait disparaître. Utilisez les widgets HAL_HBar et HAL_VBar à sa place.</td>
</tr>
</tbody>
</table>

La HAL\_ProgressBar est dérivée de gtk.ProgressBar et a deux pins d’entrée de HAL float:

 &lt;nomwidget&gt;   
la valeur courante à afficher.

 &lt;nomwidget&gt;-scale   
la valeur maximum absolue en entrée.

Elle a les propriétés suivantes:

 scale   
Valeur d'échelle. fixe la valeur maximum absolue en entrée. Pareil que la configuration de la pin &lt;nomwidget&gt;.scale. Un flottant, compris entre *-2<sup>24</sup>* et *+2<sup>24</sup>*.

 green\_limit   
Limite basse de la zone verte

 yellow\_limit   
Limite basse de la zone jaune

 red\_limit   
Limite basse de la zone rouge

 text\_template   
Texte modèle pour afficher la valeur courante de la pin `<nomwidget>`. Formaté pour Python, peut être utilisé pour dict `{"valeur":valeur}`.

Exemple de barre de progression: <span class="image"> ![](images/progressbar2.png) </span>

### La boîte combinée (ComboBox)

La comboBox HAL est dérivée de gtk.ComboBox. Elle valide le choix d’une valeur dans une liste déroulante.

Elle exporte deux pins de HAL:

 &lt;nomwidget&gt;-f   
La valeur courante, de type FLOAT

 &lt;nomwidget&gt;-s   
La valeur courante, de type S32

Elle a la propriété suivante, qui est configurable dans Glade:

 column   
L’index de colonne, type S32, défaut à -1, échelle de -1 à 100.

En mode par défaut, ces réglages du widget mettent les pins à la valeur d’index de l’entrée choisie dans la liste. Aussi, si le widget a trois labels, il peut seulement assumer les valeurs 0, 1 et 2.

En mode colonne (colonne &gt; -1), la valeur reportée est choisie dans le tableau de stockage de liste défini dans Glade. Ainsi, typiquement la définition du widget devrait comprendre deux colonnes dans le tableau de stockage, une avec le texte affiché dans la liste déroulante, l’autre une valeur entière ou flottante correspondante au choix.

Il y a un exemple dans `configs/gladevcp/by-widget/combobox/combobox.{py,ui}` qui utilise le mode colonne pour prendre une valeur flottante dans un stockage de liste.

Si comme moi, vous êtes désorienté pour éditer une liste de stockage de ComboBox ou de CellRenderer, voyez <http://www.youtube.com/watch?v=Z5_F-rW2cL8>.

### Les barres

Les widgets HAL, HBar et VBar pour barres Horizontale et Verticale, représentent des valeurs flottantes. Elles ont une pin d’entrée de HAL FLOAT. Chaque barre a les propriétés suivantes:

 invert   
Inverse les directions min avec max. Une HBar inversée croît de la droite vers la gauche, un VBar inversée croît du haut vers le bas.

 min, max   
Valeurs minimum et maximum de l'étendue souhaitée. Ce n’est pas une erreur si la valeur courante dépasse cette étendue.

 zero   
Point le plus bas de l'étendue. Si il est entre min et max, alors la barre croît à partir de cette valeur et non de la gauche du widget (ou de sa droite). Utile pour représenter des valeurs qui peuvent être à la fois, positives ou négatives.

 force\_width, force\_height   
Force la largeur ou la hauteur du widget. Si inutilisés, la taille sera déduite du conteneur ou de la taille des widgets et des barres qui remplissent la zone.

 text\_template   
Détermine le texte à afficher, comme pour le Label, pour les valeurs min/max/courante. Peut être utilisé pour arrêter l’affichage de la valeur.

 bg\_color   
Couleur de fond pour la barre (inactive).

 z0\_color, z1\_color, z2\_color   
Couleurs des zones des différentes valeurs. Par défaut, *green*, *yellow* et *red*. Pour une description des zones voir propriétés des *z \_border*.

 z0\_border, z1\_border   
Définissent les limites des zones de couleur. Par défaut, seule une zone est validée. Pour en activer plus d’une, fixer *z0\_border* et *z1\_border* aux valeurs souhaitées. Ainsi, zone 0 va remplir depuis 0 à la première bordure, zone 1 va remplir de la première à la seconde bordure et zone 2 depuis la dernière bordure jusqu'à 1. Les bordures se règlent comme des fractions, les valeurs vont de 0 à 1.

Barre horizontale: <span class="image"> ![](images/hal_hbar.png) </span> Barre verticale: <span class="image"> ![](images/vscale.png) </span>

### L’indicateur (HAL Meter)

L’indicateur est un widget similaire à celui de PyVCP, il représente une valeur flottante et a une pin d’entrée de HAL FLOAT. L’indicateur a les deux propriétés suivantes:

 min, max   
Valeurs minimum et maximum de l'étendue souhaitée. Ce n’est pas une erreur si la valeur courante dépasse cette étendue.

 force\_size   
Force le diamètre du widget. Si inutilisé, alors la taille sera déduite du conteneur ou des dimensions d’un widget à taille fixe. L’indicateur occupera alors l’espace le plus grand disponible, tout en respectant les proportions.

 text\_template   
Détermine le texte à afficher, comme pour le Label, pour la valeur courante. Peut être utilisé pour arrêter l’affichage de la valeur.

 label   
Label large au dessus du centre de l’indicateur.

 sublabel   
Petit label, sous le centre de l’indicateur.

 bg\_color   
Couleur de fond de l’indicateur.

 z0\_color, z1\_color, z2\_color   
Valeurs des couleurs des différentes zones. Par défaut, *green*, *yellow* et *red*. For description of zones see *z \_border* properties.

 z0\_border, z1\_border   
Définissent les limites externes des zones de couleur. Par défaut, une seule zone de couleur est définie. Pour en activer plus d’une, fixer *z0\_border* et *z1\_border* aux valeurs souhaitées. Ainsi, zone 0 va remplir depuis min à la première bordure, zone 1 va remplir de la première à la seconde bordure et zone 2 depuis la dernière bordure jusqu'à max. Les bordures se règlent sur une étendue comprise en min et max.

Exemples d’indicateurs:

![](images/hal_meter.png)

### Gremlin, visualiseur de parcours d’outil pour fichiers .ngc

Gremlin est un traceur de parcours d’outil similaire à celui d’Axis. Il demande un environnement Machinekit en fonctionnement, comme Axis ou Touchy. Pour se connecter à lui, inspecter la variable d’environnement INI\_FILE\_NAME. Gremlin affiche le fichiers .ngc courant. Si le fichier ngc est modifié, il doit être rechargé pour actualiser le tracé. Si il est lancé dans une application GladeVCP quand Machinekit n’est pas en marche, un message va être affiché parce-que le widget Gremlin ne trouve pas le statut de Machinekit, comme le nom du fichier courant.

Gremlin n’exporte aucune pin de HAL. Il a les propriétés suivantes:

 view   
Peut être la vue en *x*, *y*, *z*, *p* (perspective) . Par défaut, vue en *z*.

 enable\_dro   
Booléen; afficher une visu sur le tracé ou non. Par défaut,à *True*.

Exemple:

![](images/gremlin.png)

### Fonction de diagrammes animés: Widgets HAL dans un bitmap

Pour certaines applications, il est intéressant d’avoir une image de fond, comme un diagramme fonctionnel et positionner les widgets aux endroits appropriés dans le diagramme. Une bonne combinaison consiste à placer une image de fond comme un fichier .png, mettre la fenêtre GladeVCP en taille fixe, et utiliser Glade pour fixer la position du widget sur cette image.

Le code pour l’exemple ci-dessus peut être trouvé dans `configs/gladevcp/animated-backdrop`:

![](images/small-screenshot.png)

Références des Widgets Machinekit Action
----------------------------------------

GladeVcp inclus une collection d’actions préprogrammées appelées widgets *Machinekit Action* qui sont des Widgets pour l'éditeur Glade. À la différence des widgets HAL, qui interagissent avec les pins de HAL, les widgets Machinekit Actions, interagissent avec Machinekit et son interpréteur de G-code.

Les widgets Machinekit Action sont dérivés du widget Gtk.Action. Le widget Machinekit Action en quelques mots:

-   C’est un objet disponible dans l'éditeur Glade.

-   Il n’a pas d’apparence visuelle par lui-même.

-   Son but: associer à un composant d’interface visible, à un composant d’interface sensitif, comme un menu, un bouton outil, un bouton avec une commande. Voir les propriétés des widgets Action dans *Général → Related Action* de l'éditeur.

-   L’action préprogrammée sera exécutée quand l'état du composant associé basculera (bouton pressé, menu cliqué…)

-   Ils fournissent une voie facile pour exécuter des commandes sans avoir à faire appel à la programmation en Python.

L’apparence des Machinekit Actions dans Glade est approximativement la suivante:

![](images/emc-actions.png)

Le survol de la souris donne une infobulle.

### Les widgets Machinekit Action

Les widgets Machinekit Action sont des widgets de type simple état. Ils implémentent une seule action par l’usage, d’un seul bouton, d’une option de menu, d’un bouton radio ou d’une case à cocher.

### Les widgets Machinekit bascule action (ToggleAction)

Ce sont des widgets double état. Ils implémentent deux actions ou utilisent un second état (habituellement, *pressé*) pour indiquer qu’une action est actuellement en cours. Les bascules action sont prévues pour être utilisées avec les boutons à bascule (ToggleButtons) et les boutons à bascule d’outil (ToggleToolButtons) ou encore, pour basculer les items de menu. Un exemple simple est le bouton à bascule d’Arrêt d’Urgence (EStop).

Actuellement, les widgets suivants sont disponibles:

-   La bascule *d’Arrêt d’Urgence* (ESTOP) envoie la commande ESTOP ou ESTOP\_RESET à Machinekit, selon l'état courant.

-   La bascule *ON/OFF* envoie la commande STATE\_ON ou STATE\_OFF.

-   La bascule *Pause/Reprise* envoie la commande AUTO\_PAUSE ou AUTO\_RESUME.

Les bascules action suivantes ont seulement une commande associée et utilisent l'état *pressé* pour indiquer que l’opération demandée est lancée:

-   La bascule *Run* envoie la commande AUTO\_RUN et attends dans l'état pressé jusqu'à ce que l’interpréteur soit de nouveau au repos.

-   La bascule *Stop* est inactive jusqu'à ce que l’interpréteur passe à l'état actif (Un G-code est lancé) et permet alors à l’utilisateur d’envoyer la commande AUTO\_ABORT.

-   La bascule *MDI* envoie la commande passée dans le MDI et attends sa complétion dans l'état inactif *pressé*.

### La bascule Action\_MDI et les widgets Action\_MDI

Ces widgets fournissent le moyen d’exécuter des commandes MDI. Le widget Action\_MDI n’attends pas la complétion de la commande, comme le fait la bascule Action\_MDI, qui reste elle, désactivée tant que la commande n’est pas terminée.

### Un exemple simple: Exécuter une commande MDI lors de l’appui sur un bouton.

`configs/gladevcp/mdi-command-example/whoareyou.ui` est un fichier UI Glade qui transmet cette action basique:

L’ouvrir dans Glade et étudier comment il est fait. Lancer Axis puis dans un terminal faire: *+gladevcp whoareyou.ui+*. Voir l’action `hal_action_mdi1` et les propriétés de `MDI command` qui exécute juste `(MSG, "Hi, I’m an Machinekit_Action_MDI")` ce qui ouvre un popup de message dans Axis, comme ci-dessous:

![](images/whoareyou.png)

Noter que le bouton, associé à l’Action\_MDI, est grisé si la machine est arrêtée, en A/U ou si l’interpréteur est déjà en marche. Il deviendra automatiquement actif quand la machine sera mise en marche donc, sortie de l’A/U (E-Stop), et que le programme est au repos.

### Paramètres passés avec les widgets Action\_MDI et ToggleAction\_MDI

Optionnellement, la chaine *MDI command* peut avoir des paramètres substitués avant d'être passée à l’interpréteur. Ces paramètres sont actuellement les noms des pins de HAL dans les composants GladeVCP. Voici comment cela fonctionne:

-   Supposons que nous avons une *SpinBox HAL* nommée `speed`, nous voulons passer sa valeur courante comme paramètre dans une commande MDI.

-   La SpinBox HAL aura une pin de HAL de type flottant, nommée speed-f (voir la description des Widgets Hal).

-   Pour substituer cette valeur dans la commande MDI, insérons le nom de la pin de HAL

-   Pour la spinbox HAL précédente, il aurait été possible d’utiliser

L’exemple de fichier UI est `configs/gladevcp/mdi-command-example/speed.ui`. Voici ce qui ce qui est obtenu en le lançant:

![](images/speed.png)

### Un exemple plus avancé: Passer des paramètres à un sous-programme O-word

Il est parfaitement permis d’appeler un sous-programme O-word dans une commande MDI et passer la valeur des pins de HAL comme paramètres actuels. Un exemple de fichier UI est dans `configs/gladevcp/mdi-command-example/owordsub.ui`.

Placer `configs/gladevcp/nc_files/oword.ngc` de sorte qu’Axis puisse le trouver, et lancer *gladevcp owordsub.ui* depuis un terminal. Ce qui devrait ressembler à celà:

![](images/oword.png)

### Préparation d’une Action\_MDI

L’interpréteur de G-code de Machinekit dispose d’un simple jeu de variables globales, comme la vitesse travail, la vitesse broche, le mode relatif/absolu et autres. Si on utilise des commandes G-code ou des sous-programmes O-word, certaines de ces variables doivent être modifiées par la commande ou le sous-programme. Par exemple, un sous-programme de sonde a très probablement besoin de définir la vitesse d’avance à une valeur très faible. Sans autres précautions, le réglage de vitesse précédent serait écrasé par la valeur du sous-programme de sonde.

Pour faire avec ce surprenant, autant qu’indésirable effet de bord produit par un sous-programme O-word ou un G-code exécuté avec une bascule Action MDI, le gestionnaire pré-MDI et post-MDI doit être associé avec une bascule Action\_MDI donnée. Ces gestionnaires sont optionnels et fournissent une voie pour sauver tous les états avant d’exécuter l’action MDI et pour les restaurer ensuite aux valeurs précédentes. Les noms de signaux sont `mdi-command-start` et `mdi-command-stop`, les noms de gestionnaire peuvent être fixés dans Glade comme tout autre gestionnaire.

Voici un exemple, montrant comment la valeur de la vitesse d’avance est sauvée puis restaurée par de tels gestionnaires, noter que la commande Machinekit et le statut des voies sont disponibles comme `self.emc` et `self.stat` à travers la classe Machinekit\_ActionBase:

        def on_mdi_command_start(self, action, userdata=None):
            action.stat.poll()
            self.start_feed = action.stat.settings[1]

        def on_mdi_command_stop(self, action, userdata=None):
            action.emc.mdi('F%.1f' % (self.start_feed))
            while action.emc.wait_complete() == -1:
                pass

Seule le widget de la bascule Action\_MDI, supporte ces signaux.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Note
</div></td>
<td align="left">Dans une prochaine version de Machinekit, les nouveaux M-codes M70 à M72 seront disponibles, ils enregistreront l'état avant l’appel du sous-programme, la restauration de l'état au retour sera plus aisée.</td>
</tr>
</tbody>
</table>

### Utiliser l’objet Machinekit Stat pour traiter les changements de statut

Beaucoup d’actions dépendent du statut de Machinekit, est-il en mode manuel, en mode MDI ou en mode auto ? Un programme est-il en cours d’exécution, est-il en pause ou au repos ? Il est impossible de lancer une commande MDI tant qu’un programme G-code est en cours d’exécution, cela doit donc être pris en compte. Beaucoup d’actions Machinekit prennent cela en compte d’elle même, les boutons et les options de menu sont désactivés quand leurs actions sont rendues impossibles.

Avec l’utilisation des gestionnaires d'événements Python, qui sont à un niveau inférieur aux Actions, on doit prendre soin de traiter les dépendances de statut soit-même. À cette fin, existe le widget *Machinekit Stat*, il associe les changements de statut de Machinekit avec les gestionnaires d'événements.

Machinekit Stat n’a pas de composant visible, il suffi de l’ajouter dans l'éditeur Glade. Une fois ajouté, vous pouvez associer des gestionnaires avec les signaux suivants:

-   relatif au statut: émis quand l’arrêt d’urgence est activé, ou désactivé,

    -   `state-estop` la machine est totalement arrêtée, puissance coupée.

    -   `state-estop-reset` la machine passe à l’arrêt.

    -   `state-on`, la machine est mise en marche

    -   `state-off` la machine passe à l’arrêt.

-   relatif au mode: émis quand Machinekit entre dans un de ces modes particuliers

    -   `mode-manual`

    -   `mode-mdi`

    -   `mode-auto`

-   relatif à l’interpréteur: émis quand l’interpréteur de G-code passe dans un de ces modes

    -   `interp-run`

    -   `interp-idle`

    -   `interp-paused`

    -   `interp-reading`

    -   `interp-waiting`

Programmation de GladeVCP
-------------------------

### Actions définies par l’utilisateur

La plupart des jeux de widgets, par le biais de l'éditeur Glade, supportent le concept de fonction de rappel, fonctions écrites par l’utilisateur, qui sont exécutées quand *quelque chose arrive* dans l’UI, événements tels que clics de souris, caractère tapé, mouvement de souris, événements d’horloge, fenêtre iconisée ou agrandie et ainsi de suite.

Les widgets de sortie HAL, typiquement, scrutent les événements de type *entrée*, tels qu’un bouton pressé, provoquant un changement de la valeur d’une pin HAL associée par le biais d’une telle fonction de rappel prédéfinie. Dans PyVCP, c’est réellement le seul type d'événement qui peut être défini à la main. Faire quelque chose de plus complexe, comme exécuter une commande MDI pour appeler un sous-programme G-code, n’est pas supporté.

Dans GladeVCP, les changement sur les pins de HAL sont juste un type de la classe générale d'événements (appelés signaux) dans GTK+. La plupart des widgets peuvent générer de tels signaux et l'éditeur de Glade supporte l’association de ces signaux avec une méthode Python ou nom de fonction.

Si vous décidez d’utiliser les actions définies par l’utilisateur, votre travail consistera à écrire un module Python dont la méthode, une fonction suffit dans les cas simples, peut être référencée à un gestionnaire d'événements dans Glade. GladeVCP fournit un moyen d’importer votre module au démarrage, il sera alors lié automatiquement au gestionnaire d'événements avec les signaux de widget comme un ensemble dans la description de l'éditeur Glade.

### Un exemple: ajouter une fonction de rappel en Python

Ceci est juste un exemple minimal pour exprimer l’idée, les détails sont donnés dans le reste de cette section.

GladeVCP peut, non seulement manipuler ou afficher les pins de HAL, il est possible aussi d'écrire des gestionnaires d'événements en Python. Ce qui peut être utilisé, entre autre, pour exécuter des commandes MDI. Voici comment faire:

Écrire un module Python comme le suivant, et l’enregistrer sous le nom handlers.py

    nhits = 0
    def on_button_press(gtkobj,data=None):
        global nhits nhits += 1 gtkobj.set_label("hits: %d" % nhits)

Dans Glade, définir un bouton ou un bouton HAL, sélectionner l’onglet *Signal*, et dans les propriétés GtkButton sélectionner la ligne *pressed*. Entrer *on\_button\_press* ici, puis enregistrer le fichier Glade.

Ensuite, ajouter l’option *-u handlers.py* à la ligne de commande de gladevcp. Si les gestionnaires d'événements son répartis sur plusieurs fichiers, ajouter de multiples options *-u &lt;pynomfichier&gt;*.

Maintenant, presser le bouton devrait modifier son label car il est défini dans la fonction de rappel.

Que fait le drapeau `-u`: toutes les fonctions Python dans ce fichier sont collectées et configurées comme des gestionnaires de fonction de rappel potentiels pour les widgets Gtk, ils peuvent être référencés depuis l’onglet *Signaux* de Glade. Le gestionnaire de fonction de rappel est appelé avec l’instance de l’objet particulier comme paramètre, comme l’instance du GtkButton précédente, ainsi, il est possible d’appliquer n’importe quelle méthode GtkButton depuis ici.

Ou faire des choses plus utiles, par exemple, appeler une commande MDI!

### L'événement valeur de HAL modifiée

Les widgets d’entrée HAL, comme la Led, ont l'état de leur pin de HAL (on/off), automatiquement associé avec l’apparence optique du widget (Led allumée/éteinte).

Au delà de cette fonctionnalité primitive, on peut associer n’importe quelle pin de HAL avec une fonction de rappel, y compris les widgets de HAL prédéfinis. Cela correspond bien avec la structure événementielle de l’application typique du widget: chaque activité, qu’elle soit un simple clic de souris, une touche pressée, une horloge expirée ou le changement de valeur d’une pin de HAL, générera une fonction de rappel et sera gérée par le même mécanisme.

Pour les pins de HAL définies par l’utilisateur, non associées à un widget de HAL particulier, le nom du signal est *value-changed*. Voir la section [Ajouter des pins de HAL](#gladevcp:Adding_HAL_pins) pour plus de détails.

Les widgets HAL sont fournis avec un signal prédéfini appelé *hal-pin-changed*. Voir la section sur [les Widgets HAL](#gladevcp::hal-pin-changed_signal) pour d’autres détails.

### Modèle de programmation

L’approche globale est la suivante:

-   Concevoir l’interface graphique avec Glade, fixer les gestionnaires de signaux associés aux widgets action.

-   Écrire un module Python qui contient des objets appelables (voir 'gestionnaire de modèles, plus loin)

-   Passer le chemin du modules à gladevcp avec l’option *-u &lt;module&gt;*.

-   gladevcp importe le module, inspecte les gestionnaires de signaux et les connecte à l’arbre des widgets.

-   La boucle principale d'événements est exécutée.

#### Modèle du gestionnaire simple

Pour des tâches simple, il est suffisant de définir des fonctions nommées après les gestionnaires de signaux de Glade. Elles seront appelées quand l'événement correspondant se produira dans l’arbre des widgets. Voici un exemple très simple, il suppose que le signal *pressed* d’un bouton Gtk ou d’un bouton HAL est lié à une fonction de rappel appelée *on\_button\_press*:

    nhits = 0
    def on_button_press(gtkobj,data=None):
        global nhits
        nhits += 1
        gtkobj.set_label("hits: %d" % nhits)

Ajouter cette fonction dans un fichier Python et le lancer avec:

    gladevcp -u <myhandler>.py mygui.ui

Noter que la communication entre les gestionnaires doit passer par des variables globales, qui s’adaptent mal est ne sont pas très "pythonique". C’est pourquoi nous en arrivons au gestionnaire de classes.

#### Modèle de gestionnaire basé sur les classes

L’idée ici est la suivante: les gestionnaires sont liés aux méthodes de classe. La classe sous-jacente est instanciée et inspectée durant le démarrage de GladeVCP et liée à l’arbre des widgets comme gestionnaire de signaux. Donc, la tâche est maintenant d'écrire:

-   Une ou plusieurs définitions de classe avec une ou plusieurs méthodes, dans un module ou répartis sur plusieurs modules.

-   Une fonction *get\_handlers* dans chaque module, qui retournera la liste des instances de classe à GladeVCP, leurs noms de méthode seront liés aux gestionnaires de signaux.

Voici un exemple minimaliste de module de gestionnaire définit par l’utilisateur:

    class MyCallbacks :
        def on_this_signal(self,obj,data=None):
            print "this_signal happened, obj=",obj
        def get_handlers(halcomp,builder,useropts):
            return [MyCallbacks ()]

Maintenant, *on\_this\_signal* est disponible comme gestionnaire de signal dans l’arbre des widgets.

#### Le protocole get\_handlers

Si durant l’inspection du module GladeVCP trouve une fonction *get\_handlers*, Il l’appelle de la manière suivante:

    get_handlers(halcomp,builder,useropts)

Les arguments sont:

-   halcomp - Se réfère au composant de HAL en construction.

-   builder - arbre du widget - résulte de la lecture de la définition de l’UI (soit, en référence à un objet de type GtkBuilder ou de type libglade).

-   useropts - Une liste de chaines collectée par l’option de la ligne de commande de gladevcp *-U &lt;useropts&gt;*.

GladeVCP inspecte alors la liste des instances de classe et récupère leurs noms. Les noms de méthode sont connectés à l’arbre des widgets comme gestionnaire de signaux. Seuls, les noms de méthode ne commençant pas par un **\_** (tiret bas) sont considérés.

Noter que peu importe si la libglade ou le nouveau format GtkBuilder est utilisé pour l’UI Glade, les widgets peuvent toujours être soumis au *builder.get\_object(&lt;nomwidget&gt;)*. En outre, la liste complète des widgets est disponible par *builder.get\_objects()*, indépendamment du format de l’UI.

### Séquence d’initialisation

Il est important de connaitre pour quoi faire, la fonction *get\_handlers()* est appelée, et connaitre ce qui est sûr et ce qui ne l’est pas. Tout d’abord, les modules sont importés et initialisés dans leur ordre d’apparition sur la ligne de commande. Après le succès de l’importation, *get\_handlers()* est appelé selon les étapes suivantes:

-   L’arbre du widget est créé, mais pas encore réalisé (pas tant que le niveau supérieur *window.show()* n’aura pas été exécuté)

-   Le composant de HAL, halcomp, est configuré et toutes les pins de HAL des widgets lui sont ajoutées.

-   Il est sûr d’ajouter plus de pins de HAL parce-que *halcomp.ready()* n’a pas encore été appelé à ce point, ainsi, on peut ajouter ses propres pins, par exemple, dans la méthode de classe **init*()*.

Après que tous les modules ont été importés et que les noms des méthodes ont été extraits, les étapes suivantes se produisent:

-   Tous les noms de méthode qualifiés seront connectés à l’arbre du widget avec *connect\_signals() ou signal\_autoconnect()* (selon le type de l’UI importée, format GtkBuilder ou l’ancien libglade).

-   Le composant de HAL est finalisé avec halcomp.ready().

-   Si un ID de fenêtre est passé comme argument, l’arbre du widget est re-apparenté pour démarrer dans cette fenêtre, et la fenêtre de niveau supérieur de Glade, window1 est abandonnée (voir la FAQ)

-   Si un fichier de commandes de HAL, est passé avec *-H halfile*, il est exécuté avec halcmd.

-   La boucle principal de Gtk est lancée.

Ainsi, lorsque le gestionnaire de classe est initialisé, tous les widgets sont existants mais pas encore réalisés (affichés à l'écran). Et le composant de HAL n’est pas prêt non plus, de sorte qu’il n’est pas sûr d’accéder aux valeurs des pins dans la méthode **init*()*.

Si on doit avoir une fonction de rappel à exécuter au démarrage du programme mais, après qu’il soit sûr d’accéder aux pins de HAL, alors connecter un gestionnaire au signal de la fenêtre de niveau supérieur réalisée, window1 (qui pourrait être sa seule raison d'être). A ce point, GladeVCP en a terminé avec toutes les configurations, le halfile a bien été lancé et GladeVCP est sur le point d’entrer dans la boucle principale Gtk.

### Multiple fonctions de rappel avec le même nom

Dans une classe, les noms de méthode doivent être unique. Cependant, il est permis d’avoir de multiples instances de classe passées à GladeVCP par get\_handlers() avec des méthodes portant le même nom. Lorsque le signal correspondant survient, les méthodes sont appelées dans l’ordre dans lequel elles ont été définies, module par module et dans un module, dans l’ordre des instances de classe retourné *get\_handlers()*.

### Le drapeau GladeVCP **-U &lt;useropts&gt;**

Au lieu d'étendre GladeVCP à toutes les options concevables qui pourraient potentiellement être utilisées par un gestionnaire de classe, on peut utiliser le drapeau -U&lt;useroption&gt; (répétitivement si nécessaire). Ce drapeau collecte la liste des chaines de &lt;useroption&gt;. Cette liste est passée à la fonction get\_handlers() (argument useropts). Le code est libre d’interpréter ces chaines comme bon lui semble. Un utilisation possible serait de les passer à la fonction exec de Python dans le *get\_handlers()*, comme suit:

    debug = 0
    ...
    def get_handlers(halcomp,builder,useropts):
        ...
        global debug # suppose qu'il y a une variable globale
        pour cmd dans useropts:
            exec cmd in globals()

De cette façon, on peut passer des déclarations Python arbitraires au module grâce à l’option gladevcp -U, Par exemple:

    gladevcp -U debug=42 -U "print 'debug=%d' % debug" ...

Debug devrait être mis à 2, et confirmer ce que le module fait actuellement.

### Variables persistantes dans GladeVCP

Un aspect gênant de GladeVCP dans sa forme initiale avec pyvcp est le fait qu’on peut changer les valeurs des pins de HAL au travers du texte saisi, curseurs, bouton tournant, bouton à bascule etc, mais leurs paramètres ne sont pas enregistrés ni restaurés à la prochaine exécution de Machinekit. Ils commencent aux valeurs par défaut fixées dans le panneau ou la définition du widget.

GladeVCP dispose d’un mécanisme facile à utiliser pour enregistrer et restaurer l'état des widgets de HAL, ainsi que les variables du programme (en fait, n’importe quel attribut d’instance de type int, float, bool ou string).

Ce mécanisme utilise le format du populaire fichier *.ini* pour enregistrer et recharger les attributs persistants.

#### Examen de la persistance, de la version et de la signature du programme

Imaginons renommer, ajouter ou supprimer des widgets dans Glade: un fichier .ini qui traîne depuis une version précédente du programme, ou une interface utilisateur entièrement différente, ne serait pas en mesure de restaurer correctement l'état des variables et des types puisqu’ils ont changé depuis.

GladeVCP détecte cette situation par la signature qui dépends de tous les noms d’objets et de types qui ont été enregistrés et qui doivent être restaurés. Dans le cas de signatures incompatibles, un nouveau fichier .ini avec la configuration pas défaut est généré.

### Utilisation des variables persistantes

Pour que tous les états des widgets Gtk, que toutes les valeurs des pins de sortie des widget HAL et/ou que tous les attributs de classe du gestionnaire de classe soient conservés entre les invocations, procéder comme suit:

-   Importer le module `gladevcp.persistence`.

-   Décider quels attributs d’instance et leurs valeurs par défaut doivent être conservés, le cas échéant,

-   décider quels widgets doivent avoir leur état conservé.

-   Décrire ces décisions dans le gestionnaire de classe par la méthode `init()` grâce à un dictionnaire imbriqué comme suit:

    def __init__(self, halcomp,builder,useropts):
        self.halcomp = halcomp
        self.builder = builder
        self.useropts = useropts
        self.defaults = {
            # les noms suivants seront enregistrés/restaurés comme attributs de méthode,
            # le mécanisme d'enregistrement/restauration est fortement typé,
            # les types de variables sont dérivés depuis le type de la valeur initiale.
            # les types couramment supportées sont: int, float, bool, string
            IniFile.vars : { 'nhits' : 0, 'a': 1.67, 'd': True ,'c' : "a string"},
            # pour enregistrer/restaurer l'état de tous les widgets pour lesquels
            # c'est sensé, ajouter cela:
            IniFile.widgets : widget_defaults(builder.get_objects())
            # une alternative sensée pourrait être de ne retenir que l'état de
            # tous les widgets de sortie HAL:
            # IniFile.widgets: widget_defaults(select_widgets(self.builder.get_objects(),
    hal_only=True,output_only = True)),
        }

Puis associer un fichier .ini avec ce descripteur:

    self.ini_filename = __name__ + '.ini'
    self.ini = IniFile(self.ini_filename,self.defaults,self.builder)
    self.ini.restore_state(self)

Ensuite *restore\_state()*, aura automatiquement les attributs définis si ce qui suit a été exécuté:

    self.nhits = 0
    self.a = 1.67
    self.d = True
    self.c = "a string"

Noter que les types sont enregistrés et conservés lors de la restauration. Cet exemple suppose que le fichier .ini n’existe pas ou qu’il contient les valeurs par défaut depuis self.defaults.

Après cette incantation, on peut utiliser les méthodes IniFil suivantes:

 ini.save\_state(obj)   
enregistre les attributs des objets depuis le dictionnaire IniFil.vars l'état du widget comme décrit par IniFile.widgets dans self.defaults

 ini.create\_default\_ini()   
crée un fichier .ini avec les valeurs par défaut

 ini.restore\_state(obj)   
restaure les pins de HAL et les attributs des objets enregistrés/initialisés par défaut comme précédemment

Pour enregistrer le widget et/ou l'état des variables en quittant, connecter un gestionnaire de signal à la fenêtre de niveau supérieur `window1`, détruire l'événement:

    def on_destroy(self,obj,data=None):
        self.ini.save_state(self)

La prochaine fois que l’application GladeVCP démarrera, les widgets doivent retrouver l'état qu’ils avaient à la fermeture de l’application.

### Édition manuelle des fichiers .ini

Il est possible de faire cela, mais noter que les valeurs dans self.defaults écraseront votre édition si il y a erreur de frappe ou de syntaxe. Une erreur détectée, un message émis dans la console, donneront des indices sur ce qui s’est passé et le mauvais fichier ini sera renommé avec le suffixe .BAD. Après une mauvaise initialisation, les fichiers .BAD les plus anciens seront écrasés.

### Ajouter des pins de HAL

Si il faut des pins de HAL non associées avec un widget HAL, les ajouter comme ci-dessous:

    import hal_glib
    ...
    # dans le gestionnaire de classe __init__():
    self.example_trigger = hal_glib.GPin(halcomp.newpin('example-trigger', hal.HAL_BIT, hal.HAL_IN))

Pour appeler une fonction de rappel quand la valeur de cette pin change il faut associer une fonction de rappel `value-changed` avec cette pin, ajouter pour cela:

    self.example_trigger.connect('value-changed', self._on_example_trigger_change)

et définir une méthode de fonction de rappel (ou une fonction, dans ce cas laisser tomber le paramètre `self`):

    # noter *_* - cette méthode n'est pas visible dans l'arbre du widget
    def _on_example_trigger_change(self,pin,userdata=None):
        print "pin value changed to:" % (pin.get())

### Ajout de timers

Depuis que GladeVCP utilise les widgets Gtk qui se rattachent sur les classes de base [GObject](http://www.pygtk.org/pygtk2reference/gobject-functions.html), la totalité des fonctionnalités de la glib est disponible. Voici un exemple d' horloge de fonction de rappel:

    def _on_timer_tick(self,userdata=None):
        ...
        return True # pour relancer l'horloge; return False pour un monostable
    ...
    # démonstration d'une horloge lente en tâche de fond - la granularité est de une seconde
    # pour une horloge rapide (granularité 1 ms), utiliser cela:
    # glib.timeout_add(100, self._on_timer_tick,userdata) # 10Hz
    glib.timeout_add_seconds(1, self._on_timer_tick)

### Exemples, et lancez votre propre application GladeVCP

Visiter `machinekit/configs/gladevcp` pour des exemples prêt à l’emploi et points de départ de vos propres projets.

Questions & réponses
--------------------

1.  *Je reçois un événement unmap inattendu dans ma fonction de gestionnaire juste après le démarrage, qu’est-ce que c’est?*

    C’est la conséquence d’avoir dans votre fichier d’UI Glade la propriété de la fenêtre window1 visible fixée à True, il y a changement de parents de la fenêtre GladeVCP dans Axis ou touchy. L’arbre de widget de GladeVCP est créé, incluant une fenêtre de niveau supérieur puis *re-aparenté dans Axis*, laissant trainer les orphelins de la fenêtre de niveau supérieur. Pour éviter d’avoir cette fenêtre vide qui traine, elle est unmapped (rendue invisible) et la cause du signal unmap que vous avez eux. Suggestion pour fixer le problème: fixer window1.visible à False et ignorer le message initial d'événement unmap.

2.  *Mon programme GladeVCP démarre, mais aucune fenêtre n’apparait alors qu’elle devrait.*

    La fenêtre allouée par Axis pour GladeVCP obtient la *taille naturelle* de tous ses enfants combinés. C’est au widget enfant a réclamer une taille (largeur et/ou hauteur). Cependant, toutes le fenêtres ne demandent pas une plus grande que 0, par exemple, le widget Graph dans sa forme courante. Si il y a un tel widget dans votre fichier Glade et que c’est lui qui défini la disposition vous devrez fixer sa largeur explicitement. Noter que la largeur et la hauteur de la fenêtre window1 dans Glade n’a pas de sens puisque cette fenêtre sera orpheline lors du changement de parent et donc sa géométrie n’aura aucun impact sur les mise en page (voir ci-dessus). La règle générale est la suivante: si vous exécutez manuellement un fichier UI avec *gladevcp &lt;fichierui&gt;* et que sa fenêtre a une géométrie raisonnable, elle devrait apparaitre correctement dans Axis.

3.  *Je veux une Led clignotante, alors j’ai coché une case pour la laisser clignoter avec un intervalle de 100ms. Elle devrait clignoter, mais je reçois un :Warning: value *0* le type *gint* est invalide ou hors de l'étendue pour les propriétés de *led-blink-rate*, c’est quoi le type gint?*

    Il semble qu’il s’agisse d’un bug de Glade. Il faut re-saisir une valeur sur le champ de la fréquence de clignotement et enregistrer à nouveau. Ça a marché pour moi.

4.  *Mon panneau gladevcp ne marche pas dans Axis, il n’enregistre pas les états quand je ferme Axis, j’ai pourtant défini un gestionnaire on\_destroy attaché au signal destroy de la fenêtre.*

    Ce gestionnaire est très probablement lié à window1, qui en raison du changement de parent ne peux pas assurer cette fonction. Attachez le gestionnaire on\_destroy handler au signal destroy d’une fenêtre intérieure. Par exemple: J’ai un notebook dans window1, attaché on\_destroy au signal destroy de notebooks et ça marche bien. Il ne marcherait pas pour window1.

Troubleshooting
---------------

<span class="comment"> FIXME this is out of date</span>

-   make sure your have the development version of Machinekit installed. You don’t need the axisrc file any more, this was mentioned in the old GladeVcp wiki page.

-   run GladeVCP or Axis from a terminal window. If you get Python errors, check whether there’s still a `/usr/lib/python2.6/dist-packages/hal.so` file lying around besides the newer `/usr/lib/python2.6/dist-packages/_hal.so` (note underscore); if yes, remove the `hal.so` file. It has been superseded by hal.py in the same directory and confuses the import mechanism.

-   if you’re using run-in-place, do a *make clean* to remove any accidentally left over hal.so file, then *make*.

-   if you’re using *HAL\_table* or *HAL\_HBox* widgets, be aware they have an HAL pin associated with it which is off by default. This pin controls whether these container’s children are active or not.

Notes d’implémentation: la gestion des touches dans Axis
--------------------------------------------------------

Nous pensons que la gestion des touches fonctionne bien, mais comme c’est un nouveau code, nous devons vous informer à ce propos pour que vous puissiez surveiller ces problèmes; S’il vous plaît, faites nous savoir si vous connaissez des erreurs ou des choses bizarres. Voici l’histoire:

Axis utilise le jeu de widget de TkInter. L’application GladeVCP utilise les widgets Gtk et démarre dans un contexte de processus différent. Ils sont attachés dans Axis avec le protocole Xembed. Ce qui permet à une application enfant comme GladeVCP de bien tenir proprement dans la fenêtre d’un parent et, en théorie, d'être intégrée au gestionnaire d'événements.

Toutefois, cela suppose que parent et enfant supportent tous les deux proprement le protocole Xembed, c’est le cas avec Gtk, pas avec TkInter. Une conséquence de cela, c’est que certaines touches ne sont pas transmises correctement dans toutes les circonstances depuis un panneau GladeVCP vers Axis. Une d’elle est la touche *Entrée*. Ou quand le widget SpinButton a le focus, dans ce cas, par exemple la touche Échap n’est pas bien transmise à Axis et cause un abandon avec des conséquences potentiellement désastreuses.

Par conséquent, les événements touches dans GladeVCP, sont traités explicitement, et sélectivement transmises à Axis, pour assurer que de telles situations ne puissent pas survenir. Pour des détails, voir la fonction *keyboard\_forward()* dans la *lib/python/gladevcp/xembed.py*.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


