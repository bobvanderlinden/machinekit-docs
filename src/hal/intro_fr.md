Introduction à HAL
==================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:HAL-Introduction"></span>

HAL est le sigle de Hardware Abstraction Layer, le terme Anglais pour Couche d’Abstraction Matériel.<span class="footnote">
\[Note du traducteur: nous garderons le sigle HAL dans toute la documentation.\]
</span> Au plus haut niveau, il s’agit simplement d’une méthode pour permettre à un grand nombre de *modules* d'être chargés et interconnectés pour assembler un système complexe. La partie *matériel* devient abstraite parce que HAL a été conçu à l’origine pour faciliter la configuration de Machinekit pour une large gamme de matériels. Bon nombre de ces modules sont des pilotes de périphériques. Cependant, HAL peut faire beaucoup plus que configurer les pilotes du matériel.

HAL est basé sur le système d'étude des projets techniques
----------------------------------------------------------

HAL est basé sur le même principe que celui utilisé pour l'étude des circuits et des systèmes techniques, il va donc être utile d’examiner d’abord ces principes.

N’importe quel système, y compris les machines CNC, est fait de composants interconnectés. Pour les machines CNC, ces composants pourraient être le contrôleur principal, les amplis de servomoteurs, les amplis ou les commandes de puissance des moteurs pas à pas, les moteurs, les codeurs, les contacts de fin de course, les panneaux de boutons de commande, les manivelles électroniques, peut être aussi un variateur de fréquence pour le moteur de broche, un automate programmable pour gérer le changeur d’outils, etc. Le constructeur de machine doit choisir les éléments, les monter et les câbler entre eux pour obtenir un système complet et fonctionnel.

### Choix des organes

Il ne sera pas nécessaire au constructeur de machine de se soucier du fonctionnement de chacun des organes, il les traitera comme des boîtes noires. Durant la phase de conception, il décide des éléments qu’il va utiliser, par exemple, moteurs pas à pas ou servomoteurs, quelle marque pour les amplis de puissance, quels types d’interrupteurs de fin de course et combien il en faudra, etc. La décision d’intégrer tel ou tel élément spécifique plutôt qu’un autre, repose sur ce que doit faire cet élément et sur ses caractéristiques fournies par le fabricant. La taille des moteurs et la charge qu’ils doivent supporter affectera le choix des interfaces de puissance nécessaires pour les piloter. Le choix de l’ampli affectera le type des signaux de retour demandés ainsi que le type des signaux de vitesse et de position qui doivent lui être transmis.

Dans le monde de HAL, l’intégrateur doit décider quels composants de HAL sont nécessaires. Habituellement, chaque carte d’interface nécessite un pilote. Des composants supplémentaires peuvent être demandés, par exemple, pour la génération logicielle des impulsions d’avance, les fonctionnalités des automates programmables, ainsi qu’une grande variété d’autres tâches.

### Étude des interconnections

Le créateur d’un système matériel, ne sélectionnera pas seulement les éléments, il devra aussi étudier comment ils doivent être interconnectés. Chaque boîte noire dispose de bornes, deux seulement pour un simple contact, ou plusieurs douzaines pour un pilote de servomoteur ou un automate. Elles doivent être câblées entre elles. Les moteurs câblés à leurs interfaces de puissance, les fins de course câblés au contrôleur et ainsi de suite. Quand le constructeur de machine commence à travailler sur le câblage, il crée un grand plan de câblage représentant tous les éléments de la machine ainsi que les connections qui les relient entre eux.

En utilisant HAL, les *composants* sont interconnectés par des *signaux* . Le concepteur peut décider quels signaux sont nécessaires et à quoi ils doivent être connectés.

### Implémentation

Une fois que le plan de câblage est complet, il est possible de construire la machine. Les pièces sont achetées et montées, elles peuvent alors être câblées et interconnectées selon le plan de câblage. Dans un système physique, chaque interconnexion est un morceau de fil qui doit être coupé et raccordé aux bornes appropriées.

HAL fournit un bon nombre d’outils d’aide à la *construction* d’un système HAL. Certains de ces outils permettent de *connecter* (ou déconnecter) un simple *fil*. D’autres permettent d’enregistrer une liste complète des organes, du câblage et d’autres informations à propos du système, de sorte qu’il puisse être *reconstruit* d’une simple commande.

### Mise au point

Très peu de machines marchent bien dès la première fois. Lors des tests, le technicien peut utiliser un appareil de mesure pour voir si un fin de course fonctionne correctement ou pour mesurer la tension fournie aux servomoteurs. Il peut aussi brancher un oscilloscope pour examiner le réglage d’une interface ou pour rechercher des interférences électriques et déterminer leurs sources. En cas de problème, il peut s’avérer indispensable de modifier le plan de câblage, peut être que certaines pièces devront être re-câblées différemment, voir même remplacées par quelque chose de totalement différent.

HAL fournit les équivalents logiciels du voltmètre, de l’oscilloscope, du générateur de signaux et les autres outils nécessaires à la mise au point et aux réglages d’un système. Les même commandes utilisées pour construire le système, seront utilisées pour faire les changements indispensables.

### En résumé

Ce document est destiné aux personnes déjà capables de concevoir ce type de réalisation matérielle, mais qui ne savent pas comment connecter le matériel à Machinekit, par exemple pour une [commande distante](#sec:Exemple-Commande-Distante) telle que décrite dans la documentation de Halui.

La conception de matériel, telle que décrite précédemment, s’arrête à l’interface de contrôle. Au delà, il y a un tas de boîtes noires, relativement simples, reliées entre elles pour faire ce qui est demandé. À l’intérieur, un grand mystère, c’est juste une grande boîte noire qui fonctionne, nous osons l’espérer.

HAL étend cette méthode traditionnelle de conception de matériel à l’intérieur de la grande boîte noire. Il transforme les pilotes de matériels et même certaines parties internes du matériel, en petites boîtes noires pouvant être interconnectées, elles peuvent alors remplacer le matériel externe. Il permet au *plan de câblage* de faire voir une partie du contrôleur interne et non plus, juste une grosse boîte noire. Plus important encore, il permet à l’intégrateur de tester et de modifier le contrôleur en utilisant les mêmes méthodes que celles utilisées pour le reste du matériel.

Les termes tels que moteurs, amplis et codeurs sont familiers aux intégrateurs de machines. Quand nous parlons d’utiliser un câble extra souple à huit conducteurs blindés pour raccorder un codeur de position à sa carte d’entrées placée dans l’ordinateur. Le lecteur comprend immédiatement de quoi il s’agit et se pose la question, *quel type de connecteurs vais-je devoir monter de chaque côté de ce câble ?* Le même genre de réflexion est indispensable pour HAL mais le cheminement de la pensée est différent. Au début les mots utilisés par HAL pourront sembler un peu étranges, mais ils sont identiques au concept de travail évoluant d’une connexion à la suivante.

HAL repose sur une seule idée, l’idée d'étendre le plan de câblage à l’intérieur du contrôleur. Si vous êtes à l’aise avec l’idée d’interconnecter des boîtes noires matérielles, vous n’aurez sans doute aucune difficulté à utiliser HAL pour interconnecter des boites noires logicielles.

Concept de HAL
--------------

Cette section est un glossaire qui définit les termes clés de HAL mais il est différent d’un glossaire traditionnel en ce sens que les termes ne sont pas classés par ordre alphabétique. Ils sont classés par leur relation ou par le sens du flux à l’intérieur de HAL.

 Component   
(Composant) Lorsque nous avons parlé de la conception du matériel, nous avons évoqué les différents éléments individuels comme *pièces*, *modules*, *boîtes noires*, etc. L'équivalent HAL est un *component* ou *HAL component*. (ce document utilisera: *HAL component* quand la confusion avec un autre type de composant est possible, mais normalement, utilisez juste: *component*.) Un HAL component est une pièce logicielle avec, bien définis, des entrées, des sorties, un comportement, qui peuvent éventuellement être interconnectés.

 Parameter   
(Paramètre) De nombreux composants matériels ont des réglages qui ne sont raccordés à aucun autre composant mais qui sont accessibles. Par exemple, un ampli de servomoteur a souvent des potentiomètres de réglage et des points tests sur lesquels on peut poser une pointe de touche de voltmètre ou une sonde d’oscilloscope pour visualiser le résultat des réglages. Les HAL components aussi peuvent avoir de tels éléments, ils sont appelés *parameters*. Il y a deux types de paramètres: *Input parameters* qui sont des équivalents des potentiomètres. Ce sont des valeurs qui peuvent être réglées par l’utilisateur, elles gardent leur valeur jusqu'à un nouveau réglage. *Output parameters* qui ne sont pas ajustables. Ils sont équivalents aux points tests qui permettent de mesurer la valeur d’un signal interne.

 Pin   
(Broche) Les composants matériels ont des broches qui peuvent être interconnectées entre elles. L'équivalent HAL est une *pin* ou *HAL pin*. (*HAL pin* est utilisé quand c’est nécessaire pour éviter la confusion.) Toutes les HAL pins sont nommées et les noms des pins sont utilisés lors des interconnexions entre elles. Les HAL pins sont des entités logicielles qui n’existent qu'à l’intérieur de l’ordinateur.

 Physical\_Pin   
(Broche physique) La plupart des interfaces d’entrées/sorties ont des broches physiques réelles pour leur connexion avec l’extérieur, par exemple, les broches du port parallèle. Pour éviter la confusion, elles sont appelées *physical\_pins*. Ce sont des repères pour faire penser au monde physique réel. Vous vous demandez peut être quelle relation il y a entre les HAL\_pins, les Physical\_pins et les éléments extérieurs comme les codeurs ou une carte stg. Nous avons ici, affaire à des interfaces de type translation/conversion de données.

 Signal   
Dans une machine physique réelle, les terminaisons des différents organes sont reliées par des fils. L'équivalent HAL d’un fil est un *signal* ou *HAL signal*. Ces signaux connectent les *HAL pins* entre elles comme le requiert le concepteur de la machine. Les *HAL signals* peuvent être connectés et déconnectés à volonté (même avec la machine en marche).

 Type   
Quand on utilise un matériel réel, il ne viendrait pas à l’idée de connecter la sortie 24V d’un relais à l’entrée analogique +/-10V de l’ampli d’un servomoteur. Les *HAL pins* ont les même restrictions, qui sont fondées sur leur type. Les *pins* et les *signals* ont tous un type, un *signals* ne peux être connecté qu'à une *pins* de même type. Il y a actuellement les 4 types suivants:

-   bit - une simple valeur vraie ou fausse TRUE/FALSE ou ON/OFF

-   float - un flottant de 32 bits, avec approximativement 24 bits de résolution et plus de 200 bits d'échelle dynamique.

-   u32 - un entier non signé de 32 bits, les valeurs légales vont de 0 à +4,294,967,295

-   s32 - un entier signé de 32 bits, les valeurs légales vont de -2,147,483,648 à +2,147,483,647

 Function   
(Fonction) Les composants matériels réels ont tendance à réagir immédiatement à leurs signaux d’entrée. Par exemple, si la tension d’entrée d’un ampli de servo varie, la sortie varie aussi automatiquement. Les composants logiciels ne peuvent pas réagir immédiatement. Chaque composant a du code spécifique qui doit être exécuté pour faire ce que le composant est sensé faire. Dans certains cas, ce code tourne simplement comme une partie du composant. Cependant dans la plupart des cas, notamment dans les composants temps réel, le code doit être exécuté selon un ordre bien précis et à des intervalles très précis. Par exemple, les données en entrée doivent d’abord être lues avant qu’un calcul ne puisse être effectué sur elles et les données en sortie ne peuvent pas être écrites tant que le calcul sur les données d’entrée n’est pas terminé. Dans ces cas, le code est confié au système sous forme de *functions*. Chaque *function* est un bloc de code qui effectue une action spécifique. L’intégrateur peut utiliser des *threads* pour combiner des séries de *functions* qui seront exécutées dans un ordre particulier et selon des intervalles de temps spécifiques.

 Thread   
(Fil) Un *thread* est une liste de *functions* qui sont lancées à intervalles spécifiques par une tâche temps réel. Quand un *thread* est créé pour la première fois, il a son cadencement spécifique (période), mais pas de *functions*. Les *functions* seront ajoutées au *thread* et elle seront exécutées dans le même ordre, chaque fois que le *tread* tournera.

Prenons un exemple, supposons que nous avons un composant de port parallèle nommé *hal\_parport*. Ce composant défini une ou plusieurs *HAL pins* pour chaque *physical pin*. Les *pins* sont décrites dans ce composant, comme expliqué dans la section *component* de cette doc, par: leurs noms, comment chaque *pin* est en relation avec la *physical pin*, est-elle inversée, peut-on changer sa polarité, etc. Mais ça ne permet pas d’obtenir les données des *HAL pins* aux *physical pins*. Le code est utilisé pour faire ça, et c’est la où les *functions* entrent en œuvre. Le composant parport nécessite deux *functions*: une pour lire les broches d’entrée et mettre à jour les *HAL pins*, l’autre pour prendre les données des *HAL pins* et les écrire sur les broches de sortie *physical pins*. Ce deux fonctions font partie du pilote *hal\_parport*.

Composants HAL
--------------

Chaque composant HAL est un morceau de logiciel avec, bien définis, des entrées, des sorties et un comportement. Ils peuvent être installés et interconnectés selon les besoins. Cette section liste certains des composants actuellement disponibles et décrit brièvement ce que chacun fait. Les détails complets sur chacun seront donnés plus loin dans ce document.

Programmes externes attachés à HAL
----------------------------------

 motion   
Un module temps réel qui accepte les commandes de mouvement en NML et inter-agit avec HAL

 iocontrol   
Un module d’espace utilisateur qui accepte les commandes d’entrée/sortie (I/O) en NML et inter-agit avec HAL

 classicladder   
Un automate programmable en langage à contacts utilisant HAL pour les entrées/sorties (I/O)

 halui   
Un espace de utilisateur de programmation qui inter-agit avec HAL et envoie des commandes NML, Il est destiné à fonctionner comme une interface utilisateur en utilisant les boutons et interrupteurs externes.

Composants internes
-------------------

 stepgen   
Générateur d’impulsions de pas avec boucle de position. Plus de détails [sur stepgen](#sec:Stepgen).

 encoder   
Codeur/compteur logiciel. Plus de détails [sur le codeur](#sec:Codeur).

 pid   
Boucle de contrôle Proportionnelle/Intégrale/Dérivée. Plus de détails [sur le PID](#sec:PID).

 siggen   
Générateur d’ondes: sinusoïdale/cosinusoïdale/triangle/carrée, pour la mise au point. Plus de détails [sur siggen](#sec:Siggen).

 supply   
Une simple alimentation, pour la mise au point

 blocks   
Un assortiment de composants (mux, demux, or, and, integ, ddt, limit, wcomp, etc.)

Pilotes de matériels
--------------------

 hal\_ax5214h   
Un pilote pour la carte d’entrées/sorties Axiom Measurement & Control AX5241H

 hal\_m5i20   
Un pilote pour la carte Mesa Electronics 5i20

 hal\_motenc   
Un pilote pour la carte Vital Systems MOTENC-100

 hal\_parport   
Pilote pour le(ou les) port(s) parallèle(s). Plus de détails sur les [ports parallèles](#cha:Parport).

 hal\_ppmc   
Un pilote pour la famille de contrôleurs Pico Systems (PPMC, USC et UPC)

 hal\_stg   
Un pilote pour la carte Servo To Go (versions 1 & 2)

 hal\_vti   
Un pilote pour le contrôleur Vigilant Technologies PCI ENCDAC-4

Outils-Utilitaires
------------------

 halcmd   
Ligne de commande pour la configuration et les réglages.

 halgui   
Outil graphique pour la configuration et les réglages. (pas encore implémenté).

 halmeter   
Un multimètre pour les signaux HAL. Plus de détails pour utiliser [halmeter](#sec:Tutoriel-halmeter).

 halscope   
Un oscilloscope digital à mémoire, complétement fonctionnel pour les signaux HAL.

Chacun de ces modules est décrit en détail dans les chapitres suivants.

Les réflexions qui ont abouti à la création de HAL
--------------------------------------------------

Cette première introduction au concept de HAL peut être un peu déconcertante pour l’esprit. Construire quelque chose avec des blocs peut être un défi, pourtant certains jeux de construction avec lesquels nous avons joué étant enfants peuvent nous aider à construire un système HAL.

### Une tour

-   Je regardais mon fils et sa petite fille de six ans construire une tour à partir d’une boîte pleine de blocs de différentes tailles, de barres et de pièces rondes, des sortes de couvercles. L’objectif était de voir jusqu’où la tour pouvait monter. Plus la base était étroite plus il restait de pièces pour monter. Mais plus la base était étroite, moins la tour était stable. Je les voyais étudier combien de blocs ils pouvaient poser et où ils devaient les poser pour conserver l'équilibre avec le reste de la tour.

-   La notion d’empilage de cartes pour voir jusqu’où on peut monter est une très vieille et honorable manière de passer le temps. En première lecture, l’intégrateur pourra avoir l’impression que construire un HAL est un peu comme ça. C’est possible avec une bonne planification, mais l’intégrateur peut avoir à construire un système stable aussi complexe qu’une machine actuelle l’exige.

### Erector Sets <span class="footnote">
\[Le jeu Erector Set est une invention de AC Gilbert (Meccano en France)\]
</span>

C'était une grande série de boites de construction en métal, des tôles perforées, plates ou en cornières, toutes avaient des trous régulièrement espacés. Vous pouviez concevoir des tas de choses et les monter avec ces éléments maintenus entre eux par des petits boulons.

J’ai eu ma première boîte Erector pour mon quatrième anniversaire. Je sais que la boîte était prévue pour des enfants beaucoup plus âgés que moi. Peut être que mon père se faisait vraiment un cadeau à lui même. J’ai eu une période difficile avec les petites vis et les petits écrous. J’ai vraiment eu envie d’avoir quatre bras, un pour visser avec le tournevis, un pour tenir la vis, les pièces et l'écrou. En persévérant, de même qu’en agaçant mon père, j’ai fini par avoir fait tous les montages du livret. Bientôt, je lorgnais vers les plus grandes boîtes qui étaient imprimées sur ce livret. Travailler avec ces pièces de taille standard m’a ouvert le monde de la construction et j’ai bientôt été au delà des projets illustrés.

Les composants Hal ne sont pas tous de même taille ni de même forme mais ils permettent d'être regroupés en larges unités qui feront bien du travail. C’est dans ce sens qu’ils sont comme les pièces d’un jeu Erector. Certains composants sont longs et minces. Ils connectent essentiellement les commandes de niveau supérieur aux *physical pins*. D’autres composants sont plus comme les plateformes rectangulaires sur lesquelles des machines entières pourraient être construites. Un intégrateur parviendra rapidement au delà des brefs exemples et commencera à assembler des composants entre eux d’une manière qui lui sera propre.

### Tinkertoys <span class="footnote">
\[Tinkertoy est maintenant registered trademark of the Hasbro company.\]
</span>

Le jouet en bois Tinkertoys est plus humain que l’acier froid de l’Erector. Le cœur de la construction avec TinkerToys est un connecteur rond avec huit trous équidistants sur la circonférence. Il a aussi un trou au centre, perpendiculaire aux autres trous répartis autour du moyeu.

Les moyeux pouvaient être connectés avec des tiges rondes de différentes longueurs. Le constructeur pouvait faire une grosse roue à l’aide de rayons qui partaient du centre.

Mon projet favori était une station spatiale rotative. De courtes tiges rayonnaient depuis les trous du moyeu central et étaient connectées avec d’autres moyeux aux extrémités des rayons. Ces moyeux extérieurs étaient raccordés entre eux avec d’autres rayons. Je passais des heures à rêver de vivre dans un tel dispositif, marchant de moyeu en moyeu et sur la passerelle extérieure qui tournait lentement à cause de la gravité dans l’espace en état d’apesanteur. Les provisions circulaient par les rayons et les ascenseur qui les transféraient dans la fusée arrimée sur le rayon central pendant qu’on déchargeait sa précieuse cargaison.

L’idée qu’une *pin* ou qu’un *component* est la plaque centrale pour de nombreuses connections est aussi une notion facile avec le HAL. Les exemples deux à quatre connectent le multimètre et l’oscilloscope aux signaux qui sont prévus pour aller ailleurs. Moins facile, la notion d’un moyeu pour plusieurs signaux entrants. Mais, c’est également possible avec l’utilisation appropriée des fonctions dans ce composant de moyeu qui manipulent les signaux quand ils arrivent, venant d’autres composants.

Tous les détails dans le [tutoriel de HAL](#cha:Tutoriel-HAL).

Une autre réflexion qui vient à partir de ce jouet mécanique est une représentation de *HAL threads*. Un *thread* pourrait ressembler un peu à un chilopode, une chenille, ou un perce-oreille. Une épine dorsale, des *HAL components*, raccordés entre eux par des tiges, les *HAL signals*. Chaque composant prend dans ses propres paramètres et selon l'état de ses broches d’entrée, les passe sur ses broches de sortie à l’intention du composant suivant. Les signaux voyagent ainsi de bout en bout, le long de l'épine dorsale où ils sont ajoutés ou modifiés par chaque composant son tour venu.

Les *Threads* sont tous synchronisés et exécutent une série de tâches de bout en bout. Une représentation mécanique est possible avec Thinkertoys si on pense à la longueur du jouet comme étant la mesure du temps mis pour aller d’un bout à l’autre. Un thread, ou épine dorsale, très différent est créé en connectant le même ensemble de modules avec des tiges de longueur différente. La longueur totale de l'épine dorsale peut aussi être changée en jouant sur la longueur des tiges pour connecter les modules. L’ordre des opérations est le même mais le temps mis pour aller d’un bout à l’autre est très différent.

Un exemple en Lego <span class="footnote">
\[The Lego name is a trademark of the Lego company.\]
</span>
-----------------------------------------------------

Lorsque les blocs de Lego sont arrivés dans nos magasins, ils étaient à peu près tous de la même taille et de la même forme. Bien sûr il y avait les demi taille et quelques uns en quart de taille mais tous rectangulaires. Les blocs de Lego se relient ensembles en enfonçant les broches mâles d’une pièce dans les trous femelles de l’autre. En superposant les couches, les jonctions peuvent être rendues très solides, même aux coins et aux tés.

J’ai vu mes enfants et mes petits-enfants construire avec des pièces Lego (les mêmes Lego). Il y en a encore quelques milliers dans une vieille et lourde boîte en carton qui dort dans un coin de la salle de jeux. Ils sont stockés dans cette boîte car c'était trop long de les ranger et de les ressortir à chacune de leur visite et ils étaient utilisés à chaque fois. Il doit bien y avoir les pièces de deux douzaines de boîtes différentes de Lego. Les petits livrets qui les accompagnaient ont été perdus depuis longtemps, mais la magie de la construction avec l’imbrication de ces pièces toutes de la même taille est quelque chose à observer.

Problèmes de timming dans HAL
-----------------------------

Contrairement aux modèles physiques du câblage entre les boîtes noires sur lequel, nous l’avons dit, HAL est basé, il suffit de relier deux broches avec un signal hal, on est loin de l’action physique.

La vraie logique à relais consiste en relais connectés ensembles, quand un relais s’ouvre ou se ferme, le courant passe (ou s’arrête) immédiatement. D’autres bobines peuvent changer d'état etc. Dans le style langage à contacts d’automate comme le Ladder ça ne marche pas de cette façon. Habituellement dans un Ladder simple passe, chaque barreau de l'échelle est évalué dans l’ordre où il se présente et seulement une fois par passe. Un exemple parfait est un simple Ladder avec un contact en série avec une bobine. Le contact et la bobine actionnent le même relais.

Si c'était un relais conventionnel, dès que la bobine est sous tension, le contact s’ouvre et coupe la bobine, le relais retombe etc. Le relais devient un buzzer.

Avec un automate programmable, si la bobine est OFF et que le contact est fermé quand l’automate commence à évaluer le programme, alors à la fin de la passe, la bobine sera ON. Le fait que la bobine ouvre le contact qui la prive de courant est ignoré jusqu'à la prochaine passe. À la passe suivante, l’automate voit que le contact est ouvert et désactive la bobine. Donc, le relais va battre rapidement entre on et off à la vitesse à laquelle l’automate évalue le programme.

Dans HAL, c’est le code qui évalue. En fait, la version Ladder HAL temps réel de Classic Ladder exporte une fonction pour faire exactement cela. Pendant ce temps, un thread exécute les fonctions spécifiques à intervalle régulier. Juste comme on peut choisir de régler la durée de la boucle de programme d’un automate programmable à 10ms, ou à 1 seconde, on peut définir des *HAL threads* avec des périodes différentes.

Ce qui distingue un thread d’un autre n’est pas ce qu’il fait mais quelles fonctions lui sont attachées. La vraie distinction est simplement combien de fois un thread tourne.

Dans Machinekit on peut avoir un thread à 50μs et un thread à 1ms. En se basant sur les valeurs de BASE\_PERIOD et de SERVO\_PERIOD. Valeurs fixées dans le fichier ini.

La prochaine étape consiste à décider de ce que chaque thread doit faire. Certaines de ces décisions sont les mêmes dans (presque) tous les systèmes Machinekit. Par exemple, le gestionnaire de mouvement est toujours ajouté au servo-thread.

D’autres connections seront faites par l’intégrateur. Il pourrait s’agir de brancher la lecture d’un codeur par une carte STG à un DAC pour écrire les valeurs dans le servo thread, ou de brancher une fonction stepgen au base-thread avec la fonction parport pour écrire les valeurs sur le port.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


