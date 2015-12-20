comp: outil pour créer les modules HAL
======================================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:comp-hal-component-generator"></span>

Introduction
------------

Écrire un composant de HAL peut se révéler être une tâche ennuyeuse, la plupart de cette tâche consiste à appeler des fonctions *rtapi* et *hal* et à contrôler les erreurs associées à ces fonctions. *comp* va écrire tout ce code pour vous, automatiquement.

Compiler un composant de HAL est également beaucoup plus simple en utilisant *comp* , que le composant fasse partie de l’arborescence de Machinekit, ou qu’il en soit extérieur.

Par exemple, cette portion des blocs *ddt*, codée en C, fait environ 80 lignes de code, alors que le composant équivalent est vraiment très court quand il est créé en utilisant le préprocesseur *comp*.

Exemple pour comp <span id="code:exemple-comp"></span>

    component ddt "Calcule la dérivée de la fonction d'entrée";
    pin in float in;
    pin out float out;
    variable float old;
    function _;
    license "GPLv2 or later";
    ;;
    float tmp = in;
    out = (tmp - old) / fperiod;
    old = tmp;

Installation
------------

Si une version pré-installée de Machinekit est utilisée, il sera nécessaire d’installer les paquets de développement en passant par Synaptic depuis le menu *Système → Administration → Gestionnaire de paquets Synaptic* ou en utilisant la commande suivante dans un terminal:

Installation des paquets de développement

    sudo apt-get install machinekit-dev

Définitions
-----------

 component   
Un composant est un simple module temps réel, qui se charge avec *halcmd loadrt*. Un fichier *.comp* spécifie un seul composant.

 instance   
Un composant peut avoir zéro ou plusieurs instances. Chaque instance d’un composant est créée égale (elles ont toutes les mêmes pins, les mêmes paramètres, les mêmes fonctions et les mêmes données) mais elle se comporte de manière différente quand leurs pins, leurs paramètres et leur données ont des valeurs différentes.

 singleton   
Il est possible pour un composant d'être un *singleton* (composant dont il n’existe qu’une seule instance), dans ce cas, exactement une seule instance est créée. Il est rarement logique d'écrire un composant *singleton* , à moins qu’il n’y ait qu’un seul objet de ce type dans le système (par exemple, un composant ayant pour but de fournir une pin avec le temps Unix courant, ou un pilote matériel pour le haut parleur interne du PC)

Création d’instance
-------------------

Pour un singleton, une seule instance est créée quand le composant est chargé.

Pour un non-singleton, le paramètre *count* du module détermine combien d’instances seront créées.

Paramètres implicites
---------------------

Le paramètres *period* est implicitement passé aux fonctions, c’est la durée, en nanosecondes, de la dernière période d’exécution du comp. Les fonctions qui utilisent des flottants peuvent aussi se référer à *fperiod*, qui est la durée en secondes, soit (period\*1e-9). Cela peut être utile pour les comps ayant besoin de l’information de timming.

Syntaxe
-------

Un fichier *.comp* commence par un certain nombre de déclarations, puis par un délimiteur constitué de deux points virgule *;;* seuls sur leur propre ligne et enfin du code C implémentant les fonctions du module.

Déclarations d’include:

-   *component HALNAME (DOC);*

-   *pin PINDIRECTION TYPE HALNAME (\[SIZE\]|\[MAXSIZE : CONDSIZE\]) (if CONDITION) (= STARTVALUE) (DOC);*

-   *param PARAMDIRECTION TYPE HALNAME (\[SIZE\]|\[MAXSIZE : CONDSIZE\]) (if CONDITION) (= STARTVALUE) (DOC);*

-   *function HALNAME (fp | nofp) (DOC);*

-   *option OPT (VALUE);*

-   *variable CTYPE NAME (\[SIZE\]);*

-   *description DOC;*

-   *see\_also DOC;*

-   *license LICENSE;*

-   *author AUTHOR;*

Les parenthèses indiquent un item optionnel. Une barre verticale indique une alternative. Les mots en *MAJUSCULES* indiquent une variable texte, comme ci-dessous:

 NAME   
Un identifiant C standard.

 STARREDNAME   
Un identifiant C, précédé ou non d’une *\**. Cette syntaxe est utilisée pour déclarer les variables qui sont des pointeurs. Noter qu'à cause de la grammaire, il ne doit pas y avoir d’espace entre *\** et le nom de la variable.

 HALNAME   
Un identifiant étendu. Lorsqu’ils sont utilisés pour créer un identifiant de HAL, tous les caractères soulignés sont remplacés par des tirets, tous les points et les virgules de fin, sont supprimés, ainsi **ce\_nom\_** est remplacé par **ce-nom**, si le nom est `"_"`, alors le point final est enlevé aussi, ainsi `"function_"` donne un nom de fonction HAL tel que `"component.<num>"` au lieu de `"component.<num>."`

S’il est présent, le préfixe *hal\_* est enlevé du début d’un nom de composant lors de la création des pins, des paramètres et des fonctions.

Dans l’identifiant de HAL pour une pin ou un paramètre, *\#* indique un membre de tableau, il doit être utilisé conjointement avec une déclaration *\[SIZE\]*. Les *hash marks* sont remplacées par des nombres de 0-barrés équivalents aux nombres de caractères \#.

Quand ils sont utilisés pour créer des identifiants C, les changements de caractères suivants sont appliqués au HALNAME:

1.  Tous les caractères "`#`" sont enlevés ainsi que tous les caractères "`.`", "`_`" ou "`-`" immédiatement devant eux.

2.  Dans un nom, tous les caractères "`.`" et "`-`" sont remplacés par "`_`".

3.  Les caractères "`__`" répétitifs sont remplacés par un seul caractère "`_`".

Un "`_`" final est maintenu, de sorte que les identifiants de HAL, qui autrement seraient en conflit avec les noms ou mots clé réservés (par exemple: *min*), puissent être utilisés.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">HALNAME</th>
<th align="left">Identifiant C</th>
<th align="left">Identifiant HAL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>x_y_z</p></td>
<td align="left"><p>x_y_z</p></td>
<td align="left"><p>x-y-z</p></td>
</tr>
<tr class="even">
<td align="left"><p>x-y.z</p></td>
<td align="left"><p>x_y_z</p></td>
<td align="left"><p>x-y.z</p></td>
</tr>
<tr class="odd">
<td align="left"><p>x_y_z_</p></td>
<td align="left"><p>x_y_z_</p></td>
<td align="left"><p>x-y-z</p></td>
</tr>
<tr class="even">
<td align="left"><p>x.##.y</p></td>
<td align="left"><p>x_y(MM)</p></td>
<td align="left"><p>x.MM.z</p></td>
</tr>
<tr class="odd">
<td align="left"><p>x.##</p></td>
<td align="left"><p>x(MM)</p></td>
<td align="left"><p>x.MM</p></td>
</tr>
</tbody>
</table>

 if CONDITION   
Une expression impliquant la *personnalité* d’une variable non nulle quand la variable ou le paramètre doit être créé.

 SIZE   
Un nombre donnant la taille d’un tableau. Les items des tableaux sont numérotés de 0 à *SIZE*-1.

 MAXSIZE : CONDSIZE   
Un nombre donnant la taille maximum d’un tableau, suivi d’une expression impliquant la *personnalité* d’une variable et qui aura toujours une valeur inférieure à *MAXSIZE*. Quand le tableau est créé sa taille est égale à *CONDSIZE*.

 DOC   
Une chaine qui documente l’item. La chaine doit être au format C, entre guillemets, comme *"Sélectionnez le front désiré: TRUE pour descendant, FALSE pour montant"* ou au format Python triples guillemets, pouvant inclure des caractères newlines et des guillemets, comme: param rw bit zot=TRUE La chaine de documentation est en format *groff -man*. Pour plus d’informations sur ce format de markup, voyez *groff\_man(7)* . Souvenez vous que comp interprète backslash comme Echap dans les chaines, ainsi par exemple pour passer le mot *example* en font italique, écrivez *\\\\fIexample\\\\fB*.

 TYPE   
Un des types de HAL: *bit*, *signed* (signé), *unsigned* (non signé) ou *float* (flottant). Les anciens noms *s32* et *u32* peuvent encore être utilisés, mais *signed* et *unsigned* sont préférables.

 PINDIRECTION   
Une des ces directions: *in*, *out*, ou *io* . Le composant pourra positionner la valeur d’une pin de sortie, il pourra lire la valeur sur une pin d’entrée et il pourra lire ou positionner la valeur d’une pin *io*.

 PARAMDIRECTION   
Une des valeurs suivantes: *r* ou *rw*. Le composant pourra positionner la valeur d’un paramètre *r* et il pourra positionner ou lire la valeur d’un paramètre rw.

 STARTVALUE   
Spécifie la valeur initiale d’une pin ou d’un paramètre. Si il n’est pas spécifié, alors la valeur par défaut est *0* ou *FALSE*, selon le type de l’item.

### Fonctions HAL

 fp   
Indique que la fonction effectuera ses calculs en virgule flottante.

 nofp   
Indique que la fonction effectuera ses calculs sur des entiers. Si il n’est pas spécifié, *fp* est utilisé. Ni comp ni gcc ne peuvent détecter l’utilisation de calculs en virgule flottante dans les fonctions marquées *nofp*.

### Options

Selon le nom de l’option OPT, les valeurs VALUE varient. Les options actuellement définies sont les suivantes:

OPT, VALUE: option singleton yes;; (défaut: no) Ne crée pas le paramètre numéro de module et crée toujours une seule instance. Avec *singleton*, les items sont nommés *composant-name.item-name* et sans *singleton*, les items des différentes instances sont nommés *composant-name.&lt;num&gt;.item-name*.

 option default\_count   
*number* (défaut: 1) Normalement, le paramètre *count* par défaut est 0. Si spécifié, *count* remplace la valeur par défaut.

 option count\_function yes   
(défaut: no) Normalement, le numéro des instances à créer est specifié dans le paramètre *count* du module, si *count\_function* est spécifié, la valeur retournée par la fonction *int get\_count(void)* est utilisée à la place de la valeur par défaut et le paramètre *count* du module n’est pas défini.

 option rtapi\_app no   
(défaut: yes) Normalement, les fonctions *rtapi\_app\_main* et *rtapi\_app\_exit* sont définies automatiquement. Avec *option rtapi\_app no*, elles ne le seront pas et doivent être fournies dans le code C.

    Quand vous implémentez votre propre _rtapi_app_main_, appellez la
    fonction _int export(char _prefix, long extra_arg)_ pour enregistrer
    les pins, paramètres et fonctions pour _préfix_er.

 option data   
*type* (défaut: none) deprecated If specified, each instance of the component will have an associated data block of *type* (which can be a simple type like *float* or the name of a type created with *typedef*).

    In new components, _variable_ should be used instead.

 option extra\_setup yes   
(défaut: no)

 option extra\_cleanup yes   
(défaut: no) Si spécifié, appelle la fonction définie par *EXTRA\_CLEANUP* depuis la fonction définie automatiquement *rtapi\_app\_exit*, ou une erreur est détectée dans la fonction automatiquement définie *rtapi\_app\_main*.

 option userspace yes   
(défaut: no) Si spécifié, ce fichier décrit un composant d’espace utilisateur, plutôt que le réel. Un composant d’espace utilisateur peut ne pas avoir de fonction définie par la directive de fonction. Au lieu de cela, après que toutes les instances soient construites, la fonction C *user\_mainloop()* est appelée. Dès la fin de cette fonction, le composant se termine. En règle générale, *user\_mainloop()* va utiliser *FOR\_ALL\_INSTS()* pour effectuer la mise à jour pour chaque action, puis attendre un court instant. Une autre action commune dans *user\_mainloop()* peut être d’appeler le gestionnaire de boucles d'événements d’une interface graphique.

 option userinit yes   
(défaut: no) Si spécifiée, la fonction *userinit(argc,argv)* est appelée avant *rtapi\_app\_main()* (et cela avant l’appel de *hal\_init()* ). Cette fonction peut traiter les arguments de la ligne de commande ou exécuter d’autres actions. Son type de retour est *void*; elle peut appeler *exit()* et si elle le veut, se terminer sans créer de composant HAL (par exemple, parce que les arguments de la ligne de commande sont invalides).

    Si aucune option VALUE n'est spécifiée, alors c'est équivalent à
    spécifier la valeur _… yes_ . Le résultat consécutif à l'assignation
    d'une valeur inappropriée à
    une option est indéterminé. Le résultat consécutif à n'utiliser aucune
    autre option est indéfini.

### Licence et auteur

 LICENSE   
Spécifie la license du module, pour la documentation et pour le module déclaré dans MODULE\_LICENSE().

 AUTHOR   
Spécifie l’auteur du module, pour la documentation

### Stockage des données **par instance**

 variable   
*CTYPE NAME*;

 variable   
*CTYPE NAME*\[*SIZE*\];

 variable   
*CTYPE NAME = default*;

 variable   
*CTYPE NAME*\[*SIZE*\] = *default*; Déclare la variable *par-instance* *NAME* de type *CTYPE*, optionnellement comme un tableau de *SIZE* items et optionnellement avec une valeur par default. Les items sans *default* sont initialisés *all-bits-zero*. *CTYPE* est un simple mot de type C, comme *float*, *u32*, *s32*, etc. Les variables d’un tableau sont mises entre crochets.

### Commentaires

Les commentaires de style C++ une ligne (*// …*) et de style C multi-lignes (*/* … */*) sont supportés tous les deux dans la section déclaration.

Restrictions sur les fichiers comp
----------------------------------

Bien que HAL permette à une pin, un paramètre et une fonction d’avoir le même nom, comp ne le permet pas.

Les noms de variable et de fonction qui ne doivent pas être utilisés ou qui posent problème sont les suivants:

-   Tous noms commençant par *\_comp*.

-   *comp\_id*

-   *fperiod*

-   *rtapi\_app\_main*

-   *rtapi\_app\_exit*

-   *extra\_setup*

-   *extra\_cleanup*

Conventions des macros
----------------------

En se basant sur les déclarations des items de section, *comp* crée une structure C appellée *structure d'état* . Cependant, au lieu de faire référence aux membres de cette structure (par exemple: *\*(inst→name)* ), il leur sera généralement fait référence en utilisant les macros ci-dessous. Certains détails de la structure d'état et de ces macros peuvent différer d’une version de *comp* à la suivante.

 FUNCTION(name)   
Cette macro s’utilise au début de la définition d’une fonction temps réel qui aura été précédemment déclarée avec *function NAME*. function inclus un paramètre *period* qui est le nombre entier de nanosecondes entre les appels à la fonction.

 EXTRA\_SETUP()   
Cette macro s’utilise au début de la définition de la fonction appelée pour exécuter les réglages complémentaires à cette instance. Une valeur de retour négative Unix *errno* indique un défaut (par exemple: *elle retourne -EBUSY* comme défaut à la réservation d’un port d’entrées/sorties), une valeur égale à 0 indique le succés.

 EXTRA\_CLEANUP()   
Cette macro s’utilise au début de la définition de la fonction appelée pour exécuter un nettoyage (cleanup) du composant. Noter que cette fonction doit nettoyer toutes les instances du composant, pas juste un. Les macros *pin\_name*, *parameter\_name* et *data* ne doivent pas être utilisées ici.

 pin\_name ou parameter\_name   
Pour chaque pin, *pin\_name* ou pour chaque paramètre, *parameter\_name* il y a une macro qui permet d’utiliser le nom seul pour faire référence à la pin ou au paramètre. Quand *pin\_name* ou *parameter\_name* sont des tableaux, la macro est de la forme *pin\_name(idx)* ou *param\_name(idx)* dans laquelle *idx* est l’index dans le tableau de pins. Quand le tableau est de taille variable, il est seulement légal de faire référence aux items par leurs *condsize*.

    Quand un item est conditionnel, il est seulement légal de faire
    référence à cet item quand ses conditions sont évaluées à des
    valeurs différentes de zéro.

 variable\_name   
Pour chaque variable, *il y a une macro variable\_name* qui permet au nom seul d'être utilisé pour faire référence à la variable. Quand *variable\_name* est un tableau, le style normal de C est utilisé: *variable\_name\[idx\]*

 data   
Si l'*option data* est spécifiée, cette macro permet l’accès à l’instance de la donnée.

 fperiod   
Le nombre de secondes en virgule flottante entre les appels à cette fonction temps réel.

 FOR\_ALL\_INSTS() {**…**}   
Pour les composants de l’espace utilisateur. Cette macro utilise la variable \*struct state *inst* pour itérer au dessus de toutes les instances définies. Dans le corps de la boucle, les macros *pin\_name*, *parameter\_name* et *data* travaillent comme elles le font dans les fonctions temps réel.

Composants avec une seule fonction
----------------------------------

Si un composant a seulement une fonction et que la chaine *FUNCTION* n’apparaît nulle part après *;;*, alors la portion après *;;* est considérée comme étant le corps d’un composant simple fonction.

Personnalité du composant
-------------------------

Si un composant a n’importe combien de pins ou de paramètres avec un if condition ou *\[maxsize : condsize\]*, il est appelé un composant avec personnalité. La personnalité de chaque instance est spécifiée quand le module est chargé. La personnalité peut être utilisée pour créer les pins seulement quand c’est nécessaire. Par exemple, la personnalité peut être utilisée dans un composant logique, pour donner un nombre variable de broches d’entrée à chaque porte logique et permettre la sélection de n’importe quelle fonction de logique booléenne de base *and*, *or* et *xor*.

Compiler un fichier *.comp* dans l’arborescence
-----------------------------------------------

Placer le fichier *.comp* dans le répertoire *machinekit/src/hal/components* et lancer/relancer *make*. Les fichiers Comp sont automatiquement détectés par le système de compilation.

Si un fichier *.comp* est un pilote de périphérique, il peut être placé dans *machinekit/src/hal/components* et il y sera construit excepté si Machinekit est configuré en mode simulation.

Compiler un composant temps réel hors de l’arborescence<span id="sec:Compiler-composants-rt"></span>
----------------------------------------------------------------------------------------------------

*comp* peut traiter, compiler et installer un composant temps réel en une seule étape, en plaçant *rtexample.ko* dans le répertoire du module temps réel de Machinekit:

    comp --install rtexample.comp

Ou il peut aussi être traité et compilé en une seule étape en laissant *example.ko* (ou *example.so* pour la simulation) dans le répertoire courant:

    comp --compile rtexample.comp

Ou il peut simplement être traité en laissant *example.c* dans le répertoire courant:

    comp rtexample.comp

*comp* peut aussi compiler et installer un composant écrit en C, en utilisant *les options --install* et *--compile* comme ci-dessous:

    comp --install rtexample2.c

La documentation au format man peut être créée à partir des informations de la section *declaration*:

    comp --document rtexample.comp

La manpage résultante, *exemple.9* peut être lue avec:

    man ./exemple.9

ou copiée à un emplacement standard pour une page de manuel.

Compiler un composant de l’espace utilisateur hors de l’arborescence
--------------------------------------------------------------------

*comp* peut traiter, compiler et installer un document de l’espace utilisateur:

    comp usrexample.comp

Cela fonctionne seulement pour les fichiers *.comp* mais pas pour les fichiers *.c*.

Exemples
--------

### constant

Ce composant fonctionne comme dans *blocks*, y compris la valeur par défaut à 1.0. La déclaration *function \_* crée les fonctions nommées *constant.0*, etc.

    component constant;

### sincos

Ce composant calcule le sinus et le cosinus d’un angle entré en radians. Il a différentes possibilités comme les sorties *sinus* et *cosinus* de siggen, parce que l’entrée est un angle au lieu d'être librement basé sur un paramètre *frequency*.

Les pins sont déclarées avec les noms *sin *et \_cos* dans le code source pour que ça n’interfère pas avec les fonctions \_sin()* et *cos()*. Les pins de HAL sont toujours appelées *sincos.&lt;num&gt;.sin*.

    component sincos;

### out8

Ce composant est un pilote pour une carte imaginaire appelée *out8*, qui a 8 pins de sortie digitales qui sont traitées comme une simple valeur sur 8 bits. Il peut y avoir un nombre quelconque de ces cartes dans le système et elles peuvent avoir des adresses variées. La pin est appelée *out\_* parce que *out* est un identifiant utilisé dans *&lt;asm/io.h&gt;*. Il illustre l’utilisation de *EXTRA\_SETUP* et de *EXTRA\_CLEANUP* pour sa requête de région d’entrées/sorties et libère cette région en cas d’erreur ou quand le module est déchargé.

    component out8;
    pin out unsigned out_ "Output value; only low 8 bits are used";
    param r unsigned ioaddr;

    function _;

    option count_function;
    option extra_setup;
    option extra_cleanup;
    option constructable no;

    license "GPL";
    ;;
    #include <asm/io.h>

    #define MAX 8
    int io[MAX] = {0,};
    RTAPI_MP_ARRAY_INT(io, MAX, "I/O addresses of out8 boards");

    int get_count(void) {
        int i = 0;
        for(i=0; i<MAX && io[i]; i++) { /* Nothing */ }
        return i;
    }

    EXTRA_SETUP() {
        if(!rtapi_request_region(io[extra_arg], 1, "out8")) {
            // set this I/O port to 0 so that EXTRA_CLEANUP does not release the IO
            // ports that were never requested.
            io[extra_arg] = 0;
            return -EBUSY;
        }
        ioaddr = io[extra_arg];
        return 0;
    }

    EXTRA_CLEANUP() {
        int i;
        for(i=0; i < MAX && io[i]; i++) {
            rtapi_release_region(io[i], 1);
        }
    }

    FUNCTION(_) { outb(out_, ioaddr); }

### hal\_loop

    component hal_loop;

Ce fragment de composant illustre l’utilisation du préfixe *hal\_* dans un nom de composant. *loop* est le nom d’un module standard du kernel Linux, donc un composant *loop* ne pourrait pas être chargé si le module loop de Linux est également présent.

Quand il le charge, halcmd montre un composant appelé *hal\_loop*. Cependant, les pins affichées par halcmd sont *loop.0.example* et non *hal-loop.0.example*.

### arraydemo

Ce composant temps réel illustre l’utilisation d’un tableau de taille fixe:

    component arraydemo "Registre à décalage 4-bits";

### rand

Ce composant de l’espace utilisateur modifie la valeur de ses pins de sortie vers une nouvelle valeur aléatoire dans l'étendue (0,1) à chaque 1ms.

    component rand;
    option userspace;

    pin out float out;
    license "GPL";
    ;;
    #include <unistd.h>

    void user_mainloop(void) {
        while(1) {
            usleep(1000);
            FOR_ALL_INSTS() out = drand48();
        }
    }

### logic

Ce composant temps réel montre l’utilisation de la personnalité pour créer un tableau de taille variable et des pins optionnelles.

    component logic "Machinekit HAL component providing experimental logic functions";
    pin in bit in-##[16 : personality & 0xff];
    pin out bit and if personality & 0x100;
    pin out bit or if personality & 0x200;
    pin out bit xor if personality & 0x400;
    function _ nofp;
    description """
    Experimental general logic function component.  Can perform and, or
    and xor of up to 16 inputs.  Determine the proper value for personality
    by adding:
    .IP \\(bu 4
    The number of input pins, usually from 2 to 16
    .IP \\(bu
    256 (0x100)  if the and output is desired
    .IP \\(bu
    512 (0x200)  if the or output is desired
    .IP \\(bu
    1024 (0x400)  if the xor (exclusive or) output is desired""";
    license "GPL";
    ;;
    FUNCTION(_) {
        int i, a=1, o=0, x=0;
        for(i=0; i < (personality & 0xff); i++) {
            if(in(i)) { o = 1; x = !x; }
            else { a = 0; }
        }
        if(personality & 0x100) and = a;
        if(personality & 0x200) or = o;
        if(personality & 0x400) xor = x;
    }

Une ligne de chargement typique pourrait être:

        loadrt logic count=3 personality=0x102,0x305,0x503

qui créerait les pins suivantes:

-   Une porte AND à 2 entrées: logic.0.and, logic.0.in-00, logic.0.in-01

-   des portes AND et OR à 5 entrées: logic.1.and, logic.1.or, logic.1.in-00, logic.1.in-01, logic.1.in-02, logic.1.in-03, logic.1.in-04,

-   des portes AND et XOR à 3 entrées: logic.2.and, logic.2.xor, logic.2.in-00, logic.2.in-01, logic.2.in-02

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


