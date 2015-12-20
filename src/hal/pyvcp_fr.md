PyVCP
=====

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Panneau-Virtuel-Control"></span>

Introduction
------------

Panneau virtuel de contrôle en python (**Py**thon **V**irtual **C**ontrol **P**anel)

Le panneau de contrôle virtuel pyVCP a été créé pour donner à l’intégrateur la possibilité de personnaliser l’interface graphique d’AXIS avec des boutons et des indicateurs destinés aux tâches spéciales.

Le coût d’un panneau de contrôle physique est très élevé et il peut utiliser un grand nombre de broches d’entrées/sorties. C’est là que le panneau virtuel prends l’avantage car il ne coûte rien d’utiliser pyVCP.

Les panneaux de contrôle virtuels peuvent être utilisés pour tester ou monitorer le matériel, les entrées/sorties, remplacer temporairement d’autres matériels d’entrées/sorties pendant le déboguage d’une logique ladder ou pour simuler un panneau physique, avant de le construire et de le câbler vers les cartes électroniques.

L’image suivante montre quelques widgets pyVCP.

![](images/pyvcp_group.png)

Construction d’un panneau pyVCP
-------------------------------

La disposition d’un panneau pyVCP est spécifiée dans un fichier XML qui contient les balises des widgets entre &lt;pyvcp&gt; et &lt;/pyvcp&gt;. Par exemple:

    <pyvcp>
       <label text="Ceci est un indicateur à LED"/>
       <led/>
    </pyvcp>

![](images/pyvcp_mypanel_fr.png)

Si vous placez ce texte dans un fichier nommé tiny.xml et que vous le lancez avec:

    pyvcp -c panneau tiny.xml

pyVCP va créer le panneau pour vous, il y inclut deux widgets, un Label avec le texte *Ceci est un indicateur à LED* et une LED rouge, utilisée pour afficher l'état d’un signal HAL de type BIT. Il va aussi créer un composant HAL nommé *panneau* (tous les widgets dans ce panneau sont connectés aux pins qui démarrent avec *panneau*). Comme aucune balise &lt;halpin&gt; n'était présente à l’intérieur de la balise &lt;led&gt;, pyVCP nomme automatiquement la pin HAL pour le widget LED panneau.led.0

Pour obtenir la liste des widgets, leurs balises et options, consultez [la documentation des widgets](#sec:Documentation-des-widgets).

Un fois que vous avez créé votre panneau, connectez lui les signaux HAL, vers et à partir des pins pyVCP et avec la commande habituelle:

    net <signal-name> <pin-name> <opt-direction> <opt-pin-name>signal-name

Si vous débutez avec HAL, [le tutoriel de HAL](#cha:Tutoriel-HAL) est vivement recommandé.

Sécurité avec pyVCP
-------------------

Certaines parties de pyVCP sont évaluées comme du code Python, elles peuvent donc exécuter n’importe quelle action disponible dans les programmes Python. N’utilisez que des fichiers pyVCP en .xml à partir d’une source de confiance.tag

Utiliser pyVCP avec AXIS
------------------------

Puisque AXIS utilise le même environnement graphique et les même outils (Tkinter) que pyVCP, il est possible d’inclure un panneau pyVCP sur le côté droit de l’interface utilisateur normale d’AXIS. Un exemple typique est présenté ci-dessous.

Placer le fichier pyVCP XML décrivant le panneau dans le même répertoire que le fichier .ini. Nous voulons afficher la vitesse courante de la broche sur un widget barre de progression. Copier le code XML suivant dans un fichier appelé broche.xml:

    <pyvcp>
        <label>
            <text>"Vitesse broche:"</text>
        </label>
        <bar>
            <halpin>"spindle-speed"</halpin>
            <max_>5000</max_>
        </bar>
    </pyvcp>

Ici nous avons fait un panneau avec un label *Vitesse broche:* et un widget barre de progression. Nous avons spécifié que la pin HAL connectée à la barre de progression devait s’appeler *spindle-speed* et régler la valeur maximum de la barre à 5000 (se reporter à la [documentation des widgets](#sec:Documentation-des-widgets), pour toutes les options disponibles). Pour faire connaître ce fichier à AXIS et qu’il l’appelle au démarrage, nous devons préciser ce qui suit dans la section \[DISPLAY\] du fichier .ini:

    PYVCP = broche.xml

Pour que notre widget affiche réellement la vitesse de la broche *spindle-speed*, il doit être raccordé au signal approprié de HAL. Le fichier .hal qui sera exécuté quand AXIS et pyVCP démarreront doit être spécifié, de la manière suivante, dans la section \[HAL\] du fichier .ini:

    POSTGUI_HALFILE = broche_vers_pyvcp.hal

Ce changement lancera la commande HAL spécifiée dans *broche\_vers\_pyvcp.hal*. Dans notre exemple, ce fichier contiendra juste la commande suivante:

    net spindle-rpm-filtered  => pyvcp.spindle-speed

ce qui suppose que le signal appelé *spindle-rpm-filtered* existe aussi. Noter que lors de l’exécution avec AXIS, toutes les pins des widgets de pyVCP ont des noms commençant par *pyvcp.*.

![](images/pyvcp_AXIS_fr.png)

Voila à quoi ressemble le panneau pyVCP que nous venons de créer, incorporé à AXIS. La configuration *sim/lathe* fournie en exemple, est configurée de cette manière.

Panneaux PyVCP autonomes
------------------------

Cette section va décrire comment les panneaux PyVCP peuvent être affichés par eux même, par l’intermédiaire ou non des contrôleurs machine de Machinekit.

Pour charger un panneau PyVCP autonome avec Machinekit utiliser cette commande:

    loadusr -Wn monpanneau pyvcp -g WxH+X+Y -c monpanneau <path/>fichier_panneau.xml

Vous l’utiliserez pour avoir un panneau flottant ou un panneau avec une interface graphique autre que Axis.

-   *-Wn monpanneau* - Fait attendre à HAL que le composant *monpanneau* soit chargé (devienne *ready* en langage HAL), avant d’exécuter d’autres commandes HAL. C’est important parce-que les panneaux PyVCP exportent des pins de HAL ainsi que d’autres composants de HAL qui doivent être présents pour pouvoir se connecter à eux. Noter la lettre **W** en majuscule et la lettre **n** en minuscule. Si vous utilisez l’option -Wn vous devez également utiliser l’option -c pour nommer le panneau.

-   *pyvcp &lt; -g&gt; &lt; -c&gt; panneau.xml* - Construit le panneau avec la géométrie optionnelle et/ou le nom de panneau depuis le fichier panneau.xml. Le fichier panneau.xml peut avoir n’importe quel nom avec l’extension .xml. Le fichier .xml décrit comment construire le panneau. Il est nécessaire d’ajouter le nom du chemin si le panneau n’est pas dans le répertoire dans lequel se trouve le script HAL.

-   *-g &lt;WxH&gt;&lt;+X+Y&gt;* - Spécifie la géométrie à utiliser quand le panneau est construit. La syntaxe est *Largeur x Hauteur + Ancrage X + Ancrage Y*. La taille ou la position, ou les deux peuvent être fixés. Le point d’ancrage est le coin supérieur gauche du panneau. Par exemple; -g 250x500+800+0 fixe le panneau à 250 pixels de large, 500 pixels de haut avec le point d’ancrage placé en X800 Y0.

-   *-c nompanneau* - Indique à PyVCP quel composant appeler et le titre de la fenêtre. Le nom du fichier *nompanneau* peut être n’importe quel nom sans espace.

Pour charger un panneau PyVCP autonome, sans Machinekit utiliser cette commande:

    loadusr -Wn monpanneau pyvcp -g 250x500+800+0 -c monpanneau monpanneau.xml

La commande minimale pour charger un panneau pyvcp est la suivante:

    loadusr pyvcp monpanneau.xml

Vous pourrez utiliser cette commande si vous voulez un panneau sans passer par un des contrôleurs machine de Machinekit, par exemple pour des tests ou une visu autonome.

La commande loadusr est utilisée quand vous chargez aussi un composant qui stoppera HAL depuis la fermeture jusqu'à ce qu’il soit prêt. Si vous avez chargé un panneau puis chargé Classic Ladder en utilisant la commande *loadusr -w classicladder*, CL maintiendra HAL et le panneau ouverts jusqu'à ce que vous fermiez Classic Ladder. Le *-Wn* signifie d’attendre que le composant *-Wn "nom"* devienne prêt. (*nom* peut être n’importe quel nom. Noter la lettre **W** en majuscule et le **n** en minuscule.) Le -c indique à PyVCP de construire un panneau avec le nom *monpanneau* en utilisant les infos contenues dans le fichier *monpanneau.xml*. Le nom du fichier *monpanneau.xml* est sans importante mais doit porter l’extension .xml. C’est le fichier qui décrit comment construire le panneau. Il est nécessaire d’ajouter le nom du chemin si le panneau n’est pas dans le répertoire dans lequel se trouve le script HAL.

Une commande optionnelle à utiliser si vous voulez que le panneau stoppe HAL depuis les commandes *Continuer* / *Quitter*. Après avoir chargé n’importe quelles autres composants la dernière commande HAL sera:

    waituser nompanneau

Cette commande indique à HAL d’attendre que le composant *nompanneau* soit fermé avant de continuer avec d’autres commandes. C’est généralement défini comme étant la dernière commande, de sorte que HAL s’arrêtera si le panneau est fermé.

Documentation des widgets de pyVCP
----------------------------------

Les signaux de HAL existent en deux variantes, BIT et FLOAT. pyVCP peut afficher la valeur d’un signal avec un widget indicateur, ou modifier la valeur d’un signal avec un widget de contrôle. Ainsi, il y a quatre classes de widgets pyVCP connectables aux signaux de HAL. Une cinquième classe de widgets d’aide permet d’organiser et d’appliquer des labels aux panneaux.

-   Widgets de signalisation, signaux de type bit: led, rectled

-   Widgets de contrôle, signaux de type bit: button, checkbutton, radiobutton

-   Widgets de signalisation de type nombre: number, s32, u32, bar, meter

-   Widgets de contrôle de type nombre: spinbox, scale, jogwheel

-   Widgets d’aide: hbox, vbox, table, label, labelframe

### Syntaxe

Chaque widget sera décrit brièvement, suivi par la forme d'écriture utilisée et d’une capture d'écran. Toutes les balises contenues dans la balise du widget principal, sont optionnelles.

### Notes générales

Á l’heure actuelle, les deux syntaxes, basée sur les balises et basée sur les attributs, sont supportées. Par exemple, les deux fragments de code XML suivants sont traités de manière identique:

    <led halpin="ma-led"/>

et

    <led><halpin>"ma-led"</halpin></led>

Quand la syntaxe basée sur les attributs est utilisée, les règles suivantes sont utilisées pour convertir les valeurs des attributs en valeurs Python:

1.  Si le premier caractère de l’attribut est un des suivants: *{(\["'* , il est évalué comme une expression Python.

2.  Si la chaine est acceptée par int(), la valeur est traitée comme un entier.

3.  Si la chaine est acceptée par float(), la valeur est traitée comme un flottant.

4.  Autrement, la chaine est acceptée comme une chaine.

Quand la syntaxe basée sur les balises est utilisée, le texte entre les balises est toujours évalué comme un expression Python.

Les exemples ci-dessous montrent un mélange des deux formats.

### Commentaires

Pour ajouter un commentaire utiliser la syntaxe de xml.

    <!-- Mon commentaire -->

### Editer un fichier XML

Editer le fichier XML avec un éditeur de texte. La plupart du temps un double click sur le nom de fichier permet de choisir *ouvrir avec l’editeur de texte* ou similaire.

### Couleurs

Les couleurs peuvent être spécifiées en utilisant les couleurs RGB de X11 soit par le nom, par exemple: *gray75* ou soit en hexa décimal, par exemple: *\#0000ff*. Une liste complète est consultable ici: <http://sedition.com/perl/rgb.html>.

Couleurs les plus courantes (les numéros suivant la couleur indiquent la nuance de la couleur)

-   white (blanc)

-   black (noir)

-   blue et blue1 - blue4 (bleu)

-   cyan et cyan1 - cyan4 (cyan)

-   green et green1 - green4 (vert)

-   yellow et yellow1 - yellow4 (jaune)

-   red et red1 - red4 (rouge)

-   purple et purple1 - purple4 (violet/pourpre)

-   gray et gray0 - gray100 (gris)

### Pins de HAL

Les pins de HAL fournisse le moyen de connecter les widgets aux autres éléments. Quand une pin de HAL est créée pour un widget, il est possible de la *connecter* à une autre pin de HAL avec une commande *net* dans un fichier .hal. Pour plus de détails, voir la commande *net* dans le manuel de HAL.

### Label

Un label est un texte qui s’affiche sur le panneau.

Le label a une pin optionnelle de désactivation en ajoutant: *&lt;disable\_pin&gt;True&lt;/disable\_pin&gt;*.

    <label>
        <text>"Ceci est un label:"</text>
        <font>("Helvetica",20)</font>
    </label>

Ce code produira:

![](images/pyvcp_label_fr.png)

### Les leds

Une led est utilisée pour indiquer l'état d’une pin de HAL de type bit. La couleur de la led sera on\_color quand le signal est vrai et off\_color autrement. \* *&lt;halpin&gt;* définit le nom de la pin, par défaut: *led.n*, où n est un entier. \* *&lt;size&gt;* définit la taille de la led, par défaut: 20. \* *&lt;on\_color&gt;* définit la couleur de la led led quand la pin est vraie, par défaut: *green* \* *&lt;off\_color&gt;* définit la couleur de la led quand la pin est fausse, par défaut: *ref*

### La led ronde

    <led>
        <halpin>"ma-led"</halpin>
        <size>50</size>
        <on_color>"verte"</on_color>
        <off_color>"rouge"</off_color>
    </led>

Le résultat du code ci-dessus.

![](images/pyvcp_led.png)

### La led rectangulaire

C’est une variante du widget *led*.

    <vbox>
        <relief>RIDGE</relief>
        <bd>6</bd>
        <rectled>
            <halpin>"ma-led-rect"</halpin>
            <height>"50"</height>
            <width>"100"</width>
            <on_color>"green"</on_color>
            <off_color>"red"</off_color>
        </rectled>
    </vbox>

Le code ci-dessus produit cette led, entourée d’un relief.

![](images/pyvcp_rectled.png)

### Le bouton (button)

Un bouton permet de contrôler une pin de type bit. La pin sera mise vraie quand le bouton sera pressé et maintenu enfoncé, elle sera mise fausse quand le bouton sera relâché.

Les boutons peuvent suivre les options de formatage suivantes:

-   &lt;padx&gt;n&lt;/padx&gt; où *n* est le nombre d’espaces horizontaux supplémentaires

-   &lt;pady&gt;n&lt;/pady&gt; où *n* est le nombre d’espaces verticaux supplémentaires

-   &lt;activebackground&gt;"color"&lt;/activebackground&gt; Couleur au survol du curseur

-   &lt;bg&gt;"color"&lt;/bg&gt; Couleur du bouton

#### Bouton avec texte (Text Button)

    <button>
        <halpin>"Bouton-OK"</halpin>
        <text>"OK"</text>
    </button>
    <button>
        <halpin>"Bouton-Abandon"</halpin>
        <text>"Abort"</text>
    </button

Le code ci-dessus produit:

![](images/pyvcp_button.png)

#### Case à cocher (checkbutton)

Une case à cocher contrôle une pin de type bit. La pin sera mise vraie quand la case est cochée et fausse si la case est décochée.

Une case non cochée:

![](images/pyvcp_checkbutton1.png)

et une case cochée:

![](images/pyvcp_checkbutton2.png)

Exemple de code:

    <checkbutton>
        <halpin>"coolant-chkbtn"</halpin>
        <text>"Coolant"</text>
    </checkbutton>
    <checkbutton>
        <halpin>"chip-chkbtn"</halpin>
        <text>"Chips    "</text>
    </checkbutton>

Le code ci-dessus produit:

![](images/pyvcp_checkbutton.png)

#### Bouton radio (radiobutton)

Un bouton radio placera une seule des pins vraie. Les autres seront mises fausses.

    <radiobutton>
        <choices>["un","deux","trois"]</choices>
        <halpin>"mon-radiobtn"</halpin>
    </radiobutton>

Le code ci-dessus donne ce résultat:

![](images/pyvcp_radiobutton_fr.png)

Noter que dans l’exemple ci-dessus, les pins de HAL seront nommées mon-radiobtn.un, mon-radiobtn.deux et mon-radiobtn.trois. Dans l’image précédente, *trois* est la valeur sélectionnée courante.

### Affichage d’un nombre (number)

L’affichage d’un nombre peux recevoir les options de formatage suivantes:

-   &lt;font&gt;("Font Name",n)&lt;/font&gt; où *n* est la taille de la police

-   &lt;width&gt;n&lt;/width&gt; où *n* est la largeur totale utilisée

-   &lt;justify&gt;pos&lt;/justify&gt; où "pos" peut être LEFT, CENTER ou RIGHT (devrait marcher)

-   &lt;padx&gt;n&lt;/padx&gt; où "n" est le nombre d’espaces horizontaux supplémentaires

-   &lt;pady&gt;n&lt;/pady&gt; où "n" est le nombre d’espaces verticaux supplémentaires

#### Number

Le widget *number* affiche la valeur d’un signal de type flottant.

    <number>
        <halpin>"number"</halpin>
        <font>("Helvetica",24)</font>
        <format>"+4.4f"</format>
    </number>

Le code ci-dessus donne ce résultat:

![](images/pyvcp_number.png)

#### Flottant

Le widget number affiche la valeur d’un signal de type flottant.

    <number>
        <halpin>"my-number"</halpin>
        <font>("Helvetica",24)</font>
        <format>"+4.4f"</format>
    </number>

![](images/pyvcp_number.png)

&lt;font&gt; est une police de caractères de type Tkinter avec la spécification de sa taille. Une police qui peut être agrandie jusqu'à la taille 200 est la police *courier 10 pitch*, que vous pouvez spécifier de la manière suivante, pour afficher des chiffres réellement grands:

    <font>('courier 10 pitch',100)</font>

&lt;format&gt; est un format *style C*, spécifié pour définir le format d’affichage du nombre.

#### Nombre s32

Le widget s32 affiche la valeur d’un nombre s32. La syntaxe est la même que celle de *number* excepté le nom qui est &lt;s32&gt;. Il faut prévoir une largeur suffisante pour afficher le nombre dans sa totalité.

    <s32>
        <halpin>"simple-number"</halpin>
        <font>("Helvetica",24)</font>
        <format>"6d"</format>
        <width>6</width>
    </s32>

![](images/pyvcp_s32.png)

#### Nombre u32

Le widget u32 affiche la valeur d’un nombre u32. La syntaxe est la même que celle de *number* excepté le nom qui est &lt;u32&gt;.

### Affichage d’images

Seul l’affichage d’images au format gif est possible. Toutes les images doivent avoir la même taille. Les images doivent être toutes dans le même répertoire que le fichier ini (ou dans le répertoire courant pour un fonctionnement en ligne de commande avec halrun/halcmd).

#### Image Bit

La bascule *image\_bit* bascule entre deux images selon la position vraie ou fausse de halpin.

    <pyvcp>
        <image name='fwd' file='fwd.gif'/>
        <image name='rev' file='rev.gif'/>
        <vbox>
            <image_bit halpin='selectimage' images='fwd rev'/>
        </vbox>
    </pyvcp>

En utilisant les deux images fwd.gif et rev.gif. FWD est affiché quand *selectimage* est fausse et REV est affiché quand *selectimage* est vraie.

![](images/pyvcp_image01.png)

Figure 1. selectimage fausse

![](images/pyvcp_image02.png)

Figure 2. selectimage vraie

#### Image u32

La bascule *image\_u32* est la même que *image\_bit* excepté que le nombre d’images n’est pratiquement plus limité, il suffit de *selectionner* l’image en ajustant halpin à une valeur entière commençant à 0 pour la première image de la liste, à 1 pour la seconde image etc.

    <pyvcp>
        <image name='stb' file='stb.gif'/>
        <image name='fwd' file='fwd.gif'/>
        <image name='rev' file='rev.gif'/>
        <vbox>
            <image_u32 halpin='selectimage' images='stb fwd rev'/>
        </vbox>
    </pyvcp>

Même résultat mais en ajoutant l’image stb.gif.

![](images/pyvcp_image_u32_01.png)

Figure 3. Halpin = 0

![](images/pyvcp_image01.png)

Figure 4. Halpin = 1

![](images/pyvcp_image02.png)

Figure 5. Halpin = 2

### Barre de progression (bar)

Le widget barre de progression affiche la valeur d’un signal FLOAT, graphiquement dans une barre de progression et simultanément, en numérique.

    <bar>
        <halpin>"bar"</halpin>
        <min_>0</min_>
        <max_>123</max_>
        <bgcolor>"grey"</bgcolor>
        <fillcolor>"red"</fillcolor>
    </bar>

Le code ci-dessus donne ce résultat:

![](images/pyvcp_bar.png)

### Galvanomètre (meter)

Le galvanomètre affiche la valeur d’un signal FLOAT dans un affichage à aiguille *à l’ancienne*.

    <meter>
        <halpin>"mymeter"</halpin>
        <text>"Battery"</text>
        <subtext>"Volts"</subtext>
        <size>250</size>
        <min_>0</min_>
        <max_>15.5</max_>
        <majorscale>1</majorscale>
        <minorscale>0.2</minorscale>
        <region1>(14.5,15.5,"yellow")</region1>
        <region2>(12,14.5,"green")</region2>
        <region3>(0,12,"red")</region3>
    </meter>

Le code ci-dessus donne ce résultat:

![](images/pyvcp_meter.png)

### Boîte d’incrément (spinbox)

La boîte d’incrément contrôle une pin FLOAT. La valeur de la pin est augmentée ou diminuée de la valeur de *resolution*, à chaque pression sur une flèche, ou en positionnant la souris sur le nombre puis en tournant la molette de la souris.

    <spinbox>
        <halpin>"my-spinbox"</halpin>
        <min_>-12</min_>
        <max_>33</max_>
        <inival>0</inival>
        <resolution>0.1</resolution>
        <format>"2.3f"</format>
        <font>("Arial",30)</font>
    </spinbox>

Le code ci-dessus donne ce résultat:

![](images/pyvcp_spinbox.png)

### Curseur (scale)

Le curseur contrôle une pin FLOAT. La valeur de la pin est augmentée ou diminuée en déplaçant le curseur, ou en positionnant la souris sur le curseur puis en tournant la molette de la souris.

    <scale>
        <font>("Helvetica",16)</font>
        <width>"25"</width>
        <halpin>"my-hscale"</halpin>
        <resolution>0.1</resolution>
        <orient>HORIZONTAL</orient>
        <initval>-15</initval>
        <min_>-33</min_>
        <max_>26</max_>
    </scale>
    <scale>
        <font>("Helvetica",16)</font>
        <width>"50"</width>
        <halpin>"my-vscale"</halpin>
        <resolution>1</resolution>
        <orient>VERTICAL</orient>
        <min_>100</min_>
        <max_>0</max_>
    </scale>

Le code ci-dessus donne ce résultat:

![](images/pyvcp_scale.png)

Noter que par défaut c’est min qui est affiché même si il est supérieur à max, à moins que min ne soit négatif.

### Bouton tournant (dial)

Le bouton tournant imite le fonctionnement d’un vrai bouton tournant, en sortant sur un FLOAT HAL la valeur sur laquelle est positionné le curseur, que ce soit en le faisant tourner avec un mouvement circulaire, ou en tournant la molette de la souris. Un double click gauche augmente la résolution et un double click droit la diminue d’un digit. La sortie est limitée par les valeurs min et max. La variable cpr fixe le nombre de graduations sur le pourtour du cadran (prudence avec les grands nombres).

    <dial>
        <size>200</size>
        <cpr>100</cpr>
        <min_>-15</min_>
        <max_>15</max_>
        <text>"Dial"</text>
        <initval>0</initval>
        <resolution>0.001</resolution>
        <halpin>"anaout"</halpin>
        <dialcolor>"yellow"</dialcolor>
        <edgecolor>"green"</edgecolor>
        <dotcolor>"black"</dotcolor>
    </dial>

Le code ci-dessus donne ce résultat:

![](images/pyvcp_dial.png)

### Manivelle (jogwheel)

La manivelle imite le fonctionnement d’une vraie manivelle, en sortant sur une pin FLOAT la valeur sur laquelle est positionné le curseur, que ce soit en le faisant tourner avec un mouvement circulaire, ou en tournant la molette de la souris.

    <jogwheel>
        <halpin>"my-wheel"</halpin>
        <cpr>45</cpr>
        <size>250</size>
    </jogwheel>

Le code ci-dessus donne ce résultat:

![](images/pyvcp_jogwheel.png)

Documentation des containers de pyVCP
-------------------------------------

Les containers sont des widgets qui contiennent d’autres widgets.

### Bordures

Le container bordure est spécifié avec deux balises utilisées ensembles. La balise &lt;relief&gt; spécifie le type de bordure et la balise &lt;bd&gt; spécifie la largeur de la bordure.

 &lt;relief&gt;type&lt;/relief&gt;   
La valeur de *type* peut être: FLAT, SUNKEN, RAISED, GROOVE, ou RIDGE

 &lt;bd&gt;n&lt;/bd&gt;   
La valeur de **n** fixe la largeur de la bordure.

    <hbox>
        <button>
            <relief>FLAT</relief>
            <text>"FLAT"</text>
            <bd>3</bd>
        </button>

        <button>
            <relief>SUNKEN</relief>
            <text>"SUNKEN"</text>
            <bd>3</bd>
        </button>

        <button>
            <relief>RAISED</relief>
            <text>"RAISED"</text>
            <bd>3</bd>
        </button>

        <button>
            <relief>GROOVE</relief>
            <text>"GROOVE"</text>
            <bd>3</bd>
        </button>

        <button>
            <relief>RIDGE</relief>
            <text>"RIDGE"</text>
            <bd>3</bd>
        </button>
    </hbox>

![](images/pyvcp_borders.png)

### Hbox

Utilisez une Hbox lorsque vous voulez aligner les widgets, horizontalement, les uns à côtés des autres.

    <hbox>
        <relief>RIDGE</relief>
        <bd>6</bd>
        <label><text>"a hbox:"</text></label>
        <led></led>
        <number></number>
        <bar></bar>
    </hbox>

![](images/pyvcp_hbox.png)

Á l’intérieur d’une Hbox, il est possible d’utiliser les balises *&lt;boxfill fill=/&gt;*, *&lt;boxanchor anchor=/&gt;* et *&lt;boxexpand expand=/&gt;* pour choisir le comportement des éléments contenus dans la boîte, lors d’un redimensionnement de la fenêtre. Pour des détails sur le comportement de fill, anchor, et expand, référez vous au manuel du pack Tk, *pack(3tk)*. Valeurs par défaut, *fill='y'*, *anchor='center'*, *expand='yes'*.

### Vbox

Utilisez une Vbox lorsque vous voulez aligner les widgets verticalement, les uns au dessus des autres.

    <vbox>
        <relief>RIDGE</relief>
        <bd>6</bd>
        <label><text>"a vbox:"</text></label>
        <led></led>
        <number></number>
        <bar></bar>
    </vbox>

![](images/pyvcp_vbox.png)

Á l’intérieur d’une Vbox, vous pouvez utiliser les balises *&lt;boxfill fill=/&gt;*, *&lt;boxanchor anchor=/&gt;* et *&lt;boxexpand expand=/&gt;* pour choisir le comportement des éléments contenus dans la boîte, lors d’un redimensionnement de la fenêtre. Pour des détails sur le comportement de fill, anchor, et expand, référez vous au manuel du pack Tk, *pack(3tk)*. Valeurs par défaut, *fill='y'*, *anchor='center'*, *expand='yes'*.

### Labelframe

Un labelframe est un cadre entouré d’un sillon et un label en haut à gauche.

    <labelframe text="Label: Leds groupées">

    <labelframe text="Label: Leds groupées">
        <font>("Helvetica",16)</font>
        <hbox>
        <led/>
        <led/>
        <led/>
        </hbox>
    </labelframe>

![](images/pyvcp_labelframe_fr1.png)

### Table

Une table est un container qui permet d'écrire dans une grille de lignes et de colonnes. Chaque ligne débute avec la balise *&lt;tablerow/&gt;* . Un widget contenu peut être en lignes ou en colonnes par l’utilisation de la balise *&lt;tablespan rows= cols=/&gt;*. Les bordures des cellules contenant les widgets *sticky* peuvent être réglées grâce à l’utilisation de la balise *&lt;tablesticky sticky=/&gt;*. Une table flexible peut s'étirer sur ses lignes et ses colonnes (sticky).

Exemple:

    <table flexible_rows="[2]" flexible_columns="[1,4]">
    <tablesticky sticky="new"/>
    <tablerow/>
        <label>
            <text>" A (cell 1,1) "</text>
            <relief>RIDGE</relief>
            <bd>3</bd>
        </label>
        <label text="B (cell 1,2)"/>
        <tablespan columns="2"/>
        <label text="C, D (cells 1,3 and 1,4)"/>
    <tablerow/>
        <label text="E (cell 2,1)"/>
        <tablesticky sticky="nsew"/>
        <tablespan rows="2"/>
        <label text="'spans\n2 rows'"/>
        <tablesticky sticky="new"/>
        <label text="G (cell 2,3)"/>
        <label text="H (cell 2,4)"/>
    <tablerow/>
        <label text="J (cell 3,1)"/>
        <label text="K (cell 3,2)"/>
        <u32 halpin="test"/>
    </table>

![](images/pyvcp_table.png)

### Onglets (Tabs)

Une interface à onglets permet d'économiser l’espace en créant un container pour chaque nom d’onglet (tabs). Une seule section *tabs* peut exister, les *tabs* ne peuvent pas être imbriqués ni empilés. La largeur de l’onglet le plus large, determine la largeur des onglets.

    <tabs>
        <names>["Spindle", "Green Eggs", "Ham"]</names>
        <vbox>
            <label>
                <text>"Spindle speed:"</text>
            </label>
            <bar>
                <halpin>"spindle-speed"</halpin>
                <max_>5000</max_>
            </bar>
        </vbox>
        <vbox>
            <label>
                <text>"(this is the green eggs tab)"</text>
            </label>
        </vbox>
        <vbox>
            <label>
                <text>"(this tab has nothing on it)"</text>
            </label>
        </vbox>
    </tabs>

![\] image::images/pyvcp\_tabs2.png\[\] image::images/pyvcp\_tabs3.png\[](images/pyvcp_tabs1.png)

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


