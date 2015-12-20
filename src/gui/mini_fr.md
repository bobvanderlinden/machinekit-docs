L’interface graphique MINI
==========================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Mini"></span>

Introduction
------------

<span class="footnote">
\[Une grande partie de ce chapitre provient du manuel Sherline CNC operators.\]
</span>

![](images/mini01_fr.png)

Figure 1. L’interface graphique Mini

Mini a été prévu pour être une interface graphique plein écran. Il a été écrit initialement pour Sherline CNC mais est disponible pour ceux qui veulent l’utiliser, le copier le distribuer selon les termes de la GPL.

Au lieu d’ouvrir une nouvelle fenêtre pour chaque chose que l’opérateur veux faire, Mini permet de les afficher toutes dans la même fenêtre. Certaines parties de ce chapitre sont des copies des instructions écrites par Joe Martin et Ray Henry.

L'écran
-------

Affichage de Mini lors du fonctionnement de Machinekit

<span class="image"> ![](images/mini_fr.png) </span>

L'écran de Mini est visible dans plusieurs sections, voir la figure ci-dessus. Il inclu une barre de menu, un jeu de boutons de contrôle juste en dessous et deux larges colonnes d’information montrant l'état de la machine et permettant à l’opérateur d’entrer des commandes ou des programmes.

Quand on compare [entre l'écran d’accueil](#fig:startminif) et [l'écran en cours de fonctionnement](#fig:runminif), des différences apparaissent. Sur la seconde image:

-   Chaque axe a été référencé sur son origine — L’affichage de la visu est vert sombre

-   Machinekit est en mode auto — le bouton est vert clair

-   Le parcours d’outil a été activé — le parcours est visible dans la fenêtre

-   Le bloc de G-code en cours est en surbrillance blanche sur fond rouge parmi les lignes défilantes du programme.

Dès le début de l’utilisation de Mini on découvre combien il est facile de visualiser le fonctionnement de Machinekit et d’agir sur lui.

Barre de menu
-------------

La première ligne est occupée par la barre de menu. Elle permet de configurer les informations optionnelles que l'écran doit afficher. Certains des items du menu sont très différents de ce qu’il est coutume de rencontrer avec d’autres programmes. Il faut prendre quelques minutes pour regarder chaque item du menu et se familiariser avec les possibilités qu’il offre.

Le menu inclus chacune des sections et sous-sections suivantes:

 Programme   
ce menu inclus les deux fonctions *reset* et *quitter*. Reset renvoie Machinekit dans l'état dans lequel il était quand il à démarré. Certains items de conditions de démarrage comme les unités du programme peuvent être spécifiés dans le fichier ini.

 Vues   
ce menu inclus plusieurs éléments d’affichage pouvant être ajoutés pendant qu’un programme s’exécute, pour donner des information supplémentaires. Ce qui inclus:

 Type\_de\_position   
cet item ajoute une ligne, au dessus de l’afficheur principal de position, qui indique si la position affichée est en coordonnées machine (absolues) ou en coordonnées pièce (relatives), si la position affichée est actuelle ou commandée et si l’unité et le pouce le millimètre. Ce qui peut être modifié en agissant sur le menu *Réglages* décrit plus loin.

 Informations\_outil   
cet item ajoute une ligne immédiatement sous l’afficheur principal, elle affiche quel outil est selectionné et la valeur de l’offset appliqué.

 Informations\_offset   
ajoute une ligne immédiatement sous celle des informations outil, elle affiche les offsets de travail actuellement appliqués. C’est la distance totale pour chaque axe depuis son point d’origine machine.

 Afficher\_Reprise   
ajoute, en mode Auto, un bloc de boutons en bas à droite de l’affichage du programme. Ces boutons permettent à l’opérateur de relancer un programme après un A/U ou un Abandon. Le programme est figé lorsqu’un A/U ou un Abandon est engagé, mais l’opérateur peut ici choisir le bloc sur lequel redémarrer dans le mode Auto, en activant cet item de menu.

 Masquer\_Reprise   
enlève le bloc de boutons contrôlant la reprise d’un programme stoppé par Abandon ou Arrêt d’Urgence.

 Diviser\_la\_vue\_droite   
modifie la nature de l’affichage de la colonne de droite pour qu’il montre plus d’informations.

 Afficher\_en\_mode\_complet   
modifie l’affichage de la colonne de droite, soit en mode boutons et la colonne divisée, soit affichage sur la colonne entière. En mode manuel et en utilisant ce mode entier, les boutons de la broche et du lubrifiant sont visibles en plus des boutons de mouvement.

 Afficher\_dans\_toute\_la\_fenêtre   
modifie l’affichage dans la colonne de droite pour qu’elle soit remplie entièrement.

 Réglages   
Les items de ce menu permettent à l’opérateur de contrôler certains paramètres en cours d’usinage.

 Position\_Actuelle   
régle l’afficheur principal sur des valeurs de position actuelle (basées sur la machine).

 Position\_Commandée   
régle l’afficheur principal sur des valeurs de position commandée (positions cibles).

 Position\_Machine   
régle l’afficheur principal sur des valeurs de position absolue, distance depuis le point d’origine machine.

 Position\_Relative   
régle l’affichage principal pour qu’il affiche les valeurs de la position courante en incluant tous les offsets comme les origines pièces qui seraient actives. Pour plus de détails voir le chapitre sur les *Systèmes de coordonnées*.

 Informations   
Indique diverses activités en affichant des valeurs dans la zône MESSAGES.

 Fichier\_programme   
Indique le nom du fichier programme actif.

 Editeur\_de\_Fichiers   
Indique le nom de l'éditeur de texte lancé quand un fichier est choisi pour être édité.

 Fichier\_de\_paramètres   
Indique le nom du fichier devant être utilisé pour enregistrer les paramètres du programme. Il est possible d’avoir plus d’informations dans le chapitre sur les offsets et l’utilisation des paramètres dans les programmes.

 Fichiers\_d’outils   
Indique le nom du fichier d’outils utilisé pour l’usinage en cours.

 G-Codes\_actifs   
Indique la liste des G-codes modaux actifs au moment ou cet item a été sélectionné. Pour plus d’informations sur les codes modaux, se reporter aux chapitres sur la programmation.

 Aide   
Ouvre une fenêtre de texte contenant un fichier d’aide.

Noter que entre le menu informations et le menu d’aide il y a un jeu de quatre cases à cocher. Elles sont appelées cases sélectionnables car elles passent au rouge si elles sont sélectionnées. Ces quatre cases, Editeur, Parcours d’outil, Outils et Offsets activent les différents écrans. Si plus d’une de ces cases est validée (case en rouge) il est possible de passer de l’un à l’autre des écrans surgissant par un clic droit de la souris.

Barre de boutons de contrôle
----------------------------

Sous la ligne de menu se trouve une ligne de boutons de contrôle. Ce sont les boutons principaux de l’interface. En utilisant ces boutons il est possible de changer de mode cycliquement entre *MANUEL* puis *AUTO* puis *MDI* (Manual Data Input). Ces boutons ont un fond vert clair quand le mode correspondant est actif.

Il est également possible d’utiliser les boutons *VITESSE-0*, *ABANDON* et *A/U* pour contrôler les mouvements du programme.

### MANUEL

Ce bouton, ou la touche **F3** placent Machinekit en mode Manuel et affiche un jeu condensé de boutons que l’opérateur peut utiliser pour effectuer des commandes de mouvements manuels. Les labels des boutons de jog varient pour refléter l’axe actif. Quand en plus de ce mode manuel, la vue en mode complet est activée, les boutons de la broche et du lubrifiant sont visibles en plus de ceux de mouvement. La touche **i** ou **I** basculera entre jog en continu et jog par incréments. Presser cette touche plusieurs fois provoquera le changement cyclique de la taille de l’incrément de jog.

![](images/miniman_fr.png)

Figure 2. Boutons du mode Manuel

Un bouton à été ajouté pour désigner la position actuelle comme étant la position d’origine. Une machine simple telle que la Sherline 5400 serait facile à utiliser sans avoir de position d’origine. Ce bouton met les offsets de tous les axes à zéro et place l’origine de tous les axes au point courant.

Ce concentrer sur les axes est important ici. Noter que sur la capture [du début de chapitre](#fig:startminif) qui est en mode manuel, on voit un cadre autour de l’axe X pour mettre son affichage en évidence. Ce cadre (le focus) indique que l’axe X est l’axe actif. Il est l’axe cible pour les mouvements de jog faits par appui sur les boutons *plus* ou *moins*. Il est possible de changer l’axe actif en cliquant sur l’affichage d’un autre axe. C’est également possible en mode manuel en pressant la touche de son nom sur le clavier. La casse n’a pas d’importance **Y** ou **y** donneront le focus à l’axe Y. **A** ou **a** le donneront à l’axe A. Pour aider à se rappeller quel est l’axe actif avant de faire un jog, son nom est indiqué sur les boutons de jog.

Machinekit peut faire un jog (mouvement d’un axe particulier) aussi longtemps que le bouton est maintenu pressé, si le jog est réglé sur *continu* , ou il peut se déplacer d’une valeur prédéfinie, quand il est réglé sur *incrément* s. Il est aussi possible de faire un jog de l’axe actif en pressant les touches **+** ou moins **-** du clavier. De nouveau, la casse n’a pas d’importance pour les jogs au clavier. Les deux petits boutons situés entre les deux gros du jog, permettent de choisir quel type de jog est souhaîté. Quand on est en mode incréments, les boutons de taille d’incréments sont accessibles. La sélection d’une taille d’incrément se fait par clic sur la case à cocher avec la souris ou cycliquement en pressant la touche **i** ou **I** sur le clavier. Le jog par incréments présente quelques effets intéressants autant qu’inattendus. Si vous pressez le bouton de jog alors qu’un mouvement de jog est déjà en cours, la distance à laquelle il était lorsqu’est arrivée la commande du second jog, sera ajoutée à la position. Deux pressions successives rapides de 10mm d’incrément ne donneront pas 20mm de mouvement. Vous devez attendre que le premier soit terminé avant d’envoyer le second.

La vitesse de jog est affichée au dessus du curseur. Il est possible de régler le curseur en cliquant dans la glissière du curseur, du côté où vous voulez son déplacement, ou en cliquant sur les boutons *Défaut* ou *Rapide*. Ces réglages n’affectent que les mouvements de jog en mode manuel. Tant qu’un mouvement de jog est en cours, un changement de la vitesse de jog est sans effet sur le jog. Par exemple, disons que vous avez réglé le jog par incréments de 10mm. Même si vous pressez le bouton *Jog*, le déplacement de dix millimètres se finira à la vitesse à laquelle il a commencé.

### AUTO

Quand le bouton Auto est pressé, ou **F4** sur le clavier, Machinekit passe dans ce mode, un jeu de boutons traditionnels des opérations en auto est affiché et une petite fenêtre textuelle s’ouvre, montrant une partie du programme. Durant un usinage, la ligne active est affichée en surbrillance blanche sur fond rouge.

Dans le mode auto, beaucoup de touches sont liées aux contrôles. Par exemple, les touches numérotées au dessus du clavier sont liées aux réglages du correcteur de vitesse d’avance travail. Le **0** l’ajuste à 100%, le **9** l’ajuste à 90% etc. D’autres touches fonctionnent de la même manière qu’avec l’interface TkLinuxcnc.

![](images/miniauto_fr.png)

Figure 3. Mode Auto

Le mode Auto n’affiche normalement pas les G-codes actifs ou modaux. Si l’opérateur veut les voir, il doit utiliser le menu *Informations → G-codes actifs* et afficher ainsi la liste des codes modaux dans la zône de texte MESSAGES.

Si un Arrêt d’Urgence ou un Abandon est pressé pendant un usinage, un jeu de boutons s’affiche en bas et à droite, ils permettent à l’opérateur de décaler la ligne de reprise vers l’avant ou vers l’arrière. Si la ligne de reprise n’est pas la dernière ligne active, elle sera mise en surbrillance blanche sur fond bleu. ATTENTION, une vitesse très faible et un doigt collé sur le bouton de pause est prudent pendant toute reprise de programme!

Ce qui est le mieux avec une machine CNC, c’est le mode auto. Le mode auto affiche les fonctions typiques que tout le monde espère utiliser avec Machinekit. Au dessus de la fenêtre un jeu de boutons qui contrôlent ce qui se passe en mode auto. En dessous, la fenêtre montrant la partie du programme en cours d’exécution. Quand le programme est lancé, la ligne active s’affiche en surbrillance blanche sur fond rouge. Les trois premiers boutons, *Ouvrir*, *Lancer* et *Pause* font ce que leurs noms indiquent. *Pause* stoppe le programme en cours d’exécution là où il est. Le bouton suivant, *Reprise*, reprend le mouvement. Le résultat est le même si le bouton *Vitesse-0* était pressé au lieu du bouton *Pause*, le mouvement est stoppé, *Pas à pas* reprend aussi le mouvement, mais il ne continue que jusqu'à la fin du bloc courant. Presser une nouvelle fois *Pas à pas* exécutera le mouvement du bloc suivant. Presser *Reprise* à ce moment là et l’interpréteur reviendra en arrière lire et relancer le programme. La combinaison entre *Pause* et *Pas à pas* marche un peu comme un seul bloc dans plusieurs interpréteurs. Avec la différence que *Pause* ne laisse pas le mouvement se poursuivre jusqu'à la fin du bloc courant. Le correcteur de vitesse d’avance travail peut se révéler très pratique pour s’approchez de la matière pour un premièr usinage. Le placer sur 100% pour les déplacements rapides, le régler sur 10% et basculez entre *Vitesse-0* et 10% en utilisant le bouton *Pause*. Quand vous êtes satisfait et que les choses se présentent bien, pressez le zéro à la droite du neuf en haut du clavier, et c’est parti.

Le bouton *Vérifier* passe le code dans l’interpréteur sans production de mouvements. Si Vérifier trouve un problème il stoppe la lecture juste derrière le bloc posant problème et affiche un message. La plupart du temps vous serez en mesure de régler le problème avec votre programme par la lecture du message et en vérifiant la ligne de code en surbrillance dans la fenêtre du programme. Certains messages, toutefois, ne sont pas d’un grand secours. Parfois vous devrez lire une ou deux lignes en avant de celle en surbrillance pour voir le problème. Occasionnellement le message fait référence à quelque chose très en avant de la ligne en surbrillance. Le plus souvent ça se produit si vous oubliez de terminer votre programme par un code correct comme %, M2, M30, ou M60.

### MDI<span id="sub:MDI"></span>

Le bouton MDI ou la touche **F5** activent le mode d’entrée de données en manuel (Manual Data Input). Ce mode affiche un simple champ de saisie d’une ligne de texte et montre les codes modaux actuellement actifs dans l’interpréteur.

Le mode MDI vous permet d’entrer de simples blocs et de les faire exécuter par l’interpréteur comme si ils étaient une partie d’un programme (Une sorte de programme d’une seule ligne). Vous pouvez exécuter des cercles, des arcs, des lignes et autres. Vous pouvez aussi mettre au point une ligne de programme en entrant cette ligne comme un seul bloc, attendre que le mouvement se termine et entrer le bloc suivant. Sous la fenêtre d’entrée, se trouve une liste des codes modaux courants. Cette liste peut être très pratique. J’oublie parfois d’entrer un G0 avant une commande de mouvement. Si rien ne se passe je regarde dans la liste si G80 est actif. G80 stoppe tous les mouvements. Si il y est je me rappelle de mettre un bloc comme G00 X0 Y0 Z0. Dans le MDI, vous entrez du texte depuis le clavier, ainsi toutes les touches principales ne fonctionnent pas comme raccourcis clavier pour les commandes machine. **F1** engage l’Arrêt d’Urgence.

Puisque les touches du clavier sont nécessaires à la saisie du texte, beaucoup des raccourcis clavier disponibles en mode auto ne le sont pas ici.

### VITESSE-0 et CONTINUER

Vitesse-0 est une bascule. Quand Machinekit est prêt pour exécuter, ou qu’il exécute une commande de mouvement, ce bouton affiche son label *VITESSE-0* sur fond rouge. Si Vitesse-0 a été pressé il affiche le label *CONTINUER*. Utiliser ce bouton pour faire une pause dans un mouvement présente l’avantage d'être capable de relancer le programme d’où il a été stoppé. Vitesse-0 bascule simplement entre vitesse zéro et la vitesse d’avance travail avec l'éventuel correcteur qui était actif au moment de l’arrêt. Ce bouton et la fonction qu’il active sont également liés à la touche pause de la plupart des claviers.

### ABANDON

Le bouton Abandon stoppe tous les mouvements quand il est pressé. Il désactive aussi la commande de marche de Machinekit. Plus aucun mouvement ne survient après l’appui sur ce bouton. Si vous êtes en mode auto, ce bouton enlève le reste du programme du planificateur de mouvements. Il enregistre aussi le numéro de la ligne qui s’exécutait quand il a été pressé. Vous pouvez vous servir de ce numéro de ligne pour redémarrer le programme après avoir supprimé la raison de l’appui…

### Arrêt d’Urgence

Le bouton d’Arrêt d’Urgence est une bascule mais il a trois fonctionnements possibles.

-   Au démarrage de Mini c’est un bouton avec le texte *A/U* écrit en noir sur fond rouge. C’est état de la machine est correct pour charger un programme ou faire un jog sur un axe. L’Arrêt d’Urgence est libéré quand il s’affiche dans cet état.

-   Si vous pressez sur le bouton d’Arrêt d’Urgence pendant qu’un mouvement est exécuté, le texte sur le bouton devient *A/U Engagé* sur fond gris et le bouton s’enfonce. Plus aucun mouvement n’est possible et plus rien ne réagi sur l’interface Mini tant que l’Arrêt d’Urgence est dans cet état. Le presser à nouveau à la souris le fera repasser en conditions normales de fonctionnement.

-   Un troisième état est encore possible. Un bouton enfoncé portant le texte *A/U Libéré* sur fond vert signifie que l’A/U a bien été libéré mais que la machine n’a pas été mise en marche. Normalement cet état apparaît quand l’A/U était libéré mais que la touche Marche Machine **F2** a été pressée.

Joe Martin disait, *Quand tout le reste a échoué, pressez un Arrêt d’Urgence software*. Si vous avez un circuit externe qui gère l’A/U en surveillant une broche du port parallèle ou celle d’une carte d’entrées/sorties, un arrêt d’urgence software pourra couper la puissance sur les moteurs.

La plupart du temps, quand un Abandon ou un Arrêt d’Urgence est engagé c’est parce que quelque chose se passe mal. Peut être un outil cassé et qui doit être changé. On passe alors en mode manual et on arrête la broche, on change l’outil et en supposant que sa longueur reste la même, on est prêt pour relancer le programme. Si on renvoie l’outil à la même place ou il était avant, Machinekit va fonctionner parfaitement. Il est aussi possible de se déplacer sur la ligne suivante ou précédente de celle ou c’est produit l’abandon. Si vous pressez le bouton *Arrière* ou *Avant* vous voyez une ligne en surbrillance bleue montrant l'écart entre la ligne sur laquelle l’abandon s’est produit (restée en surbrillance rouge) et la ligne à laquelle Machinekit va redémarrer. En réfléchissant à ce qui va se produire au moment de la reprise vous serez en mesure de placer l’arête de l’outil là où la reprise pourra se faire de manière acceptable. Vous aurez peut être à solutionner certaines difficultés comme celles créées par les compensations de rayon d’outil le long d’une ligne diagonale et vous devrez être sûr de vous avant de presser sur le bouton *Reprise*.

Colonne de gauche
-----------------

Il y a deux colonnes sous la barre de contrôle. La colonne de gauche affiche les informations intéressant l’opérateur. Il y a seulement deux boutons dans cette zône.

### Afficheurs de position des axes

Ces afficheurs se comportent exactement comme ceux de TkeMachinekit. La couleur des afficheurs est importante.

-   Rouge, elle indique que la machine est en appui sur un contact de fin de course ou que la polarité d’une limite est mal positionnée dans le fichier ini.

-   Jaune, elle indique que la machine est prête pour faire ses prises d’origine.

-   Verte, elle indique que la machine a bien été référencée sur ses points d’origine.

Le type de position affichée peut varier, selon les options choisies dans le menus *Réglages*. Les réglages par défaut, ou de démarrage, peuvent être changés dans le fichier ini pour correspondre à vos besoins.

### Correcteur de vitesse travail

Immédiatement sous les afficheurs de position on trouve un curseur, c’est le correcteur de vitesse travail. Vous pouvez agir sur le correcteur de vitesse et sur le bouton Vitesse-0 dans tous les modes de marche. Le correcteur agit sur la vitesse de jog et sur la vitesse d’avance travail dans les modes manuel ou MDI. Il est possible de modifier la position du curseur en le déplaçant à la souris le long de sa glissière. Il est également possible de modifier le correcteur de 1% à chaque fois qu’un clic de souris est fait dans la glissière du curseur. En mode Manuel il est possible d’ajuster le correcteur par incréments de 10% avec les touches chiffrées du haut du clavier. Le curseur est une référence visuelle très pratique pour estimer la correction appliquée sur la vitesse d’avance programmée.

### Messages

L’affichage des messages situé sous le curseur du correcteur de vitesse est une sorte de bloc-notes pour Machinekit. Si un problème survient, il est reporté sur ce bloc-notes. Si vous essayez de déplacer un axe alors que l’Arrêt d’Urgence est engagé, vous recevez un message qui dit quelques choses à propos des conditions de marche empêchant Machinekit de répondre à la commande de mouvement. Si un axe est en défaut, par exemple un dépassement de limite, le message affiché sur le bloc-notes indiquera ce qui se passe. Pour demander à l’opérateur de changer d’outil par exemple, vous pouvez aussi ajouter une ligne de code dans le programme qui s’affichera sur l'écran, dans la boîte de messages. Un exemple pourrait être: (msg, Montez l’outil N°3 puis pressez Reprise). Cette ligne de code, incluse dans un programme, va afficher *Montez l’outil 3 puis pressez Reprise* dans la boîte de messages. Le mot msg, (avec la virgule) est la commande qui fait apparaître le commentaire, sans *msg*, le message ne serait pas affiché.

Pour effacer les messages cliquer simplement sur le bandeau --MESSAGES-- au dessus du bloc-notes ou au clavier, presser **m** tout en maintenant la touche *Alt* appuyée.

Colonne de droite
-----------------

La colonne de droite est l’emplacement sur lequel seront affichés les différents éléments résultants des choix de l’utilisateur. C’est ici que seront visibles les différentes entrées de texte, les différents affichages et les boutons du mode manuel. C’est ici que le parcours d’outil sera affiché pendant l’exécution d’un programme. C’est également ici que l'éditeur de texte s’ouvrira pour permettre l'édition des programmes, l'édition des tables d’outils ou d’offsets. L'écran du mode manuel a déjà été décrit précédemment. Chaque écran surgissant va être décrit en détail ci-dessous.

### Editeur de texte

![](images/miniedit_fr.png)

Figure 4. Editeur de texte de Mini

L'éditeur de texte de Mini peut sembler un peu limité par rapport aux éditeurs de texte modernes. Il a été inclus parce qu’il comporte l’excellente possibilité de numéroter et renuméroter un programme de la même manière que l’interpréteur le fait avec un fichier. Il permet également le couper/coller d’une partie vers une autre du fichier. En plus, il permet d’enregistrer les changements faits dans le programme et de soumettre celui-ci à l’interpréteur de Machinekit depuis le même menu. Il est possible de travailler sur un fichier ouvert dans cet éditeur puis de l’enregistrer et de le recharger si Machinekit est en mode Auto. Si vous avez lancé un fichier et que vous avez besoin de l'éditer, ce fichier sera placé dans l'éditeur quand vous cliquez sur le bouton *Editeur* du menu.

### Parcours d’outil

![](images/minibkplot_fr.png)

Figure 5. Parcours d’outil de Mini

Le bouton *Parcours d’outil* du menu, affiche un tracé produit par le cheminement de l’outil pendant l’usinage. Cette trace est affichable selon plusieurs plans et en **3-D** qui est le mode par défaut. Les choix sont affichés au dessus et à droite de cette fenêtre. Si l’usinage est déjà en cours depuis un certain temps quand cette fenêtre est activée, il lui faudra un peu de temps pour recalculer la vue du parcours.

Sur le côté droit de la fenêtre une petite pyramide graphique montre l’angle depuis lequel l’opérateur voit le tracé du parcours d’outil. En dessous, une série de curseurs permettent de modifier les angles et la taille de la vue. Il est possible de contrôler les modifications en regardant l’attitude de la petite pyramide. Les modifications ne prendront effet qu’après appui sur le bouton *Rafraîchir*. Le bouton *Reset* efface toutes les traces du parcours affiché et prépare la fenêtre pour un nouveau lancement du programme, il conserve toutefois les orientations et le niveau de zoom actuellement définis.

Si un parcours d’outil est lancé avant qu’un programme ne soit démarré, il va afficher quelques lignes de couleur pour indiquer le type de tracé qui sera utilisé. Une ligne verte représente un mouvement en vitesse d’avance rapide. Une ligne noire, un mouvement en vitesse d’avance travail. Une ligne bleue et une ligne rouge indiquent respectivement un arc en sens anti-horaire et un arc en sens horaire.

Le tracé du parcours d’outil de Mini permet de zoomer et d’orienter les vues du programme en cours d’exécution mais il n’est pas conçu pour enregistrer un chemin d’outil sur une longue période de temps.

### Page des outils

La page des outils permet d’ajuster la longueur et le diamètre des outils, ces valeurs deviennent effectives dès l’appui sur la touche *Entrée*. Il est nécessaire de définir les paramètres des outils avant de lancer le programme. Les offsets d’outils ne peuvent pas être modifiés une fois le programme démarré ni quand il est en pause.

![](images/minitool_fr.png)

Figure 6. Affichage de la table d’outils de Mini

Les boutons *Ajouter un outil* et *Enlever le dernier outil* portent sur la fin de la liste des outils de sorte que l’ajout d’outils s’effectuent vers le bas. Quand un nouvel outil est ajouté, il est utilisable dans le programme avec la commande G-code habituelle. Le nombre maximum d’outils est de 32 dans le fichier de configuration de Machinekit mais vous serez arrivé au bout des possibilités d’affichage de Mini bien avant celà. A la place vous pouvez utiliser le *menu → Vues → Afficher dans toute la fenêtre* pour voir plus d’outils si nécessaire.

### Page des offsets

La page des offsets peut être utilisée pour afficher et ajuster les décalages d’origine des différents systèmes de coordonnées. Le système de coordonnées est choisi dans la colonne de gauche, par sélection d’une case à cocher. Quand un système de coordonnées est choisi il est possible d’entrer directement les valeurs ou de déplacer l’axe à une position d’apprentissage.

![](images/minioffsets_fr.png)

Figure 7. Mini Offset Display

Il est également possible d’utiliser une pinnule puis d’ajouter le rayon et la longueur par le bouton *Apprentissage*. Pour celà, il peur être nécessaire d’ajouter ou de soustraire le rayon de la pinnule selon la surface qui est touchée. C’est la case à cocher *Soustraire* ou *Ajouter* qui permet ce choix.

Le bouton *Tous les Gxx à zéro* enléve tous les décalages du système courant visibles sur l'écran mais ne modifie pas les variables dans le fichier tant que le bouton *\_écrire et recharger* n’est pas pressé. Le bouton *écrire et recharger* est à presser pour valider et recharger tous les changements effectués sur les valeurs des systèmes de coordonnées.

Raccourcis clavier
------------------

Un certain nombre de raccourcis clavier utilisés avec TkLinuxcnc ont été conservés avec Mini. Quelques uns ont changé pour étendre leurs fonctions ou pour faciliter les opérations sur une machine utilisant cette interface. Certaines touches opérent de la même manière quelque soit le mode. D’autres change en fonction du mode de travail de Machinekit.

### Touches courantes

 Pause   
Bascule du correcteur de vitesse travail

 Echap   
Abandonne le mouvement

 F1   
Bascule l'état de l’Arrêt d’Urgence

 F2   
Marche/Arrêt machine

 F3   
Mode manuel

 F4   
Mode Auto

 F5   
Mode MDI

 F6   
Réinitialise l’interpréteur

Ces touches fonctionnent seulement pour une machine utilisant des entrées sorties auxiliaires

 F7   
Marche/Arrêt du brouillard

 F8   
Marche/Arrêt arrosage

 F9   
Marche/Arrêt broche sens horaire

 F10   
Marche/Arrêt broche sens anti-horaire

 F11   
Diminue la vitesse de broche

 F12   
Augmente la vitesse de broche

### Mode Manuel

 1-9 0   
Positionne le correcteur de vitesse d’avance travail de 10% en 10%, de 0 à 100%

 ~   
Positionne le correcteur de vitesse à 0

 x   
Sélectionne l’axe X

 y   
Sélectionne l’axe Y

 z   
Sélectionne l’axe Z

 a   
Sélectionne l’axe A

 b   
Sélectionne l’axe B

 c   
Sélectionne l’axe C

 Gauche Droite   
jog de l’axe X

 Haut Bas   
jog de l’axe Y

 Pg.préc Pg.suiv   
jog de l’axe Z

-   \_:: jog de l’axe actif dans la direction moins

 + =   
jog l’axe actif dans la direction plus

 Origine   
POM de l’axe actif

 i I   
Bascule cyclique des incréments de jog

Les touches suivantes ne fonctionnent que sur les machines utilisant des entrées sorties auxiliaires

 b   
Relâcher le frein de broche

 Alt-b   
Engager le frein de broche

### Mode Auto

 1-9,0   
Positionne le correcteur de vitesse d’avance travail de 10% en 10%, de 0 à 100%

 ~   
Positionne le correcteur de vitesse à 0

 o/O   
Ouvre un programme

 r/R   
Lance le programme ouvert

 p/P   
Met le programme en pause

 s/S   
Reprise d’un programme en pause

 a/A   
Avance d’une ligne dans un programme en pause

Divers
------

Une des possibilités de Mini est d’afficher n’importe quel axe, au delà de 2, comme étant un axe rotatif, il affiche alors des degrés pour celui-ci. Il converti également les unités en degrés quand un axe rotatif a le focus.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


