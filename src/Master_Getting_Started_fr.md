Guide de démarrage V, 2015-12-20
================================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

![common/images/emc2-intro.\*]()

The Machinekit Team

Ce manuel est en évolution permanente. Si vous voulez nous aider à son écriture, sa rédaction, sa traduction ou la préparation des graphiques, merci de contactez n’importe quel membre de l'équipe de traduction ou envoyez un courrier électronique à <emc-users@lists.sourceforge.net>.

    Copyright © 2000-2012 Machinekit.org

Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.3 or any later version published by the Free Software Foundation; with no Invariant Sections, no Front-Cover Texts, and no Back-Cover Texts. A copy of the license is included in the section entitled "GNU Free Documentation License".. If you do not find the license you may order a copy from Free Software Foundation, Inc. 59 Temple Place, Suite 330 Boston, MA 02111-1307

LINUX® is the registered trademark of Linus Torvalds in the U.S. and other countries. The registered trademark Linux® is used pursuant to a sublicense from LMI, the exclusive licensee of Linus Torvalds, owner of the mark on a world-wide basis.

Permission est donnée de copier, distribuer et/ou modifier ce document selon les termes de la « GNU Free Documentation License », Version 1.3 ou toute version ultérieure publiée par la « Free Software Foundation »; sans sections inaltérables, sans texte de couverture ni quatrième de couverture. Une copie de la licence est incluse dans la section intitulée « GNU Free Documentation License ». Si vous ne trouvez pas la licence vous pouvez en commander un exemplaire chez Free Software Foundation, Inc. 59 Temple Place, Suite 330 Boston, MA 02111-1307

    (La version de langue anglaise fait foi)

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
<td align="left"><div class="title">
AVIS
</div>
<div class="paragraph">
<p>La version Française de la documentation de Machinekit est toujours en retard sur l’originale faute de disponibilité des traducteurs.</p>
</div>
<div class="paragraph">
<p>Il est recommandé d’utiliser la documentation en Anglais chaque fois que possible.</p>
</div>
<div class="paragraph">
<p>Si vous souhaitez être un traducteur bénévole pour la documentation française de Machinekit, merci de nous contactez.</p>
</div></td>
</tr>
</tbody>
</table>

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
<td align="left"><div class="title">
NOTICE
</div>
<div class="paragraph">
<p>The French version of the Machinekit documentation is always behind the original fault availability of translators.</p>
</div>
<div class="paragraph">
<p>It’s recommended to use the English documentation whenever possible.</p>
</div>
<div class="paragraph">
<p>If you would like to be a volunteer editor for the French translation of Machinekit, please contact us.</p>
</div></td>
</tr>
</tbody>
</table>

Quelle configuration est requise ?
----------------------------------

<span id="cha:Configuration-requise"></span>

### Configuration minimale

La configuration minimale pour faire tourner Machinekit sous Debian varie selon l’usage envisagé. Les moteurs pas à pas en général exigent, pour générer leurs trains d’impulsions de pas, des machines plus rapides que les systèmes à servomoteurs. Il est possible de tester le logiciel à partir du CD-Live avant de l’installer sur un ordinateur. Guarder à l’esprit que les valeurs retournées par le test de latence (Latency Test), sont plus importantes que la vitesse du µP pour la génération logicielle des pas. Plus d’informations à ce propos dans la section relative au [test de latence](#cha:test-de-latence).

Des informations additionnelles sont disponibles sur le [wiki de machinekit.org.](http://wiki.machinekit.org/cgi-bin/emcinfo.pl?Hardware_Requirements)

Machinekit et Debian doivent fonctionner raisonnablement bien sur la configuration matérielle minimale suivante. Ces valeurs ne sont pas des valeurs minimales absolues mais donneront des performances acceptables de la plupart des systèmes à moteurs pas à pas.

-   Microprocesseur x86 à 700 MHz (x86 à 1.2 GHz recommandé)

-   384 Mio de RAM (512 Mio ou plus de 1 Gio recommandé)

-   8 GB d’espace disque

-   Carte graphique avec une résolution minimale de 1024x768

-   Une connection Internet ou réseau (optionnelle mais très pratique pour les mises à jour)

### Problématique du matériel

#### Les portables

Les portables donnent généralement de piètres performances pour les tâches temps réel utilisées pour la génération logicielle de pas. Encore une fois, lancer un test de latence sur une grande période de temps vous permettra de déterminer si le portable envisagé est utilisable ou non.

#### Les cartes graphiques

Si votre installation se termine par un écran avec une résolution de 800 x 600 il est alors probable que Debian n’a pas reconnu votre carte graphique. Les cartes graphiques intégrées aux cartes mères donnent souvent de mauvaises performances temps réel.

Se procurer et installer Machinekit
-----------------------------------

### À propos du logiciel Machinekit

-   Machinekit est un logiciel de contrôle de machines-outils telles que fraiseuses, tours, robots etc.

-   Machinekit est un logiciel libre avec code source ouvert. Les versions actuelles de Machinekit sont entièrement sous licence GNU Lesser General Public et de GNU General Public License (GPL et LGPL)

-   Machinekit propose:

    -   Une installation facile à partir d’un CD live.

    -   Un assistant de configuration simple à utiliser pour créer rapidement une configuration spécifique à la machine.

    -   Une interface graphique (plusieurs interfaces au choix).

    -   Un outil de création d’interface graphique (GladeVCP).

    -   Un interpréteur de G-code (RS-274NGC, langage de programmation des machines-outils).

    -   Un système prédictif de planification de trajectoire.

    -   La gestion du fonctionnement de l'électronique machine de bas niveau, tels que les capteurs et les moteurs.

    -   Un logiciel d’automate programmable pour schémas à contacts (Ladder).

-   Il ne fournit pas directement de logiciel de dessin ni de générateur de G-code, mais il en existe de nombreux, faciles à mettre en œuvre.

-   Il peut piloter simultanément jusqu'à 9 axes et supporte une très grande variété d’interfaces.

-   Le contrôleur peut fonctionner avec de vrais servomoteurs (analogiques ou PWM) en boucle fermée, ou avec des *step-servos* ou encore, des moteurs pas à pas en boucle ouverte.

-   Le contrôleur de mouvement assure: les compensations de rayon et/ou de longueur d’outil, le suivi de trajectoire d’usinage avec tolérance spécifiée, le filetage sur tour, le taraudage rigide, les mouvements avec axes synchronisés, la vitesse d’avance adaptative, la correction de vitesse par l’opérateur, le contrôle de vitesse constante etc.

-   Il supporte les systèmes à mouvements non cartésiens grâce aux modules de cinématique personnalisée. Les architectures disponibles incluent les hexapodes (plate-forme de Stewart et concepts similaires) et les systèmes à articulations rotatives pour assurer les mouvements de robots tels que PUMA ou SCARA.

-   Machinekit fonctionne sous Linux en utilisant ses extensions temps réel RTAI.

### Le système d’exploitation

La distribution Debian a été choisie car elle s’intègre parfaitement dans les vues Open Source de Machinekit:

-   Debian sera toujours gratuit, et il n’y a aucun frais supplémentaire pour la version *"Enterprise Edition"*, nous rendons nos travaux disponibles pour tout le monde dans les mêmes conditions de gratuité.

-   Machinekit est jumelé avec les versions LTS d’Debian qui apportent le soutien et les correctifs de sécurité de l'équipe Debian pour 3 à 5 ans.

-   Debian utilise les meilleurs outils de traductions et d’accessibilité à l’infrastructure, que la communauté du logiciel libre a à offrir, pour rendre Debian accessible à un maximum de personnes.

-   La communauté Debian a entièrement souscrit aux principes du développement de logiciels libres, nous encourageons tout le monde à utiliser des logiciels open source, à les améliorer et à les transmettre.

### Trouver de l’aide<span id="sec:Trouver-aide"></span>

#### Les salons IRC

IRC signifie Internet Relay Chat. Il s’agit d’une connexion en direct avec d’autres utilisateurs Machinekit. Le canal Machinekit sur IRC est: \#machinekit sur freenode.net.

La manière la plus simple d’aller sur IRC est d’utiliser le client embarqué sur cette page: <http://webchat.freenode.net/?channels=machinekit>

Un peu d'éthique sur le canal IRC:

-   Posez des questions précises… Évitez le "quelqu’un peut m’aider?", ce type de questions, *ne fonctionnera pas*.

-   Si vous êtes vraiment nouveau dans tout cela, réfléchissez à votre question avant de la poser. Assurez-vous de donner suffisamment d’informations pour que quelqu’un puisse résoudre votre problème.

-   Faites preuve de patience quand vous attendez une réponse, il faut parfois du temps pour formuler une réponse. Tout les participants peuvent être occupés, à travailler par exemple :-) ou à autre chose.

-   Configurez votre compte IRC avec un pseudo unique afin que les participants sachent qui vous êtes. Si vous utilisez le client java, utilisez le même pseudo à chaque fois que vous vous connecter, cela aidera les participants à se rappeler qui vous êtes, ainsi, si vous êtes déjà venu avant, beaucoup se souviendront des discussions passées, ce qui fait gagner du temps à tout le monde.

#### Partage de fichiers sur IRC

La façon la plus courante de partager des fichiers sur IRC est de charger le fichier sur un des services suivants ou service similaire, puis collez le lien vers le fichier, sur l’IRC:

 Pour le texte   
<http://pastebin.com/>, <http://pastie.org/>, <https://gist.github.com/>

 Pour les photos   
<http://imagebin.org/>, <http://imgur.com/>, <http://bayimg.com/>

 Pour les fichiers   
<https://filedropper.com/>, <http://filefactory.com/>, <http://1fichier.com/>

#### Les listes de diffusion

Une liste de diffusion sur Internet est un moyen de poser des questions, tout les abonnés à cette liste pourrons lire et répondre à leur convenance. Vous obtiendrez une meilleurs visibilité de vos questions sur une liste de diffusion plutôt que sur l’IRC et vous aurez plus de réponses. En quelques mots, vous envoyez un e-mail à la liste et selon comment vous avez configuré votre compte, vous aurez les réponses soit, regroupées quotidiennement, soit individuellement.

Inscription sur la liste de diffusion des utilisateurs de Machinekit sur: <https://lists.sourceforge.net/lists/listinfo/machinekit-users>

#### Le Wiki de Machinekit

Un site Wiki est un site web maintenu et enrichi par les utilisateurs, n’importe qui peut ajouter ou modifier les pages.

Le Wiki de Machinekit est également maintenu par les utilisateurs, il contient un très grand nombre d’informations et d’astuces sur: <http://wiki.machinekit.org/>

### Se procurer Machinekit

#### Par téléchargement classique

Téléchargez le CD Live sur le [site de Machinekit](http://www.machinekit.org/index.php?lang=french)

#### Par téléchargement fragmenté

Si le fichier est trop important pour être téléchargé en une seule fois parce que votre connexion Internet est lente ou mauvaise, utilisez *wget* qui permet la reprise après un téléchargement interrompu.

 La commande Wget sous Linux   
Ouvrez un terminal. Dans Gnome, il est dans *Applications* → *Accessoires* → *Terminal*. Utilisez *cd* pour changer le répertoire dans lequel vous voulez stocker l’image ISO. Utilisez *mkdir* pour créer un nouveau répertoire si nécessaire.

Notez que les noms de fichiers réels peuvent changer, vous pourriez avoir à aller sur [le site de Machinekit](http://www.machinekit.org/index.php/french) et y suivre le lien *Téléchargement* pour obtenir le nom réel du fichier. Dans la plupart des navigateurs, vous pouvez faire un clic droit sur le lien et sélectionner *Copier le lien vers*, ou similaire, coller ensuite ce lien dans la fenêtre du terminal avec un clic droit puis en choisissant *Coller*.

Debian 10.04 Lucid Lynx et Machinekit (version actuelle)

Pour obtenir la version de Debian 10.04 Lucid Lynx, copier l’un des liens ci-dessous dans la fenêtre du terminal et appuyez sur *Entrée*:

Pour le miroir Etats-Unis: wget <http://www.machinekit.org/iso/ubuntu-10.04-machinekit3-i386.iso>

Pour le miroir européen: wget <http://dsplabs.upt.ro/~juve/emc/get.php?file=ubuntu-10.04-machinekit3-i386.iso>

La somme MD5 du fichier ci-dessus est: `76dc2416b917679b71255e464ede84ec`

Pour continuer un téléchargement partiel qui aurait été interrompu par exemple, ajoutez l’option -c à wget:

wget -c <http://www.machinekit.org/iso/ubuntu-10.04-machinekit3-i386.iso>

Pour arrêter un téléchargement, utilisez Ctrl-C ou fermer la fenêtre du terminal.

Debian 8.04 Hardy Heron et Machinekit (plus)

Si vous avez besoin d’une ancienne version d’Debian, vous pouvez télécharger Debian 8.04. L’image du CD ci-dessous est l’ancienne version EMC 2.3.x, mais elle peut être modifiée vers la version 2.4.x en suivant les instructions sur: [le wiki Machinekit.org](http://wiki.machinekit.org/cgi-bin/wiki.pl?UpdatingTo2.4)

Pour le miroir Etats-Unis: wget <http://www.machinekit.org/iso/ubuntu-8.04-desktop-emc2-aj13-i386.iso>

Pour le miroir européen: wget <http://dsplabs.upt.ro/~juve/emc/get.php?file=ubuntu-8.04-desktop-emc2-aj13-i386.iso>

La somme MD5 du fichier ci-dessus est: `1bab052ec879f941628927c988863f14`

Quand le téléchargement est terminé, vous trouverez le fichier ISO dans le répertoire que vous avez sélectionné précédemment. Ensuite, il ne vous restera plus qu'à graver le CD.

 La commande Wget sous Windows   
Le programme wget est également disponible pour Windows depuis:

<http://gnuwin32.sourceforge.net/packages/wget.htm>

Suivez les instructions de la page web pour télécharger et installer le programme wget sous Windows.

Pour lancer wget ouvrez l’invite de commande.

Dans la plupart des Windows elle est dans *Programmes* → *Accessoires* → *Commande*

Naviguez jusqu’au répertoire dans lequel s’est installé wget. Habituellement il est dans *C:\\Program Files\\GnuWin32\\bin* si c’est le cas, tapez la commande:

    cd C:\Program Files\GnuWin32\bin

et le prompt devrait changer pour: *C:\\Program Files\\GnuWin32*

Tapez les commandes *wget* dans la fenêtre et pressez Entrée comme précédemment.

### Graver l’image ISO du CD

Machinekit est distribué sous la forme d’un fichier image de CD, l’image ISO du CD. Pour installer Machinekit, vous devez d’abord graver cette image ISO sur un CD. Vous devez disposer d’un graveur de CD/DVD et d’un CD vierge de 80 minutes (700 Mio). Pour éviter tout échec de gravure, graver à la vitesse la plus lente possible.

#### Sous Linux

##### Vérifier la somme de contrôle sous Linux

Avant de graver un CD, il est fortement recommandé de vérifier la somme de contrôle md5 (hash) du fichier de l’image iso.

Ouvrez un terminal. Dans Debian il est dans *Applications* → *Accessoires* → *Terminal*.

Allez dans le répertoire contenant l’image ISO précédemment téléchargée avec:

    cd répertoire_de_l'image

Puis lancez la commande *md5sum* suivie du nom du fichier, exemple:

    md5sum -b ubuntu-10.04-machinekit3-i386.iso

La commande md5sum doit retourner une simple ligne après le calcul de la somme de contrôle. Sur une machine lente le calcul peut prendre plusieurs minutes:

    76dc2416b917679b71255e464ede84ec ubuntu-10.04-machinekit3-i386.iso

Il reste à comparer avec la somme md5 fournie sur la page de téléchargement.

Si vous avez téléchargé le md5sum ainsi que l’ISO, vous pouvez demander au programme md5sum de faire la vérification pour vous. Dans le même répertoire:

    md5sum -c ubuntu-10.04-machinekit1-i386.iso.md5

Si tout va bien, après un court délai le terminal affichera:

    ubuntu-10.04-machinekit1-i386.iso: OK

##### Graver le CD sous Linux

-   Insérez un CD vierge dans votre graveur. Une fenêtre surgissante *CD/DVD Creator* ou *Choisissez le type de disque* va s’ouvrir. Fermez la, elle ne sera pas utilisée.

-   Naviguez jusqu’au répertoire contenant l’image ISO.

-   Faites un clic droit sur le fichier de l’image ISO et choisissez *Graver le Disque*.

-   Sélectionnez la vitesse de gravure. Pour graver le CD Live de Machinekit il est recommandé de graver à la vitesse la plus lente possible pour éviter toute erreur de gravure.

-   Lancez la gravure.

-   Si le choix d’un nom de fichier est demandé pour l’image disque, cliquez juste *OK*.

#### Sous Windows

##### Vérifier la somme de contrôle sous Windows

Avant de graver un CD, il est fortement recommandé de vérifier la somme de contrôle md5 (hash) du fichier de l’image iso, malheureusement Windows ne dispose pas de programme de contrôle du md5. Vous devrez en installer un pour vérifier la somme de contrôle de l’ISO. Plus d’informations sont disponibles ici: <http://doc.ubuntu-fr.org/md5sum>

##### Gravez le CD sous Windows

-   Si votre Windows n’intègre pas un logiciel de gravure d’image vous pouvez télécharger Infra Recorder, un logiciel de gravure d’images gratuit et open source sur <http://infrarecorder.org/>

-   Insérez un CD vierge dans le graveur, sélectionnez *Quitter* ou *Cancel* si un auto-run démarre.

-   Cliquez bouton droit sur le fichier ISO et sélectionnez le menu *Graver l’image disque* ou lancez Infra Recorder et choisissez le menu *Actions→Graver l’image*.

### Tester Machinekit

Avec le CD Live de Machinekit dans le lecteur de CD/DVD, redémarrez votre PC de sorte qu’il démarre sur le CD Live. Quand l’ordinateur a redémarré vous pouvez essayer Machinekit sans l’installer. Vous ne pouvez pas créer de configuration personnalisée ni modifier les réglages du système comme la résolution de l'écran sans installer Machinekit.

Pour lancer Machinekit allez dans le menu Applications/CNC et choisissez Machinekit. Puis sélectionnez une configuration en sim (simulation) et essayez le.

Pour savoir si votre ordinateur est utilisable par le générateur de trains d’impulsions du logiciel, lancez un test de latence comme indiqué [dans ce chapitre](#cha:test-de-latence).

### Installer la distribution Debian de Machinekit sur votre PC

Si vous avez envie d’aller plus loin, cliquez juste sur l’icône *Install* se trouvant sur le bureau, répondez à quelques questions (votre nom, votre fuseau horaire, le mot de passe) et faites une installation complète en quelques minutes. Notez bien le mot de passe indiqué et le nom d’utilisateur. Une fois l’installation complète et que vous êtes connecté, le gestionnaire de mises à jour vous proposera d’effectuer une mise à jour vers la dernière version stable de Machinekit.

#### Lancer Machinekit

Machinekit se lance comme un autre programme Linux: depuis un terminal en passant la commande *machinekit*, ou depuis le menu *Applications* → *CNC*.

#### Sélecteur de configuration

Le *Sélecteur de configuration* s’affichera à chaque fois que vous lancerez Machinekit depuis le menu *Applications* → *CNC* → *Machinekit*. Vos propres configurations personnalisées s’affichent dans le haut de la liste, suivies par les différentes configurations fournies en standard. Étant donné que chaque exemple de configuration utilise un type différent d' interface matérielle, la plupart ne fonctionneront pas sur votre système. Les configurations listées dans la catégorie *Sim* fonctionneront toutes, même sans matériel raccordé, ce sont des simulations de machines.

![images/configuration-selector1\_fr.png]()

Figure 1. Sélecteur de configuration pour Machinekit<span id="cap:Selecteur-de-configuration"></span>

Cliquez dans la liste, sur les différentes configurations pour afficher les informations les concernant. Double-cliquez sur une configuration ou cliquez *OK* pour démarrer Machinekit avec cette configuration. Cochez la case *Créer un raccourci sur le bureau* puis cliquez OK pour ajouter une icône sur le bureau d’Debian. Cette icône vous permettra par la suite de lancer directement Machinekit avec cette configuration, sans passer par le sélecteur de configuration.

Quand vous choisissez un exemple de configuration dans le sélecteur, un dialogue vous demandera si vous voulez en faire une copie dans votre répertoire home. Si vous répondez *oui*, un dossier *machinekit* autorisé en écriture sera créé, il contiendra un jeu de fichiers que vous pourrez éditer pour les adapter à vos besoins. Si vous répondez *non*, Machinekit démarrera mais pourra se comporter de façon étrange, par exemple, les décalages d’origine pièce entrés avec la commande *Toucher* ne seront pas pris en compte, ce comportement est lié à ce moment, à l’absence de répertoire autorisé en écriture sans lequel les paramètres ne peuvent être enregistrés.

![images/copy-configuration\_fr.png]()

Figure 2. Dialogue de copie de la configuration

#### L’interface utilisateur graphique Axis

L’interface AXIS est une des interfaces parmi lesquelles vous avez à choisir. Elle peut être configurée pour lui ajouter un panneau de commandes virtuel personnalisé en fonction des besoins. AXIS est l’interface utilisateur par défaut et est activement développée. C’est aussi la plus populaire.

![images/axis\_25\_fr.png]()

Figure 3. Interface Axis<span id="cap:Interface-Axis"></span>

#### Les étapes suivantes de la configuration

Après avoir trouvé l’exemple de configuration qui utilise le même matériel que votre machine, et en avoir enregistré une copie dans votre répertoire personnel, vous pouvez la personnaliser en fonction des besoins spécifiques à votre machine. Consultez le *Manuel de l’intégrateur* pour tous les détails de configuration.

Si vous souhaitez créer une configuration personnalisée, vous pouvez utiliser pour cela, un des assistants graphiques de configuration, *StepConf* ou *PncConf* selon votre type de machine.

### Les mises à jour de Machinekit

Avec l’installation standard, le gestionnaire de mises à jour vous avertira des mises à jour de Machinekit disponibles quand vous serez en ligne et vous permettra de mettre à jour facilement sans connaissance particulière de Linux. Si vous souhaitez passer en 10.04 à partir d’une 8.04, une installation propre à partir du CD live est recommandée.

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
<td align="left"><span class="red">Ne pas mettre à jour Debian vers une nouvelle distribution comme de 10.04 vers 12.04, car elle ne permettrait plus d’utiliser Machinekit, ne pas mettre non plus à jour le kernel, car les modules temps réels ne fonctionnerait plus.</span></td>
</tr>
</tbody>
</table>

### Problème d’installation possible

Dans de rares cas, vous pourriez avoir à réinitialiser le BIOS aux réglages par défaut si lors de l’installation du Live CD, le disque dur n’est pas reconnu pendant le démarrage.

Mises à jour de Machinekit
--------------------------

### Mise à jour de 2.4.x vers 2.5.x

La version 2.5.0 de Machinekit change de nom, elle passe de *EMC2* à *Machinekit*. Tous les programmes dont les noms contenaient *emc* ont été renommés pour contenir *machinekit*. Toute la documentation à été mise à jour.

De plus, le nom du paquet debian contenant le logiciel a changé. Malheureusement les mises à jour automatiques sont cassées. Pour mettre à jour depuis emc2 2.4.X vers machinekit 2.5.X, suivez ces méthodes:

#### Sous Debian Lucid 10.04

Déclarer d’abord où se trouve le nouveau logiciel Machinekit, pour cela:

-   Dans le menu, cliquer sur *Système → Administration → Sources de logiciels*.

-   Sélectionner l’onglet *Autres logiciels*.

-   Sélectionner la ligne indiquant *http://www.machinekit.org/emc2 lucid base emc2.4* ou *http://www.machinekit.org/emc2 lucid base emc2.4-sim* puis cliquer le bouton *Éditer*.

-   Dans le champ *URI*, remplacer la ligne courante par *http://www.machinekit.org*

-   Dans le champ *Composants*, modifier *emc2.4* par *machinekit2.5*, ou *emc2.4-sim* par *machinekit2.5-sim*.

-   Cliquer enfin sur le bouton *Valider*.

-   De retour dans la fenêtre des sources de logiciels, onglet *Autres logiciels*, cliquer sur le bouton *Fermer*.

-   Une fenêtre surgissante informe alors que *Les informations sur les logiciels disponibles sont obsolètes*. Cliquer le bouton *Actualiser*.

-   Suivre la même procédure pour le *Code source*.

Maintenant l’ordinateur sait où trouver le nouveau logiciel, ensuite il faudra lui demander de l’installer:

-   Dans le menu, cliquer sur *Système → Administration → Gestionnaire de paquets Synaptic*

-   Dans la barre de recherche rapide, en haut, taper *machinekit*.

-   Cocher la case pour valider l’installation du nouveau paquet machinekit.

-   Cliquer sur le bouton *Appliquer* et laisser le paquet s’installer.

-   L’ancien paquet emc 2.4 sera automatiquement supprimé pour laisser place au nouveau paquet Machinekit 2.5.

#### Sous Debian Hardy 8.04

Déclarer d’abord où se trouve le nouveau logiciel Machinekit, pour cela:

-   Dans le menu, cliquer sur *Système → Administration → Gestionnaire de paquets Synaptic*

-   Aller dans le menu *Configuration → Dépôts*.

-   Sélectionner l’onglet *Logiciels tiers*.

-   Sélectionner la ligne indiquant *http://machinekit.org/hardy hardy emc2.4* ou *http://machinekit.org/hardy hardy emc2.4-sim* et cliquer sur le bouton *Éditer*.

-   Dans le champs *Composants*, modifier *emc2.4* par *machinekit2.5* ou *emc2.4-sim* par *machinekit2.5-sim*.

-   Cliquer sur le bouton *Valider*.

-   De retour dans la fenêtre des sources de logiciels, cliquer sur le bouton *Fermer*.

-   De retour dans la fenêtre de Synaptic, cliquer sur le bouton *Actualiser*.

Maintenant l’ordinateur sait où trouver le nouveau logiciel, ensuite il faudra lui demander de l’installer:

-   Dans le menu, cliquer sur *Système → Administration → Gestionnaire de paquets Synaptic*, cliquer sur le bouton *Rechercher*.

-   Dans le champ du dialogue de recherche qui s’ouvre, taper *machinekit* puis cliquer sur le bouton *Rechercher*.

-   Cocher la case pour valider l’installation du paquet *machinekit*.

-   Cliquer sur le bouton *Appliquer* et laisser le nouveau paquet s’installer.

-   L’ancien paquet emc 2.4 sera automatiquement supprimé pour laisser place au nouveau paquet Machinekit 2.5.

### Changement de configuration

Les configurations utilisateur ont migré de $HOME/emc2 vers $HOME/machinekit, il sera nécessaire de renommer l’ancien répertoire si il existe, ou de déplacer les fichiers vers ce nouvel endroit. Le watchdog de hostmod2 dans Machinekit 2.5 ne démarre qu’après que les threads de HAL soient eux-même démarrés. Cela signifie qu’il tolère désormais un délai d’attente de l’ordre de la période servo thread, au lieu de nécessiter un délai qui soit de l’ordre du temps entre le chargement du pilote et le démarrage des threads de HAL. Ce qui signifie, de l’ordre de quelques millisecondes (quelques périodes du thread servo) au lieu de plusieurs centaines de millisecondes préalablement. La valeur par défaut est descendue de 1 seconde à 5 millisecondes. Vous ne devriez donc plus avoir a ajuster le délai du watchdog, à moins que vous ne modifiez la période du threads servo.

Les anciens pilotes pour les cartes Mesa 5i20, hal\_m5i20, ont été enlevés, ils étaient obsolètes et remplacés par hostmot2 depuis 2009 (version 2.3.) Si vous utilisiez ces pilotes, vous devrez reconstruire une nouvelle configuration utilisant le pilote hostmod2. Pncconf peux vous y aider et il contient quelques exemples de configurations (hm2-servo et hm2-stepper) qui vous serviront d’exemple.

### Mise à jour 2.3.x à 2.4.x

Les instructions suivantes s’appliquent à Debian 8.04 "Hardy Heron". Machinekit 2.4 n’est pas disponible pour des versions plus anciennes de Debian.

En raison de plusieurs incompatibilités mineures entre les versions 2.3.5 et 2.4.x, votre installation existante ne sera pas automatiquement mise à jour vers la version 2.4.x. Si vous voulez exécuter la version 2.4.x, veuillez modifier le dépôt Machinekit-2.4 en suivant ces instructions :

Lancez Système/Administration/Gestionnaire de paquets Synaptic Sélectionnez Configuration/Dépôt Dans l’onglet "Autres logiciels" vous trouverez deux lignes concernant machinekit.org.

Pour chacune:

-   Sélectionnez la ligne et cliquez sur le bouton "Editer"

-   Dans le champ composants remplacez emc2.3 par emc2.4

-   Cliquez sur "Valider"

-   Fermer la fenêtre "Source de logiciels" avec le bouton fermer

-   Cliquez sur "Recharger" dans la barre d’outils

-   Cliquez sur "Tout mettre à jour" dans la barre d’outils

Utilisateurs de carte Mesa et hostmot2 :

Si vous utilisez une carte *mesa*, trouvez le paquet hostmot2-firmware approprié à votre carte et marquez-le pour l’installation. Astuce: faites une recherche "hostmot2-firmware" dans gestionnaire de paquet Synaptic.

-   Cliquez "Appliquer"

### Changements entre 2.3.x et 2.4.x

Une fois que vous avez fait la mise à jour, mettez à jour les configurations personnalisées en suivant ces instructions

#### Changement emc.nml (2.3.x to 2.4.x)

Pour les configurations qui ont personnalisé emc.nml, enlevez la ligne `NML_FILE = emc.nml` dans le fichier inifile. Cela forcera l’utilisation de la version la plus à jour de emc.nml.

Pour les configurations qui n’ont pas personnalisé emc.nml, un changement similaire est requis.

Un échec de cette opération provoque une erreur comme:

    libnml/buffer/physmem.cc 143: PHYSMEM_HANDLE:
    Can't write 10748 bytes at offset 60 from buffer of size 10208.

#### Changements de la table d’outils (2.3.x to 2.4.x)

Le nouveau format de la table d’outil est incompatible. La documentation explique le nouveau format. La table d’outil sera automatiquement convertie dans le nouveau format.

#### Images du micro logiciel hostmot2 (2.3.x to 2.4.x)

Les images du micro logiciel hostmot2 sont dorénavent dans un paquet séparé. Vous pouvez :

-   Continuer à utiliser un paquet déjà installé `emc2-firmware-mesa-*`

-   Installer les nouveaux paquets du gestionnaire de paquets Synaptic. Les nouveaux paquets sont nommés `hostmot2-firmware-*`

-   Télécharger les fichiers .tar des images du microprogramme depuis <http://emergent.unpy.net/01267622561> et les installer manuellement.

Configuration rapide pour moteurs pas à pas
-------------------------------------------

<span id="cha:stepper-quickstart"></span>

Cette section suppose qu’une installation du logiciel à partir du CD Live a été faite. Après cette installation et avant de continuer, il est recommandé de connecter le PC sur Internet pour y faire les dernières mises à jour. Pour les installations plus complexes se référer au Manuel de l’intégrateur.

### Test de latence (Latency Test)

Le test de latence détermine la capacité du processeur à répondre aux requêtes qui lui sont faites. Certains matériels peuvent interrompre ce processus, causant des pertes de pas lorsque le PC pilote une machine CNC. Ce test est la toute première chose à faire pour valider un PC. Pour le lancer, suivre les instructions de la section [sur le test de latence](#cha:test-de-latence).

### Sherline

Si vous avez une machine Sherline plusieurs configurations prédéfinies sont fournies. Au premier démarrage de Machinekit, le sélecteur de configuration s’ouvre, sélectionnez alors le modèle correspondant à votre machine *Sherline*, puis acceptez d’enregistrer une copie.

### Xylotex

Si vous avez une machine *Xylotex* vous pouvez utiliser l’assistant graphique de configuration fourni avec Machinekit et créer rapidement votre configuration personnalisée [avec l’assistant Stepconf](#cha:Assistant-graphique-StepConf).

### Informations relatives à la machine

But, regrouper les informations à propos des axes de la machine.

Les timings des pilotes sont exprimés en nanosecondes. Si vous n'êtes pas sûr de vous à propos des timings de votre interface, les caractéristiques les plus populaires sont incluses dans l’assistant graphique de configuration. Notez que les pilotes Gecko ont des timings différents les uns des autres. Une liste des caractéristiques courantes est également maintenue sur le Wiki [de machinekit.org](http://wiki.machinekit.org/cgi-bin/wiki.pl?Stepper_Drive_Timing).

<table>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Axes</th>
<th align="left">Type de pilote</th>
<th align="left">Step Time ns</th>
<th align="left">Step Space ns</th>
<th align="left">Direction Hold ns</th>
<th align="left">Direction Setup ns</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>X</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>Y</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>Z</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

<span class="footnote">
\[ndt: les termes sont laissés dans la langue d’origine pour correspondre aux documentations des constructeurs.\]
</span>

### Informations relatives au brochage

But, regrouper les informations à propos des différentes broches du port parallèle utilisées.

<table>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pin de sortie</th>
<th align="left">Fonction</th>
<th align="left">Si différent</th>
<th align="left">Pin d’entrée</th>
<th align="left">Fonction</th>
<th align="left">Si différent</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>Sortie A/U</p></td>
<td align="left"><p></p></td>
<td align="left"><p>10</p></td>
<td align="left"><p>Limite et OM X</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p>X Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p>11</p></td>
<td align="left"><p>Limite et OM Y</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>X Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>Limite et OM Z</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>Y Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p>13</p></td>
<td align="left"><p>Limite et OM A</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>5</p></td>
<td align="left"><p>Y Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p>15</p></td>
<td align="left"><p>Entrée palpeur</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>6</p></td>
<td align="left"><p>Z Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>7</p></td>
<td align="left"><p>Z Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>8</p></td>
<td align="left"><p>A Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>9</p></td>
<td align="left"><p>A Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>14</p></td>
<td align="left"><p>Broche sens horaire</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>16</p></td>
<td align="left"><p>PWM broche</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>17</p></td>
<td align="left"><p>Valide puissance</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

Noter que toutes les broches inutilisées doivent être explicitement indiquées *Inutilisé* dans le choix déroulant de l’assistant. Elles pourront être modifiées par la suite en relançant Stepconf.

### Informations relatives à la mécanique

But, regrouper les informations à propos des réducteurs. Utilisées pour définir la taille d’un pas dans l’unité utilisateur. La taille du pas est utilisée par SCALE dans le fichier .ini.

<table>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Axes</th>
<th align="left">Pas par tour</th>
<th align="left">Micropas</th>
<th align="left">Dents moteur</th>
<th align="left">Dents vis</th>
<th align="left">Pas de la vis</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>X</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>Y</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>Z</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

*Pas par tour* indique combien de pas moteur sont nécessaires pour que celui-ci fasse un tour. Valeur typique: 200.

*Micropas* indique combien d’impulsions le pilote doit recevoir pour que le moteur tourne d’un angle équivalent à un pas.

Si les micropas ne sont pas utilisés, cette valeur devra être mise à 1. Si les micropas sont utilisés, les valeurs les plus courantes sont, 2 pour le demi-pas, 4 pour le quart de pas, 8 ou 10.

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
<td align="left">Le meilleur choix sera un compromis entre les petites valeurs, qui peuvent rendre le système bruyant à cause des vibrations et les valeurs élevées, qui exigent beaucoup de pas, ce qui diminue la vitesse maximale.</td>
</tr>
</tbody>
</table>

*Dents moteur* et *Dents vis* à indiquer si vous avez une réduction poulies/courroie entre le moteur et la vis. Sinon mettez 1 pour les deux.

*Pas de la vis* indique la longueur de déplacement du mobile pour un tour de la vis d’entrainement de l’axe.

Un exemple en pouces:

    Moteur            = 200 pas par tour
    Pilote            = 10 micropas par pas
    Dents côté moteur = 20
    Dents côté vis    = 40
    Pas de vis        = 0.2000 pouces par tour

D’après les informations ci-dessus:

-   le mobile se déplacera de 0.200 pouces par tour de vis.

-   Le moteur fera 2000 micropas par tour de vis.

-   Le pilote demandera 10 micropas pour faire un pas.

-   Le pilote recevra 2000 impulsions de pas pour faire tourner le moteur d’un tour.

Encore un autre exemple, en millimètres cette fois:

    Pas par tour      = 200 pas par tour
    Micropas          =   8 micropas
    Dents côté moteur =  30
    Dents côté vis    =  90
    Pas de la vis     =   5.00 mm par tour

D’après les informations ci-dessus:

-   la vis déplacera le mobile de 5.00 mm par tour.

-   Le moteur fera 3 tours pour 1 tour de vis. (90/30)

-   Le pilote utilisera 8 micropas pour faire un pas.

-   Le pilote aura besoin de 1600 impulsions pour un tour moteur et donc de 4800 pour 1 tour de vis.

### Assistants de configuration graphique

-   Pour les moteurs pas à pas, voir la documentation de l’assistant graphique Stepconf au chapitre [concernant cet assistant.](#cha:Assistant-graphique-StepConf)

-   Pour les servomoteurs et les moteurs pas à pas, voir la documentation de l’assistant graphique PNCconf au chapitre [relatif à cet assistant](#cha:Assistant-graphique-PNCConf)

Assistant graphique de configuration StepConf
---------------------------------------------

<span id="cha:Assistant-graphique-StepConf"></span>

### Introduction

Machinekit est capable de contrôler un large éventail de machines utilisant de nombreuses interfaces matérielles différentes.

Stepconf est un programme qui génère des fichiers de configuration Machinekit pour une classe spécifique de machine CNC: celles qui sont pilotées via un, ou plusieurs ports parallèles standards et contrôlées par des signaux de type pas/direction (step/dir).

Stepconf est installé en même temps que Machinekit et un lanceur se trouve dans le menu *Application → CNC → Machinekit StepConf*.

Stepconf place les fichiers qu’il crée dans le répertoire *~/machinekit/config* pour y stocker les paramètres de chaque configuration. Lorsque quelque chose doit être modifié, il faut choisir le fichier correspondant au nom de la configuration et portant l’extension .stepconf.

L’Assistant Stepconf a besoin, au minimum, d’une résolution de 800 x 600 pour que les boutons sur le bas des pages soient apparents.

### Page d’accueil

![images/stepconf-config\_fr.png]()

Figure 4. La page d’accueil de stepconf

-   *Créer une nouvelle configuration* - Créera une configuration nouvelle.

-   *Modifier une configuration déjà créée* - Modifiera une configuration existante, déjà créée avec Stepconf. Après la sélection de celle-ci un sélecteur de fichier s’ouvre pour y choisir le fichier .stepconf à modifier. Si des modifications sont faites aux fichiers .hal et .ini avec un autre éditeur, ils ne seront plus utilisables par Stepconf. Les modifications de custom.hal et de custom\_postgui.hal, par contre, sont conservées par Stepconf.

-   *Créer un lien* - Placera un lien sur le bureau, pointant sur le dossier des fichiers de configuration.

-   *Créer un lanceur* - Placera un lanceur sur le bureau pour démarrer l’application avec sa configuration.

### Informations machine

![images/stepconf-basic\_fr.png]()

Figure 5. Page d’informations sur la machine

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

### Options de configuration avancée

![images/stepconf-advanced\_fr.png]()

Figure 6. Configuration avancée

-   *Inclure l’interface Halui* - Ajoutera l’interface utilisateur Halui. Voir le manuel de l’intégrateur pour plus d’informations sur Halui.

-   *Inclure un panneau pyVCP* - Ceci ajoutera un panneau pyVCP de base, avec son fichier de configuration sur lequel il sera possible de travailler. Quelques options sont disponibles pour enrichir le panneau grâce à des cases à cocher. Voir le manuel de l’intégrateur pour plus d’information sur pyVCP.

-   *Inclure l’API ClassicLadder* - Cette option ajoutera l’automate programmable en logique à contacts ClassicLadder. Un certain nombre d’options sont disponibles pour enrichir l’API grâce à des cases à cocher. L'éditeur de programme ladder est accessible par le bouton *Editer prog. ladder* Voir le manuel de l’intégrateur pour plus d’information sur ClassicLadder.

### Réglage du port parallèle

![images/stepconf-pinout\_fr.png]()

Figure 7. Page de réglage du port parallèle

-   *Sorties (PC vers machine)* - Pour chacune des broches, choisir le signal correspondant au brochage entre le port parallèle et l’interface matérielle. Cocher la case inverser si le signal est inversé (0V pour vrai/actif, 5V pour faux/inactif).

-   *Sorties présélectionnées* - Réglage automatique des pins 2 à 9 Direction sur les pins 2, 4, 6, 8, selon le *type Sherline* Direction sur les pins 3, 5, 7, 9, selon le *type Xylotex*

-   *Entrées et sorties* - Les entrées ou les sorties non utilisées doivent être placées sur Inutilisé.

-   *Sortie arrêt d’urgence* - Sélectionnable dans la liste déroulante des sorties. La sortie d’arrêt d’urgence est utilisée pour actionner l’organe de coupure du circuit de puissance de la machine. Le contact de cet organe est câblé en série avec les contacts des boutons d’arrêt d’urgence extérieurs ainsi qu’avec tous les contacts compris dans la boucle d’arrêt d’urgence.

-   *Entrées (machine vers PC)* - Ces choix se font dans la liste déroulante des entrées.

-   *Pompe de charge* - Si la carte de contrôle accepte un signal pompe de charge, dans la liste déroulante des sorties, sélectionner *Pompe de charge* sur la sortie correspondant à l’entrée Pompe de charge de la carte de contrôle. La sortie pompe de charge sera connectée en interne par Stepconf. Le signal de pompe de charge sera d’environ la moitié de la fréquence maxi des pas affichée sur la page des informations machine.

### Configuration des axes

![images/stepconf-axis\_fr.png]()

Figure 8. Page de configuration des axes

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

### Tester cet axe

![images/stepconf-test\_fr.png]()

Figure 9. Tester cet axe

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

### Trouver la vitesse maximale

Commencer avec une faible valeur d’accélération (par exemple, **`2 pouces/s2`** ou **`50 mm/s2`**) et la vitesse que espérée. En utilisant les boutons de jog, positionner l’axe vers son centre. Il faut être prudent, car avec peu d’accélération, la distance d’arrêt peut être très surprenante. Après avoir évalué le déplacement possible dans chaque direction en toute sécurité, entrer une distance dans le champs *Zone de test* garder à l’esprit qu’après un décrochage, le moteur peut repartir dans la direction inattendue. Puis cliquer sur *Lancer*. La machine commencera à aller et venir le long de cet axe. Dans cet essai, il est important que la combinaison entre l’accélération et la zone de test, permette à la machine d’atteindre la vitesse sélectionnée et de s’y déplacer au moins, sur une courte distance. La formule **`d = 0.5 * v * v/a`**, donne la distance minimale requise pour atteindre la vitesse de *croisière*. Si la sécurité est garantie, pousser sur la table dans la direction inverse du mouvement pour simuler les efforts de coupe. Si la table décroche, réduire la vitesse et recommencer le test. Si la machine ne présente aucun décrochage, cliquer sur le bouton *Lancer*. L’axe revient alors à sa position de départ. Si cette position est incorrecte, c’est que l’axe a calé ou a perdu des pas au cours de l’essai. Réduire la vitesse et relancer le test. Si la machine ne se déplace pas, cale, vibre ou perd des pas, même à faible vitesse, vérifier les éléments suivants:

-   Corriger les paramètres de temps des impulsions de commande.

-   Le brochage du port et la polarité des impulsions. Les cases *Inverser*.

-   La qualité des connexions et le blindage des câbles.

-   Les problèmes mécaniques avec le moteur, l’accouplement moteur, vis, raideurs etc.

Quand la vitesse à laquelle l’axe ne perd plus de pas et à laquelle les mesures sont exactes pendant le test a été déterminée, réduire cette vitesse de 10% et l’utiliser comme vitesse maximale pour cet axe.

### Trouver l’accélération maximale

Avec la vitesse maximale déterminée à l'étape précédente, entrer une valeur d’accélération approximative. Procéder comme pour la vitesse, en ajustant la valeur d’accélération en plus ou en moins selon le résultat. Dans cet essai, il est important que la combinaison de l’accélération et de la zone de test permette à la machine d’atteindre la vitesse sélectionnée. Une fois que la valeur à laquelle l’axe ne perd plus de pas pendant le test a été déterminée, la réduire de 10% et l’utiliser comme accélération maximale pour cet axe.

### Configuration de la broche

![images/stepconf-spindle\_fr.png]()

Figure 10. Page configuration de la broche<span id="cap:Page-Configuration-de-la-broche"></span>

Ces options ne sont accessibles que quand *PWM broche*, *Phase A codeur broche* ou *index broche* sont configurés dans le réglage du port parallèle.

### Contrôle de la vitesse de broche

Si *PWM broche* apparaît dans le réglage du port parallèle, les informations suivantes doivent être renseignées:

-   *Fréquence PWM* - La fréquence porteuse du signal PWM (modulation de largeur d’impulsions) du moteur de broche. Entrer 0 pour le mode PDM (modulation de densité d’impulsions), qui est très utile pour générer une tension de consigne analogique. Se reporter à la documentation du variateur de broche pour connaître la valeur appropriée.

-   *Vitesse 1 et 2, PWM 1 et 2* - Le fichier de configuration généré utilise une simple relation linéaire pour déterminer la valeur PWM correspondant à une vitesse de rotation. Si les valeurs ne sont pas connues, elles peuvent être déterminées. Voir la section sur [la calibration de la broche](#sub:Determiner-broche-Etalonnage-broche).

### Mouvement avec broche synchronisée (filetage sur tour, taraudage rigide)

Lorsque les signaux appropriés, provenant d’un codeur de broche, sont connectés au port parallèle, Machinekit peut être utilisé pour les usinages avec broche synchronisée comme le filetage ou le taraudage rigide. Ces signaux son:

-   *Index broche* - Également appelé PPR broche, c’est une impulsion produite à chaque tour de broche.

-   *Phase A broche* - C’est une suite d’impulsions carrées générées sur la voie A du codeur pendant la rotation de la broche. Le nombre d’impulsions pour un tour correspond à la résolution du codeur.

-   *Phase B broche* (optionnelle) - C’est une seconde suite d’impulsions, générées sur la voie B du codeur et décalées par rapport à celle de la voie A. L’utilisation de ces deux signaux permet d’accroitre l’immunité au bruit et la résolution d’un facteur 4.

Si *Phase A broche* et *Index broche* apparaissent dans le réglage des broches du port, l’information suivante doit être renseignée sur la page de configuration broche:

-   *Cycles par tour* - Le nombre d’impulsions par tour sur la broche Phase A broche.

-   *La vitesse maximale en filetage* - La vitesse de broche maximale utilisée en filetage. Pour exploiter un moteur de broche rapide ou un codeur ayant une résolution élevée, une valeur basse de BASE\_PERIOD est requise.

### Calibrer la broche

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

### Terminer la configuration

Cliquer *Appliquer* pour enregistrer les fichiers de configuration. Ensuite, il sera possible de relancer ce programme et ajuster les réglages entrés précédemment.

### Position des fins de course sur les axes

![images/HomeAxisTravel.png]()

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

### Exploitation sans fin de course

Une machine peut être utilisée sans contact de fin de course. Dans ce cas, seules les limites logicielles empêcheront la machine d’atteindre les butées mécaniques. Les limites logicielles n’opèrent qu’après que la POM (prise d’origine machine) soit faite sur la machine. Puisqu’il n’y a pas de contact, la machine doit être déplacée à la main et à l’œil, à sa position d’origine avant de presser le bouton *POM des axes* ou le sous-menu *Machine → Prises d’origines machine → POM de l’axe*. L’opérateur devra cocher chacun des axes individuellement pour faire la POM de chacun d’eux.

### Exploitation sans contact d’origine

Une machine peut être utilisée sans contact d’origine machine. Si la machine dispose de contacts de fin de course, mais pas de contact d’origine machine, il est préférable d’utiliser le contact de fin de course comme contact d’origine machine (exemple, choisir *Limite mini + origine X* dans le réglage du port). Si la machine ne dispose d’aucun contact, ou que le contact de fin de course n’est pas utilisable pour une autre raison, alors la prise d’origine machine peut toujours être réalisée à la main. Faire la prise d’origine à la main n’est certes pas aussi reproductible que sur des contacts, mais elle permet tout de même aux limites logicielles d'être utilisables.

### Câblage des contacts de fin de course et d’origine machine

Le câblage idéal des contacts externes serait une entrée par contact. Toutefois, un seul port parallèle d’ordinateur offre un total de 5 entrées, alors qu’il n’y a pas moins de 9 contacts sur une machine 3 axes. Au lieu de cela, plusieurs contacts seront câblés ensembles, selon diverse combinaisons, afin de nécessiter un plus petit nombre d’entrées.

Les figures ci-dessous montrent l’idée générale du câblage de plusieurs contacts à une seule broche d’entrée. Dans chaque cas, lorsqu’un contact est actionné, la valeur vue sur l’entrée va passer d’une logique haute à une logique basse. Cependant, Machinekit s’attend à une valeur VRAIE quand un contact est fermé, de sorte que les cases Inverser correspondantes devront être cochées sur la page de réglage du port parallèle. Une résistance de rappel est nécessaire dans le circuit pour tirer l’entrée au nivaux haut. La valeur typique pour un port parallèle est de 47K. Une bonne sécurité utilise des contacts normalement fermés sans pièce de commande souple.

![images/switch-nc-series\_fr.png]()

Figure 11. Contacts normalement fermés<span id="cap:Contacts-Normalement-Fermes"></span>

Câblage de contacts NC en série (schéma simplifié)

![images/switch-no-parallel\_fr.png]()

Figure 12. Contacts normalement ouverts<span id="cap:Contacts-Normalement-Ouverts"></span>

Câblage de contacts NO en parallèle (schéma simplifié)

Les combinaisons suivantes sont permises dans Stepconf:

-   Les contacts d’origine machine de tous les axes combinés.

-   Les contacts de fin de course de tous les axes combinés.

-   Les contacts de fin de course d’un seul axe combinés.

-   Les contacts de fin de course et le contact d’origine machine d’un seul axe combinés.

-   Un seul contact de fin de course et le contact d’origine machine d’un seul axe combinés.

Les deux dernières combinaisons sont également appropriées quand le type contact + origine est utilisé.

Assistant graphique de configuration Mesa
-----------------------------------------

<span id="cha:Assistant-graphique-PNCConf"></span>

L’assistant de configuration PNCconf couvre toute la gamme des cartes d’entrées/ sorties Mesa et jusqu'à trois ports parallèles. Il est destiné à la configuration de systèmes à servomoteurs en boucle fermée ou de systèmes à moteurs pas à pas avec pilotage *matériel* externe. Il utilise une approche d’assistance similaire à celle de StepConf qui est lui, utilisé pour configurer les systèmes avec pilotage *logiciel* des moteurs pas à pas, au travers de ports parallèles. Pour une machine n’utilisant qu’un à trois ports parallèles standards, le logiciel StepConf est un assistant mieux adapté.

L’assistant PNCconf permet de produire des configurations avancées sans connaitre quoi que ce soit de HAL.

PNCconf en est encore au stade du développement (Beta 1) il peut exister quelques bogues et manques de fonctionnalités. Merci de rapporter les bogues et les suggestions à la page du forum ou par courriel à la liste de diffusion.

Il y a deux manières d’utiliser PNCconf:

1.  La première consiste à l’utiliser pour configurer le système et si, par la suite, certaines options doivent être modifiées, il suffira alors de recharger PNCconf et d’apporter les modifications aux réglages. Cela fonctionne bien si la machine est assez standard, pour les machines particulières il est possible d’ajouter à la main les nouvelles fonctionnalités. PCNConf est bien adapté pour cette utilisation.

2.  La seconde consiste à l’utiliser pour construire une configuration la plus proche possible de ce qui est souhaité, puis à la modifier à la main pour l’adapter aux besoins. C’est le bon choix si les besoins de modifications vont au-delà des possibilités de PNCconf ou pour expérimenter et en apprendre plus sur Machinekit.

Il est possible de naviguer dans l’assistant, revenir sur des pages, annuler des choix, obtenir de l’aide et des diagrammes puis enfin, de valider la configuration par la page de sortie du programme.

NDT: Certaines divergences entre cette traduction et l’aspect réel de l’interface peuvent apparaitre pendant la phase de développement de PNCconf. Elles disparaitront quand le logiciel sera finalisé.

### Instructions pas à pas

Démarrer le programme depuis le menu *Applications → CNC → PNCconf* ou depuis un terminal avec la commande:

    pncconf

![images/pncconf-splash\_fr.png]()

Figure 13. Écran d’accueil de PnCConf

### Créer ou éditer une configuration

![images/pncconf-file\_fr.png]()

Figure 14. Créer ou éditer

Il est possible de créer une nouvelle configuration ou d’en modifier une existante. Si *Modifier une configuration déjà créée* est choisi, suivi d’un clic sur *Suivant*, un sélecteur de fichier apparait pour choisir la configuration existante à modifier. Par défaut, Pncconf présélectionne le dernier fichier enregistré. Il est possible de cocher les options *Créer un lien sur le bureau* qui créera un lien sur le bureau pointant sur ce nouveau fichier de configuration, *Créer un lanceur* qui créera un lanceur sur le bureau qui démarrera Machinekit dans cette configuration. Si ces options ne sont pas utilisées, le nouveau fichier de configuration se trouvera dans le dossier *~/machinekit/configs*. Il est toujours possible de lancer Machinekit normalement et de sélectionner la configuration souhaitée dans la liste.

### Informations machine

![images/pncconf-basic\_fr.png]()

Figure 15. Informations machine

*Éléments de base*

 Nom de la machine   
Préciser ici le nom de la machine à configurer, les espaces dans les noms seront remplacés par des ***\_*** (en règle générale, Linux n’aime pas les espaces dans les noms de fichiers).

 Configuration des axes   
Cette liste déroulante précise le nombre d’axes de la machine, sélectionner selon la machine XYZ (fraiseuse 3 axes), XYZA (fraiseuse 4 axes) ou XZ (tour).

 Unité machine   
Définit l’unité de mesure utilisée par la machine, pouce ou millimètre, toutes les données introduites par la suite devront être données dans l’unité choisie ici.

    Les valeurs introduites par défaut dans cet assistant ne sont pas converties
    automatiquement dans l'unité choisie ici, bien vérifier toutes ces valeurs.

*Temps de réponse de l’ordinateur*

 Période servo actuelle   
La période d’asservissement. C’est l’horloge du système. La latence donne la variation de cette horloge. Machinekit demande une chronologie serrée et cohérente, sinon des problèmes surviendront.

Quelques explications: Machinekit requiert et utilise un système d’exploitation temps réel, ce qui signifie qu’il a une latence très faible et un temps de réponse très court. Les évènements arrivent avec précision dans le temps quand Machinekit nécessite pour ses calculs, de ne pas être interrompu par des demandes de priorité inférieure (interruptions) comme des saisies au clavier ou des demandes d’affichage.

Le test de latence est très important, il est un élément clef qui doit être effectué au plus tôt. Heureusement les cartes Mesa se chargent des tâches critiques en temps de réponse, comme le comptage d’impulsions, la génération de PWM et cela leur permet de supporter une latence supérieure à celle d’un système utilisant les ports parallèles de la carte mère.

Le test standard dans Machinekit, consiste à vérifier la latence de base du PC. Un appui sur le bouton *Test de latence* lancera le test de latence, il est également possible de le lancer depuis le menu *application → cnc → latency test*. Une fenêtre s’ouvre dans laquelle s’affichent les temps mesurés. Ce test doit fonctionner plusieurs minutes, en fait, le plus longtemps possible. 15 minutes est un minimum. Pendant le test, essayer d’utiliser le plus possible l’ordinateur, le réseau, le port USB, les disques durs, l’affichage. Observer et noter si une action particulière dégrade le temps de latence. A la fin, il sera possible de connaitre la *base period jitter*, la latence de base. Une valeur en dessous de 20000 est excellente et permet une génération rapide des impulsions de pas avec cette machine.+ 20000 à 50000 est assez bon pour la génération de pas.
 50000 à 100000 ce n’est pas très bon mais la machine peu encore servir pour la génération de pas avec une carte ayant des temps de réponse courts.
 Plus grand que 100000, la machine n’est pas utilisable pour cette fonction

Si la latence est médiocre ou si des problèmes intermittents surviennent régulièrement il sera toujours possible de l’améliorer.

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
<td align="left">Il y a une liste d'équipements et de leurs temps de latence sur <a href="http://wiki.machinekit.org/cgi-bin/wiki.pl?Latency-Test">le wiki de Machinekit</a><br />
 SVP, pensez à ajouter vos infos à la liste. Sur cette page il y a des liens vers des informations pour résoudre certains problèmes de latence.</td>
</tr>
</tbody>
</table>

Maintenant que nous avons un temps de latence acceptable nous devons choisir une période d’asservissement (Période servo actuelle). Dans la plupart des cas une période d’asservissement de 1000000ns est bonne, cela donne un taux de calcul de 1 kHz soit 1000 calculs par seconde. Si le système d’asservissement est construit en boucle fermée avec contrôle de couple (courant) plutôt que de vitesse (tension) le taux sera meilleur, quelque chose comme 5000 calculs par seconde (5 kHz). Le problème avec l’abaissement de la période, c’est qu’elle laisse moins de temps disponible à l’ordinateur pour faire d’autres choses. Typiquement la réponse de l’affichage (GUI) est moins bonne. Il faut choisir un équilibre. Garder à l’esprit que sur un mécanisme en boucle fermée, une modification de la période d’asservissement nécessitera de réajuster l’ensemble des paramètres de la boucle.

*Ports et cartes d’entrées/sorties*

PNCconf est capable de configurer une machines avec deux cartes Mesa et trois ports parallèles. Les ports parallèles ne sont utilisables que pour des actions simples et peu rapide.

 Mesa   
Au moins une carte Mesa doit être choisie. PNCconf ne peut pas configurer les ports parallèles pour des codeurs, des signaux de pas ou pour la génération de signaux PWM. La liste de sélection des cartes Mesa présentes dans la liste de sélection est construite selon les micros logiciels des cartes trouvées sur le système. Il existe des options permettant d’ajouter des micros logiciels personnalisés ou pour ignorer (blacklister) certaines versions de micros logiciels ou certaines cartes, en utilisant un fichier de préférences. Si aucune carte n’est détectée PNCconf affichera un avertissement et utilisera des valeurs par défaut mais aucun test ne sera possible. Il faut noter que, si plusieurs cartes Mesa sont utilisées, il n’existe aucun moyen de déterminer laquelle sera la carte N°0 ou N°1 et il sera indispensable de le tester. Déplacer les cartes dans les ports PCI, peut changer leur ordre. Si la configuration est créée pour deux cartes, elles doivent être installées pour que les tests fonctionnent.

 Ports parallèles   
Jusqu'à 3 ports parallèles, appelés parports par Mesa, peuvent être utilisés comme de simples entrées sorties. L’adresse du port parallèle doit être définie. Il est possible soit d’entrer le N° du port parallèle selon le système de numérotation de Linux 0, 1 ou 2 ou, d’entrer l’adresse réelle en hexadécimal. Les adresses des ports parallèles intégrés à la carte mère son le plus souvent aux adresses 0x0378 et 0x0278, elles peuvent être trouvées dans la configuration du BIOS. Le Bios s’ouvre en enfonçant une touche du clavier au tout début du cycle de démarrage de l’ordinateur, souvent (Del ou F2) se reporter au document de la carte mère. Sur une des pages du BIOS, il est possible de choisir l’adresse des ports parallèles et de définir leurs modes de fonctionnement comme SPP, EPP, etc, sur certains ordinateurs cette information est affichée pendant quelques secondes lors du démarrage du PC. Pour les ports parallèles sur carte PCI les adresses sont trouvées en cliquant sur le bouton *Outil d’aide à la recherche d’adresse de ports parallèles* qui affichera la liste des périphériques PCI découverts. Dans cette liste, se trouvera une référence aux ports parallèles avec une liste d’adresses. Une de ces adresses doit fonctionner. Noter que tous les ports parallèles PCI ne fonctionnent pas correctement en EPP. Chaque port peut être sélectionné comme *Entrée* pour augmenter le nombre d’entrées sur ce port ou *Sortie* pour un maximum de sorties. Par défaut, les ports parallèles sont configurés avec leurs broches 2 à 9 en *Sortie*.

*Liste des interfaces graphiques*

Spécifie les interfaces utilisateur graphiques que Machinekit peut utiliser. Chacune dispose d’options particulières.

*AXIS*

-   Supporte les tours.

-   C’est l’interface la plus utilisée et la plus développée.

-   Elle est conçue pour être utilisée à la souris est avec un clavier.

-   Elle est basée sur tkinter et intègre donc PYVCP (contrôle visuel python).

-   Elle dispose d’un affichage graphique en 3D.

-   Elle est intégrable sur les barres de tâches ou sur le bureau.

*TOUCHY*

-   Touchy est une interface conçue pour les écrans tactiles.

-   Elle ne nécessite que quelques interrupteurs physiques et une manivelle de jog.

-   Elle nécessite les boutons *Départ cycle*, *Abandon*, *Marche par pas*.

-   Elle nécessite également un bouton sélecteur d’axe sur le jog.

-   Elle est basée sur GTK et intègre naturellement GladeVCP (création de panneaux de contrôle).

-   Elle permet d’intégrer les panneaux de contrôle virtuels (VCP).

-   Elle n’a pas de fenêtre de suivi du parcours d’outil.

-   L’aspect peut être modifié avec des thèmes personnalisés.

\*MINI\_

-   Est fourni en standard sur les machines Sherline.

-   N’utilise pas d’arrêt d’urgence (ESTOP).

-   Pas de possibilité d’intégrer un panneau de contrôle.

*TkMachinekit*

-   Contraste élevé grâce à un fond bleu.

-   Fenêtre graphique séparée.

-   Pas d’intégration de panneau de contrôle possible.

### Contrôles externes

Cette page permet de sélectionner des contrôles externes pour la commande manuelle de déplacement des axes (jog) ou des curseurs des correcteurs de vitesse.

![images/pncconf-external\_fr.png]()

Figure 16. Contrôles externes

Si une manette de jeu externe est sélectionnée pour le jog, il faudra toujours la connecter à Machinekit avant de démarrer celui-ci. Si la manette est analogique il faudra probablement ajouter du code personnalisé à HAL. Les manivelles de jog à vernier et micro impulsion nécessitent d'être connectées à une carte Mesa sur un compteur de codeur. Pour les correcteurs de vitesses externe il est possible d’utiliser un mécanisme à générateur d’impulsions ou à commutation comme un commutateur rotatif. Les boutons externes peuvent être ceux d’une manette de jeu.

 Joystick USB pour le jog   
Demande des règlages spécifiques personnalisés pour être installé dans le système. Il s’agi d’un fichier qui est utilisé par Machinekit pour se connecter à la liste des périphériques Linux. PNCconf aidera à la construction de ce fichier.

-   Ajouter règle dispositif: s’utilise pour configurer un nouveau périphérique en suivant les instructions. Le périphérique doit être branché et disponible.

-   test dispositif: permet de charger un périphérique, d’afficher les noms de ses broches et de visualiser ses fonctions avec l’outil halmeter.

-   Rechercher règles pour le dispositif: va rechercher les règles dans le système, utilisable pour trouver le nom des périphériques déjà construits avec PNCconf.

Les manettes de jeu utilisées en jog utilisent HALUI et le composant hal\_input.

 Boutons de jog externes   
Permet le jog de l’axe avec de simples boutons à une vitesse spécifiée. Probablement mieux adapté pour le jog en vitesse rapide.

 Manivelle de jog externe   
Permet d’utiliser un générateur d’impulsions manuel pour faire du jog sur les axes de la machine. Les manivelles à impulsions (MPG) sont souvent présentes sur les machines de bonne qualité. Elles délivrent en sortie des impulsions en quadrature qui peuvent être comptées avec un compteur de codeur MESA. PNCconf gère une manivelle par axe ou une manivelle partagée entre les axes. Il permet la sélection des vitesses de jog en utilisant des commutateurs rotatifs. L’option de sélection des incréments de jog utilise le composant mux16. Ce composant dispose d’options telles que l’anti-rebond et l’utilisation du code Gray pour filtrer l’entrée physique du commutateur.

 Correcteurs de vitesses   
PNCconf permet de modifier les vitesses d’avances ou de broche en utilisant une manivelle à micros impulsions ou un commutateur rotatif. Les incréments sont configurables.

### Configuration des GUI

Ici il est possible de configurer l’interface graphique utilisateur (GUI), lui ajouter des panneaux de commande virtuels (VCP) et définir certaines options d’Machinekit.

![images/pncconf-gui\_fr.png]()

Figure 17. Configuration des GUI

*Options des interfaces graphiques*

 Valeurs communes par défaut   
Permet de fixer des valeurs générales par défaut, communes à toutes les interfaces graphiques.

 Options par défaut d’AXIS   
Ici se trouve les options spécifiques à AXIS. Si une des options *Taille*, *Position* ou *Forcer à maximiser* et choisie, il sera possible de modifier les valeurs de vitesse minimale ou maximale, le choix de l'éditeur de fichiers, la géométrie de la machine affichée. Ensuite, PNCconf demandera si il peut écraser le fichier de préférences (.Axisrc). Ce qui écrasera les données qui aurait été ajoutées extérieurement dans ce fichier.

 Touchy   
Ici se trouve les options spécifiques à Touchy. La plupart des options de Touchy peuvent être modifiées dans la page des préférences de l’application même quand elle est en marche. Touchy utilise GTK pour dessiner son écran, et supporte les thèmes GTK. Les thèmes modifient l’apparence et l’ergonomie du programme. il est possible de télécharger des thèmes depuis le net ou de les modifier soit-même. Il y a déjà une liste des thèmes utilisables sur le système. PNCconf permet de modifier facilement le thème par défaut.

*Panneaux de contrôle virtuels*

Les panneaux de contrôle virtuels permettent d’ajouter des contrôles et des afficheurs personnalisés. AXIS et Touchy peuvent intégrer ces contrôles dans une zone déterminée de leur écran. Il y a deux sortes de panneaux de contrôle (VCP), pyVCP qui utilise *Tkinter* pour dessiner l'écran ou GLADE VCP qui utilise *GTK*.

 Panneau PyVCP   
PyVCP est un écran construit par un fichier XML. Il ne peut pas être construit à la main. Les PyVCP s’intègrent naturellement avec AXIS car ils utilisent tous les deux Tkinter. Des *HAL pins* sont créées pour que l’utilisateur puisse les connecter dans son fichier HAL personnalisé. Il existe par exemple, un tachymètre pour la vitesse de broche ou un panneau de boutons XYZ pour le jog, l’utilisateur peut les utiliser tel quel ou les reconstruire à son gout. Sélectionner un fichier vide où les contrôles (widgets) personnels seront enregistrés ou sélectionner un des modèles d’affichage prêts à l’emploi, PCCcong établira alors lui-même les bonnes connexions avec HAL. Si AXIS est utilisé, le panneau sera intégré sur le côté droit. Si AXIS n’est pas utilisé, le panneau sera distinct de l'écran frontal. Il est possible d’utiliser les options de géométrie et de dimensions et de déplacer le panneau, par exemple si le système le permet vers un second écran. Si le bouton *Ouvrir un panneau simple* est pressé, les données de géométrie et de dimensions seront utilisées et le panneau affiché.

 Panneau GladeVCP   
GladeVCP s’intègre naturellement à l’intérieur de l'écran TOUCHY car ils utilisent tous les deux GTK pour leurs interfaces, mais en modifiant le thème de GladeVCP il se fond très bien dans AXIS. Il utilise un éditeur graphique pour créer ses fichiers XML. Des *HAL pins* sont créées, que l’utilisateur pourra connecter dans son fichier HAL personnalisé. GladeVCP permet aussi une interaction de programmation beaucoup plus sophistiquée et compliquée, ce qui n’est actuellement pas possible par PNCconf. Voir le chapitre sur GladeVCP et [la création d’interfaces graphiques](#cha:GladeVCP)

PNCconf propose des exemples de panneaux à utiliser tel quel ou à reconstruire. Avec PNCconf, GladeVCP permettra de sélectionner différentes options d’affichage sur le modèle. Sous *Echantillon d’options* sélectionner les options souhaitées. Les boutons de zéro utilisent des commandes HALUI qui pourront être modifiées ultérieurement dans la section HALUI. Le bouton *Toucher Z automatique* nécessite le programme *Touch-off* de classicladder et que l’entrée de sonde soit sélectionnée. Il faut aussi un palpeur qui peut être réalisé avec une plaque conductrice reliée à la masse. Pour avoir une idée sur la façon dont cela fonctionne, voir:

Sous *Options d’affichage*, les options de géométrie et de dimensions permettent de déplacer le panneau, par exemple vers un second écran, si le système le permet. Sélectionner un thème GTK pour définir l’aspect du panneaux. En général, on le souhaite identique à l’aspect de l'écran frontal. Le panneau créé et ses options seront visibles en appuyant sur le bouton *Ouvrir un panneau simple*. GladeVCP placé sur l'écran frontal permet de sélectionner la position du panneau sur celui-ci. Il peut fonctionner de manière autonome ou avec AXIS, il peut être au centre ou sur le côté droit, avec Touchy il peut être au centre.

*Défauts et options*

 Prise d’origine requise avant tout mouvement   
Pour pouvoir déplacer la machine sans passer par une recherche du point d’origine machine décocher la case. Dans ce cas la plus grande vigilance est nécessaire pour ne pas percuter une limite.

 Dialogue pour le changement d’outil   
Permet le choix entre l’utilisation d’un dialogue de changement d’outil et l’exportation d’un signal standard pour utiliser un changeur d’outils automatique externe et la table d’outils.

 Laisser tourner la broche pendant le changement d’outil   
Laisse tourner la broche pendant le changement d’outil. Utile pour les tours.

 Forcer la prise d’origine individuelle en manuel   
Oblige à effectuer la prise d’origine individuelle de chaque axe en manuel.

 Relever la broche avant le changement d’outil   
Met la broche en position haute avant le changement d’outil.

 Récupérer position jointure après arrêt   
Mémorise la position des articulations lors de l’arrêt. Utilisé pour les machines a cinématique complexe.

 Changeur d’outil à position aléatoire   
Utilisé pour les changeurs d’outils qui ne reçoivent pas toujours les outils au mêmes emplacements. Des codes HAL doivent être ajoutés pour le support de ces changeurs d’outils.

### Configuration Mesa

Les pages de configuration Mesa permettent d’utiliser les différents micros logiciels. Sur la page de configuration, si une carte Mesa a été sélectionnée, ici s’effectue le choix du micro logiciel parmi ceux disponibles, puis le choix et le paramétrage des composants nécessaires à la machine.

![images/pncconf-mesa-config\_fr.png]()

Figure 18. Configuration Mesa

 Adresse du port parallèle MESA   
Un port parallèle est utilisé seulement avec la carte Mesa 7i43. Les ports parallèles sur la carte mère ont généralement les adresses 0x378 et 0x278 il est possible de trouver l’adresse sur la page du BIOS. Le 7i43 nécessite de programmer le port parallèle dans le mode EPP, encore une fois cela se configure dans la page du BIOS. Si un port parallèle sur carte PCI est utilisé, les adresses peuvent être recherchées en utilisant le bouton de recherche sur la page de base de PNCConf.

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
<td align="left">Noter que beaucoup de cartes PCI ne prennent pas en charge le protocole EPP correctement.</td>
</tr>
</tbody>
</table>

 Fréquence de base PWM, PDM et 3PWM   
<span class="footnote">
\[PDM: acronyme de Modulation de Densité d’Impulsions, PWM: acronyme de Modulation de Largeur d’Impulsions\]
</span> Règle l'équilibrage entre entrainement et linéarité. Si des cartes filles Mesa sont utilisées, les documents de celles-ci devraient donner des recommandations. Il est important de les suivre pour éviter des dommages et obtenir les meilleures performances.

 Par exemple….   
-   La carte 7i33 demande un PDM et une fréquence de base de 6 mHz.

-   La carte 7i29 demande un PWM et une fréquence de base de 20 Khz.

-   La carte 7i30 demande un PWM et une fréquence de base de 20 Khz.

-   La carte 7i40 demande un PWM et une fréquence de base de 50 Khz.

-   La carte 7i48 demande un PWM et une fréquence de base de 24 Khz.

 Délai du chien de garde   
Définit le délai durant lequel la carte Mesa va attendre avant de déconnecter les sorties si la communication est interrompue avec l’ordinateur. Les carte Mesa utilisent sur ce contact un niveau actif bas ce qui signifie que lorsque la sortie est activée son niveau logique est à 0 et si la sortie est inactive son niveau logique est à 1 soit environ 5 volts. S’assurer que l'équipement est en sécurité quand le chien de garde est déclenché.

 Nombre de codeurs
 Nombre de générateur de PWM
 Nombre de générateur de PAS   
Il est possible de choisir les composants en dé-sélectionnant ceux qui sont inutilisés. Les types de composants disponibles varient selon le micro logiciel et les cartes installées. Si des composants ne sont pas sélectionnés, des broches GPIO seront gagnées. Si des cartes filles sont utilisées, garder à l’esprit que les pins que les cartes utilisent ne doivent pas être dé-sélectionnées. Par exemple, certain micros logiciels supportent deux cartes 7i33, si une seule est installée, il est possible de dé-sélectionner assez de composants non nécessaires pour utiliser le connecteur qui était prévus pour la seconde 7i33. Les composants sont dé-sélectionnés numériquement en commençant par le plus grand nombre d’abord, puis en descendant sans en sauter. Si en faisant cela, les composants ne sont pas là où il devraient, alors il faut utiliser un micro logiciel différent. Le micro logiciel dicte où, quoi et les nombre maximum de composants. Un micro logiciel personnalisé est possible en le demandant gentiment aux développeurs Machinekit et Mesa. Les micros logiciels dans PNCconf nécessitent des procédures spéciales et ce n’est pas toujours possible. Bien que nous essayons de rendre PNCconf aussi souple que possible. Après avoir choisi toutes les options, appuyer sur le bouton *Accepter le changement de composants* et PNCconf mettra à jour les pages de configuration des E / S. Seuls les onglets nécessaires seront affichés pour les connexions disponibles, selon les documents de Mesa.

### Réglages des E/S Mesa

Les onglets sont utilisés pour configurer les broches d’entrée et de sortie des cartes Mesa. PNCconf permet de créer des noms de signaux personnalisés à utiliser dans les fichiers de HAL personnalisés.

![images/pncconf-mesa-io2\_fr.png]()

Figure 19. Réglages des E/S Mesa C2

Sur cet onglet, avec ce micro logiciel, les composants sont liés à l’installation d’une carte fille 7i33, généralement utilisée avec des servomoteurs en boucle fermée. Noter que les numéros de composant des codeurs, des compteurs et des pilotes PWM ne sont pas dans l’ordre numérique. Cela fait suite aux exigences de l’architecture des cartes filles.

![images/pncconf-mesa-io3\_fr.png]()

Figure 20. Réglages des E/S Mesa C3

Sur cet onglet, il n’y a que des broches GPIO. Noter les numéros à trois chiffres, ils correspondent au numéros des *HAL pins*. Les broches GPIO peuvent être sélectionnées comme des entrées ou des sorties et elles peuvent être inversées.

![images/pncconf-mesa-io4\_fr.png]()

Figure 21. Réglages des E/S Mesa C4

Sur cet onglet, il y a un mélange entre des broches GPIO et des générateurs de pas. Les sorties générateur de pas et de direction peuvent être inversées. Noter que l’inversion d’un signal Step Gen modifie les délais de pas, il doivent correspondre à ce que le contrôleur attend.

*Configuration des ports parallèles*

![images/pncconf-parport\_fr.png]()

Les ports parallèles peuvent être utilisés pour de simples E/S similaires aux broches GPIO Mesa.

### Configuration des axes

![images/pncconf-axis-drive\_fr.png]()

Figure 22. Configuration des axes

Cette page permet de configurer et tester un moteur combiné ou non à un codeur. Si un servomoteur est utilisé, un test en boucle ouverte est disponible. si un moteur pas à pas est utilisé, un test de réglage est disponible.

 Test en boucle ouverte   
Le test en boucle ouverte est important car il confirme la bonne direction du moteur et du codeur. Le moteur doit se déplacer dans le sens positif sur l’axe lorsque le bouton est pressé dans le sens positifs et aussi le codeur doit compter dans le même sens. Le mouvement de l’axe doit suivre les normes conventionnelles des machine-outil, sinon l’affichage graphique de l’axe n’aura pas de sens. Espérons que la page d’aide et le diagramme vous aideront à comprendre cela. Noter que les directions des axes sont celles du mouvement de l’outil et non celle du mouvement de la table. Il n’y a pas de rampe d’accélération lors du test en boucle ouverte, il convient donc de commencer avec une valeur faible du DAC. Déplacer l’axe sur une distance connue, confirmera la bonne mise à l'échelle du codeur. Le codeur doit compter dans le même sens, même sans la puissance sur le moteur, mais cela dépend de la manière dont le codeur est alimenté.

<span class="red">AVERTISSEMENT:</span> Si le moteur et le codeur ne comptent pas dans le même sens, le servomoteur sera incontrôlable et s’emballera lors de l’utilisation en boucle fermée sous régulation PID.<span class="footnote">
\[ PID: acronyme de Proportionnelle, Intégrale, Dérivée. Ce sont les 3 composantes de la régulation en boucle fermée de type PID.\]
</span>

Pour le moment les paramètres PID ne peuvent pas être testés dans PNCconf, ces réglages sont vraiment, pour quand vous rééditerez une configuration pour y mettre vos paramètres PID testés…

 Echelle du DAC   
<span class="footnote">
\[ DAC, acronyme pour Convertisseur Analogique Digital\]
</span> Deux valeurs de mise à l'échelle, *Max Output* et *Offset* sont utilisées pour linéariser le DAC.

 Théorie   
Ces deux valeurs sont les facteurs d'échelle et d’offset de la sortie vers l’amplificateur moteur, de l’axe. La deuxième valeur, l’offset, est soustraite de la sortie calculée (en Volts) et divisée par la première valeur (le facteur d'échelle), avant d'être écrite dans le DAC. La valeur d'échelle (Scale) s’exprime en Volts/Volts de sortie du DAC. Le décalage (offset) s’exprime en Volts. Elles peuvent être utilisées pour linéariser le DAC. Plus précisément, lors de l'écriture des sorties, Machinekit convertit d’abord la valeur effective de la sortie concernée, qui est en quasi-unités SI, en valeurs brute d’actionneur. Par exemple, des Volts pour un amplificateur DAC. La valeur de l'échelle peut être obtenue en analysant l’unité c’est-à-dire en déterminant le rapport \[sortie unités SI\]/\[unités actionneur\]. Par exemple, sur une machine avec un amplificateur en mode vitesse, qui fourni 1 Volt pour une vitesse résultante de 250 mm/s. Noter que les unités de l’offset sont en unités machine, ici des mm/s et qu’elles sont pré-soustraites des lectures capteur. La valeur de cet offset est obtenue en trouvant la valeur de sortie qui donne 0,0 sur la sortie de l’actionneur. Si le DAC est linéarisé, cet offset est normalement de 0,0. L'échelle et l’offset peuvent être utilisés pour linéariser le DAC, il en résultera des valeurs qui reflèteront les effets combinés du gain de l’amplificateur, de la non-linéarité du DAC, des unités du DAC, etc. Pour le faire, suivre cette procédure:

Construire une table de calibration pour la sortie.

Piloter le DAC avec la tension souhaitée et mesurer le résultat:

Mesure des tensions de sortie:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><strong>Sortie brute</strong></p></td>
<td align="left"><p><strong>Mesure</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p>-10</p></td>
<td align="left"><p><strong>-9.93</strong></p></td>
</tr>
<tr class="odd">
<td align="left"><p>-9</p></td>
<td align="left"><p><strong>-8.83</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p><strong>-0.96</strong></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p><strong>-0.03</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p>9</p></td>
<td align="left"><p><strong>9.87</strong></p></td>
</tr>
<tr class="odd">
<td align="left"><p>10</p></td>
<td align="left"><p><strong>10.07</strong></p></td>
</tr>
</tbody>
</table>

-   Par la méthode des moindres carrés, déterminer les coefficients **`a`**, **`b`** tels que **`Mesure=a*Sortiebrute+b`**

-   Noter que nous voulons une sortie effective telle que la valeur mesurée soit identique à la consigne. Cela signifie

    -   **`cmd=a*Sortiebrute+b`**

    -   **`Sortiebrute=(cmd-b)/a`**

-   Par conséquent, les coefficients **`a`** et **`b`** de l’ajustement linéaire peuvent être utilisés directement comme échelle et offset pour le contrôleur.

     Valeur maximale de sortie   
    La valeur maximale pour la sortie de compensation PID qui est écrite sur l’ampli moteur, exprimée en volts. La valeur de sortie calculée est alignée sur cette limite. La limite est appliquée avant la mise à l'échelle des unités de sortie effective. La valeur est appliquée de manière symétrique aux deux limites, positive et négative.

     Test de réglage   
    Le test de réglage ne fonctionne, malheureusement, qu’avec les systèmes à base moteur pas à pas. Encore une fois vérifier que les directions de déplacements sur l’axe sont correctes. Puis tester le système en déplaçant l’axe d’avant en arrière, si l’accélération ou la vitesse maximum sont trop élevées, des pas seront perdus. Attention: Au cours de ce déplacement manuel garder à l’esprit que la distance d’arrêt est inversement proportionnelle à l’accélération et qu’avec une accélération faible il faut du temps et de la distance pour arrêter l’axe. Les fins de course ne sont pas fonctionnels pendant ce test. Un temps de pause peut être défini entre chaque mouvement d’essai. Cela permet de vérifier la position de l’axe et de voir si des pas sont perdus.

     Timing des moteur pas à pas   
    La séquence de signaux des sorties pas a pas, doit être adaptée aux exigences du pilote des moteurs. Pncconf propose par défaut, certaines de ces séquences et il est possible de les personnaliser. Voir <http://wiki.machinekit.org/cgi-bin/wiki.pl?Stepper_Drive_Timing> pour y trouver des séquences pour le matériel le plus commun (n’hésitez pas à ajouter celles que vous avez expérimenté). En cas de doute utiliser une valeur élevée comme 5000, cela ne fera que limiter la vitesse maximale.

     Contrôle de moteur Brushless   
    Ces options sont utilisées pour permettre le contrôle bas niveau des moteurs *brushless* avec un micro logiciel spécial et des cartes filles. Elles permettent également la conversion des capteurs à effet Hall d’un fabricant à l’autre. Ce n’est que partiellement pris en charge et aura besoin d’une intervention pour terminer les connexions de HAL. Contacter la mail-liste ou un forum pour avoir de l’aide.

![images/pncconf-scale-calc\_fr.png]()

Figure 23. Calcul de l'échelle d’axe

Les paramètres d'échelle peuvent être saisis directement ou, on peut utiliser le bouton *calculer échelle* pour être assisté. Utiliser alors les cases à cocher pour sélectionner les calculs appropriés. Noter que *Dents des poulies* exige le nombre de dents et non le rapport de réduction. *Rapport de réduction*, le rapport de réduction est exactement le contraire, il exige le rapport entre poulie menante et poulie menée (Entrée/Sortie). Si l'échelle à déjà été calculée manuellement, il est possible de la saisir directement sans passer par l’assistant.

![images/pncconf-axis-config\_fr.png]()

Figure 24. Configuration des axes

Se référer également à l’onglet diagramme pour deux exemples de disposition des contacts de fin de course d’origine machine et de limites. Ce sont deux exemples parmi les nombreuses façons différentes de placer ces contacts.

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
<td align="left">Il est très important de commencer avec l’axe se déplaçant dans la bonne direction sinon l’acquisition du point d’origine est impossible !</td>
</tr>
</tbody>
</table>

Se souvenir que les directions positives et négatives se référent toujours à l’outil et jamais à la table.

 Sur une fraiseuse classique   
-   Lorsque la table se déplace vers l’opérateur, c’est la direction positive de l’axe Y.

-   Lorsque la table se déplace à gauche, c’est la direction positive de l’axe X.

-   Lorsque la table se déplace vers le bas, c’est la direction positive de l’axe Z.

-   Lorsque la tête se déplace vers le haut, c’est aussi la direction positive de l’axe Z.

 Sur un tour classique   
-   Lorsque l’outil se déplace à droite, en s'éloignant du mandrin, c’est le sens positif de l’axe Z.

-   Lorsque l’outil se déplace vers l’opérateur, c’est le sens positif de l’axe X.

-   Certains tours ont un axe X opposé, dans ce cas l’outil est à l’arrière, cela fonctionne bien, mais l’affichage graphique d’AXIS ne peut pas refléter cette configuration.

Lorsque des contacts d’origine machine et des contacts de fin de course sont utilisés, Machinekit attend des signaux de HAL au niveaux haut lorsque le contact est actionné. Si le signal d’un fin de course est inversé, Machinekit détectera en permanence que la machine est en bout de course. Si la logique de recherche du contact d’origine machine est mauvaise (fichier ini), Machinekit lancera la séquence de recherche d’origine machine de l’axe dans la mauvaise direction.

 Décider de l’emplacement des fins de courses   
Les fins de course de limite d’axe sont au delà des limites logicielles, ils protègent la machine en cas de problème électrique, par exemple, l’emballement d’un servomoteur. Les fins de course doivent être placés de manière à ce que l’axe ne puisse pas percuter une butée mécanique. Attention: si la distance d’activation du contact de fin de course est trop faible, avec l’inertie du mobile il pourra le dépasser. Les fins de course des limites d’axes, doivent être actifs à l'état bas et ils doivent aussi couper la puissance sur l’axe concerné. Le contact doit s’ouvrir à l’activation du fin de course. Utiliser un autre câblage est possible mais il est moins sécurisé. Il peut être nécessaire d’inverser le signal de HAL dans Machinekit pour avoir un état actif haut, TRUE signifie que le contact a été activé. Lorsqu’au démarrage de Machinekit un avertissement de limite et affiché même si l’axe n’est pas sur un des fins de course, le signal est probablement inversé. Utiliser HALMETER pour vérifier l'état du signal de HAL correspondant, par exemple, axis.0.pos-lim-sw-in, fin de course positif de l’axe X.

 Décider de l’emplacement des contacts d’origine machine   
Si des fins de course de limite d’axe sont utilisés, il est possible de les utiliser également comme contacts d’origine machine. Un contact d’origine machine séparé est utile si les axes sont longs et que le déplacement vers un fin de course dure trop longtemps pour un usage normale ou que le déplacement vers une extrémité présente des problèmes d’interférences avec le porte-pièce ou la pièce. Par exemple sur un tour, le déplacement en bout de banc n’est pas efficace pour un point d’origine machine et un contact placé vers le centre est certainement meilleur. Si codeur avec un index est utilisé, le contact agit comme point de référence et l’index suivant sera le point d’origine machine effectif.

 Décider de la position de l’origine machine   
L’origine machine dans Machinekit sert de référence à tous les systèmes de coordonnées utilisateur. Il n’y a pas d’emplacement particulier pour ce point. Seuls quelques G-codes accèdent au système de coordonnées machine (G53, G30 et G28). Si l’option de changement d’outil sur G30 est utilisée, placer l’origine machine à cet endroit peut être commode. Par convention, il est plus simple d’avoir l’origine machine sur le contact d’origine.

 Décider de la position finale de l’origine   
Ça consiste simplement à placer le chariot ou la broche à la position la plus commode après que Machinekit soit initialisé et que les points d’origines machine de chacun des axes lui soit connus.

 Définition des côtés positifs/négatifs et des longueurs de courses maximales   
Placer l’axe à l’origine. Faire un repère sur le mobile et un autre sur la partie fixe. Déplacer la machine jusqu’au contact de limite d’axe. Mesurer la distance entre les deux repères pour obtenir la longueur de déplacement maximale dans ce sens. Déplacer dans l’autre sens, sur le contact de limite de l’autre côté. Mesurer de nouveau les repères pour obtenir la longueur de déplacement maximale dans l’autre sens. Si l’origine machine est située sur une des limites d’axe, alors cette distance de déplacement sera évidemment de zéro.

 Point d’origine machine   
Ce point est le point de référence de la machine. (Ne pas confondre avec le point zéro de l’outil ou de la pièce). Machinekit référence tout à partir de ce point. Il doit être à l’intérieur des limites logicielles sinon la machine ne pourrait jamais l’atteindre. Machinekit utilise la position du contact d’origine machine pour calculer la position d’origine. Si la machine ne dispose pas de contact il faudra la positionner manuellement sur les points d’origine, cocher les axes l’un après l’autre et pour chacun, presser le bouton *POM des axes*. Dans Axis, le symbole indiquant que l’origine machine de l’axe est connue s’affichera alors à droite de la visu de l’axe concerné.

 Course de la table   
C’est la distance maximale que l’axe peut parcourir dans chaque direction. Ceci peut ou ne peut pas être mesuré directement de l’origine aux contacts de fin de course. Le cumul des courses positives et négatives sera égal à la longueur de course totale.

 Course positive   
C’est la distance depuis l’origine de l’axe, jusqu’au fin de course de limite du côté positif. Si l’origine de l’axe est placée sur le fin de course de limite positive, cette valeur est égale à zéro. Les valeurs possibles sont positives ou égales à zéro.

 Course négative   
C’est la distance depuis l’origine de l’axe, jusqu’au fin de course de limite du coté négatif. Ou la course totale moins la course positive. Si l’origine de l’axe est placée sur le fin de course de limite négative, cette valeur est de zéro. Les valeurs possibles sont négatives égales à zéro. Si la valeur entrée dans PNCconf n’est pas négative, elle sera déduite des autres valeurs.

 Position de l’origine   
C’est la position ou se termine la séquence de prise d’origine machine. Elle est référencée par rapport à l’origine et peut être positive, si cette position finale est du coté positif ou négative, si cette position finale est du coté négatif.

 Position du contact d’origine machine   
C’est la distance depuis le contact d’origine jusqu'à la position de l’origine. Il peut être négatif ou positif selon de quel côté de l’origine il est placé. Depuis ce point, si l’axe doit être déplacé dans la direction positive pour arriver à l’origine, alors la valeur sera négative, sinon elle sera positive. Si il est mis à zéro, l’origine sera à l’emplacement du contact (plus la distance éventuelle pour attendre l’index suivant, si une règle de mesure, ou un codeur de position avec index sont utilisés).

 Vitesse de recherche du contact d’origine machine   
Vitesse utilisée pendant le déplacement vers le contact d’origine machine en unités par minute.

 Direction de recherche du contact d’origine machine   
Direction de la recherche de l’origine machine. Négatif ou Positif selon le coté de l’axe où se trouve le contact d’origine machine.

 Vitesse d’acquisition du contact d’origine machine   
Vitesse lente de détection du contact d’origine machine, en unités par minute.

 Vitesse vers la position de l’origine   
Vitesse utilisée pour déplacer le mobile de la position d’acquisition du contact d’origine machine, vers la position finale de l’origine, en unités par minute. Si réglée à 0 c’est la vitesse de déplacement rapide qui sera utilisée.

 Direction d’acquisition du contact d’origine machine   
Direction d’acquisition de l’origine machine, peut être dans la même direction que la recherche, ou à l’opposé.

 Origine machine sur l’index du codeur   
Machinekit attendra l’impulsion d’index du codeur après l’acquisition du contact d’origine machine.

 Utiliser un fichier de compensation de jeu   
Permet de spécifier le nom et le type d’un fichier de compensation de jeu. Permet une compensation sophistiquée. Voir le manuel.

 Utiliser la compensation de jeu   
Permet de régler la compensation du jeu de la vis, ne peut pas être utilisé en même temps qu’un fichier de compensation. Voir le manuel.

![images/pncconf-diagram-lathe\_fr.png]()

Figure 25. Dessin d’aide à l’identification des axes et fins de course

Ce dessin devrait aider à comprendre un exemple de positionnement des contacts de fin de course et les directions standards sur un tour. Sur ce tour, l’axe Z a deux contacts de fin de course, le contact positif est utilisé également comme contact de prise d’origine machine. La position du zéro machine (origine machine de l’axe) est placée à la limite négative. Le bord gauche du chariot est la came qui active le fin de course de la limite négative et le côté droit, la came qui active le fin de course de la limite positive. Nous voyons que la position finale de l’origine se trouve à 4 pouces de distance de l’origine de l’axe, du côté positif. Si le chariot était déplacé jusqu'à la limite positive, nous mesurerions 10 pouces entre la limite négative et la came du côté négatif du chariot (fin de course bord gauche du chariot).

Configuration de la broche

Si un signal de contrôle de la broche est présent, cette page permet de le configurer.

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
<td align="left">Beaucoup d’options de cette page ne sont visibles que si les sélections appropriées ont été choisies dans les pages précédentes. Si des signaux de broche ont été sélectionnés, alors cette page est disponible pour les configurer.</td>
</tr>
</tbody>
</table>

![images/pncconf-spindle-config\_fr.png]()

Figure 26. Configuration de la broche

Cette page est semblable à la page de configuration des moteurs d’axe mais il y a quelques différences: À moins que l’on ait choisi un moteur pas à pas pour la conduite de la broche il n’y a pas d’accélération ni de limitation de vitesse. Il n’y a pas de support pour les changements de vitesse ni pour les gammes de vitesses. Si une option VCP d’affichage de vitesse broche est choisie, alors la *Vitesse broche atteinte*, *l'échelle*, *la vitesse* et *les réglages des filtres* seront visibles. L’information sur la vitesse de broche permet à Machinekit d’attendre que celle-ci ait atteint la vitesse de consigne, avant de déplacer les axes. C’est particulièrement pratique sur les tours, lors de l’utilisation d’une vitesse de coupe constante avec de grands changements de diamètre. Il exige un retour d’information par codeur ou par un signal de vitesse broche numérique, typiquement connecté à un variateur de vitesse (VFD).

En utilisant le retour d’information d’un codeur, il est possible de choisir une plage de *vitesse broche atteinte* comme tolérance de vitesse, au delà de laquelle, la vitesse broche sera admise comme étant la vitesse de consigne.

En utilisant le retour d’information d’un codeur, l’affichage de vitesse VCP peut être irrégulier, des filtres peuvent dans ce cas, être utilisés pour corriger l’affichage. L'échelle du codeur doit être réglée à la valeur *comptage codeur/rapport de réduction utilisé*. Si une seule entrée est utilisée pour le codeur de broche, la ligne suivante doit être ajoutée:

    setp hm2_7i43.0.encoder.00.counter-mode 1

(Changer le nom de la carte et le numéro de codeur selon besoins) dans le fichier HAL personnalisé. Lire la section codeurs dans Hostmot2 pour plus d’information sur les modes de comptage.

### Options avancées

Cette page permet de régler les commandes HALUI, de charger classicladder. Elle propose des exemples de programmes en Ladder. Si l’option GladeVCP a été choisie, comme pour la mise à zéro de l’axe sur l’origine pièce. Les commandes nécessaires s’afficheront. Voir le manuel de HALUI pour utiliser des commandes personnalisées halcmds. Parmi les exemples de programmes ladder: Le programme Estop permet de gérer un contact externe d’arrêt d’urgence ou permet à l’interface graphique de déclencher l’arrêt d’urgence. La commande périodique de la pompe du graissage centralisé est disponible.
 Le contact de mise au zéro pièce de l’axe Z (longueur d’outil) s’utilise avec une plaque de référence, le contact (touch-off) de GladeVCP et les commandes spéciales HALUI sont là pour permettre rapidement, une recherche de l’origine pièce.

Le programme série *modbus* est un squelette de programme, vierge, préréglé pour l’utilisation de classicladder avec le protocole série modbus. Voir la section classicladder dans le manuel.

![images/pncconf-advanced\_fr.png]()

Figure 27. Options avancées

### Composants de HAL

Cette page permet d’ajouter des composants de HAL supplémentaires qui seront utilisés dans les fichiers HAL personnalisés. De cette manière il n’est pas nécessaire d'éditer le fichier HAL principal en permettant malgré tout à l’utilisateur de définir ses propres composants.

![images/pncconf-hal\_fr.png]()

Figure 28. Composants de HAL

La première sélection est prévue pour les composants que pncconf utilise en interne. Il est possible de configurer pncconf pour qu’il charge les instances additionnelles pour votre fichier HAL personnalisé. Sélectionner le nombres d’instances dont a besoin le fichier de personnalisation et pncconf ajoutera ce qui est nécessaire. Si 2 composants sont nécessaires et que pncconf à besoin d’un composant interne, il chargera 3 composants et utilisera le dernier.

 Composants de commande personnalisés   
Cette sélection permettra de charger des composants de HAL que pncconf n’utilise pas. Ajoute les commandes loadrt ou loadusr dans l’entête *loading command*. Ajoute la commande addf dans l’entête *Thread command*. Les composants seront ajoutés au thread entre la lecture des entrées et l'écriture des sorties, dans l’ordre ou ils sont écrits dans thread command.

### Utilisation avancée de PNCConf

PNCconf fait de son mieux pour permettre un personnalisation souple à l’utilisateur, PNCconf supporte les noms de signaux particuliers, le chargement de composants personnalisés comme la personnalisation des fichiers de HAL et des microprogrammes. Il y a aussi les noms de signaux que PNCconf fournit, indépendamment des options choisies, pour les fichiers HAL personnalisés.

Avec une conception réfléchie, la plupart des personnalisations devraient fonctionner, même si des options doivent être modifiées par la suite dans PNCCONF. Finalement, si les personnalisations vont au-delà du périmètre de travail de PNCCONF, il sera possible d’utiliser PNCCONF pour construire une configuration de base, ou d’utiliser une des configurations fournies en standards par Machinekit et de l'éditer pour obtenir ce que est souhaité.

 Nom de signaux personnalisés   
Si un composant doit être connecté à quelque chose dans un fichier HAL personnalisé, écrire un nom de signal unique dans la boîte de dialogue. Certains composants ajouteront des suffixes au nom du signal personnalisé.

    Les codeurs ajoutent  < Nom personnalisé >:
    -position
    -count
    -velocity
    -index-enable
    -reset

    Les contrôles de moteurs pas à pas ajoutent:
    -enable
    -counts
    -position-cmd
    -position-fb
    -velocity-fb

    Les PWM ajoutent:
    -enable
    -value

    Les broches GPIO auront juste le nom du signal d'entrée qui leur est connecté.

De cette façon on peut établir des connexions à ces signaux dans les fichiers personnalisés de HAL et avoir toujours la possibilité de les déplacer plus tard.

 Charger un microprogramme personnalisé   
PNCconf cherche le microprogramme sur le système et cherche ensuite le fichier XML qu’il peut convertir et qu’il comprend. Ces fichiers XML sont seulement fournis pour les microprogrammes officiellement délivrés par l'équipe Machinekit. Pour utiliser un microprogramme personnalisé, il faut le convertir en tableau que PNCconf comprend et ajouter son chemin dans le fichier de préférences de PNCCONF. Par défaut le chemin recherché est sur le bureau, dans un dossier nommé *custom\_firmware* contenant un fichier nommé firmware.py.

Le fichier caché des préférence est dans le dossier home de l’utilisateur et se nomme .pncconf-preferences, pour l'éditer il faut sélectionner *Afficher les fichiers cachés*. On peut voir le contenu de ce fichier au premier démarrage de PNCCONF. Presser le bouton d’aide et regarder la page de sortie. Demander sur la liste de diffusion Machinekit ou sur le forum pour des renseignements pour convertir un microprogramme personnalisé. Tous les microprogrammes ne peuvent pas être utilisés avec PNCCONF.

 Fichiers HAL Personnalisés   
Il y a quatre fichiers personnalisés utilisables pour ajouter des commandes a HAL:

-   custom.hal est prévu pour les commandes HAL utilisées avant le chargement de l’interface graphique. Il est exécuté après le fichier HAL de configuration nommé : non-de-la-configuration.hal

-   custom\_postgui.hal est prévu pour les commandes qui doivent être exécutées après le chargement de l’interface graphique Axis ou PYVCP autonomes.

-   custom\_gvcp.hal est prévu pour les commandes qui doivent être exécutées après le chargement de GLADE VCP.

-   shutdown.hal est prévu pour des commandes exécutées quand Machinekit se ferme de façon contrôlée.

Petite FAQ Linux
----------------

<span id="cha:FAQ-Linux"></span>

Voici quelques commandes et techniques de base pour l’utilisateur débutant sous Linux. Beaucoup d’autres informations peuvent être trouvées sur [le site web de Machinekit](http://www.machinekit.org/) ou dans les man pages.

### Login automatique

Quand vous installez Machinekit avec le CD-Live Debian, par défaut vous devez passer par la fenêtre de connexion à chaque démarrage du PC. Pour activer le login automatique ouvrez le menu *Système → Administration → Fenêtre de connexion*. Si l’installation est récente la fenêtre de connexion peut prendre quelques secondes pour s’ouvrir. Vous devez entrer le mot de passe utilisé pour l’installation pour accéder à la fenêtre des préférences. Ouvrez alors l’onglet *Sécurité*, cochez la case *Activer les connexions automatiques* et saisissez votre nom d’utilisateur ou choisissez en un dans la liste déroulante. Vous êtes maintenant dispensé de la fenêtre de connexion.

### Les Man Pages<span id="sec:Man-Pages"></span>

Les Man pages sont des pages de manuel générées automatiquement le plus souvent. Les Man pages existent pour quasiment tous les programmes et les commandes de Linux.

Pour visualiser une man page ouvrez un terminal depuis *Applications → Accessoires → Terminal*. Par exemple si vous voulez trouver quelques choses concernant la commande *find*, tapez alors dans le terminal:

    man find

Utilisez les touches *Vers le haut* et *Vers le bas* pour faire défiler le texte et la touche **Q** pour quitter.

### Lister les modules du noyau

En cas de problème il est parfois utile de connaître la liste des modules du noyau qui sont chargés. Ouvrez une console et tapez:

    lsmod

Si vous voulez, pour le consulter tranquillement, envoyer le résultat de la commande dans un fichier, tapez la sous cette forme:

    lsmod > mes_modules.txt

Le fichier mes\_modules.txt résultant, se trouvera alors dans votre répertoire home si c’est de là que vous avez ouvert la console.

### Éditer un fichier en root<span id="sec:Editer-un-fichier-en-root"></span>

Éditer certains fichiers du système en root peut donner des résultats inattendus! Soyez très vigilant quand vous éditez en root, une erreur peut compromettre tout le système et l’empêcher de redémarrer. Vous pouvez ouvrir et lire de nombreux fichiers systèmes appartenant au root qui sont en mode lecture seule.

#### A la ligne de commande

Ouvrir un terminal depuis *Applications → Accessoires → Terminal*.

Dans ce terminal, tapez:

    sudo gedit

Ouvrez un fichier depuis *Fichiers → Ouvrir* puis éditez le.

#### En mode graphique

1.  Faites un clic droit sur le bureau et choisissez *Créer un lanceur..*.

2.  Tapez un nom tel que *éditeur*, dans la zone *Nom*.

3.  Entrez *gksudo "gnome-open %u"* dans la zone *Commande* et validez.

4.  Glissez un fichier et déposez le sur votre lanceur, il s’ouvrira alors directement dans l'éditeur.

### Commandes du terminal<span id="sec:Commandes-Terminal"></span>

#### Répertoire de travail

Pour afficher le chemin du répertoire courant dans le terminal tapez:

    pwd

#### Changer de répertoire

Pour remonter dans le répertoire précédent, tapez dans le terminal:

    cd ..

Pour remonter de deux niveaux de répertoire, tapez dans le terminal:

    cd ../..

Pour aller directement dans le sous-répertoire machinekit/configs tapez:

    cd machinekit/configs

#### Lister les fichiers du répertoire courant

Pour voir le contenu du répertoire courant tapez:

    ls

#### Trouver un fichier

La commande *find* peut être un peu déroutante pour le nouvel utilisateur de Linux. La syntaxe de base est:

    find répertoire_de_départ <paramètres> <actions>

Par exemple, pour trouver tous les fichiers .ini dans votre répertoire machinekit utilisez d’abord la commande *pwd* pour trouver le répertoire courant. Ouvrez un nouveau terminal et tapez:

    pwd

il vous est retourné par exemple le résultat suivant:

    /home/robert

Avec cette information vous pouvez taper, par exemple, la commande:

    find /home/robert/machinekit -name *.ini -print

Le *-name* est le nom du fichier que vous recherchez et le *-print* indique à find d’afficher le résultat dans le terminal. Le *\*.ini* indique à find de retourner tous les fichiers portant l’extension *.ini*

#### Rechercher un texte

Tapez dans un terminal:

    grep -lir "texte à rechercher" *

Pour trouver tous les fichiers contenant le texte *"texte à rechercher"* dans le répertoire courant, tous ses sous-répertoires et en ignorant la casse. Le paramètre **-l** demande de ne pas afficher les résultats normalement mais à la place, d’indiquer le nom des fichiers pour lesquels des résultats auraient été affichés. Le paramètre **-i** demande d’ignorer la casse. Le paramètre **-r** demande une recherche récursive (qui inclus tous les sous-répertoires dans la recherche). Le caractère **\*** est un jocker indiquant *tous les fichiers*.

#### Messages du boot

Pour visualiser les messages du boot utilisez la commande *dmesg* depuis un terminal. Pour enregistrer ces messages dans un fichier, redirigez les avec:

    dmesg > dmesg.txt

Le contenu de ce fichier pourra alors être copié et collé à destination des personnes en ligne qui vous aideront à diagnostiquer votre problème.

Pour nettoyer le tampon des messages tapez cette commande:

    sudo dmesg -c

C’est utile avant de lancer Machinekit, pour que ne soit enregistrés que les messages relatifs au fonctionnement courant de Machinekit.

Pour trouver les adresses des ports parallèles de la machine, tapez cette commande grep pour filtrer les informations contenues dans dmesg.

    dmesg|grep parport

### Problèmes matériels

#### Informations sur le matériel

Pour voir la liste du matériel installé sur les ports PCI de votre carte mère, tapez la commande suivante dans un terminal:

    lspci -v

Pour voir la liste du matériel installé sur les ports USB de votre carte mère, tapez la commande suivante dans un terminal:

    lsusb -v

#### Résolution du moniteur

Lors de l’installation d’Debian les réglages du moniteur sont automatiquement détectés. Il peut arriver que la détection fonctionne mal et que la résolution ne soit que celle d’un moniteur générique en 800x600.

Pour résoudre ce cas, suivez les instructions données ici:

<https://help.ubuntu.com/community/FixVideoResolutionHowto>

Legal Section
-------------

Ce document n’est pas traduit en raison de la complexité de la validation des documents juridiques. Vous trouverez sur ce site plus d’information: <http://www.gnu.org/licenses/licenses.fr.html>

### Copyright Terms

Copyright (c) 2000-2012 Machinekit.org

Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.1 or any later version published by the Free Software Foundation; with no Invariant Sections, no Front-Cover Texts, and one Back-Cover Text: "This Machinekit Handbook is the product of several authors writing for linuxCNC.org. As you find it to be of value in your work, we invite you to contribute to its revision and growth." A copy of the license is included in the section entitled "GNU Free Documentation License". If you do not find the license you may order a copy from Free Software Foundation, Inc. 59 Temple Place, Suite 330, Boston, MA 02111-1307

### GNU Free Documentation License

GNU Free Documentation License Version 1.1, March 2000

Copyright © 2000 Free Software Foundation, Inc. 59 Temple Place, Suite 330, Boston, MA 02111-1307 USA Everyone is permitted to copy and distribute verbatim copies of this license document, but changing it is not allowed.

**0. PREAMBLE**

The purpose of this License is to make a manual, textbook, or other written document "free" in the sense of freedom: to assure everyone the effective freedom to copy and redistribute it, with or without modifying it, either commercially or noncommercially. Secondarily, this License preserves for the author and publisher a way to get credit for their work, while not being considered responsible for modifications made by others.

This License is a kind of "copyleft", which means that derivative works of the document must themselves be free in the same sense. It complements the GNU General Public License, which is a copyleft license designed for free software.

We have designed this License in order to use it for manuals for free software, because free software needs free documentation: a free program should come with manuals providing the same freedoms that the software does. But this License is not limited to software manuals; it can be used for any textual work, regardless of subject matter or whether it is published as a printed book. We recommend this License principally for works whose purpose is instruction or reference.

**1. APPLICABILITY AND DEFINITIONS**

This License applies to any manual or other work that contains a notice placed by the copyright holder saying it can be distributed under the terms of this License. The "Document", below, refers to any such manual or work. Any member of the public is a licensee, and is addressed as "you".

A "Modified Version" of the Document means any work containing the Document or a portion of it, either copied verbatim, or with modifications and/or translated into another language.

A "Secondary Section" is a named appendix or a front-matter section of the Document that deals exclusively with the relationship of the publishers or authors of the Document to the Document’s overall subject (or to related matters) and contains nothing that could fall directly within that overall subject. (For example, if the Document is in part a textbook of mathematics, a Secondary Section may not explain any mathematics.) The relationship could be a matter of historical connection with the subject or with related matters, or of legal, commercial, philosophical, ethical or political position regarding them.

The "Invariant Sections" are certain Secondary Sections whose titles are designated, as being those of Invariant Sections, in the notice that says that the Document is released under this License.

The "Cover Texts" are certain short passages of text that are listed, as Front-Cover Texts or Back-Cover Texts, in the notice that says that the Document is released under this License.

A "Transparent" copy of the Document means a machine-readable copy, represented in a format whose specification is available to the general public, whose contents can be viewed and edited directly and straightforwardly with generic text editors or (for images composed of pixels) generic paint programs or (for drawings) some widely available drawing editor, and that is suitable for input to text formatters or for automatic translation to a variety of formats suitable for input to text formatters. A copy made in an otherwise Transparent file format whose markup has been designed to thwart or discourage subsequent modification by readers is not Transparent. A copy that is not "Transparent" is called "Opaque".

Examples of suitable formats for Transparent copies include plain ASCII without markup, Texinfo input format, LaTeX input format, SGML or XML using a publicly available DTD, and standard-conforming simple HTML designed for human modification. Opaque formats include PostScript, PDF, proprietary formats that can be read and edited only by proprietary word processors, SGML or XML for which the DTD and/or processing tools are not generally available, and the machine-generated HTML produced by some word processors for output purposes only.

The "Title Page" means, for a printed book, the title page itself, plus such following pages as are needed to hold, legibly, the material this License requires to appear in the title page. For works in formats which do not have any title page as such, "Title Page" means the text near the most prominent appearance of the work’s title, preceding the beginning of the body of the text.

**2. VERBATIM COPYING**

You may copy and distribute the Document in any medium, either commercially or noncommercially, provided that this License, the copyright notices, and the license notice saying this License applies to the Document are reproduced in all copies, and that you add no other conditions whatsoever to those of this License. You may not use technical measures to obstruct or control the reading or further copying of the copies you make or distribute. However, you may accept compensation in exchange for copies. If you distribute a large enough number of copies you must also follow the conditions in section 3.

You may also lend copies, under the same conditions stated above, and you may publicly display copies.

**3. COPYING IN QUANTITY**

If you publish printed copies of the Document numbering more than 100, and the Document’s license notice requires Cover Texts, you must enclose the copies in covers that carry, clearly and legibly, all these Cover Texts: Front-Cover Texts on the front cover, and Back-Cover Texts on the back cover. Both covers must also clearly and legibly identify you as the publisher of these copies. The front cover must present the full title with all words of the title equally prominent and visible. You may add other material on the covers in addition. Copying with changes limited to the covers, as long as they preserve the title of the Document and satisfy these conditions, can be treated as verbatim copying in other respects.

If the required texts for either cover are too voluminous to fit legibly, you should put the first ones listed (as many as fit reasonably) on the actual cover, and continue the rest onto adjacent pages.

If you publish or distribute Opaque copies of the Document numbering more than 100, you must either include a machine-readable Transparent copy along with each Opaque copy, or state in or with each Opaque copy a publicly-accessible computer-network location containing a complete Transparent copy of the Document, free of added material, which the general network-using public has access to download anonymously at no charge using public-standard network protocols. If you use the latter option, you must take reasonably prudent steps, when you begin distribution of Opaque copies in quantity, to ensure that this Transparent copy will remain thus accessible at the stated location until at least one year after the last time you distribute an Opaque copy (directly or through your agents or retailers) of that edition to the public.

It is requested, but not required, that you contact the authors of the Document well before redistributing any large number of copies, to give them a chance to provide you with an updated version of the Document.

**4. MODIFICATIONS**

You may copy and distribute a Modified Version of the Document under the conditions of sections 2 and 3 above, provided that you release the Modified Version under precisely this License, with the Modified Version filling the role of the Document, thus licensing distribution and modification of the Modified Version to whoever possesses a copy of it. In addition, you must do these things in the Modified Version: A. Use in the Title Page (and on the covers, if any) a title distinct from that of the Document, and from those of previous versions (which should, if there were any, be listed in the History section of the Document). You may use the same title as a previous version if the original publisher of that version gives permission. B. List on the Title Page, as authors, one or more persons or entities responsible for authorship of the modifications in the Modified Version, together with at least five of the principal authors of the Document (all of its principal authors, if it has less than five). C. State on the Title page the name of the publisher of the Modified Version, as the publisher. D. Preserve all the copyright notices of the Document. E. Add an appropriate copyright notice for your modifications adjacent to the other copyright notices. F. Include, immediately after the copyright notices, a license notice giving the public permission to use the Modified Version under the terms of this License, in the form shown in the Addendum below. G. Preserve in that license notice the full lists of Invariant Sections and required Cover Texts given in the Document’s license notice. H. Include an unaltered copy of this License. I. Preserve the section entitled "History", and its title, and add to it an item stating at least the title, year, new authors, and publisher of the Modified Version as given on the Title Page. If there is no section entitled "History" in the Document, create one stating the title, year, authors, and publisher of the Document as given on its Title Page, then add an item describing the Modified Version as stated in the previous sentence. J. Preserve the network location, if any, given in the Document for public access to a Transparent copy of the Document, and likewise the network locations given in the Document for previous versions it was based on. These may be placed in the "History" section. You may omit a network location for a work that was published at least four years before the Document itself, or if the original publisher of the version it refers to gives permission. K. In any section entitled "Acknowledgements" or "Dedications", preserve the section’s title, and preserve in the section all the substance and tone of each of the contributor acknowledgements and/or dedications given therein. L. Preserve all the Invariant Sections of the Document, unaltered in their text and in their titles. Section numbers or the equivalent are not considered part of the section titles. M. Delete any section entitled "Endorsements". Such a section may not be included in the Modified Version. N. Do not retitle any existing section as "Endorsements" or to conflict in title with any Invariant Section.

If the Modified Version includes new front-matter sections or appendices that qualify as Secondary Sections and contain no material copied from the Document, you may at your option designate some or all of these sections as invariant. To do this, add their titles to the list of Invariant Sections in the Modified Version’s license notice. These titles must be distinct from any other section titles.

You may add a section entitled "Endorsements", provided it contains nothing but endorsements of your Modified Version by various parties—for example, statements of peer review or that the text has been approved by an organization as the authoritative definition of a standard.

You may add a passage of up to five words as a Front-Cover Text, and a passage of up to 25 words as a Back-Cover Text, to the end of the list of Cover Texts in the Modified Version. Only one passage of Front-Cover Text and one of Back-Cover Text may be added by (or through arrangements made by) any one entity. If the Document already includes a cover text for the same cover, previously added by you or by arrangement made by the same entity you are acting on behalf of, you may not add another; but you may replace the old one, on explicit permission from the previous publisher that added the old one.

The author(s) and publisher(s) of the Document do not by this License give permission to use their names for publicity for or to assert or imply endorsement of any Modified Version.

**5. COMBINING DOCUMENTS**

You may combine the Document with other documents released under this License, under the terms defined in section 4 above for modified versions, provided that you include in the combination all of the Invariant Sections of all of the original documents, unmodified, and list them all as Invariant Sections of your combined work in its license notice.

The combined work need only contain one copy of this License, and multiple identical Invariant Sections may be replaced with a single copy. If there are multiple Invariant Sections with the same name but different contents, make the title of each such section unique by adding at the end of it, in parentheses, the name of the original author or publisher of that section if known, or else a unique number. Make the same adjustment to the section titles in the list of Invariant Sections in the license notice of the combined work.

In the combination, you must combine any sections entitled "History" in the various original documents, forming one section entitled "History"; likewise combine any sections entitled "Acknowledgements", and any sections entitled "Dedications". You must delete all sections entitled "Endorsements."

**6. COLLECTIONS OF DOCUMENTS**

You may make a collection consisting of the Document and other documents released under this License, and replace the individual copies of this License in the various documents with a single copy that is included in the collection, provided that you follow the rules of this License for verbatim copying of each of the documents in all other respects.

You may extract a single document from such a collection, and distribute it individually under this License, provided you insert a copy of this License into the extracted document, and follow this License in all other respects regarding verbatim copying of that document.

**7. AGGREGATION WITH INDEPENDENT WORKS**

A compilation of the Document or its derivatives with other separate and independent documents or works, in or on a volume of a storage or distribution medium, does not as a whole count as a Modified Version of the Document, provided no compilation copyright is claimed for the compilation. Such a compilation is called an "aggregate", and this License does not apply to the other self-contained works thus compiled with the Document, on account of their being thus compiled, if they are not themselves derivative works of the Document.

If the Cover Text requirement of section 3 is applicable to these copies of the Document, then if the Document is less than one quarter of the entire aggregate, the Document’s Cover Texts may be placed on covers that surround only the Document within the aggregate. Otherwise they must appear on covers around the whole aggregate.

**8. TRANSLATION**

Translation is considered a kind of modification, so you may distribute translations of the Document under the terms of section 4. Replacing Invariant Sections with translations requires special permission from their copyright holders, but you may include translations of some or all Invariant Sections in addition to the original versions of these Invariant Sections. You may include a translation of this License provided that you also include the original English version of this License. In case of a disagreement between the translation and the original English version of this License, the original English version will prevail.

**9. TERMINATION**

You may not copy, modify, sublicense, or distribute the Document except as expressly provided for under this License. Any other attempt to copy, modify, sublicense or distribute the Document is void, and will automatically terminate your rights under this License. However, parties who have received copies, or rights, from you under this License will not have their licenses terminated so long as such parties remain in full compliance.

**10. FUTURE REVISIONS OF THIS LICENSE**

The Free Software Foundation may publish new, revised versions of the GNU Free Documentation License from time to time. Such new versions will be similar in spirit to the present version, but may differ in detail to address new problems or concerns. See <http://www.gnu.org/copyleft/>.

Each version of the License is given a distinguishing version number. If the Document specifies that a particular numbered version of this License "or any later version" applies to it, you have the option of following the terms and conditions either of that specified version or of any later version that has been published (not as a draft) by the Free Software Foundation. If the Document does not specify a version number of this License, you may choose any version ever published (not as a draft) by the Free Software Foundation.

**ADDENDUM**: How to use this License for your documents

To use this License in a document you have written, include a copy of the License in the document and put the following copyright and license notices just after the title page:

Copyright (c) YEAR YOUR NAME. Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.1 or any later version published by the Free Software Foundation; with the Invariant Sections being LIST THEIR TITLES, with the Front-Cover Texts being LIST, and with the Back-Cover Texts being LIST. A copy of the license is included in the section entitled "GNU Free Documentation License".

If you have no Invariant Sections, write "with no Invariant Sections" instead of saying which ones are invariant. If you have no Front-Cover Texts, write "no Front-Cover Texts" instead of "Front-Cover Texts being LIST"; likewise for Back-Cover Texts.

If your document contains nontrivial examples of program code, we recommend releasing these examples in parallel under your choice of free software license, such as the GNU General Public License, to permit their use in free software.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


