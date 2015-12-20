Configuration de Machinekit
===========================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:ini-configuration"></span>

Script de lancement
-------------------

Machinekit est lancé par le fichier de script *machinekit*.

    machinekit [options] [<ini-file>]

Avec les options suivantes: \* *-v* = verbose - informations de fonctionnement \* *-d* = commande d'écho à l'écran pour le débogage

Le fichier de script *machinekit* lit le fichier ini puis lance Machinekit. La section \[HAL\] du fichier ini, spécifie l’ordre de chargement des fichiers de HAL, si plusieurs sont utilisés. Après que les fichiers HAL soient chargés, l’interface graphique est chargée à son tour puis le fichier HAL POSTGUI. Si des objets pyvcp ont été créés avec des pins de HAL, le fichier *postgui.hal* doit effectuer les raccordements à ces pins, se reporter à la section [HAL](#sub:Section-HAL) pour plus de détails.

Si aucun fichier ini n’est passé en argument au script *machinekit*, le sélecteur de configuration est lancé pour permettre à l’utilisateur de choisir parmi les exemples de configuration existants.

![](../common/images/configuration-selector1_fr.png)

Figure 1. Sélecteur de configuration

Fichiers utilisés pour la configuration
---------------------------------------

Machinekit est entièrement configuré avec des fichiers textes classiques. Tous ces fichiers peuvent être lus et modifiés dans n’importe quel éditeur de texte disponible dans toute distribution Linux.<span class="footnote">
\[Ne pas confondre un éditeur de texte et un traitement de texte. Un éditeur de texte comme gedit ou kwrite produisent des fichiers uniquement en texte. Les lignes de textes sont séparées les unes des autres. Un traitement de texte comme Open Office produit des fichiers avec des paragraphes, des mises en formes des mots. Ils ajoutent des codes de contrôles, des polices de formes et de tailles variées etc. Un éditeur de texte n’a rien de tout cela.\]
</span> Soyez prudent lorsque vous modifierez ces fichiers, certaines erreurs pourraient empêcher le démarrage de Machinekit. Ces fichiers sont lus à chaque fois que le logiciel démarre. Certains d’entre eux sont lus de nombreuses fois pendant l’exécution de Machinekit.

Les fichiers de configuration inclus:

-   *INI* Le fichier ini écrase les valeurs par défaut compilées dans le code de Machinekit. Il contient également des sections qui sont lues directement par HAL (Hardware Abstraction Layer, couche d’abstraction matérielle).

-   *HAL* Les fichiers hal installent les modules de process, ils créent les liens entre les signaux de Machinekit et les broches spécifiques du matériel.

-   *VAR* Ce fichier contient une suite de numéros de variables. Ces variables contiennent les paramètres qui seront utilisés par l’interpréteur. Ces valeurs sont enregistrées et réutilisées d’une exécution à l’autre.

-   *TBL* Ce fichier contient les informations relatives aux outils. Voir la section *Fichier d’outils* du Manuel de l’utilisateur pour plus d’infos.

-   *NML* Ce fichier configure les voies de communication utilisées par Machinekit. Il est normalement réglé pour lancer toutes les communications avec un seul ordinateur, peut être modifié pour communiquer entre plusieurs ordinateurs.

-   *.machinekitrc* Ce fichier enregistre des informations spécifiques à l’utilisateur, il a été créé pour enregistrer le nom du répertoire lorsque l’utilisateur choisit sa première configuration de Machinekit.<span class="footnote">
    \[Habituellement, ce fichier est dans le répertoire home de l’utilisateur (ex: */home/robert/* )\]
    </span>

Les éléments avec le repère *(hal)* sont utilisés seulement pour les fichiers de HAL en exemples. C’est une bonne convention. D’autres éléments sont utilisés directement par Machinekit et doivent toujours avoir la section et le nom donné à l’item.

Double passe (TWOPASS)
----------------------

Machinekit 2.5 supporte le processus dit TWOPASS des fichiers de configuration hal, ce qui aide à la modularité des fichiers hal et améliore leur lisibilité. (les fichiers Hal sont spécifiés dans le fichier ini de Machinekit, dans l’instance HAL sous la forme *\[HAL\]HALFILE=nomdufichier*.

Normalement, un jeu de un ou plusieurs fichiers de configuration HAL doivent utiliser une seule et unique ligne loadrt pour charger le module du kernel qui pourra gérer de multiples instances d’un même composant. Par exemple: si vous utilisez une portes AND à deux entrées, composant (and2), à trois endroits différents de votre configuration, vous ne devez avoir que cette seule ligne quelque part pour le spécifier:

    loadrt and2 count=3

Ce qui fournira finalement les composants and2.0, and2.1, and2.2.

Les configurations seront plus lisibles si vous spécifiez les composants sous la forme names=option quand c’est supporté, par exemple:

    loadrt and2 names=aa,ab,ac

Ce qui nommera les composants aa, ab, ac.

Il pourrait apparaitre un problème de maintenance pour garder la trace des composants et de leur noms après avoir ajouté (ou enlevé) un composant, vous devrez trouver et mettre à jour, la ligne de directives de loadrt, applicable à ce composant.

Le processus TWOPASS est activé par inclusion d’un paramètre dans le fichier ini:

    [HAL]TWOPASS=anything

Avec ce réglage, vous pouvez avoir de multiples spécifications comme:

    loadrt and2 names=aa
    ...
    loadrt and2 names=ab,ac
    ...
    loadrt and2 names=ad

Ces commandes peuvent être placées dans différents fichiers HALFILES. Les HALFILES sont traités dans leur ordre d’apparition dans le fichier ini.

Avec le processus double passe, tous les \[HAL\]HALFILES sont lus une première fois et les multiples apparitions de la directive loadrt sont cumulées pour chaque module. Aucune commande hal n’est exécutée lors de cette passe initiale.

Après la passe initiale, les modules sont automatiquement chargés en nombre égal au nombre total lors de l’utilisation de count=option ou de tous les noms spécifiés individuellement lors de l’utilisation de names=option.

Une seconde passe est alors faite pour exécuter toutes les autres instructions de hal spécifiées dans les HALFILES. Les commandes addf qui associent les fonctions de composants avec l’exécution du thread sont exécutées selon leur ordre d’apparition avec les autres commandes dans cette seconde passe.

Bien que vous puissiez utiliser indifféremment les options avec count= ou names=, elles sont toutefois exclusives. Un seul type peut être utilisé pour un même module.

Le processus TWOPASS n’est pas effectif lors de l’usage de names=option. Cette option permet d’avoir un nom unique qui soit mnémonique ou plus pertinent avec la configuration. Par exemple: si vous utilisez un composant *dérivé* pour estimer la vitesse et l’accélération de chacun des coordonnées (x,y,z), utiliser la méthode count= donnera un composant au nom ésotérique comme ddt.0, ddt.1, ddt.2, etc.

Alternativement, l’utilisation de names=option comme:

    loadrt ddt names=xvit,yvit,zvit
    ...
    loadrt ddt names=xaccel,yaccel,zaccel

donnera des composants plus parlants, nommés xvit,yvit,zvit, xaccel,yaccel, zaccel.

Beaucoup de composants fournis avec la distribution ont été créés avec *comp utility* et supportent la méthode names=option. Il s’agit notamment de composants logiques qui sont les briques de beaucoup de configurations HAL.

Exemples d’inclusions:

    and2,ddt,deadzone,flipflop,or2,or4,mux2,mux4,scale,sum2,timedelay,lowpass

et beaucoup d’autres.

Les composants utilisateur créés avec *comp utility* supportent également automatiquement la méthode names=option. En plus des composants générés avec *comp utility*, quelques autres composants comme *encoder* et *pid* supportent aussi names=option.

Organisation du fichier ini
---------------------------

 Organisation du fichier ini   
Un fichier ini typique suit une organisation simple;

-   les commentaires.

-   les sections.

-   les variables.

Chacun de ces éléments est séparé, sur une seule ligne. Chaque fin de ligne ou retour chariot crée un nouvel élément.

### Les commentaires

Une ligne de commentaires débute avec un **;** ou un **\#**. Si le logiciel qui analyse le fichier ini rencontre l’un ou l’autre de ces caractères, le reste de la ligne est ignoré. Les commentaires peuvent être utilisés pour décrire ce que font les éléments du fichier ini.

    ; Ceci est le fichier de configuration de ma petite fraiseuse.

Des commentaires peuvent également être utilisés pour choisir entre plusieurs valeurs d’une seule variable.

    DISPLAY = axis
    # DISPLAY = touchy

Dans cette liste, la variable DISPLAY est positionnée sur axis puisque l’autre est commentée. Si quelqu’un édite une liste comme celle-ci et par erreur, dé-commente deux lignes, c’est la première rencontrée qui sera utilisée.

Noter que dans une ligne de variables, les caractères **\#** et **;** n’indiquent pas un commentaire.

    INCORRECT = valeur     # et un commentaire

    # Commentaire correct
    CORRECT = valeur

### Les sections

Les différentes parties d’un fichier .ini sont regroupées en sections. Une section commence par son nom en majuscules entre crochets \[UNE\_SECTION\]. L’ordre des sections est sans importance.

Les sections suivantes sont utilisées par Machinekit:

-   *\[[EMC](#sub:Section-EMC)\]* informations générales.

-   *\[[DISPLAY](#sub:Section-DISPLAY)\]* sélection du type d’interface graphique.

-   *\[[FILTER](#sub:Section-FILTER)\]* sélection d’un programme de filtrage.

-   *\[[RS274NGC](#sub:Section-RS274NGC)\]* ajustements utilisés par l’interpréteur de g-code.

-   *\[[EMCMOT](#sub:Section-EMCMOT)\]* réglages utilisés par le contrôleur de mouvements temps réel.

-   *\[[TASK](#sub:Section-TASK)\]* réglages utilisés par le contrôleur de tâche.

-   *\[[HAL](#sub:Section-HAL)\]* spécifications des fichiers .hal.

-   *\[[HALUI](#sub:Section-HALUI)\]* commandes MDI utilisées par HALUI.

-   *\[[TRAJ](#sub:Section-TRAJ)\]* réglages additionnels utilisés par le contrôleur de mouvements temps réel.

-   *\[[AXIS\_n](#sub:Sections-AXIS)\]* groupes de variables relatives à chaque axe.

-   *\[[EMCIO](#sub:Section-EMCIO)\]* réglages utilisés par le contrôleur d’entrées/sorties.

### Les variables

Une ligne de variables est composée d’un nom de variable, du signe égal (=) et d’une valeur. Tout, du premier caractère non blanc qui suit le signe = jusqu'à la fin de la ligne, est passé comme valeur à la variable. Vous pouvez donc intercaler des espaces entre les symboles si besoin. Un nom de variable est souvent appelé un mot clé.

Les paragraphes suivants détaillent chaque section du fichier de configuration, en utilisant des exemples de variables dans les lignes de configuration.

Certaines de ces variables sont utilisées par Machinekit. Elles doivent toujours utiliser le nom de section et le nom de variable dans leur appellation. D’autres variables ne sont utilisées que par HAL. Les noms des sections et les noms des variables indiquées, sont ceux qui sont utilisés dans les exemples de fichiers de configuration.

Les variables personnalisées peuvent être utilisées dans vos fichiers HAL avec la syntaxe suivante:

    MACHINE = MaVariable

### Sections et variables utilisateur<span id="sub:variables-utilisateur"></span>

Certaines configurations utilisent des sections utilisateur et des variables personnalisées pour regrouper les paramètres en un seul emplacement pour améliorer la lisibilité du fichier ini.

Pour utiliser une section de variable utilisateur dans un fichier HAL, ajouter la section et la variable dans le fichier INI.

Exemple de section utilisateur

    [OFFSETS]
    OFFSET_1 = 0.1234

Pour ajouter une variable utilisateur à une section Machinekit, inclure simplement cette variable dans la section souhaitée.

Exemple de variable utilisateur

    [AXIS_0]
    TYPE = LINEAR
    ...
    SCALE = 16000

Pour utiliser une variable utilisateur dans un fichier HAL, utiliser les noms de section et de variable en lieu et place de leurs valeurs.

Exemple d’utilisation dans un fichier HAL

    setp offset.1.offset [OFFSETS]OFFSET_1
    setp stepgen.0.position-scale [AXIS_0]SCALE

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
<td align="left">La valeur stockée dans la variable doit correspondre au type spécifié pour la pin du composant.</td>
</tr>
</tbody>
</table>

Détails des sections du fichier ini
-----------------------------------

### Section \[EMC\]

-   *VERSION* = $Revision: 1.5 $\_ - Le numéro de version du fichier INI. La valeur indiquée ici semble étrange, car elle est automatiquement mise à jour lors de l’utilisation du système de contrôle de révision. C’est une bonne idée de changer ce numéro à chaque fois que vous modifiez votre fichier. Si vous voulez le modifier manuellement, il suffit de changer le numéro sans toucher au reste.

-   *MACHINE = ma machine* - C’est le nom du contrôleur, qui est imprimé dans le haut de la plupart des fenêtres. Vous pouvez insérer ce que vous voulez ici tant que ça reste sur une seule ligne.

-   *DEBUG = 0* - Niveau de débogage 0 signifie qu’aucun message ne sera affiché dans le terminal pendant le fonctionnement de Machinekit. Les drapeaux de débogage ne sont généralement utiles que pour les développeurs.

### Section \[DISPLAY\]

Les différentes interfaces graphiques utilisent différentes options qui ne sont pas supportées par toutes les interfaces utilisateur. Les deux principales interfaces pour Machinekit sont *AXIS* et *Touchy*. Axis est une interface pour une utilisation avec un ordinateur classique et son moniteur, Touchy est à utiliser avec les ordinateurs à écran tactile. Pour plus d’informations, voire la section Interfaces du Manuel de l’utilisateur.

-   *DISPLAY = axis* - Le nom de l’interface graphique à utiliser. Les options disponibles sont les suivantes: *axis*, *touchy*, *keystick*, *mini*, *tkmachinekit*, *xmachinekit*,

-   *POSITION\_OFFSET = RELATIVE* - Le système de coordonnées (RELATIVE ou MACHINE) à utiliser au démarrage de l’interface utilisateur. Le système de coordonnées RELATIVE reflète le G92 et le décalage d’origine G5x actuellement actifs.

-   *POSITION\_FEEDBACK = ACTUAL* - Valeur de la position (COMMANDED ou ACTUAL) à afficher au démarrage de l’interface utilisateur. La position COMMANDED est la position exacte requise par Machinekit. La position ACTUAL est la position retournée par l'électronique des moteurs.

-   *MAX\_FEED\_OVERRIDE = 1.2* - La correction de vitesse maximum que l’opérateur peut utiliser. 1.2 signifie 120% de la vitesse programmée.

-   *MIN\_SPINDLE\_OVERRIDE = 0.5* - Correction de vitesse minimum de broche que l’opérateur pourra utiliser. 0.5 signifie 50% de la vitesse de broche programmée. (utile si il est dangereux de démarrer un programme avec une vitesse de broche trop basse).

-   *MAX\_SPINDLE\_OVERRIDE = 1.0* - Correction de vitesse maximum de broche que l’opérateur pourra utiliser. 1.0 signifie 100% de la vitesse de broche programmée.

-   *PROGRAM\_PREFIX = ~/machinekit/nc\_files* - Répertoire par défaut des fichiers de g-codes et emplacement des M-codes définis par l’utilisateur. Les recherches de fichiers s’effectueront d’abords dans cet emplacement, avant les chemins des sous-programmes et des fichiers M utilisateur, si il est spécifié dans la section \[RS274NGC\].

-   *INTRO\_GRAPHIC = machinekit.gif* - L’image affichée sur l'écran d’accueil.

-   *INTRO\_TIME = 5* - Durée d’affichage de l'écran d’accueil.

-   *CYCLE\_TIME = 0.05* - Cycle time in seconds that display will sleep between polls.

Les éléments suivants sont utilisés uniquement si AXIS est sélectionné comme programme d’interface utilisateur.

-   *DEFAULT\_LINEAR\_VELOCITY = .25* - Vitesse minimum par défaut pour les jogs linéaires, en unités machine par seconde. Seulement utilisé dans l’interface AXIS.

-   *MIN\_VELOCITY = .01* - Valeur approximative minimale du curseur de vitesse de jog.

-   *MAX\_LINEAR\_VELOCITY = 1.0* - Vitesse maximum par défaut pour les jogs linéaires, en unités machine par seconde. Seulement utilisé dans l’interface AXIS.

-   *MIN\_LINEAR\_VELOCITY = .01* - Approximativement la valeur minimale du curseur de vitesse de jog.

-   *DEFAULT\_ANGULAR\_VELOCITY = .25* - Vitesse minimum par défaut pour les jogs angulaires, en unités machine par seconde. Seulement utilisé dans l’interface AXIS.

-   *MIN\_ANGULAR\_VELOCITY = .01* - Valeur approximative minimale du curseur de vitesse angulaire de jog.

-   *MAX\_ANGULAR\_VELOCITY = 1.0* - Vitesse maximum par défaut pour les jogs angulaires, en unités machine par seconde. Seulement utilisé dans l’interface AXIS.

-   *INCREMENTS = 1 mm, .5 mm, …* - Définit les incréments disponibles pour le jog incrémental. Les incréments peuvent être utilisés pour remplacer la valeur par défaut. Ces valeurs doivent contenir des nombres décimaux (ex. 0.1000) ou des nombres fractionnaires (ex. 1/16), éventuellement suivis par une unité parmi *cm*, *mm*, *um*, *inch*, *in* ou *mil*. Si aucune unité n’est spécifiée, les unités natives de la machine seront utilisées.

-   Distances métriques et impériales peuvent être mélangées
     *INCREMENTS = 1 inch, 1 mil, 1 cm, 1 mm, 1 um* sont des entrées valides.

-   *OPEN\_FILE = /chemin/complet/du/fichier.ngc* Le fichier ngc à utiliser au démarrage d’AXIS. Utilisez une chaîne vide "" et aucun fichier ne sera chargé au démarrage.

-   *EDITOR = gedit* - L'éditeur à utiliser lors du choix *Éditer fichier* du menu d’AXIS, pour éditer le G-code. Ceci doit être configuré pour que cet item de menu s’active. Une autre possibilité valide est: *gnome-terminal -e nano*.

-   *TOOL\_EDITOR = tooledit* - L'éditeur de texte à utiliser pour éditer les tables d’outils. (par exemple en sélectionnant "Fichiers &gt; Éditer la table. d’outils" dans le menu d’Axis). D’autres entrées comme *gedit*, *gnome-terminal -e vim*, *gvim* ou *nano* sont valides.

-   *PYVCP = /filename.xml* - Le fichier de description du panneau PyVCP. Voir la section PyVCP.

-   *LATHE = 1* - Passe l’affichage en mode tour, avec vue de dessus et la visu soit en rayon, soit en diamètre.

-   *GEOMETRY = XYZABCUVW* - Contrôle de prévisualisation du parcours d’outil d’un mouvement rotatif. Cet item consiste en une suite de lettre d’axe, optionnellement précédé d’un signe **-**. Seuls, les axes définis par **\[TRAJ\]AXES** peuvent être utilisés. Cette séquence spécifie l’ordre dans lequel l’effet de chaque axe est appliqué. Un signe **-** inverse le sens de la rotation. La chaine GEOMETRY correcte dépend de la configuration de la machine et de la cinématique utilisée pour la contrôler. La chaine exemple GEOMETRY=XYZBCUVW est pour une machine à 5 axes pour laquelle la cinématique déplace UVW en coordonnées système de l’outil et XYZ déplace la pièce en coordonnées système. L’ordre des lettres est important, parce qu’il donne expressément l’ordre dans lequel les différentes transformations seront appliquées. Par exemple: tourner autour de C puis de B est différent de tourner autour de B puis de C. La géométrie n’a pas d’effet sans rotation d’axes.

-   *ARCDIVISION = 64* - Ajuste la valeur de prévisualisation des arcs. Les arcs sont visualisés en les divisant par un nombre de lignes droites; un semi-cercle est divisé en *ARCDIVISION* de tronçons. Les valeurs élevées donnent une meilleure précision à la pré-visualisation, mais sont plus lentes et donne un écran plus saccadé. Les petites valeurs sont moins précises mais plus rapides, l’affichage résultant est plus rapide. La valeur par défaut de 64 signifie qu’un cercle de 3 pouces maximum sera affiché dans moins de 3 centièmes de mm, (.03%).<span class="footnote">
    \[ Dans Machinekit 2.4 et précédents, la valeur par défaut était de 128.\]
    </span>

-   *MDI\_HISTORY\_FILE =* - Le nom du fichier d’historique des commandes MDI. Si rien n’est spécifié, Axis enregistrera cet historique dans *.axis\_mdi\_history* dans le répertoire home de l’utilisateur. C’est très pratique dans le cas de multiples configurations sur la même machine.

-   *HELP\_FILE = tklinucnc.txt* - Chemin du fichier d’aide (non utilisé avec AXIS).

### Section \[FILTER\]

AXIS a la possibilité d’envoyer les fichiers chargés au travers d’un programme de filtrage. Ce filtrage peut réaliser toutes sortes de tâches. Parfois aussi simple que s’assurer que le programme se termine bien par M2, ou parfois aussi compliqué que détecter si le fichier d’entrée est une image et en générer le G-code pour graver la forme qu’il à ainsi défini. La section *\[FILTER\]* du fichier ini, contrôle comment les filtres fonctionnent. Premièrement, pour chaque type de fichier, écrire une ligne *PROGRAM\_EXTENSION*. Puis, spécifier le programme à exécuter pour chaque type de filtre. Ce programme reçoit le nom du fichier d’entrée dans son premier argument, il doit écrire le code RS274/NGC sur la sortie standard. C’est cette sortie qui sera affichée dans la zone de texte, pré-visualisée dans la zone du parcours d’outil et enfin, exécutée par Machinekit quand il sera mis en marche.

    PROGRAM_EXTENSION = .extension Description

Si votre fichier de sortie est tout en majuscules, vous devez ajouter la ligne suivante:

    PROGRAM_EXTENSION = .NGC XYZ Post Processor

Les lignes suivantes ajoutent le support pour le convertisseur *image-to-gcode* fourni avec Machinekit:

    PROGRAM_EXTENSION = .png,.gif,.jpg Greyscale Depth Image
        png = image-to-gcode
        gif = image-to-gcode
        jpg = image-to-gcode

Il est également possible de spécifier un interpréteur:

    PROGRAM_EXTENSION = .py Python Script
        py = python

De cette façon, n’importe quel script Python pourra être ouvert et ses sorties seront traitées comme du g-code. Un exemple de script de ce genre est disponible: nc\_files/holecircle.py. Ce script crée le G-code pour percer une série de trous séquents à la périphérie d’un cercle. De nombreux générateurs de G-code sont par ailleurs disponibles sur le wiki: [à la page des générateurs de G-code](http://wiki.machinekit.org/cgi-bin/wiki.pl?Simple_Machinekit_G-Code_Generators).

Si la variable d’environnement AXIS\_PROGRESS\_BAR est activée, alors les lignes écrites sur stderr de la forme

    FILTER_PROGRESS=%d

activeront la barre de progression d’AXIS qui donnera le pourcentage. Cette fonctionnalité devrait être utilisée par tous les filtres susceptibles de fonctionner pendant un long moment.

Les filtres Python doivent utiliser la fonction *print* pour sortir le résultat dans Axis.

Cet exemple de programme filtre un fichier et ajoute un axe W correspondant à l’axe Z. Il marchera selon la présence d’un espace entre chaque mot d’axe.

    #! /usr/bin/env python

    import sys

    def main(argv):

      openfile = open(argv[0], 'r')
      file_in = openfile.readlines()
      openfile.close()

      file_out = []
      for line in file_in:
        # print line
        if line.find('Z') != -1:
          words = line.rstrip('\n')
          words = words.split(' ')
          newword = ''
          for i in words:
            if i[0] == 'Z':
              newword = 'W'+ i[1:]
          if len(newword) > 0:
            words.append(newword)
            newline = ' '.join(words)
            file_out.append(newline)
        else:
          file_out.append(line)
      for item in file_out:
        print "%s" % item

    if __name__ == "__main__":
       main(sys.argv[1:])

### Section \[RS274NGC\]

-   *PARAMETER\_FILE = monfichier.var* - Le fichier situé dans le même répertoire que le fichier ini qui contiendra les paramètres utilisés par l’interpréteur (enregistré entre chaque lancement).

-   *RS274NGC\_STARTUP\_CODE = G01 G17 G20 G40 G49 G64 P0.001 G80 G90 G92 G94 G97 G98* - Une chaine de codes NGC qui sera utilisée pour initialiser l’interpréteur. Elle ne se substitue pas à la spécification des G-codes modaux du début de chaque fichier ngc. Les codes modaux des machines diffèrent, ils pourraient être modifiés par les G-codes interprétés plutôt dans la session.

-   *SUBROUTINE\_PATH = ncsubroutines:/tmp/testsubs:lathesubs:millsubs* - Spécifie une liste, séparée par (:) d’au maximum 10 répertoires dans lesquels seront cherchés les fichier de sous-programme spécifiés dans le g-code. Ces répertoires sont inspectés après que ne le soit \[DISPLAY\]PROGRAM\_PREFIX (si il est spécifié) et avant que ne le soit \[WIZARD\]WIZARD\_ROOT (si il est spécifié). les recherches s’effectuent dans l’ordre dans lequel les chemins sont listés. La première occurrence avec le sous-programme recherché est utilisée. Les répertoires sont spécifiés relativement au répertoire courant du fichier ini ou par des chemins absolus. La liste ne doit contenir aucun espace blanc.

-   *USER\_M\_PATH = myfuncs:/tmp/mcodes:experimentalmcodes* - Spécifie une liste de répertoires, séparés par (:) (sans aucun espace blanc) pour les fonctions définies par l’utilisateur. Les répertoires sont spécifiés par rapport au répertoire courant pour les fichiers ini ou en chemins absolus. La liste ne doit contenir aucun espace blanc.

-   *USER\_DEFINED\_FUNCTION\_MAX\_DIRS=5* - Défini le nombre maximum de répertoires au moment de la compilation. Une recherche est faite pour chaque fonction utilisateur définie possible, typiquement *M100* à *M199*.
     L’ordre de recherche est le suivant:

    1.  \[DISPLAY\]PROGRAM\_PREFIX (si il est spécifié)

    2.  Si \[DISPLAY\]PROGRAM\_PREFIX n’est pas spécifié, cherche dans le répertoire par défaut: nc\_files

    3.  Recherche ensuite dans chaque répertoire de la liste \[RS274NGC\]USER\_M\_PATH Le premier M1xx trouvé au cours de la recherche est utilisé pour chaque M1xx.

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
<td align="left">[WIZARD]WIZARD_ROOT est un chemin de recherche valide mais l’assistant n’est pas encore complétement implémenté et les résultats, découlant de son utilisation, sont imprévisibles.</td>
</tr>
</tbody>
</table>

### Section \[EMCMOT\]

D’autres entrées peuvent être rencontrées dans cette section, elles ne doivent pas être modifiées.

-   *BASE\_PERIOD = 50000* - (HAL) Période de base des tâches, exprimée en ns.C’est la plus rapide des horloges de la machine. Avec un système à servomoteurs, il n’y a généralement pas de raison pour que *BASE\_PERIOD* soit plus petite que *SERVO\_PERIOD*. Sur une machine de type *step&direction* avec génération logicielle des impulsions de pas, c’est *BASE\_PERIOD* qui détermine le nombre maximum de pas par seconde. Si de longues impulsions de pas ou de longs espaces entre les impulsions ne sont pas requis par l'électronique, la fréquence maximum absolue est de un pas par *BASE\_PERIOD*. Ainsi, la *BASE\_PERIOD* utilisée ici donnera une fréquence de pas maximum absolue de 20000 pas par seconde. 50000ns est une valeur assez large. La plus petite valeur utilisable est liée au résultat [du test de latence](#cha:test-de-latence), à la longueur des impulsions de pas nécessaire et à la vitesse du µP. Choisir une BASE\_PERIOD trop basse peut amener à des messages *Unexpected realtime delay*, des blocages ou des reboots spontanés.

-   *SERVO\_PERIOD = 1000000* - (hal) Période de la tâche *Servo*, exprimée également en nanosecondes. Cette valeur sera arrondie à un multiple entier de *BASE\_PERIOD*. Elle est utilisée aussi sur des systèmes basés sur des moteurs pas à pas. C’est la vitesse avec laquelle la nouvelle position des moteurs est traitée, les erreurs de suivi vérifiées, les valeurs des sorties PID sont rafraichies etc. Sur la plupart des systèmes cette valeur n’est pas à modifier. Il s’agit du taux de mise à jour du planificateur de mouvement de bas niveau.

-   *TRAJ\_PERIOD = 1000000* - (hal) Période du planificateur de trajectoire, exprimée en nanosecondes. Cette valeur sera arrondie à un multiple entier de *SERVO\_PERIOD*. Excepté pour les machines avec une cinématique particulière (ex: hexapodes) Il n’y a aucune raison de rendre cette valeur supérieure à *SERVO\_PERIOD*.

### Section \[TASK\]

-   *TASK = milltask* - Indique le nom de la *tâche* exécutable. La tâche réalise différentes actions, telles que communiquer avec les interfaces utilisateur au dessus de NML, communiquer avec le planificateur de mouvements temps réel dans la mémoire partagée non-HAL, et interpréter le g-code. Actuellement il n’y a qu’une seule tâche exécutable qui fait sens pour 99,9% des utilisateurs, milltask.

-   *CYCLE\_TIME = 0.010* - Période exprimée en secondes, à laquelle TASK va tourner. Ce paramètre affecte l’intervalle de polling lors de l’attente de la fin d’un mouvement, lors de l’exécution d’une pause d’instruction et quand une commande provenant d’une interface utilisateur est acceptée. Il n’est généralement pas nécessaire de modifier cette valeur.

### Section \[HAL\]

-   *TWOPASS=ON* - Utilise le processus *twopass* (double passe) pour charger les composants HAL. Avec le processus TWOPASS, tous les fichiers \[HAL\]HALFILES sont premièrement lus et les occurrences multiples des directives à loadrt pour chaque module sont cumulées. Aucune commande HAL n’est exécutée à la première passe.

-   *HALFILE = example.hal* - Exécute le fichier *example.hal* au démarrage. Si *HALFILE* est spécifié plusieurs fois, les fichiers sont exécutés dans l’ordre de leur apparition dans le fichier ini. Presque toutes les configurations auront au moins un *HALFILE* . Les systèmes à moteurs pas à pas ont généralement deux de ces fichiers, un qui spécifie la configuration générale des moteurs *core\_stepper.hal* et un qui spécifie le brochage des sorties *xxx\_pinout.hal*.

-   *HAL = command* - Exécute *command* comme étant une simple commande hal. Si *HAL* est spécifié plusieurs fois, les commandes sont exécutées dans l’ordre où elles apparaissent dans le fichier ini. Les lignes *HAL* sont exécutées après toutes les lignes *HALFILE*.

-   *SHUTDOWN = shutdown.hal* - Exécute le fichier *shutdown.hal* quand Machinekit s’arrête. Selon les pilotes de matériel utilisés, il est ainsi possible de positionner les sorties sur des valeurs définies quand Machinekit s’arrête normalement. Cependant, parce qu’il n’y a aucune garantie que ce fichier sera exécuté (par exemple, dans le cas d’une panne de l’ordinateur), il ne remplace pas une véritable chaîne physique d’arrêt d’urgence ou d’autres dispositifs logiciels de protection des défauts de fonctionnement comme la pompe de charge ou le watchdog.

-   *POSTGUI\_HALFILE = example2.hal* - (Seulement avec les interfaces TOUCHY et AXIS) Exécute *example2.hal* après que l’interface graphique ait créé ses HAL pins.

### Section \[HALUI\]

-   *MDI\_COMMAND = G53 G0 X0 Y0 Z0* - Une commande MDI peut être exécuté en utilisant *halui.mdi-command-00*. Incrémente le nombre pour chaque commande énumérée dans la section \[HALUI\].

### Section \[TRAJ\]

La section \[TRAJ\] contient les paramètres généraux du module planificateur de trajectoires de EMCMOT. Vous n’aurez pas à modifier ces valeurs si vous utilisez Machinekit avec une machine à trois axes en provenance des USA. Si vous êtes dans une zone métrique, utilisant des éléments matériels métriques, vous pourrez utiliser le fichier *stepper\_mm.ini* dans lequel les valeurs sont déjà configurées dans cette unité.

-   *COORDINATES = X Y Z* - Les noms des axes à contrôler. X, Y, Z, A, B, C, U, V et W sont valides. Seuls les axes nommés dans *COORDINATES* seront acceptés dans le G-code. Cela n’a aucun effet sur l’ordonnancement des noms d’axes depuis le G-code (X- Y- Z-) jusqu’aux numéros d’articulations. Pour une *cinématique triviale*, X est toujours l’articulation 0, A est toujours l’articulation 3, U est toujours l’articulation 6 et ainsi de suite. Il est permis d'écrire les noms d’axe par paire (ex: X Y Y Z pour une machine à portique) mais cela n’a aucun effet.

-   *AXES = 3* - Une unité de plus que le plus grand numéro d’articulation du système. Pour une machine XYZ, les articulations sont numérotées 0, 1 et 2. Dans ce cas, les AXES sont 3. Pour un système XYUV utilisant une *cinématique triviale*, l’articulation V est numérotée 7 et donc les AXES devraient être 8. Pour une machine à cinématique non triviale (ex: scarakins) ce sera généralement le nombre d’articulations contrôlées.

-   *JOINTS = 3* - (Cette variable de configuration est utilisée seulement par Axis et non par le planificateur de trajectoire du contrôleur de mouvement.) Elle spécifie le nombre d’articulations (moteurs) que comporte le système. Par exemple, une machine XYZ avec un seul moteur pour chacun des 3 axes, comporte 3 articulations (joints). Une machine à portique avec un seul moteur sur deux de ses axes et deux moteurs sur le troisième axe, comporte 4 articulations (joints).

-   *HOME = 0 0 0* - Coordonnées de l’origine machine de chaque axe. De nouveau, pour une machine 4 axes, vous devrez avoir 0 0 0 0. Cette valeur est utilisée uniquement pour les machines à cinématique non triviale. Sur les machines avec cinématique triviale, cette valeur est ignorée.

-   *LINEAR\_UNITS=&lt;units&gt;* - Le nom des unités utilisées dans le fichier INI. Les choix possibles sont *in*, *inch*, *imperial*, *metric*, *mm*. Cela n’affecte pas les unités linéaires du code NC (pour cela il y a les mots G20 et G21).

-   *ANGULAR\_UNITS=&lt;units&gt;* - Le nom des unités utilisées dans le fichier INI. Les choix possibles sont *deg*, *degree* (360 pour un cercle), *rad*, *radian* (2pi pour un cercle), *grad*, ou *gon* (400 pour un cercle). Cela n’affecte pas les unités angulaires du code NC. Dans le code RS274NGC, les mots A-, B- et C- sont toujours exprimés en degrés.

-   *DEFAULT\_VELOCITY = 0.0167* - La vitesse initiale de jog des axes linéaires, en unités par seconde. La valeur indiquée ici correspond à une unité par minute.

-   *DEFAULT\_ACCELERATION = 2.0* - Dans les machines à cinématique non triviale, l’accélération utilisée pour *teleop* jog (espace cartésien), en unités machine par seconde par seconde.

-   *MAX\_VELOCITY = 5.0* - Vitesse maximale de déplacement pour les axes, exprimée en unités machine par seconde. La valeur indiquée est égale à 300 unités par minute.

-   *MAX\_ACCELERATION = 20.0* - Accélération maximale pour les axes, exprimée en unités machine par seconde par seconde.

-   *POSITION\_FILE = position.txt* - Si réglée à une valeur non vide, les positions des axes (joins) sont enregistrées dans ce fichier. Cela permet donc de redémarrer avec les mêmes coordonnées que lors de l’arrêt, ce qui suppose, que hors puissance, la machine ne fera aucun mouvement pendant tout son arrêt. C’est utile pour les petites machines sans contact d’origine machine. Si vide, les positions ne seront pas enregistrées et commenceront à 0 à chaque fois que Machinekit démarrera.

-   *NO\_FORCE\_HOMING = 1* - Machinekit oblige implicitement l’utilisateur à référencer la machine par une prise d’origine machine avant de pouvoir lancer un programme ou exécuter une commande dans le MDI, seuls les mouvements de Jog sont autorisés avant les prises d’origines. Mettre NO\_FORCE\_HOMING = 1 permet à l’opérateur averti de s’affranchir de cette restriction de sécurité lors de la phase de mise au point de la machine.

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
<td align="left"><em>NO_FORCE_HOMING</em> mise à 1 permettra à la machine de franchir les limites logicielles pendant les mouvements ce qui n’est pas souhaitable pour un fonctionnement normal!</td>
</tr>
</tbody>
</table>

### Sections \[AXIS\_n\]

Les sections \[AXIS\_0\], \[AXIS\_1\], etc. contiennent les paramètres généraux des composants individuels du module de contrôle. La numérotation des sections axis commence à 0 et augmente jusqu’au nombre d’axes spécifiés dans la variable \[TRAJ\] AXES, moins 1.

Généralement (mais pas toujours):

-   AXIS\_0 = X

-   AXIS\_1 = Y

-   AXIS\_2 = Z

-   AXIS\_3 = A

-   AXIS\_4 = B

-   AXIS\_5 = C

-   AXIS\_6 = U

-   AXIS\_7 = V

-   AXIS\_8 = W

    -   *TYPE = LINEAR* - Type des axes, soit LINEAR, soit ANGULAR.

    -   *WRAPPED\_ROTARY = 1* - Lorsque ce paramètre est réglé à 1 pour un axe angulaire l’axe se déplace de 0 à 359.999 degrés. Les nombres positifs déplacent l’axe dans le sens positif et les nombres négatifs dans le sens négatif.

    -   *LOCKING\_INDEXER = 1* - Quand ce paramètre est mis à 1, un mouvement en G0 sur cet axe va produire un signal de déblocage sur la pin *axis.N.unlock*, puis attendre le signal *axis.N.is-unlocked* de cet axe pour déplacer l’axe à la vitesse rapide prévue pour cet axe. Après ce mouvement, le signal *axis.N.unlock* retombera à false et les mouvements attendront que *axis.N.is-unlocked* redevienne false. Le mouvement des autres axes n’est pas autorisé lors du mouvement d’un axe rotatif à verrou.

    -   *UNITS = inch* - Ce réglage écrase celui des variables \[TRAJ\] UNITS si il est spécifié. (ex: \[TRAJ\]LINEAR\_UNITS si le TYPE de cet axe est LINEAR, \[TRAJ\]ANGULAR\_UNITS si le TYPE de cet axe est ANGULAR)

    -   *MAX\_VELOCITY = 1.2* - Vitesse maximum pour cet axe en unités machine par seconde.

    -   *MAX\_ACCELERATION = 20.0* - Accélération maximum pour cet axe en unités machine par seconde au carré.

    -   *BACKLASH = 0.000* - Valeur de compensation du jeu en unités machine. Peut être utilisée pour atténuer de petites déficiences du matériel utilisé pour piloter cet axe. Si un backlash est ajouté à un axe et que des moteurs pas à pas sont utilisés, la valeur de STEPGEN\_MAXACCEL doit être 1.5 à 2 fois plus grande que celle de MAX\_ACCELERATION pour cet axe.

    -   *COMP\_FILE = file.extension* - Fichier dans lequel est enregistrée une structure de compensation spécifique à cet axe. Le fichier peut être nommé *xscrew.comp*, par exemple, pour l’axe X. Les noms de fichiers sont sensibles à la casse et peuvent contenir des lettres et/ou des chiffres. Les valeurs sont des triplets par ligne séparés par un espace. La première valeur est nominale (où elle devrait l'être). Les deuxième et troisième valeurs dépendront du réglage de COMP\_FILE\_TYPE. Actuellement la limite de Machinekit est de 256 triplets par axe. Si COMP\_FILE est spécifié, BACKLASH est ignoré. Les valeurs sont en unités machine.

    -   *COMP\_FILE\_TYPE = 0 ou 1* -

        -   *Si 0:* Les deuxième et troisième valeurs spécifient la position en avant (de combien l’axe est en avance) et la position en arrière (de combien l’axe est en retard), positions qui correspondent à la position nominale.

        -   *Si 1:* Les deuxième et troisième valeurs spécifient l’ajustement avant (à quelle distance de la valeur nominale lors d’un déplacement vers l’avant) et l’ajustement arrière (à quelle distance de la valeur nominale lors d’un déplacement vers l’arrière), positions qui correspondent à la position nominale.

Exemple de triplet avec COMP\_FILE\_TYPE = 0: 1.00 1.01 0.99
 Exemple de triplet avec COMP\_FILE\_TYPE = 1: 1.00 0.01 -0.01

-   *MIN\_LIMIT = -1000* - Limite minimale des mouvements de cet axe (limite logicielle), en unités machine. Quand cette limite tend à être dépassée, le contrôleur arrête le mouvement.

-   *MAX\_LIMIT = 1000* - Limite maximale des mouvements de cet axe (limite logicielle), en unités machine. Quand cette limite tend à être dépassée, le contrôleur arrête le mouvement.

-   *MIN\_FERROR = 0.010* - Valeur indiquant, en unités machine, de combien le mobile peut dévier à très petite vitesse de la position commandée. Si MIN\_FERROR est plus petit que FERROR, les deux produisent une rampe de points de dérive. Vous pouvez imaginer un graphe sur lequel une dimension représente la vitesse et l’autre, l’erreur tolérée. Quand la vitesse augmente, la quantité d’erreurs de suivi augmente également et tend vers la valeur FERROR.

-   *FERROR = 1.0* - FERROR est le maximum d’erreur de suivi tolérable, en unités machine. Si la différence entre la position commandée et la position retournée excède cette valeur, le contrôleur désactive les calculs des servomoteurs, positionne toutes les sorties à 0.0 et coupe les amplis des moteurs. Si MIN\_FERROR est présent dans le fichier .ini, une vitesse proportionnelle aux erreurs de suivi est utilisée. Ici, le maximum d’erreur de suivi est proportionnel à la vitesse, quand FERROR est appliqué à la vitesse rapide définie dans \[TRAJ\]MAX\_VELOCITY et proportionnel aux erreurs de suivi pour les petites vitesses. L’erreur maximale admissible sera toujours supérieure à MIN\_FERROR. Cela permet d'éviter que de petites erreurs de suivi sur les axes stationnaires arrêtent les mouvements de manière impromptue. Des petites erreurs de suivi seront toujours présentes à cause des vibrations, etc. La polarité des valeurs de suivi détermine comment les entrées sont interprétées et comment les résultats sont appliqués aux sorties. Elles peuvent généralement être réglées par tâtonnement car il n’y a que deux possibilités. L’utilitaire de calibration peut être utilisé pour les ajuster interactivement et vérifier les résultats, de sorte que les valeurs puissent être mises dans le fichier INI avec un minimum de difficultés. Cet utilitaire est accessible dans Axis depuis le menu *Machine* puis *Calibration* et dans TkMachinekit depuis le menu *Réglages* puis *Calibration*.

### Section \[HOMING\]

Les paramètres suivants sont relatifs aux prises d’origine, pour plus d’informations, lire [le chapitre sur la POM](#sec:Prises-d-origine).

-   *HOME = 0.0* - La position à laquelle le mobile ira à la fin de la séquence de prise d’origine.

-   *HOME\_OFFSET = 0.0* - Position du contact d’origine machine de l’axe ou de l’impulsion d’index, en [unités machine](#sub:Section-TRAJ). Lorsque le point d’origine est détecté pendant le processus de prise d’origine, c’est cette position qui est assignée à ce point. Dans le cas du partage de capteur entre l’origine et les limites d’axe et de l’utilisation d’une séquence de prise d’origine qui laisse le capteur dans l'état activé, la valeur de HOME\_OFFSET peut être utilisée pour définir une position du capteur différente du 0 utilisé alors pour l’origine.

-   *HOME\_SEARCH\_VEL = 0.0* - Vitesse du mouvement initial de prise d’origine, en unités machine par seconde. Une valeur de zéro suppose que la position courante est l’origine machine. Si la machine n’a pas de contact d’origine, laisser cette valeur à zéro.

-   *HOME\_LATCH\_VEL = 0.0* - Vitesse du mouvement de dégagement du contact d’origine, en unités machine par seconde.

-   *HOME\_FINAL\_VEL = 0.0* - Vitesse du mouvement final entre le contact d’origine et la position d’origine, en unités machine par seconde. Si cette variable est laissée à 0 ou absente, la vitesse de déplacement rapide est utilisée. Doit avoir une valeur positive.

-   *HOME\_USE\_INDEX = NO* - Si l’encodeur utilisé pour cet axe fournit une impulsion d’index et qu’elle est gérée par la carte contrôleur, il est possible de mettre sur Yes. Quand il est sur yes, il aura une incidence sur le type de séquence de prise d’origine utilisée.

-   *HOME\_IGNORE\_LIMITS = NO* - Si la machine utilise un seul et même contact comme limite d’axe et origine machine de l’axe. Cette variable devra alors être positionnée sur yes. Dans ce cas le contact de limite de cet axe est ignoré pendant la séquence de prise d’origines. Il est nécessaire de configurer la séquence pour qu'à la fin du mouvement le capteur ne reste pas dans l'état activé qui aboutirait finalement à un message d’erreur du capteur de limite.

-   *HOME\_IS\_SHARED = &lt;n&gt;* - Si l’entrée du contact d’origine est partagée par plusieurs axes, mettre &lt;n&gt; à 0 pour permettre la POM même si un des contacts partagés est déjà attaqué. Le mettre à 1 pour interdire la prise d’origine dans ce cas.

-   *HOME\_SEQUENCE = &lt;n&gt;* - Utilisé pour définir l’ordre dans lequel les axes se succéderont lors d’une séquence de *POM générale*. **&lt;n&gt;** commence à 0, aucun numéro ne peut être sauté. Si cette variable est absente ou à -1, la POM de l’axe ne pourra pas être exécutée par la commande *POM générale*. La POM de plusieurs axes peut se dérouler simultanément.

-   *VOLATILE\_HOME = 0* - Lorsqu’il est activé (mis à 1), l’origine machine de cette articulation sera effacée si la machine est en marche et que l’arrêt d’urgence est activé. Ceci est utile si la machine possède des contacts d’origine mais n’a pas de retour de position comme une machine à moteur pas à pas de type pas/direction.

### Variables relatives aux servomoteurs

Les éléments suivants sont pour les systèmes à servomoteurs et à pseudos servomoteurs. Cette description suppose que les unités en sortie du composant PID sont des Volts.

-   *DEADBAND = 0.000015* - (dans HAL) Quelle distance est assez proche de la consigne pour considérer le moteur en position, en unités machine. Cette variable est fréquemment réglée pour une distance équivalente à 1, 1.5, 2, ou 3 impulsions de comptage du codeur, mais cela n’a rien d’une règle stricte. Un réglage lâche (large) permet de moins solliciter le servo au détriment de la précision. Un réglage serré (petit) permettra d’atteindre une grande précision mais le servo sera plus sollicité. Est-ce vraiment plus précis si c’est plus incertain ? En règle générale, il est préférable d'éviter le plus possible de solliciter le servo, si c’est possible.

Ayez la prudence de ne pas chercher à aller en dessous d’une impulsion de codeur, sinon vous enverrez votre servo quelque part où il ne sera pas heureux ! Cela peut arriver entre réglage lent et réglage nerveux et même un réglage impropre peut provoquer des couinements, des grincements dus aux oscillations provoquées par ce mauvais réglage. Il est préférable de perdre une ou deux impulsions au début des réglages, au moins jusqu'à avoir bien dégrossi les réglages.

Exemple de calcul en unités machine par top de codeur à utiliser pour décider de la valeur de DEADBAND (bande morte):

**`X pouces / top de codeur =`** **`1 tour / 1000 top de codeur * 1 top de codeur / 4 top en quadrature * 0.2 pouce / tour =`** **`0.200 pouce / 4000 top de codeur = 0.000050 pouce / top de codeur.`**

-   *BIAS = 0.000* - (dans HAL) (Parfois appelé *offset*) il est utilisé par hm2-servo et quelques autres. Le Bias est une valeur constante qui est ajoutée sur la sortie. Dans la plupart des cas, elle peut rester à zéro. Toutefois, il peut être intéressant pour compenser un décalage de l’ampli du servo, ou équilibrer le poids d’un objet se déplaçant verticalement. Le bias est mis à zéro quand la boucle PID est désactivée, comme tous les autres composants de la sortie.

-   *P = 50* - (hal) La composante Proportionnelle du gain de l’ampli moteur de cet axe. Cette valeur multiplie l’erreur entre la position commandée et la position actuelle en unités machine, elle entre dans le calcul de la tension appliquée à l’ampli moteur. Les unités du gain **P** sont des Volts sur des unités machine, par exemple: **`Volt/mm`** si l’unité machine est le millimètre.

-   *I = 0* - (hal) La composante Intégrale du gain de l’ampli moteur de cet axe. Cette valeur multiplie l’erreur cumulative entre la position commandée et la position actuelle en unités machine, elle entre dans le calcul de la tension appliquée à l’ampli moteur. Les unités du gain **I** sont des Volts sur des unités machine par seconde, exemple: **`Volt/mm*s`** si l’unité machine est le millimètre.

-   *D = 0* - (hal) La composante Dérivée du gain de l’ampli moteur de cet axe. Cette valeur multiplie la différence entre l’erreur courante et les précédentes, elle entre dans le calcul de la tension appliquée à l’ampli moteur. Les unités du gain **D** sont des Volts sur des unités machine sur des secondes, exemple: **`Volt/(mm/s)`** si l’unité machine est le millimètre.

-   *FF0 = 0* - (hal) Gain à priori (retour vitesse) d’ordre 0. Cette valeur est multipliée par la position commandée, elle entre dans le calcul de la tension appliquée à l’ampli moteur. Les unités du gain FF0 sont des Volts sur des unités machine, exemple: **`Volt/mm`** si l’unité machine est le millimètre.

-   *FF1 = 0* - (hal) Gain à priori (retour vitesse) de premier ordre. Cette valeur est multipliée par l'écart de la position commandée par seconde, elle entre dans le calcul de la tension appliquée à l’ampli moteur. Les unités du gain FF1 sont des Volts sur des unités machine par seconde, exemple: **`Volt/(mm/s)`** si l’unité machine est le millimètre.

-   *FF2 = 0* - (hal) Gain à priori (retour vitesse) de second ordre. Cette valeur est multipliée par l'écart de la position commandée par seconde au carré, elle entre dans le calcul de la tension appliquée à l’ampli moteur. Les unités du gain FF2 sont des Volts sur des unités machine par des secondes au carré, exemple: **`Volt/mm/s2`** si l’unité machine est le millimètre.

-   *OUTPUT\_SCALE = 1.000* -

-   *OUTPUT\_OFFSET = 0.000* - (hal) Ces deux valeurs sont les facteurs d'échelle et offset pour la sortie de l’axe à l’amplificateurs moteur. La seconde valeur (offset) est soustraite de la valeur de sortie calculée (en Volts) puis divisée par la première valeur (facteur d'échelle), avant d'être écrite dans le convertisseur D/A. Les unités du facteur d'échelle sont des Volts réels par Volts en sortie de DAC. Les unités de la valeur d’offset sont en Volts. Ces valeurs peuvent être utilisées pour linéariser un DAC. Plus précisément, quand les sorties sont écrites, Machinekit converti d’abord les unités quasi-SI des sorties concernées en valeurs brutes, exemple: Volts pour un amplificateur DAC. Cette mise à l'échelle ressemble à cela:
     **`raw = output-offset/scale`** la valeur d'échelle peut être obtenue par analyse des unités, exemple: les unités sont \[unités SI en sortie\]/\[unités de l’actuateur\]. Par exemple, sur une machine sur laquelle une tension de consigne de l’ampli de 1 Volt donne une vitesse de 250 mm/s :
     **`amplifier [volts] = (output[mm/s] - offset[mm/s]) / 250mm/(s/Volt)`**

    Notez que les unités d’offset sont en unités machine, exemple: mm/s et qu’elles sont déjà soustraites depuis la sonde de lecture. La valeur de cet offset est obtenue en prenant la valeur de votre sortie qui donne 0,0 sur la sortie de l’actuateur. Si le DAC est linéarisé, cet offset est normalement de 0.0.

L'échelle et l’offset peuvent être utilisés pour linéariser les DAC, d’où des valeurs qui reflètent les effets combinés du gain de l’ampli, de la non linéarité du DAC, des unités du DAC, etc. Pour ce faire, suivez cette procédure:

-   Construire un tableau de calibrage pour la sortie, piloter le DAC avec la tension souhaitée et mesurer le résultat. Voir le tableau ci-dessous pour un exemple de mesures de tension.

-   Par la méthode des moindres carrés, obtenir les coefficients **a**,**b** tels que: **`mesure = a*raw+b`**

-   Noter que nous voulons des sorties brutes de sorte que nos résultats mesurés soient identiques à la sortie commandée. Ce qui signifie:

-   **`cmd = a*raw+b`**

-   **`raw = (cmd-b)/a`**

-   En conséquence, les coefficients **a** et **b** d’ajustement linéaire peuvent être directement utilisés comme valeurs d'échelle et d’offset pour le contrôleur.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Brutes (Raw)</th>
<th align="left">Mesurées</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>-10</p></td>
<td align="left"><p>-9.93</p></td>
</tr>
<tr class="even">
<td align="left"><p>-9</p></td>
<td align="left"><p>-8.83</p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>-0.03</p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>0.96</p></td>
</tr>
<tr class="odd">
<td align="left"><p>9</p></td>
<td align="left"><p>9.87</p></td>
</tr>
<tr class="even">
<td align="left"><p>10</p></td>
<td align="left"><p>10.87</p></td>
</tr>
</tbody>
</table>

-   *MAX\_OUTPUT = 10* - (hal) La valeur maximale pour la sortie de la compensation PID pouvant être envoyée sur l’ampli moteur, en Volts. La valeur calculée de la sortie sera fixée à cette valeur limite. La limite est appliquée avant la mise à l'échelle de la sortie en unités brutes. La valeur est appliquée de manière symétrique aux deux côtés, le positif et le négatif.

-   *INPUT\_SCALE = 20000* - (hal) Spécifie le nombre d’impulsions qui correspond à un mouvement de une unité machine telle que fixée dans la section TRAJ. Pour un axe linéaire, une unité machine sera égale à la valeur de LINEAR\_UNITS. Pour un axe angulaire, une unité machine sera égale à la valeur de ANGULAR\_UNITS. Un second chiffre, si spécifié, sera ignoré. Par exemple, sur un codeur de 2000 impulsions par tour, un réducteur de 10 tours/pouce et des unités demandées en pouces, nous avons:
     **`INPUT_SCALE = 2000 top/tour * 10 tour/pouce = 20000 top/pouce`**

### Variables relatives aux moteurs pas à pas

-   *SCALE = 4000* - (hal) Spécifie le nombre d’impulsions qui correspond à un mouvement d’une unité machine comme indiqué dans la section \[TRAJ\]. Pour les systèmes à moteurs pas à pas, c’est le nombre d’impulsions de pas nécessaires pour avancer d’une unité machine. Pour un axe linéaire, une unité machine sera égale à la valeur de LINEAR\_UNITS. Pour un axe angulaire, une unité machine sera égale à la valeur de ANGULAR\_UNITS. Pour les systèmes à servomoteurs, c’est le nombre d’impulsions de retour signifiant que le mobile a avancé d’une unité machine. Un second nombre, si spécifié, sera ignoré. Par exemple, un pas moteur de 1.8 degré, en mode demi-pas, avec une réduction de 10 tours/pouce et des unités souhaitées en pouces, nous avons:
     **`scale = 2 pas/1.8 degrés * 360 degrés/tour * 10 tour/pouce = 4000 pas/pouce`**

(D’anciens fichiers .ini et .hal utilisaient INPUT\_SCALE pour cette valeur.)

-   *STEPGEN\_MAXACCEL = 21.0* - (hal) Limite d’accélération pour le générateur de pas. Elle doit être 1% à 10% supérieure à celle de l’axe MAX\_ACCELERATION. Cette valeur améliore les réglages de la *boucle de position* de stepgen. Si une correction de jeu a été appliquée sur un axe, alors STEPGEN\_MAXACCEL doit être 1,5 à 2 fois plus grande que MAX\_ACCELERATION.

-   *STEPGEN\_MAXVEL = 1.4* - (hal) Les anciens fichiers de configuration avaient également une limite de vitesse du générateur de pas. Si spécifiée, elle doit aussi être 1% à 10% supérieure à celle de l’axe MAX\_VELOCITY. Des tests ultérieurs ont montré que l’utilisation de STEPGEN\_MAXVEL n’améliore pas le réglage de la boucle de position de stepgen.

### Section \[EMCIO\]

-   *CYCLE\_TIME = 0.100* - La période en secondes, à laquelle EMCIO va tourner. La mettre à 0.0 ou à une valeur négative fera que EMCIO tournera en permanence. Il est préférable de ne pas modifier cette valeur.

-   *TOOL\_TABLE = tool.tbl* - Ce fichier contient les informations des outils, décrites dans le Manuel de l’utilisateur.

-   *TOOL\_CHANGE\_POSITION = 0 0 2* - Quand trois digits sont utilisés, spécifie la position XYZ ou le mobile sera déplacé pour le changement d’outil. Si six digits sont utilisés, spécifie l’emplacement ou sera envoyé le mobile pour réaliser le changement d’outil sur une machine de type XYZABC et de même, sur une machine de type XYZABCUVW lorsque 9 digits sont utilisés. Les variables relatives à la position du changement d’outil peuvent être combinées, par exemple; en combinant TOOL\_CHANGE\_POSITION avec TOOL\_CHANGE\_QUILL\_UP il est possible de déplacer d’abord Z puis X et Y.

-   *TOOL\_CHANGE\_WITH\_SPINDLE\_ON = 1* - Avec cette valeur à 1, la broche reste en marche pendant le changement d’outil. Particulièrement utile sur les tours.

-   *TOOL\_CHANGE\_QUILL\_UP = 1* - Avec cette valeur à 1, l’axe Z sera déplacé sur son origine machine avant le changement d’outil. C’est l'équivalent d’un G0 G53 Z0.

-   *TOOL\_CHANGE\_AT\_G30 = 1* - Avec cette valeur à 1, le mobile sera envoyé sur un point de référence prédéfini par G30 dans les paramètres 5181-5186. Pour plus de détails sur les paramètres de G30, voir le chapitre relatif au G-code dans le Manuel de l’utilisateur.

-   *RANDOM\_TOOLCHANGER = 1* - C’est pour des machines qui ne peuvent pas placer l’outil dans la poche il vient. Par exemple, les machines qui change l’outil dans la poche active avec l’outil dans la broche.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


