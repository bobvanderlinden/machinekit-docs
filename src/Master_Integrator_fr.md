Manuel de l’intégrateur V, 2015-12-20
=====================================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

![common/images/emc2-intro.png]()

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

Concepts importants pour l’intégrateur
--------------------------------------

<span id="cha:concepts-integrateur"></span>

### Système pas à pas

#### Période de base

Période de base (BASE\_PERIOD) est le *métronome* de l’ordinateur de Machinekit. <span class="footnote">
\[Cette section fait référence à l’utilisation de *stepgen* le générateur de pas intégré à Machinekit. Certains dispositifs matériels ont leur propre générateur de pas et n’utilisent pas celui incorporé à Machinekit. Dans ce cas se référer au manuel du matériel concerné.\]
</span> A chaque période le logiciel de génération de pas calcule si c’est le moment pour une autre impulsion de pas. Une période de base plus courte permet de produire plus d’impulsions de pas par seconde, mais si elle est trop courte l’ordinateur passera tout son temps à générer les impulsions et les autres services seront ralentis voir bloqués. La latence et les besoins des pilotes des moteurs pas à pas déterminent la durée minimum de la période que nous pouvons utiliser.

La latence la plus défavorable peut ne se présenter que quelquefois par minute ou même moins. La probabilité que cela se produise au même moment qu’un changement de direction du moteur et faible. Donc on peut obtenir des erreurs très rares et intermittentes qui ruinent une production de temps en temps et il est impossible d’intervenir pour régler le problème.

La façon la plus simple d'éviter ce problème est de choisir un BASE\_PERIOD qui est la somme des plus longues exigences de temps de votre périphérique et le pire cas de latence de votre ordinateur. Ceci n’est pas toujours le meilleur choix.

Par exemple si le pilote moteur a besoin d’un maintien du signal de direction d’une durée de 20 µs est que la latence maximum est de 11 µs, la période de base sera de 11+20=31 µs ce qui donne une fréquence de génération de pas de 32258 pas par seconde dans un mode et de 16129 pas par seconde dans un autre mode.

Le problème est qu’avec les exigences de 20us pour le maintien du signal et les 11 µs de latence cela nous force à utiliser une période défavorable de 31 µs. Mais le générateur de pas du logiciel Machinekit a quelques paramètres qui nous laissent régler les divers temps d’une période de différentes manières.

Par exemple, si *steplen* <span class="footnote">
\[Steplen se réfère à un paramètre qui ajuste la performance du générateur de pas incorporé à Machinekit, *stepgen*, qui est un composant de HAL. Ce paramètre ajuste la longueur de l’impulsion de pas. Continuez à lire, on expliquera tous finalement.\]
</span> est changé de 1 à 2, alors il y aura deux périodes entre le commencement et la fin de l’impulsion de pas. De même, si *dirhold* <span class="footnote">
\[dirhold se réfère à un paramètre qui adapte la longueur du maintien du signal de commande de direction.\]
</span> est changé de 1 à 3, il y aura au moins trois périodes entre l’impulsion de pas et un changement d'état de la commande de direction.

Si nous pouvons utilisez *dirhold* pour répondre aux exigences des 20µs de maintiens du signal de direction, le prochain délai à respecter est la durée de 4.5 µs du signal de pas haut, additionnons les 11 µs de latence au 4.5 µs de signal haut et nous obtenons une période minimum de 15.5 µs. Lorsque nous essayons la valeur de 15.5, nous trouvons que l’ordinateur est lent, donc nous réglons sur 16 µs. Si nous laissons *dirhold* à 1 (par défaut) alors temps minimum entre un pas et le changement de direction est de 16 µs moins les 11 µs de latence ce qui nous donne 5µs. Ce qui n’est pas suffisant il nous manque 15 µs. Puisque la période est 16 µs, nous avons besoin d’encore une période. Dans ce cas nous changeons *dirhold* de 1 à 2. Maintenant le temps minimal entre la fin de l’impulsion de pas et le changement du signal de direction et de 5+16=21 µs. Nous n’avons plus à nous inquiéter d’une erreur de direction à cause de la latence.

Pour plus d’informations sur stepgen voir la section stepgen du manuel de HAL.

#### Timing des pas

Sur certain pilote moteur le rapport entre la durée des espaces et la durée des impulsions n’est pas égal, dans ce cas le point (le moment) du pas est important. Si le pas se déclenche sur le front descendant alors, la broche de sortie doit être inversée.

### Systèmes à servomoteurs

#### Opération de base

Les systèmes à servomoteurs sont capables de vitesses plus élevées pour une précision équivalente au moteur pas-à-pas, mais ils sont plus coûteux et complexes. Contrairement aux systèmes pas à pas, les servo-systèmes nécessitent un dispositif de rétroaction pour se positionner. Ils ne fonctionnent pas immédiatement sorti de la boite et ils doivent être calibrés pour fonctionner contrairement aux moteurs pas à pas.

Cette différence s’explique par le mode de régulation différent des deux systèmes. Les servosystèmes sont régulés en *boucle fermée* et le moteur pas à pas en *boucle ouverte*.

Que signifie *boucle fermée* ? Regardons un schéma simplifié et la façon dont un système de servomoteur fonctionne.

![images/servo-feedback\_fr.png]()

Figure 1. Boucle fermée

Ce diagramme montre que le signal de consigne (de commande) et le signal de retour pilotent l’amplificateur sommateur, ensuite celui-ci pilote l’amplificateur de puissance, qui pilote le moteur, qui actionne la charge et le dispositif de retour d’information qui fournis le signal de retour. Cela se perçoit comme une boucle fermée où A contrôle B, B contrôle C, C contrôle D et D contrôle A.

Si vous n’avez pas travaillé avec des systèmes à servomoteurs auparavant, cela sera sans aucun doute étrange au premier abord, surtout par rapport aux plus normal des circuits électroniques, où le bon déroulement des entrées vers les sorties est de ne jamais revenir en arrière.<span class="footnote">
\[Si cela peut aider, l'équivalent le plus proche dans le monde numérique ce sont les machines d'état, machines séquentielles où l'état des sorties à ce moment dépend de l'état que les entrées et sorties avaient avant. Si cela n’aide pas, alors passons.\]
</span>

Si tout contrôle tout le reste comment cela peut-il fonctionner, qui en a la charge ? La réponse est que Machinekit peut contrôler ce système, mais il doit le faire en choisissant une des différentes méthodes de contrôle.

La méthode de contrôle qu’utilise Machinekit, est l’une des plus simples et la meilleure appelée PID. PID est l’acronyme de **P**roportionnelle, **I**ntégrale et **D**érivée. La valeur proportionnelle détermine la réaction à l’erreur actuelle, la valeur intégrale détermine la réaction basée sur la somme d’erreurs récentes et la valeur dérivée détermine la réaction basée sur la vitesse de variation de l’erreur. Ce sont trois techniques communes de mathématique qui sont appliquées pour fournir un processus de suivi d’une consigne. Dans le cas de Machinekit le processus que nous voulons contrôler est l’actuelle position de l’axe et le point de consigne qui est la position commandée l’axe.

![images/pid-feedback\_fr.png]()

Figure 2. Boucle PID

En ajustant trois composantes (proportionnelle, intégrale et dérivée) dans l’algorithme du contrôleur PID, nous pouvons concevoir une régulation qui s’adapte aux exigences de processus spécifiques. La réponse du contrôleur peut être décrite en trois termes de réactivité : une erreur, de tolérance, au dépassement du point de consigne et au taux d’oscillation du système.

#### Terme proportionnel

Le terme proportionnel appelé plus souvent gain proportionnel applique un changement à la sortie qui est proportionnelle a la valeur d’erreur courante. Un gain élevé provoque un grand changement à la sortie pour un petit changement de l’erreur. Si le gain est trop haut, le système peut devenir instable. Au contraire, un gain trop faible aboutit à une faible réponse de la sortie en réaction à une grande erreur d’entrée. Si le gain proportionnel est trop bas, il peut être trop faible pour répondre aux perturbations du système.

En l’absence de perturbation, un contrôle proportionnel pur ne se positionnera pas à sa valeur cible, mais conservera un état d’erreur statique qui est une fonction du gain proportionnel et du gain du processus. Malgré la compensation de l'état stationnaire, tant la théorie des systèmes asservis que la pratique industrielle indiquent que c’est le terme proportionnel qui devrait contribuer à la plus grande partie du changement de la sortie.

#### Terme intégral

La contribution du terme intégral est proportionnelle à l’amplitude de l’erreur et à sa durée. La somme des erreurs instantanées au fil du temps (intégration) donne la compensation accumulée qui devrait avoir été corrigée précédemment. L’intégration de l’erreur est alors multipliée par le gain d’intégral et ajoutée à la sortie du contrôleur.

Le terme intégral lorsqu’il est ajouté augmente le mouvement du processus vers la consigne, il élimine l’erreur de statisme qui se produit avec un régulateur proportionnel seul. Cependant, puisque le terme intégral doit répondre aux erreurs accumulées par le passé, il peut causer un dépassement de la valeur de consigne actuelle (dépasser le point de consigne et puis créer un écart dans l’autre sens).

#### Terme dérivé

Le taux de variation de l’erreur du processus est calculé en déterminant la pente de l’erreur au cours du temps (c’est-à-dire sa dérivée première en relation avec le temps) et en multipliant ce taux de changement par le gain de dérivé. Le terme dérivé ralentit le taux de variation de la sortie du régulateur, cet effet est plus visible à proximité du point de consigne du contrôleur. Par conséquent, le contrôle dérivé est utilisé pour réduire l’ampleur du dépassement que produit la composante intégrale et pour améliorer la stabilité de la combinaison contrôleur processus.

#### Réglage de la boucle

Si les paramètres du contrôleur PID (les gains des termes proportionnel, intégral et dérivé) sont mal choisis, l’entrée du processus contrôlé peut être instable, c’est-à-dire sa sortie diverge, avec ou sans oscillation et, est limitée seulement par la saturation ou la rupture mécanique. Le réglage fin d’une boucle de contrôle consiste en l’ajustement de ses paramètres de contrôle (gain proportionnel, gain intégral, gain dérivé) aux valeurs optimums pour la réponse désirée.

#### Réglage manuel

Une méthode de réglage simple consiste à régler les valeurs **I** et **D** à zéro. Augmentons la valeur de **P** jusqu'à ce que la sortie oscille, **P** devrait être paramétré approximativement à la moitié de cette valeur pour diminuer d’un quart l’amplitude de ce type de réponse. Augmentons sa valeur pour que n’importe quelle compensation soit correcte dans un temps raisonnable pour le processus. Cependant, une valeur trop élevée apporte de l’instabilité. Ensuite, augmentons la valeur de **D** pour que la réponse soit suffisamment rapide pour atteindre sa référence après une perturbation de charge. Cependant, une valeur trop grande de **D** provoquera une réponse excessive et un dépassement. Un réglage de boucle PID rapide a un dépassement léger pour atteindre le point de consigne plus rapidement, cependant, certains systèmes ne peuvent accepter de dépassement, dans ce cas, une boucle fermée sur-amortie est nécessaire, cela requière une valeur **P** significativement plus basse que celle provoquant l’oscillation.

### RTAI

La *Real Time Application Interface* (RTAI) Interface d’application temps réel est utilisée pour fournir la meilleure performance temps réel. Le noyau patché RTAI permet d'écrire des applications avec des contraintes temporelles strictes. RTAI donne la possibilité d’avoir des logiciels comme ceux de génération de pas qui ont besoin d’un timing précis.

#### ACPI

L’Advanced Configuration and Power Interface (ACPI) a de nombreuses et différentes fonctions, dont la plupart interfèrent avec les performances du système temps réel. (Pour par exemple: la gestion de l'énergie, la réduction de puissance du processeur, la variation de fréquence du CPU, etc.) Le noyau Machinekit (et probablement tous les noyaux RTAI-patché) ont les fonctions ACPI désactivées. ACPI prend également soin de mettre hors tension le système après qu’un arrêt système a été commandé, et c’est pourquoi vous pourriez avoir besoin de presser sur le bouton d’alimentation pour éteindre complètement votre ordinateur. Le groupe RTAI a amélioré cela dans les versions récentes, de sorte que votre système Machinekit peut éteindre le système par lui-même.

Configuration de Machinekit
===========================

Test des capacités temps réel
-----------------------------

<span id="cha:test-de-latence"></span>

### Test de latence

Ce test est le premier test qui doit être effectué sur un PC pour savoir si celui-ci est capable de piloter une machine CNC.

La latence correspond au temps pris par le PC pour stopper ce qui est en cours et répondre à une requête externe. Dans notre cas, la requête est l’horloge qui sert au cadencement des impulsions de pas. Plus basse est la latence, plus rapide pourra être l’horloge, plus rapides et plus douces seront les impulsions de pas.

La latence est de loin plus importante que la vitesse du CPU. Un vieux Pentium III qui répond aux interruptions avec 10 microsecondes entre chacune, peut donner de meilleurs résultats que le dernier modèle de µP ultra rapide.

Le CPU n’est pas le seul facteur déterminant le temps de latence. Les cartes mères, les cartes vidéo, les ports USB et de nombreuses autres choses peuvent détériorer le temps de latence. La meilleure façon de savoir si le matériel envisagé est apte, c’est d’exécuter *un test de latence*.

La seule façon de découvrir ce qu’il en est sur un PC est d’exécuter le test de latence de HAL. Pour exécuter ce test, ouvrir une fenêtre de terminal à partir de *Applications → Accessoires → Terminal* et exécuter la commande suivante:

    latency-test

Une fenêtre comme ci-dessous devrait s’ouvrir:

![../config/images/latency.png]()

Figure 3. Test de latence de HAL

Alors que le test est en cours d’exécution, il faut charger l’ordinateur au maximum. Déplacer les fenêtres sur l'écran. Surfer sur le Web. Écouter de la musique. Exécuter un programme OpenGL comme glxgears. L’idée est de charger le PC au maximum pour que le temps de latence soit mesuré dans le cas le plus défavorable et donc, connaitre la latence maximale.

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
<td align="left">Ne pas exécuter Machinekit ou Stepconf pendant que latency-test est en cours d’exécution.</td>
</tr>
</tbody>
</table>

La colonne *max jitter* et la ligne *Base Thread* de l’exemple ci-dessus donne *9075*. Ce qui représente 9075 nanosecondes, soit 9.075 microsecondes. Noter ce nombre et l’entrer dans Stepconf quand il sera demandé.

Dans cet exemple de test de latence il n’a fallu que quelques secondes pour afficher cette valeur. Il est toutefois préférable de le laisser tourner pendant plusieurs minutes. Parfois même, dans le pire des cas, rien ne provoque de latence ou seulement des actions particulières. Par exemple, une carte mère Intel marchait très bien la plupart du temps, mais toutes les 64 secondes elle avait une très mauvaise latence de 300µs. Heureusement, il existe un [correctif](http://wiki.machinekit.org/cgi-bin/wiki.pl?FixingSMIIssues).

Alors, comment interpréter les résultats? Si le résultat de Max Jitter est en dessous d’environ 15-20 µs (15000-20000 nanosecondes), l’ordinateur pourra donner d’excellents résultats pour la génération logicielle des pas. Si le temps de latence est à plus de 30-50 microsecondes, de bons résultats seront obtenus, mais la vitesse maximum sera un peu faible, spécialement si des micropas sont utilisés ou si le pas de la vis est fin. Si les résultats sont de 100µS ou plus (100,000 nanosecondes), alors le PC n’est pas un bon candidat à la génération des pas. Les résultats supérieurs à 1 milliseconde (1,000,000 nanosecondes) éliminent, dans tous les cas, ce PC pour faire tourner Machinekit, en utilisant des micropas ou pas.

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
<td align="left">Si une latence élevée est obtenue, il peut être possible de l’améliorer. Un PC avait une très mauvaise latence (plusieurs millisecondes) en utilisant la carte graphique interne. Un carte graphique d’occasion à $5US a résolu le problème. Machinekit n’exige pas de matériel de pointe.</td>
</tr>
</tbody>
</table>

### Adresses des ports

Pour ceux qui construisent leur matériel, il est facile et économique d’augmenter le nombre d’entrées sorties d’un PC en lui ajoutant une carte PCI fournissant un ou deux ports parallèles supplémentaires. Faire suivre ces ports d’une couche d’opto-isolation est utile pour éviter les courts circuits pouvant détruire la carte, voir même toute la carte mère. Machinekit supporte un maximum de 8 ports parallèles.

Certaines parmi les bonnes cartes parallèles sont à base de chipset Netmos. Elles fournissent un signal +5V bien propre, elles fournissent un ou deux ports parallèles.

Pour trouver les adresses d’entrées/sorties de ces cartes, ouvrir une console et utiliser la commande en ligne:

    lspci -v

Rechercher ensuite dans la liste de matériel fourni, le nom du chipset de la nouvelle carte, dans cette exemple c’est l’entrée NetMos Technology pour une carte à deux ports:

    0000:01:0a.0 Communication controller: \
          Netmos Technology PCI 9815 Multi-I/O Controller (rev 01)
          Subsystem: LSI Losgic / Symbios Logic 2POS (2 port parallel adapter)
          Flags: medium devsel, IRQ 5
          I/O ports at b800 [size=8]
          I/O ports at bc00 [size=8]
          I/O ports at c000 [size=8]
          I/O ports at c400 [size=8]
          I/O ports at c800 [size=8]
          I/O ports at cc00 [size=16]

Après expérimentation, il se trouve que le premier port (incorporé à la carte) utilise la troisième adresse de la liste (c000) et le deuxième port (raccordé par une nappe) utilise la première adresse (b800).

Il est alors possible d’ouvrir dans l'éditeur le fichier .hal de la machine et d’insérer l’adresse trouvée à l’endroit approprié.

    loadrt hal\_parport cfg="0x378 0xc000"

Noter la présence des guillemets "" encadrant les deux adresses, ils sont obligatoires dès qu’il y a plus d’une carte.

Il est nécessaire également d’ajouter les fonctions de lecture (read) et d'écriture (write) pour la nouvelle carte. Par exemple:

    addf parport.1.read base-thread 1
    addf parport.1.write base-thread -1

Noter que les valeurs peuvent être différentes de celles de cet exemple. Les cartes Netmos sont Plug-N-Play, elles peuvent donc changer leur adressage selon le connecteur PCI dans lequel elles sont placées. Si l’installation des cartes PCI de la machine est modifiée, ne pas oublier de vérifier leurs adresses avant de lancer Machinekit.

Configuration de Machinekit
---------------------------

<span id="cha:ini-configuration"></span>

### Script de lancement

Machinekit est lancé par le fichier de script *machinekit*.

    machinekit [options] [<ini-file>]

Avec les options suivantes: \* *-v* = verbose - informations de fonctionnement \* *-d* = commande d'écho à l'écran pour le débogage

Le fichier de script *machinekit* lit le fichier ini puis lance Machinekit. La section \[HAL\] du fichier ini, spécifie l’ordre de chargement des fichiers de HAL, si plusieurs sont utilisés. Après que les fichiers HAL soient chargés, l’interface graphique est chargée à son tour puis le fichier HAL POSTGUI. Si des objets pyvcp ont été créés avec des pins de HAL, le fichier *postgui.hal* doit effectuer les raccordements à ces pins, se reporter à la section [HAL](#sub:Section-HAL) pour plus de détails.

Si aucun fichier ini n’est passé en argument au script *machinekit*, le sélecteur de configuration est lancé pour permettre à l’utilisateur de choisir parmi les exemples de configuration existants.

![../common/images/configuration-selector1\_fr.png]()

Figure 4. Sélecteur de configuration

### Fichiers utilisés pour la configuration

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

### Double passe (TWOPASS)

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

### Organisation du fichier ini

 Organisation du fichier ini   
Un fichier ini typique suit une organisation simple;

-   les commentaires.

-   les sections.

-   les variables.

Chacun de ces éléments est séparé, sur une seule ligne. Chaque fin de ligne ou retour chariot crée un nouvel élément.

#### Les commentaires

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

#### Les sections

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

#### Les variables

Une ligne de variables est composée d’un nom de variable, du signe égal (=) et d’une valeur. Tout, du premier caractère non blanc qui suit le signe = jusqu'à la fin de la ligne, est passé comme valeur à la variable. Vous pouvez donc intercaler des espaces entre les symboles si besoin. Un nom de variable est souvent appelé un mot clé.

Les paragraphes suivants détaillent chaque section du fichier de configuration, en utilisant des exemples de variables dans les lignes de configuration.

Certaines de ces variables sont utilisées par Machinekit. Elles doivent toujours utiliser le nom de section et le nom de variable dans leur appellation. D’autres variables ne sont utilisées que par HAL. Les noms des sections et les noms des variables indiquées, sont ceux qui sont utilisés dans les exemples de fichiers de configuration.

Les variables personnalisées peuvent être utilisées dans vos fichiers HAL avec la syntaxe suivante:

    MACHINE = MaVariable

#### Sections et variables utilisateur<span id="sub:variables-utilisateur"></span>

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

### Détails des sections du fichier ini

#### Section \[EMC\]

-   *VERSION* = $Revision: 1.5 $\_ - Le numéro de version du fichier INI. La valeur indiquée ici semble étrange, car elle est automatiquement mise à jour lors de l’utilisation du système de contrôle de révision. C’est une bonne idée de changer ce numéro à chaque fois que vous modifiez votre fichier. Si vous voulez le modifier manuellement, il suffit de changer le numéro sans toucher au reste.

-   *MACHINE = ma machine* - C’est le nom du contrôleur, qui est imprimé dans le haut de la plupart des fenêtres. Vous pouvez insérer ce que vous voulez ici tant que ça reste sur une seule ligne.

-   *DEBUG = 0* - Niveau de débogage 0 signifie qu’aucun message ne sera affiché dans le terminal pendant le fonctionnement de Machinekit. Les drapeaux de débogage ne sont généralement utiles que pour les développeurs.

#### Section \[DISPLAY\]

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

#### Section \[FILTER\]

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

#### Section \[RS274NGC\]

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

#### Section \[EMCMOT\]

D’autres entrées peuvent être rencontrées dans cette section, elles ne doivent pas être modifiées.

-   *BASE\_PERIOD = 50000* - (HAL) Période de base des tâches, exprimée en ns.C’est la plus rapide des horloges de la machine. Avec un système à servomoteurs, il n’y a généralement pas de raison pour que *BASE\_PERIOD* soit plus petite que *SERVO\_PERIOD*. Sur une machine de type *step&direction* avec génération logicielle des impulsions de pas, c’est *BASE\_PERIOD* qui détermine le nombre maximum de pas par seconde. Si de longues impulsions de pas ou de longs espaces entre les impulsions ne sont pas requis par l'électronique, la fréquence maximum absolue est de un pas par *BASE\_PERIOD*. Ainsi, la *BASE\_PERIOD* utilisée ici donnera une fréquence de pas maximum absolue de 20000 pas par seconde. 50000ns est une valeur assez large. La plus petite valeur utilisable est liée au résultat [du test de latence](#cha:test-de-latence), à la longueur des impulsions de pas nécessaire et à la vitesse du µP. Choisir une BASE\_PERIOD trop basse peut amener à des messages *Unexpected realtime delay*, des blocages ou des reboots spontanés.

-   *SERVO\_PERIOD = 1000000* - (hal) Période de la tâche *Servo*, exprimée également en nanosecondes. Cette valeur sera arrondie à un multiple entier de *BASE\_PERIOD*. Elle est utilisée aussi sur des systèmes basés sur des moteurs pas à pas. C’est la vitesse avec laquelle la nouvelle position des moteurs est traitée, les erreurs de suivi vérifiées, les valeurs des sorties PID sont rafraichies etc. Sur la plupart des systèmes cette valeur n’est pas à modifier. Il s’agit du taux de mise à jour du planificateur de mouvement de bas niveau.

-   *TRAJ\_PERIOD = 1000000* - (hal) Période du planificateur de trajectoire, exprimée en nanosecondes. Cette valeur sera arrondie à un multiple entier de *SERVO\_PERIOD*. Excepté pour les machines avec une cinématique particulière (ex: hexapodes) Il n’y a aucune raison de rendre cette valeur supérieure à *SERVO\_PERIOD*.

#### Section \[TASK\]

-   *TASK = milltask* - Indique le nom de la *tâche* exécutable. La tâche réalise différentes actions, telles que communiquer avec les interfaces utilisateur au dessus de NML, communiquer avec le planificateur de mouvements temps réel dans la mémoire partagée non-HAL, et interpréter le g-code. Actuellement il n’y a qu’une seule tâche exécutable qui fait sens pour 99,9% des utilisateurs, milltask.

-   *CYCLE\_TIME = 0.010* - Période exprimée en secondes, à laquelle TASK va tourner. Ce paramètre affecte l’intervalle de polling lors de l’attente de la fin d’un mouvement, lors de l’exécution d’une pause d’instruction et quand une commande provenant d’une interface utilisateur est acceptée. Il n’est généralement pas nécessaire de modifier cette valeur.

#### Section \[HAL\]

-   *TWOPASS=ON* - Utilise le processus *twopass* (double passe) pour charger les composants HAL. Avec le processus TWOPASS, tous les fichiers \[HAL\]HALFILES sont premièrement lus et les occurrences multiples des directives à loadrt pour chaque module sont cumulées. Aucune commande HAL n’est exécutée à la première passe.

-   *HALFILE = example.hal* - Exécute le fichier *example.hal* au démarrage. Si *HALFILE* est spécifié plusieurs fois, les fichiers sont exécutés dans l’ordre de leur apparition dans le fichier ini. Presque toutes les configurations auront au moins un *HALFILE* . Les systèmes à moteurs pas à pas ont généralement deux de ces fichiers, un qui spécifie la configuration générale des moteurs *core\_stepper.hal* et un qui spécifie le brochage des sorties *xxx\_pinout.hal*.

-   *HAL = command* - Exécute *command* comme étant une simple commande hal. Si *HAL* est spécifié plusieurs fois, les commandes sont exécutées dans l’ordre où elles apparaissent dans le fichier ini. Les lignes *HAL* sont exécutées après toutes les lignes *HALFILE*.

-   *SHUTDOWN = shutdown.hal* - Exécute le fichier *shutdown.hal* quand Machinekit s’arrête. Selon les pilotes de matériel utilisés, il est ainsi possible de positionner les sorties sur des valeurs définies quand Machinekit s’arrête normalement. Cependant, parce qu’il n’y a aucune garantie que ce fichier sera exécuté (par exemple, dans le cas d’une panne de l’ordinateur), il ne remplace pas une véritable chaîne physique d’arrêt d’urgence ou d’autres dispositifs logiciels de protection des défauts de fonctionnement comme la pompe de charge ou le watchdog.

-   *POSTGUI\_HALFILE = example2.hal* - (Seulement avec les interfaces TOUCHY et AXIS) Exécute *example2.hal* après que l’interface graphique ait créé ses HAL pins.

#### Section \[HALUI\]

-   *MDI\_COMMAND = G53 G0 X0 Y0 Z0* - Une commande MDI peut être exécuté en utilisant *halui.mdi-command-00*. Incrémente le nombre pour chaque commande énumérée dans la section \[HALUI\].

#### Section \[TRAJ\]

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

#### Sections \[AXIS\_n\]

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

#### Section \[HOMING\]

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

#### Variables relatives aux servomoteurs

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

#### Variables relatives aux moteurs pas à pas

-   *SCALE = 4000* - (hal) Spécifie le nombre d’impulsions qui correspond à un mouvement d’une unité machine comme indiqué dans la section \[TRAJ\]. Pour les systèmes à moteurs pas à pas, c’est le nombre d’impulsions de pas nécessaires pour avancer d’une unité machine. Pour un axe linéaire, une unité machine sera égale à la valeur de LINEAR\_UNITS. Pour un axe angulaire, une unité machine sera égale à la valeur de ANGULAR\_UNITS. Pour les systèmes à servomoteurs, c’est le nombre d’impulsions de retour signifiant que le mobile a avancé d’une unité machine. Un second nombre, si spécifié, sera ignoré. Par exemple, un pas moteur de 1.8 degré, en mode demi-pas, avec une réduction de 10 tours/pouce et des unités souhaitées en pouces, nous avons:
     **`scale = 2 pas/1.8 degrés * 360 degrés/tour * 10 tour/pouce = 4000 pas/pouce`**

(D’anciens fichiers .ini et .hal utilisaient INPUT\_SCALE pour cette valeur.)

-   *STEPGEN\_MAXACCEL = 21.0* - (hal) Limite d’accélération pour le générateur de pas. Elle doit être 1% à 10% supérieure à celle de l’axe MAX\_ACCELERATION. Cette valeur améliore les réglages de la *boucle de position* de stepgen. Si une correction de jeu a été appliquée sur un axe, alors STEPGEN\_MAXACCEL doit être 1,5 à 2 fois plus grande que MAX\_ACCELERATION.

-   *STEPGEN\_MAXVEL = 1.4* - (hal) Les anciens fichiers de configuration avaient également une limite de vitesse du générateur de pas. Si spécifiée, elle doit aussi être 1% à 10% supérieure à celle de l’axe MAX\_VELOCITY. Des tests ultérieurs ont montré que l’utilisation de STEPGEN\_MAXVEL n’améliore pas le réglage de la boucle de position de stepgen.

#### Section \[EMCIO\]

-   *CYCLE\_TIME = 0.100* - La période en secondes, à laquelle EMCIO va tourner. La mettre à 0.0 ou à une valeur négative fera que EMCIO tournera en permanence. Il est préférable de ne pas modifier cette valeur.

-   *TOOL\_TABLE = tool.tbl* - Ce fichier contient les informations des outils, décrites dans le Manuel de l’utilisateur.

-   *TOOL\_CHANGE\_POSITION = 0 0 2* - Quand trois digits sont utilisés, spécifie la position XYZ ou le mobile sera déplacé pour le changement d’outil. Si six digits sont utilisés, spécifie l’emplacement ou sera envoyé le mobile pour réaliser le changement d’outil sur une machine de type XYZABC et de même, sur une machine de type XYZABCUVW lorsque 9 digits sont utilisés. Les variables relatives à la position du changement d’outil peuvent être combinées, par exemple; en combinant TOOL\_CHANGE\_POSITION avec TOOL\_CHANGE\_QUILL\_UP il est possible de déplacer d’abord Z puis X et Y.

-   *TOOL\_CHANGE\_WITH\_SPINDLE\_ON = 1* - Avec cette valeur à 1, la broche reste en marche pendant le changement d’outil. Particulièrement utile sur les tours.

-   *TOOL\_CHANGE\_QUILL\_UP = 1* - Avec cette valeur à 1, l’axe Z sera déplacé sur son origine machine avant le changement d’outil. C’est l'équivalent d’un G0 G53 Z0.

-   *TOOL\_CHANGE\_AT\_G30 = 1* - Avec cette valeur à 1, le mobile sera envoyé sur un point de référence prédéfini par G30 dans les paramètres 5181-5186. Pour plus de détails sur les paramètres de G30, voir le chapitre relatif au G-code dans le Manuel de l’utilisateur.

-   *RANDOM\_TOOLCHANGER = 1* - C’est pour des machines qui ne peuvent pas placer l’outil dans la poche il vient. Par exemple, les machines qui change l’outil dans la poche active avec l’outil dans la broche.

Prise d’origine
---------------

<span id="sec:Prises-d-origine"></span>

### La prise d’origine

La prise d’origine semble assez simple, il suffit de déplacer chaque axe à un emplacement connu et de positionner l’ensemble des variables internes de Machinekit en conséquence. Toutefois, les machines étant différentes les unes des autres, la prise d’origine est maintenant devenue assez complexe.

### Séquences de prise d’origine

Il existe quatre séquences de prise d’origine possibles. Elles sont définies par le signe des variables SEARCH\_VEL et LATCH\_VEL ainsi que les paramètres de configuration associés, la figure suivante donne le détail de ces séquences.

![images/machinekit-motion-homing-diag\_fr.png]()

Figure 5. Les séquences de POM possibles

1.  Comme on le voit sur la figure, les deux conditions de base sont les suivantes:

    1.  La direction de recherche (SEARCH\_VEL) et la direction de détection (LATCH\_VEL) sont de même signe.

    2.  La direction de recherche (SEARCH\_VEL) et la direction de détection (LATCH\_VEL) sont de signe opposé.

### Configuration

Le tableau suivant détermine exactement comment se déroule la séquence de prise d’origines définie dans la section \[AXIS\] du fichier ini.

<table>
<caption>Tableau 1. Combinaisons des variables de la POM</caption>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Type de POM</th>
<th align="left">SEARCH_VEL</th>
<th align="left">LATCH_VEL</th>
<th align="left">USE_INDEX</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Immediate</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>NON</p></td>
</tr>
<tr class="even">
<td align="left"><p>Index-seul</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>nonzero</p></td>
<td align="left"><p>OUI</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Contact-seul</p></td>
<td align="left"><p>nonzero</p></td>
<td align="left"><p>nonzero</p></td>
<td align="left"><p>NO</p></td>
</tr>
<tr class="even">
<td align="left"><p>Contact et Index</p></td>
<td align="left"><p>nonzero</p></td>
<td align="left"><p>nonzero</p></td>
<td align="left"><p>OUI</p></td>
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
<td align="left">Toute autre combinaison produira une erreur.</td>
</tr>
</tbody>
</table>

#### Vitesse de recherche (HOME\_SEARCH\_VEL)

Vitesse de la phase initiale de prise d’origine, pendant la recherche du contact d’origine machine. Une valeur différente de zéro indique à Machinekit la présence d’un contact d’origine machine. Machinekit va alors commencer par vérifier si ce contact est déjà attaqué. Si oui, il le dégagera à la vitesse établie par *HOME\_SEARCH\_VEL*, la direction du dégagement sera de signe opposé à celui de *HOME\_SEARCH\_VEL*. Puis, il va revenir vers le contact en se déplaçant dans la direction spécifiée par le signe de *HOME\_SEARCH\_VEL* et à la vitesse déterminée par sa valeur absolue. Quand le contact d’origine machine est détecté, le mobile s’arrête aussi vite que possible, il y aura cependant toujours un certain dépassement dû à l’inertie et dépendant de la vitesse. Si celle-ci est trop élevée, le mobile peut dépasser suffisamment le contact pour aller attaquer un fin de course de limite d’axe, voir même aller se crasher dans une butée mécanique. À l’opposé, si *HOME\_SEARCH\_VEL* est trop basse, la prise d’origine peut durer très longtemps.

Une valeur égale à zéro indique qu’il n’y a pas de contact d’origine machine, dans ce cas, les phases de recherche de ce contact seront occultées. La valeur par défaut est zéro.

#### Vitesse de détection (HOME\_LATCH\_VEL)

Spécifie la vitesse et la direction utilisée par le mobile pendant la dernière phase de la prise d’origine, c’est la recherche précise du contact d’origine machine, si il existe et de l’emplacement de l’impulsion d’index, si elle est présente. Cette vitesse est plus lente que celle de la phase de recherche initiale, afin d’améliorer la précision. Si *HOME\_SEARCH\_VEL* et *HOME\_LATCH\_VEL* sont de mêmes signes, la phase de recherche précise s’effectuera dans le même sens que la phase de recherche initiale. Dans ce cas, le mobile dégagera d’abord le contact en sens inverse avant de revenir vers lui à la vitesse définie ici. L’acquisition de l’origine machine se fera sur la première impulsion de changement d'état du contact. Si *HOME\_SEARCH\_VEL* et *HOME\_LATCH\_VEL* sont de signes opposés, la phase de recherche précise s’effectuera dans le sens opposé à celui de la recherche initiale. Dans ce cas, Machinekit dégagera le contact à la vitesse définie ici. L’acquisition de l’origine machine se fera sur la première impulsion de changement d'état du contact lors de son dégagement. Si *HOME\_SEARCH\_VEL* est à zéro, signifiant qu’il n’y a pas de contact et que *HOME\_LATCH\_VEL* et différente de zéro, le mobile continuera jusqu'à la prochaine impulsion d’index, l’acquisition de l’origine machine se fera à cet position. Si *HOME\_SEARCH\_VEL* est différent de zéro et que *HOME\_LATCH\_VEL* est égale à zéro, c’est une cause d’erreur, l’opération de prise d’origine échouera. La valeur par défaut est zéro.

#### HOME\_IGNORE\_LIMITS

Peut contenir les valeurs YES ou NO. Cette variable détermine si Machinekit doit ignorer les fins de course de limites d’axe. Certaines machines n’utilisent pas un contact d’origine séparé, à la place, elles utilisent un des interrupteurs de fin de course comme contact d’origine. Dans ce cas, Machinekit doit ignorer l’activation de cette limite de course pendant la séquence de prise d’origine. La valeur par défaut de ce paramètre est NO.

#### HOME\_USE\_INDEX

Spécifie si une impulsion d’index doit être prise en compte (cas de règles de mesure ou de codeurs de positions). Si cette variable est vraie (HOME\_USE\_INDEX = YES), Machinekit fera l’acquisition de l’origine machine sur le premier front de l’impulsion d’index. Si elle est fausse (=NO), Machinekit fera l’acquisition de l’origine sur le premier front produit par le contact d’origine (dépendra des signes de *HOME\_SEARCH\_VEL* et *HOME\_LATCH\_VEL*). La valeur par défaut est NO.

#### HOME\_OFFSET

Contient l’emplacement du point d’origine ou de l’impulsion d’index, en coordonnées relatives. Il peut aussi être traité comme le décalage entre le point d’origine machine et le zéro de l’axe. A la détection du point d’origine ou de l’impulsion d’origine, Machinekit ajuste les coordonnées de l’axe à la valeur de *HOME\_OFFSET*. La valeur par défaut est zéro.

#### Position de l’origine (HOME)

C’est la position sur laquelle ira le mobile à la fin de la séquence de prise d’origine machine. Après avoir détecté le contact d’origine, avoir ajusté les coordonnées de ce point à la valeur de *HOME\_OFFSET*, le mobile va se déplacer sur la valeur de *HOME*, c’est le point final de la séquence de prise d’origine. La valeur par défaut est zéro. Notez que même si ce paramètre est égal à la valeur de *HOME\_OFFSET*, le mobile dépassera très légérement la position du point d’aquisition de l’origine machine avant de s’arrêter. Donc il y aura toujours un petit mouvement à ce moment là (sauf bien sûr si *HOME\_SEARCH\_VEL* est à zéro, et que toute la séquence de POM a été sautée). Ce mouvement final s’effectue en vitesse de déplacement rapide. Puisque l’axe est maintenant référencé, il n’y a plus de risque pour la machine, un mouvement rapide est donc la façon la plus rapide de finir la séquence de prise d’origine.

#### HOME\_IS\_SHARED

Si cet axe n’a pas un contact d’origine séparé des autres, mais plusieurs contacts câblés sur la même broche d’entrée, mettre cette valeur à 1 pour éviter de commencer la prise d’origine si un de ces contacts partagés est déjà activé. Mettez cette valeur à 0 pour permettre la prise d’origine même si un contact est déjà attaqué.

#### HOME\_SEQUENCE

Utilisé pour définir l’ordre des séquences *HOME\_ALL* de prise d’origine des différents axes (exemple: la POM de l’axe X ne pourra se faire qu’après celle de Z). La POM d’un axe ne pourra se faire qu’après tous les autres en ayant la valeur la plus petite de *HOME\_SEQUENCE* et après qu’ils soient déjà tous à *HOME\_OFFSET*. Si deux axes ont la même valeur de *HOME\_SEQUENCE*, leurs POM s’effectueront simultanément. Si *HOME\_SEQUENCE* est égale à -1 ou n’est pas spécifiée, l’axe ne sera pas compris dans la séquence *HOME\_ALL*. Les valeurs de *HOME\_SEQUENCE* débutent à 0, il ne peut pas y avoir de valeur inutilisée.

#### VOLATILE\_HOME

Si ce paramètre est vrai, l’origine machine de cet axe sera effacée chaque fois que la machine sera mise à l’arrêt. Cette variable est appropriée pour les axes ne maintenant pas la position si le moteur est désactivé (gravité de la broche par exemple). Certains moteurs pas à pas, en particulier fonctionnant en micropas, peuvent se comporter de la sorte.

#### LOCKING\_INDEXER

Si cet axe comporte un verrouillage d’indexeur rotatif, celui-ci sera déverrouillé avant le début de la séquence de prise d’origine, et verrouillé à la fin.

Tours
-----

<span id="cha:Configuration-tour"></span>

### Plan par défaut

Quand l’interpréteur de Machinekit à été créé, il à été écrit pour les fraiseuses. C’est pourquoi le plan par défaut est le plan XY (G17). Sur un tour standard on utilise seulement les axes du plan XZ (G18). Pour changer le plan par défaut d’un tour, mettez la ligne suivante dans la section RS274NGC du fichier ini.

    RS274NGC_STARTUP_CODE = G18

La ligne de commande ci-dessus peut être remplacées par des G codes placés dans le préambule du programme G-code.

### Réglages INI

Les réglages du fichier .ini suivants sont nécessaires dans Axis en mode tour, en remplacement ou ajout au fichier .ini normal.

    [DISPLAY]
    DISPLAY = axis
    LATHE = 1
    [TRAJ]
    AXES = 3
    COORDINATES = X Z
    [AXIS_0]
    ...
    [AXIS_2]
    ...

Fichiers TCL pour HAL
---------------------

<span id="cha:hal-tcl"></span>

Le langage de halcmd excelle pour spécifier des composants et des connexions mais il n’offre aucune possibilité de calcul. Par conséquent, les fichiers ini sont limités en clarté et n’ont pas la concision qu’ils pourraient avoir avec un langage de haut niveau.

Haltcl fourni un moyen facile d’utiliser les scripts tcl et leurs fonctions pour les calculs, les boucles, les branchements, les procédures, etc dans les fichiers ini. Pour utiliser cette fonctionnalité, il est nécessaire d’utiliser le langage TCL ainsi que l’extension .tcl pour les fichiers de HAL.

L’extension .tcl est comprise par le script principal (machinekit) qui traite les fichiers ini. Les fichiers haltcl sont identifiés dans la section \[HAL\] des fichiers ini (comme les fichiers .hal).

Exemple

    [HAL]
    HALFILE = fichier_conventionnel.hal
    HALFILE = fichier_tcl.tcl

Avec quelques précautions appropriées, les fichiers .hal et .tcl peuvent être mélangés.

### Compatibilité

Le langage utilisé dans les fichiers .tcl a une syntaxe simple qui est un sous-ensemble du puissant et polyvalent langage de script Tcl.

### Commandes haltcl

Les fichiers haltcl utilisent le langage de script Tcl, auquel s’ajoutent les commandes spécifiques de la couche d’abstraction matériel de Machinekit (HAL). Les commandes spécifiques sont les suivantes:

    addf, alias,
    delf, delsig,
    getp, gets
    ptype,
    stype,
    help,
    linkpp, linkps, linksp, list, loadrt, loadusr, lock,
    net, newsig,
    save, setp, sets, show, source, start, status, stop,
    unalias, unlinkp, unload, unloadrt, unloadusr, unlock,
    waitusr

Il existe deux cas particuliers, les commandes *gets* et *list* en raison de conflits avec les commandes internes de Tcl. Pour haltcl, ces commandes doivent être précédées du mot clé *hal*, comme ci-dessous:

    halcmd   haltcl
    ------   ------
    gets     hal gets
    list     hal list

### Variables du fichier ini et haltcl

Les variables du fichier ini sont accessibles par halcmd comme par haltcl mais avec une syntaxe différente.

Les fichiers ini de Machinekit utilisent des spécificateurs *SECTION* et *ITEM* pour identifier les items de configuration:

    [SECTION_A]
    ITEM1 = value_1
    ITEM2 = value_2
    ...
    [SECTION_B]
    ...

Les valeurs du fichier ini sont accessibles par substitution de texte dans les fichiers .hal en utilisant la forme:

    [SECTION]ITEM

Les mêmes valeurs de fichiers ini sont accessibles dans les fichiers .tcl sous la forme de tableau de variables globales.

    $::SECTION(ITEM)

Par exemple, un item de fichier ini comme:

    [AXIS_0]
    MAX_VELOCITY = 4

est exprimé comme \[AXIS\_0\]MAX\_VELOCITY dans les fichiers .hal pour halcmd et comme $::AXIS\_0(MAX\_VELOCITY) dans les fichiers .tcl pour haltcl.

### Conversion des fichiers .hal en fichiers .tcl

Les fichiers .hal existants peuvent être convertis manuellement en fichiers .tcl en les éditant pour adapter les différences mentionnées précédemment. Ce processus peut être automatisé à l’aide de scripts de conversion procédant à ces substitutions:

    [SECTION]ITEM ---> $::SECTION(ITEM)
    gets          ---> hal gets
    list          ---> hal list

### Notes à propos de haltcl

Dans haltcl, la valeur de l’argument pour les commandes *sets* et *setp* est implicitement traitée comme une expression dans le langage Tcl.

exemple

    # set gain to convert deg/sec to units/min for AXIS_0 radius
    setp scale.0.gain 6.28/360.0*$::AXIS_0(radius)*60.0

Les espaces blancs ne sont pas autorisés dans les expressions, utiliser des guillemets doubles pour s’en affranchir:

    setp scale.0.gain "6.28 / 360.0 * $::AXIS_0(radius) * 60.0"

Dans d’autres contextes, tels que *loadrt*, il est nécessaire d’utiliser explicitement la commande Tcl expr (\[expr {}\]) pour des expressions de calcul.

exemple

    loadrt motion base_period=[expr {500000000/$::TRAJ(MAX_PULSE_RATE)}]

### Exemples pour haltcl

Prenons la question de la marge haute de stepgen (*stepgen headroom*). Le générateur de pas logiciel stepgen fonctionne mieux avec une contrainte d’accélération légèrement supérieure à celle du planificateur de mouvement. Ainsi, lorsqu’on utilise des fichiers halcmd, on force une valeur calculée manuellement dans le fichier ini.

    [AXIS_0]
    MAXACCEL = 10.0
    STEPGEN_MAXACCEL = 10.5

Avec haltcl, il est possible d’utiliser des commandes Tcl pour effectuer le calcul et éliminer totalement l’item STEPGEN\_MAXACCEL du fichier ini.

    setp stepgen.0.maxaccel $::AXIS_0(MAXACCEL)*1.05

Autres caractéristiques de haltcl, les boucles et les tests. Par exemple, beaucoup de configurations utilisent les fichiers .hal *core\_sim.hal* ou *core\_sim9.hal*. Ceux-ci diffèrent du fait de la nécessité de connecter plus ou moins d’axes. Le code haltcl suivant devrait fonctionner pour n’importe quelle combinaison d’axes dans une machine à cinématique triviale (trivkins).

    # Crée les signaux position, vitesse et accélération pour chaque axe
    set ddt 0
    foreach axis {X Y Z A B C U V W} axno {0 1 2 3 4 5 6 7 8} {
      # 'list pin' retourne une liste vide si la pin n'existe pas
      if {[hal list pin axis.$axno.motor-pos-cmd] == {}} {
        continue
      }
      net ${axis}pos axis.$axno.motor-pos-cmd => axis.$axno.motor-pos-fb \
                                              => ddt.$ddt.in
      net ${axis}vel <= ddt.$ddt.out
      incr ddt
      net ${axis}vel => ddt.$ddt.in
      net ${axis}acc <= ddt.$ddt.out
      incr ddt
    }
    puts [show sig *vel]
    puts [show sig *acc]

### Interactivité de haltcl

La commande halrun reconnaît les fichiers halctl. Avec l’option -T, haltcl peut être exécuté interactivement comme un interpréteur Tcl. Cette fonctionnalité est utile pour les tests et pour les applications hal autonomes.

exemple

    $ halrun -T fichierhaltcl.tcl

### Exemples pour haltcl fournis avec la distribution (sim)

Le répertoire *configs/sim/axis/simtcl* contient un fichier ini qui utilise un fichier .tcl pour démontrer une configuration haltcl en conjonction avec l’utilisation du processus "twopass". L’exemple montre l’utilisation des procédures Tcl, les boucles, l’utilisation des commentaires avec sortie sur le terminal.

Machinekit et HAL
-----------------

Voir également la man page *motion(9)*.

### motion (temps réel)

Ces pins et paramètres sont créés par le module temps réel *motmod*. Ce module fourni une interface vers HAL pour le planificateur de mouvements de Machinekit. En gros, motmod prends dans une liste de points de cheminement et génère un flux de positions respectant les limites de contrainte des articulations. Ce flux sera reçu simultanément par tous les pilotes de moteurs.

Optionnellement le nombre d’E/S numériques est fixé avec *num\_dio*. Le nombre d’E/S analogiques est fixé avec *num\_aio*. Le nombre par défaut est 4 de chaque.

Les noms de pin commençant par *axis* sont actuellement des valeurs d’articulations, mais les pins et les paramètres sont également appelés *axis.N*. Ils sont lus et mis à jour par la fonction *motion-controller*.

*motion* est chargé par la commande *motmod*. La cinématique doit être chargée avant motion.

    loadrt motmod [base_period_nsec=period] [servo_period_nsec=period] \
    [traj_period_nsec=period] [num_joints=[0-9] ([num_dio=1-64] num_aio=1-16])]

-   *base\_period\_nsec = 50000* - période de *Base* des tâches, en nanosecondes. C’est le *thread* le plus rapide de la machine.

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
<td align="left">Sur les systèmes à base de servomoteurs, il n’y a généralement aucune raison d’avoir une valeur <em>base_period_nsec</em> inférieure à celle de <em>servo_period_nsec</em>. Sur les machines avec une génération de pas logicielle, la valeur de <em>base_period_nsec</em> détermine le nombre maximum de pas par seconde. En l’absence de la nécessité d’une grande durée de pas ou d’un grand écart entre pas, le taux maximum de pas est de un pas par <em>base_period_nsec</em>. Ainsi, la <em>base_period_nsec</em> ci-dessus donnera un taux maximum absolu de 20000 pas par seconde. 50000ns (50 µs) est une valeur assez prudente. La plus petite valeur utilisable est relative au résultat du test de latence, à la longueur de pas nécessaire et à la vitesse du processeur. Choisir une <em>base_period_nsec</em> trop basse peut entrainer l’arrivée du message &quot;Unexpected real time delay&quot; ou &quot;délai temps réel inattendu&quot;, le blocage de la machine ou son redémarrage spontané.</td>
</tr>
</tbody>
</table>

-   *servo\_period\_nsec = 1000000* - C’est la période de la tâche *Servo* en nanosecondes. Cette valeur doit être arrondie à un entier multiple de *base\_period\_nsec*. Cette période est utilisée même sur les systèmes à moteurs pas à pas.

    C’est la vitesse à laquelle sont calculées les nouvelles positions des moteurs, que l’erreur de suivi est calculée, que les valeurs de sortie des PID sont rafraichies et ainsi de suite. Les valeurs par défaut conviennent pour la plupart des systèmes. C’est le taux de rafraichissement du planificateur de mouvement de bas niveau.

-   *traj\_period\_nsec = 100000* - C’est la période, en nanosecondes, du planificateur de mouvement. Cette valeur doit être arrondie à un entier multiple de *servo\_period\_nsec*. Excepté pour les machines ayant une cinématique particulière (ex: hexapodes) cette valeur n’a pas de raison d'être supérieure à celle de *servo\_period\_nsec*.

#### Options

Si le nombre d’entrées/sorties numériques demandées est supérieur à la valeur par défaut de 4, il est possible d’en ajouter jusqu'à 64 en utilisant l’option num\_dio au chargement de motmod.

Si le nombre d’entrées/sorties analogiques demandées est supérieur à la valeur par défaut de 4, il est possible d’en ajouter jusqu'à 16 en utilisant l’option num\_aio au chargement de motmod.

#### Pins

 motion.adaptive-feed   
(float, in) Quand la vitesse est placée en mode adaptatif avec *M52 P1* la vitesse commandée est multipliée par cette valeur. Cet effet est multiplicatif avec *motion.feed-hold* et la valeur du correcteur de vitesse du niveau NML.

 motion.analog-in-00   
(float, in) Ces pins (00, 01, 02, 03 ou plus si configurées) sont contrôlées par *M66*.

 motion.analog-out-00   
(float, out) Ces pins (00, 01, 02, 03 ou plus si configurées) sont contrôlées par *M67* ou *M68*.

 motion.coord-error   
(bit, out) TRUE quand le mouvement est en erreur, ex: dépasser une limite logicielle.

 motion.coord-mode   
(bit, out) TRUE quand le mouvement est en *mode coordonnées* par opposistion au *mode téléopération*.

 motion.current-vel   
(float, out) La vitesse courante de l’outil.

 motion.digital-in-00   
(bit, in) Ces pins (00, 01, 02, 03 ou plus si configurées) sont contrôlées par *M62* à *M65*.

 motion.digital-out-00   
(bit, out) Ces pins (00, 01, 02, 03 ou plus si configurées) sont controlées par *M62* à *M65*.

 motion.distance-to-go   
(float, out) Distance restante pour terminer le mouvement courant.

 motion.enable   
(bit, in) Si ce bit devient FALSE, les mouvements s’arrêtent, la machine est placée dans l'état “machine arrêtée” et un message est affiché pour l’opérateur. En fonctionnement normal, ce bit devra être mis TRUE.

 motion.feed-hold   
(bit, in) Quand la vitesse est placée en mode arrêt contrôlé avec *M53 P1* et que ce bit est TRUE, la vitesse est fixée à 0.

 motion.motion-inposition   
(bit, out) TRUE si la machine est en position.

 motion.motion-enabled   
(bit, out) TRUE quand l'état de la machine est *machine on*.

 motion.on-soft-limit   
(bit, out) TRUE quand la machine est sur une limite logicielle.

 motion.probe-input   
(bit, in) *G38.x* utilise la valeur de cette pin pour déterminer quand la sonde de mesure a touché. TRUE le contact de la sonde est fermé (touche), FALSE le contact de la sonde est ouvert.

 motion.program-line   
(s32, out) La ligne en cours d’exécution pendant le déroulement du programme. Zéro si pas en marche ou entre deux lignes, pendant le changement de pas de programme.

 motion.requested-vel   
(float, out) La vitesse courante requise en unités utilisateur par seconde selon le réglage F=n du fichier G-code. Les correcteurs de vitesse et autres ajustements ne s’appliquent pas à cette pin.

 motion.spindle-at-speed   
(bit, in) Les mouvements passent en pause tant que cette pin est TRUE, sous les conditions suivantes: avant le premier mouvement d’avance suivant chaque démarrage de broche ou changement de vitesse; après le démarrage de tout enchainement de mouvements avec broche synchronisée; et si en mode CSS, à chaque transition avance rapide → avance travail. Cette entrée peut être utilisée pour s’assurer que la broche a atteint sa vitesse, avant de lancer un mouvement d’usinage. Elle peut également être utilisée sur un tour travaillant en mode CSS, au passage d’un grand diamètre à un petit, pour s’assurer que la vitesse a été suffisamment réduite avant la prise de passe sur le petit diamètre et inversement, lors du passage d’un petit diamètre vers un grand, pour s’assurer que la vitesse a été suffisamment augmentée. Beaucoup de variateurs de fréquence disposent d’une sortie *vitesse atteinte*. Sinon, il est facile de générer ce signal avec le composant *near*, par comparaison entre la vitesse de broche demandée et la vitesse actuelle.

 motion.spindle-brake   
(bit, out) TRUE quand le frein de broche doit être activé.

 motion.spindle-forward   
(bit, in) TRUE quand la broche doit tourner en sens horaire.

 motion.spindle-index-enable   
(bit, I/O) Pour les mouvements avec broche synchronisée, ce signal doit être raccordé à la pin *index-enable* du codeur de broche.

 motion.spindle-on   
(bit, out) TRUE quand la broche doit tourner.

 motion.spindle-reverse   
(bit, out) TRUE quand la broche doit tourner en sens anti-horaire.

 motion.spindle-revs   
(float, in) Pour le bon fonctionnement des mouvements avec broche synchronisée, ce signal doit être raccordé à la broche *position* du codeur de broche. La position donnée par le codeur de broche doit être étalonnée pour que *spindle-revs* augmente de 1.0 pour chaque tour de broche dans le sens horaire (*M3*).

 motion.spindle-speed-in   
(float, in) Donne la vitesse actuelle de rotation de la broche exprimée en tours par seconde. Elle est utilisée pour les mouvements en unités par tour (*G95*). Si le pilote du codeur de broche ne dispose pas d’une sortie *vitesse*, il est possible d’en générer une en passant la position de la broche au travers d’un composant ddt. Si la machine n’a pas de codeur de broche, il est possible d’utiliser *motion.spindle-speed-out-rps*.

 motion.spindle-speed-out   
(float, out) Consigne de vitesse de rotation de la broche, exprimée en tours par minute. Positive pour le sens horaire (*M3*), négative pour le sens anti-horaire (*M4*).

 motion.spindle-speed-out-rps   
(float, out) Vitesse de rotation broche commandée tours par seconde. Valeur positive pour le sens horaire (*M3*), valeur négative pour le sens anti-horaire (*M4*).

 motion.teleop-mode   
(bit, out) TRUE quand motion est en *mode téléopération*, par opposition au *mode coordonné*.

 motion.tooloffset.x:: à motion.tooloffset.w   
(float, out; un par axe) montre l’offset d’outil courant. Il peut provenir de la table d’outils (*G43 actif*), ou du G-code (*G43.1 actif*)

#### Paramètres

Beaucoup de ces paramètres servent d’aide au déboguage et sont sujets aux changements ou au retrait à tout moment.

 motion-command-handler.time   
(s32, RO)

 motion-command-handler.tmax   
(s32, RW)

 motion-controller.time   
(s32, RO)

 motion-controller.tmax   
(s32, RW)

 motion.debug-bit-0   
(bit, RO) Utilisé pour le déboguage.

 motion.debug-bit-1   
(bit, RO) Utilisé pour le déboguage.

 motion.debug-float-0   
(float, RO) Utilisé pour le déboguage.

 motion.debug-float-1   
(float, RO) Utilisé pour le déboguage.

 motion.debug-float-2   
(float, RO) Utilisé pour le déboguage.

 motion.debug-float-3   
(float, RO) Utilisé pour le déboguage.

 motion.debug-s32-0   
(s32, RO) Utilisé pour le déboguage.

 motion.debug-s32-1   
(s32, RO) Utilisé pour le déboguage.

 motion.servo.last-period   
Le nombre de cycle du processeur entre les invoquations du thread servo. Typiquement, ce nombre divisé par la vitesse du processeur donne un temps en secondes. Il peut être utilisé pour determiner si le contrôleur de mouvement en temps réel respecte ses contraintes de timing.

 motion.servo.last-period-ns   
(float, RO)

 motion.servo.overruns   
(u32, RW) En voyant de grandes différences entre les valeurs successives de *motion.servo.last-period* , le contrôleur de mouvement peut déterminer qu’il a échoué dans le respect de ses contraintes de timing. Chaque fois qu’une erreur est détectée, cette valeur est incrémentée.

#### Fonctions

Généralement, ces fonctions sont toutes les deux ajoutées à servo-thread dans l’ordre suivant:

 motion-command-handler   
Processus des commandes de mouvement provenant de l’interface utilisateur.

 motion-controller   
Lance le contrôleur de mouvement de Machinekit.

### axis.N (temps réel)

Ces pins et paramètres sont créés par le module temps réel *motmod*. Ce sont en fait des valeurs d’articulations, mais les pins et les paramètres sont toujours appelés *axis.N*.<span class="footnote">
\[Dans une machine à *cinématique triviale*, il y a correspondance une pour une, entre les articulations et les axes. Note Du Traducteur: nous utilisons dans cette traduction le terme *axe*, dans le cas d’une cinématique non triviale il devra être remplacé par le terme *articulation* (joint).\]
</span> Ils sont lus et mis à jour par la fonction *motion-controller*.

#### Pins

 axis.N.active   
TRUE quand cet axe est actif.

 axis.N.amp-enable-out   
(bit, out) TRUE si l’ampli de cet axe doit être activé.

 axis.N.amp-fault-in   
(bit, in) Doit être mis TRUE si une erreur externe est détectée sur l’ampli de cet axe.

 axis.N.backlash-corr   
(float, out)

 axis.N.backlash-filt   
(float, out)

 axis.N.backlash-vel   
(float, out)

 axis.N.coarse-pos-cmd   
(float, out)

 axis.N.error   
(bit, out)

 axis.N.f-error   
(float, out)

 axis.N.f-error-lim   
(float, out)

 axis.N.f-errored   
(bit, out)

 axis.N.faulted   
(bit, out)

 axis.N.free-pos-cmd   
(float, out)

 axis.N.free-tp-enable   
(bit, out)

 axis.N.free-vel-lim   
(float, out)

 axis.N.home-sw-in   
(bit, in) Doit être mis TRUE si le contact d’origine de cet axe est activé.

 axis.N.homed   
(bit, out)

 axis.N.homing   
(bit, out) TRUE si la prise d’origine de cette axe est en cours.

 axis.N.in-position   
TRUE si cet axe, utilisant le *free planner*, a atteint un arrêt.

 axis.N.index-enable   
(bit, I/O) Doit être reliée à la broche *index-enable* du codeur de cet axe pour activer la prise d’origine sur l’impulsion d’index.

 axis.N.jog-counts   
(s32, in) Connection à la broche *counts* d’un codeur externe utilisé comme manivelle.

 axis.N.jog-enable   
(bit, in) Quand elle est TRUE (et en mode manuel), tout changement dans *jog-counts* se traduira par un mouvement. Quand elle est FALSE, *jog-counts* sera ignoré.

 axis.N.jog-scale   
(float, in) Fixe la distance, en unités machine, du déplacement pour chaque évolution de *jog-counts*.

 axis.N.jog-vel-mode   
(bit, in) Quand elle est FALSE (par défaut), la manivelle fonctionne en mode position. L’axe se déplace exactement selon l’incrément de jog sélectionné pour chaque impulsion, sans s’occuper du temps que prendra le mouvement. Quand elle est TRUE, la manivelle fonctionne en mode vitesse. Le mouvement s’arrête quand la manivelle s’arrête, même si le mouvement commandé n’est pas achevé.

 axis.N.joint-pos-cmd   
(float, out) La position commandée de l’articulation (par opposition à celle du moteur). Ca peut être un écart entre les positions articulation et moteur. Par exemple; la procédure de prise d’origine fixe cet écart.

 axis.N.joint-pos-fb   
(float, out) Le retour de position de l’articulation (par opposition à celui du moteur).

 axis.N.joint-vel-cmd   
(float, out)

 axis.N.kb-jog-active   
(bit, out)

 axis.N.motor-pos-cmd   
(float, out) Position commandée pour cette articulation.

 axis.N.motor-pos-fb   
(float, in) Position actuelle de cette articulation.

 axis.N.neg-hard-limit   
(bit, out)

 axis.N.pos-lim-sw-in   
(bit, in) Doit être mis TRUE si le fin de course de limite positive de cette articulation est activé.

 axis.N.pos-hard-limit   
(bit, out)

 axis.N.neg-lim-sw-in   
(bit, in) Doit être mis TRUE si le fin de course de limite négative de cette articulation est activé.

 axis.N.wheel-jog-active   
(bit, out)

#### Paramètres

 axis.N.home-state   
Reflète l'étape de la prise d’origine en cours actuellement.

### iocontrol (espace utilisateur)

Ces pins sont créées par le contrôleur d’entrées/sorties de l’espace utilisateur, habituellement appelé *io*.

#### Pins

 iocontrol.0.coolant-flood   
(bit, out) TRUE quand l’arrosage est demandé.

 iocontrol.0.coolant-mist   
(bit, out) TRUE quand le brouillard est demandé.

 iocontrol.0.emc-enable-in   
(bit, in) Doit être mise FALSE quand un arrêt d’urgence externe est activé.

 iocontrol.0.lube   
(bit, out) TRUE quand le graissage centralisé est commandé.

 iocontrol.0.lube\_level   
(bit, in) Doit être mise TRUE quand le niveau d’huile est correct.

 iocontrol.0.tool-change   
(bit, out) TRUE quand un changement d’outil est demandé.

 iocontrol.0.tool-changed   
(bit, in) Doit être mise TRUE quand le changement d’outil est terminé.

 iocontrol.0.tool-number   
(s32, out) Numéro de l’outil courant.

 iocontrol.0.tool-prep-number   
(s32, out) Numéro du prochain outil, donné par le mot **T** selon RS274NGC.

 iocontrol.0.tool-prepare   
(bit, out) TRUE quand une préparation d’outil est demandée.

 iocontrol.0.tool-prepared   
(bit, in) Doit être mise TRUE quand une préparation d’outil est terminée.

 iocontrol.0.user-enable-out   
(bit, out) FALSE quand un arrêt d’urgence interne est activé.

 iocontrol.0.user-request-enable   
(bit, out) TRUE quand l’arrêt d’urgence est relâché.

Configuration d’un système pas/direction (dir/step)
---------------------------------------------------

<span id="cha:config-steppers"></span>

### Introduction

Ce chapitre décrit quelques uns des réglages les plus fréquents, sur lesquels l’utilisateur aura à agir lors de la mise au point de Machinekit. En raison de l’adaptabilité de Machinekit, il serait très difficile de les documenter tous en gardant ce document relativement concis.

Le système rencontré le plus fréquemment chez les utilisateurs de Machinekit est un système à moteurs pas à pas. Les interfaces de pilotage de ces moteurs recoivent de Machinekit des signaux de pas et de direction.

C’est le système le plus simple à mettre en oeuvre parce que les moteurs fonctionnent en boucle ouverte (pas d’information de retour des moteurs), le système nécessite donc d'être configuré correctement pour que les moteurs ne perdent pas de pas et ne calent pas.

Ce chapitre s’appuie sur la configuration fournie d’origine avec Machinekit appelée *stepper* et qui se trouve habituellement dans */etc/machinekit/sample-configs/stepper*.

### Fréquence de pas maximale

Avec la génération logicielle des pas la fréquence maximale en sortie, pour les impulsions de pas et de direction, est de une impulsion pour deux BASE\_PERIOD. La fréquence de pas maximale accessible pour un axe est le produit de MAX\_VELOCITY et de INPUT\_SCALE. Si la fréquence demandée est excessive, une erreur de suivi se produira (following error), particulièrement pendant les jog rapides et les mouvements en G0.

Si votre interface de pilotage des moteurs accepte des signaux d’entrée en quadrature, utilisez ce mode. Avec un signal en quadrature, un pas est possible pour chaque BASE\_PERIOD, ce qui double la fréquence maximum admissible.

Les autres remèdes consistent à diminuer une ou plusieurs variables: BASE\_PERIOD (une valeur trop faible peux causer un bloquage du PC), INPUT\_SCALE (s’il est possible sur l’interface de pilotage de sélectionner une taille de pas différente, de changer le rapport des poulies ou le pas de la vis mère), ou enfin MAX\_VELOCITY et STEPGEN\_MAXVEL.

Si aucune combinaison entre BASE\_PERIOD, INPUT\_SCALE et MAX\_VELOCITY n’est fonctionnelle, il faut alors envisager un générateur de pas externe (parmis les contrôleurs de moteurs pas à pas universels supportés par Machinekit)

### Brochage

Machinekit est très flexible et grâce à la couche d’abstraction de HAL (Hardware Abstraction Layer) il est facile de spécifier que tel signal ira sur telle broche.

Voir le tutoriel de HAL pour plus d’informations.

Comme décrit dans l’introduction et la manuel de HAL, il comporte des composants dont il fourni les signaux, les pins et les paramètres.

Les premiers signaux et pins relatifs au brochage sont:<span class="footnote">
\[Note: pour rester concis, nous ne présenterons qu’un seul axe, tous les autres sont similaires.\]
</span>

    signaux: Xstep, Xdir et Xen
    pins: parport.0.pin-XX-out & parport.0.pin-XX-in

Pour configurer le fichier ini, il est possible de choisir entre les deux brochages les plus fréquents, devenus des standards de fait, le brochage standard\_pinout.hal ou le brochage xylotex\_pinout.hal. Ces deux fichiers indiquent à HAL comment raccorder les différents signaux aux différentes pins. Dans la suite, nous nous concentrerons sur le brochage *standard\_pinout.hal*.

### Le fichier standard\_pinout.hal<span id="sec:standard-pinout.hal"></span>

Ce fichier contient certaines commandes de HAL et habituellement ressemble à celà:

    # standard pinout config file for 3-axis steppers
    # using a parport for I/O
    #

    # first load the parport driver
    loadrt hal_parport cfg="0x0378"

    #
    # next connect the parport functions to threads
    # read inputs first
    addf parport.0.read base-thread 1

    # write outputs last
    addf parport.0.write base-thread -1

    #
    # finally connect physical pins to the signals
    net Xstep => parport.0.pin-03-out
    net Xdir  => parport.0.pin-02-out
    net Ystep => parport.0.pin-05-out
    net Ydir  => parport.0.pin-04-out
    net Zstep => parport.0.pin-07-out
    net Zdir  => parport.0.pin-06-out

    # create a signal for the estop loopback
    net estop-loop iocontrol.0.user-enable-out iocontrol.0.emc-enable-in

    # create signals for tool loading loopback
    net tool-prep-loop iocontrol.0.tool-prepare iocontrol.0.tool-prepared
    net tool-change-loop iocontrol.0.tool-change iocontrol.0.tool-changed

    # connect "spindle on" motion controller pin to a physical pin
    net spindle-on motion.spindle-on => parport.0.pin-09-out

    ###
    ### You might use something like this to enable chopper drives when machine ON
    ### the Xen signal is defined in core_stepper.hal
    ###
    # net Xen => parport.0.pin-01-out

    ###
    ### If you want active low for this pin, invert it like this:
    ###
    # setp parport.0.pin-01-out-invert 1

    ###
    ### A sample home switch on the X axis (axis 0).  make a signal,
    ### link the incoming parport pin to the signal, then link the signal
    ### to Machinekit's axis 0 home switch input pin
    ###
    # net Xhome parport.0.pin-10-in => axis.0.home-sw-in

    ###
    ### Shared home switches all on one parallel port pin?
    ### that's ok, hook the same signal to all the axes, but be sure to
    ### set HOME_IS_SHARED and HOME_SEQUENCE in the ini file.  See the
    ### user manual!
    ###
    # net homeswitches <= parport.0.pin-10-in
    # net homeswitches => axis.0.home-sw-in
    # net homeswitches => axis.1.home-sw-in
    # net homeswitches => axis.2.home-sw-in

    ###
    ### Sample separate limit switches on the X axis (axis 0)
    ###
    # net X-neg-limit parport.0.pin-11-in => axis.0.neg-lim-sw-in
    # net X-pos-limit parport.0.pin-12-in => axis.0.pos-lim-sw-in

    ###
    ### Just like the shared home switches example, you can wire together
    ### limit switches.  Beware if you hit one, Machinekit will stop but can't tell
    ### you which switch/axis has faulted.  Use caution when recovering from this.
    ###
    # net Xlimits parport.0.pin-13-in => axis.0.neg-lim-sw-in axis.0.pos-lim-sw-in

Les lignes commençant par **\#** sont des commentaires, aident à la lecture du fichier.

### Vue d’ensemble du fichier standard\_pinout.hal

Voici les opérations qui sont exécutées quand le fichier standard\_pinout.hal est lu par l’interpréteur:

1.  Le pilote du port parallèle est chargé (voir le Parport section de le Manuel de HAL pour plus de détails)

2.  Les fonctions de lecture/écriture du pilote sont assignée au thread «Base thread» <span class="footnote">
    \[Le thread le plus rapide parmis les réglages de Machinekit, habituellement il n’y a que quelques microsecondes entre les exécutions de ce code.\]
    </span>

3.  Les signaux du générateur de pas et de direction des axes X,Y,Z… sont raccordés aux broches du port parallèle

4.  D’autres signaux d’entrées/sorties sont connectés (boucle d’arrêt d’urgence, boucle du changeur d’outil…)

5.  Un signal de marche broche est défini et raccordé à une broche du port parallèle

### Modifier le fichier standard\_pinout.hal

Pour modifier le fichier standard\_pinout.hal, il suffit de l’ouvrir dans un éditeur de texte puis d’y localiser les parties à modifier.

Si vous voulez par exemple, modifier les broches de pas et de direction de l’axe X, il vous suffit de modifier le numéro de la variable nommée *parport.0.pin-XX-out*:

    net Xstep parport.0.pin-03-out
    net Xdir  parport.0.pin-02-out

peut être modifiée pour devenir:

    net Xstep parport.0.pin-02-out
    net Xdir  parport.0.pin-03-out

ou de manière générale n’importe quel numéro que vous souhaiteriez.

Attention: il faut être certain de n’avoir qu’un seul signal connecté à une broche.

### Modifier la polarité d’un signal

Si une interface attends un signal *actif bas*, ajouter une ligne avec le paramètre d’inversion de la sortie, *-invert*. Par exemple, pour inverser le signal de rotation de la broche:

    setp parport.0.pin-09-invert TRUE

### Ajouter le contrôle de vitesse broche en PWM

Si votre vitesse de broche peut être contrôlée par un signal de PWM, utilisez le composant *pwmgen* pour créer ce signal:

    loadrt pwmgen output_type=0
    addf pwmgen.update servo-thread
    addf pwmgen.make-pulses base-thread
    net spindle-speed-cmd motion.spindle-speed-out => pwmgen.0.value
    net spindle-on motion.spindle-on => pwmgen.0.enable
    net spindle-pwm pwmgen.0.pwm => parport.0.pin-09-out
    setp pwmgen.0.scale 1800 # Change to your spindle’s top speed in RPM

Ce qui donnera le fonctionnement suivant, pour un signal PWM à: 0% donnera une vitesse de 0tr/mn, 10% une vitesse de 180tr/mn, etc. Si un signal PWM supérieur à 0% est requis pour que la broche commence à tourner, suivez l’exemple du fichier de configuration *nist-lathe* qui utilise un composant d'échelle (*scale*).

### Ajouter un signal de validation **enable**

Certains pilotes de moteurs requiert un signal de validation *enable* avant d’autoriser tout mouvement du moteur. Pour celà des signaux sont déjà définis et appelés *Xen*, *Yen*, *Zen*.

Pour les connecter vous pouvez utilisez l’exemple suivant:

    net Xen parport.0.pin-08-out

Il est possible d’avoir une seule pin de validation pour l’ensemble des pilotes, ou plusieurs selon la configuration que vous voulez. Notez toutefois qu’habituellement quand un axe est en défaut, tous les autres sont invalidés aussi de sorte que, n’avoir qu’un seul signal/pin de validation pour l’ensemble est parfaitement sécurisé.

### Ajouter un bouton d’Arrêt d’Urgence externe

Comme vous pouvez [le voir ici](#sec:standard-pinout.hal), par défaut la configuration standard n’utilise pas de bouton d’Arrêt d’Urgence externe. footnote:\[Une explication complète sur la manière de gérer les circuiteries d’Arrêt d’Urgence se trouve sur le [wiki(en)](http://wiki.machinekit.org/) et dans le Manuel de l’intégrateur.

Pour ajouter un simple bouton d’AU externe (ou plusieurs en série) vous devez remplacer la ligne suivante:

    net estop-loop iocontrol.0.user-enable-out iocontrol.0.emc-enable-in

par

    net estop-loop parport.0.pin-01-in iocontrol.0.emc-enable-in

Ce qui implique qu’un bouton d’Arrêt d’Urgence soit connecté sur la broche 01 du port parallèle. Tant que le bouton est enfoncé (le contact ouvert)<span class="footnote">
\[Utiliser exclusivement des contacts normalement fermés pour les A/U.\]
</span>, Machinekit restera dans l'état *Arrêt d’Urgence* (ESTOP). Quand le bouton externe sera relâché, Machinekit passera immédiatement dans l'état *Arrêt d’Urgence Relâché* (ESTOP-RESET) vous pourrez ensuite mettre la machine en marche en pressant le bouton *Marche machine* et vous êtes alors prêt à continuer votre travail avec Machinekit. :leveloffset: 0 = Interfaces graphiques utilisateur :leveloffset: 1 :lang: fr :toc:

PyVCP
-----

<span id="cha:Panneau-Virtuel-Control"></span>

### Introduction

Panneau virtuel de contrôle en python (**Py**thon **V**irtual **C**ontrol **P**anel)

Le panneau de contrôle virtuel pyVCP a été créé pour donner à l’intégrateur la possibilité de personnaliser l’interface graphique d’AXIS avec des boutons et des indicateurs destinés aux tâches spéciales.

Le coût d’un panneau de contrôle physique est très élevé et il peut utiliser un grand nombre de broches d’entrées/sorties. C’est là que le panneau virtuel prends l’avantage car il ne coûte rien d’utiliser pyVCP.

Les panneaux de contrôle virtuels peuvent être utilisés pour tester ou monitorer le matériel, les entrées/sorties, remplacer temporairement d’autres matériels d’entrées/sorties pendant le déboguage d’une logique ladder ou pour simuler un panneau physique, avant de le construire et de le câbler vers les cartes électroniques.

L’image suivante montre quelques widgets pyVCP.

![images/pyvcp\_group.png]()

### Construction d’un panneau pyVCP

La disposition d’un panneau pyVCP est spécifiée dans un fichier XML qui contient les balises des widgets entre &lt;pyvcp&gt; et &lt;/pyvcp&gt;. Par exemple:

    <pyvcp>
       <label text="Ceci est un indicateur à LED"/>
       <led/>
    </pyvcp>

![images/pyvcp\_mypanel\_fr.png]()

Si vous placez ce texte dans un fichier nommé tiny.xml et que vous le lancez avec:

    pyvcp -c panneau tiny.xml

pyVCP va créer le panneau pour vous, il y inclut deux widgets, un Label avec le texte *Ceci est un indicateur à LED* et une LED rouge, utilisée pour afficher l'état d’un signal HAL de type BIT. Il va aussi créer un composant HAL nommé *panneau* (tous les widgets dans ce panneau sont connectés aux pins qui démarrent avec *panneau*). Comme aucune balise &lt;halpin&gt; n'était présente à l’intérieur de la balise &lt;led&gt;, pyVCP nomme automatiquement la pin HAL pour le widget LED panneau.led.0

Pour obtenir la liste des widgets, leurs balises et options, consultez [la documentation des widgets](#sec:Documentation-des-widgets).

Un fois que vous avez créé votre panneau, connectez lui les signaux HAL, vers et à partir des pins pyVCP et avec la commande habituelle:

    net <signal-name> <pin-name> <opt-direction> <opt-pin-name>signal-name

Si vous débutez avec HAL, [le tutoriel de HAL](#cha:Tutoriel-HAL) est vivement recommandé.

### Sécurité avec pyVCP

Certaines parties de pyVCP sont évaluées comme du code Python, elles peuvent donc exécuter n’importe quelle action disponible dans les programmes Python. N’utilisez que des fichiers pyVCP en .xml à partir d’une source de confiance.tag

### Utiliser pyVCP avec AXIS

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

![images/pyvcp\_AXIS\_fr.png]()

Voila à quoi ressemble le panneau pyVCP que nous venons de créer, incorporé à AXIS. La configuration *sim/lathe* fournie en exemple, est configurée de cette manière.

### Panneaux PyVCP autonomes

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

### Documentation des widgets de pyVCP

Les signaux de HAL existent en deux variantes, BIT et FLOAT. pyVCP peut afficher la valeur d’un signal avec un widget indicateur, ou modifier la valeur d’un signal avec un widget de contrôle. Ainsi, il y a quatre classes de widgets pyVCP connectables aux signaux de HAL. Une cinquième classe de widgets d’aide permet d’organiser et d’appliquer des labels aux panneaux.

-   Widgets de signalisation, signaux de type bit: led, rectled

-   Widgets de contrôle, signaux de type bit: button, checkbutton, radiobutton

-   Widgets de signalisation de type nombre: number, s32, u32, bar, meter

-   Widgets de contrôle de type nombre: spinbox, scale, jogwheel

-   Widgets d’aide: hbox, vbox, table, label, labelframe

#### Syntaxe

Chaque widget sera décrit brièvement, suivi par la forme d'écriture utilisée et d’une capture d'écran. Toutes les balises contenues dans la balise du widget principal, sont optionnelles.

#### Notes générales

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

#### Commentaires

Pour ajouter un commentaire utiliser la syntaxe de xml.

    <!-- Mon commentaire -->

#### Editer un fichier XML

Editer le fichier XML avec un éditeur de texte. La plupart du temps un double click sur le nom de fichier permet de choisir *ouvrir avec l’editeur de texte* ou similaire.

#### Couleurs

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

#### Pins de HAL

Les pins de HAL fournisse le moyen de connecter les widgets aux autres éléments. Quand une pin de HAL est créée pour un widget, il est possible de la *connecter* à une autre pin de HAL avec une commande *net* dans un fichier .hal. Pour plus de détails, voir la commande *net* dans le manuel de HAL.

#### Label

Un label est un texte qui s’affiche sur le panneau.

Le label a une pin optionnelle de désactivation en ajoutant: *&lt;disable\_pin&gt;True&lt;/disable\_pin&gt;*.

    <label>
        <text>"Ceci est un label:"</text>
        <font>("Helvetica",20)</font>
    </label>

Ce code produira:

![images/pyvcp\_label\_fr.png]()

#### Les leds

Une led est utilisée pour indiquer l'état d’une pin de HAL de type bit. La couleur de la led sera on\_color quand le signal est vrai et off\_color autrement. \* *&lt;halpin&gt;* définit le nom de la pin, par défaut: *led.n*, où n est un entier. \* *&lt;size&gt;* définit la taille de la led, par défaut: 20. \* *&lt;on\_color&gt;* définit la couleur de la led led quand la pin est vraie, par défaut: *green* \* *&lt;off\_color&gt;* définit la couleur de la led quand la pin est fausse, par défaut: *ref*

#### La led ronde

    <led>
        <halpin>"ma-led"</halpin>
        <size>50</size>
        <on_color>"verte"</on_color>
        <off_color>"rouge"</off_color>
    </led>

Le résultat du code ci-dessus.

![images/pyvcp\_led.png]()

#### La led rectangulaire

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

![images/pyvcp\_rectled.png]()

#### Le bouton (button)

Un bouton permet de contrôler une pin de type bit. La pin sera mise vraie quand le bouton sera pressé et maintenu enfoncé, elle sera mise fausse quand le bouton sera relâché.

Les boutons peuvent suivre les options de formatage suivantes:

-   &lt;padx&gt;n&lt;/padx&gt; où *n* est le nombre d’espaces horizontaux supplémentaires

-   &lt;pady&gt;n&lt;/pady&gt; où *n* est le nombre d’espaces verticaux supplémentaires

-   &lt;activebackground&gt;"color"&lt;/activebackground&gt; Couleur au survol du curseur

-   &lt;bg&gt;"color"&lt;/bg&gt; Couleur du bouton

##### Bouton avec texte (Text Button)

    <button>
        <halpin>"Bouton-OK"</halpin>
        <text>"OK"</text>
    </button>
    <button>
        <halpin>"Bouton-Abandon"</halpin>
        <text>"Abort"</text>
    </button

Le code ci-dessus produit:

![images/pyvcp\_button.png]()

##### Case à cocher (checkbutton)

Une case à cocher contrôle une pin de type bit. La pin sera mise vraie quand la case est cochée et fausse si la case est décochée.

Une case non cochée:

![images/pyvcp\_checkbutton1.png]()

et une case cochée:

![images/pyvcp\_checkbutton2.png]()

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

![images/pyvcp\_checkbutton.png]()

##### Bouton radio (radiobutton)

Un bouton radio placera une seule des pins vraie. Les autres seront mises fausses.

    <radiobutton>
        <choices>["un","deux","trois"]</choices>
        <halpin>"mon-radiobtn"</halpin>
    </radiobutton>

Le code ci-dessus donne ce résultat:

![images/pyvcp\_radiobutton\_fr.png]()

Noter que dans l’exemple ci-dessus, les pins de HAL seront nommées mon-radiobtn.un, mon-radiobtn.deux et mon-radiobtn.trois. Dans l’image précédente, *trois* est la valeur sélectionnée courante.

#### Affichage d’un nombre (number)

L’affichage d’un nombre peux recevoir les options de formatage suivantes:

-   &lt;font&gt;("Font Name",n)&lt;/font&gt; où *n* est la taille de la police

-   &lt;width&gt;n&lt;/width&gt; où *n* est la largeur totale utilisée

-   &lt;justify&gt;pos&lt;/justify&gt; où "pos" peut être LEFT, CENTER ou RIGHT (devrait marcher)

-   &lt;padx&gt;n&lt;/padx&gt; où "n" est le nombre d’espaces horizontaux supplémentaires

-   &lt;pady&gt;n&lt;/pady&gt; où "n" est le nombre d’espaces verticaux supplémentaires

##### Number

Le widget *number* affiche la valeur d’un signal de type flottant.

    <number>
        <halpin>"number"</halpin>
        <font>("Helvetica",24)</font>
        <format>"+4.4f"</format>
    </number>

Le code ci-dessus donne ce résultat:

![images/pyvcp\_number.png]()

##### Flottant

Le widget number affiche la valeur d’un signal de type flottant.

    <number>
        <halpin>"my-number"</halpin>
        <font>("Helvetica",24)</font>
        <format>"+4.4f"</format>
    </number>

![images/pyvcp\_number.png]()

&lt;font&gt; est une police de caractères de type Tkinter avec la spécification de sa taille. Une police qui peut être agrandie jusqu'à la taille 200 est la police *courier 10 pitch*, que vous pouvez spécifier de la manière suivante, pour afficher des chiffres réellement grands:

    <font>('courier 10 pitch',100)</font>

&lt;format&gt; est un format *style C*, spécifié pour définir le format d’affichage du nombre.

##### Nombre s32

Le widget s32 affiche la valeur d’un nombre s32. La syntaxe est la même que celle de *number* excepté le nom qui est &lt;s32&gt;. Il faut prévoir une largeur suffisante pour afficher le nombre dans sa totalité.

    <s32>
        <halpin>"simple-number"</halpin>
        <font>("Helvetica",24)</font>
        <format>"6d"</format>
        <width>6</width>
    </s32>

![images/pyvcp\_s32.png]()

##### Nombre u32

Le widget u32 affiche la valeur d’un nombre u32. La syntaxe est la même que celle de *number* excepté le nom qui est &lt;u32&gt;.

#### Affichage d’images

Seul l’affichage d’images au format gif est possible. Toutes les images doivent avoir la même taille. Les images doivent être toutes dans le même répertoire que le fichier ini (ou dans le répertoire courant pour un fonctionnement en ligne de commande avec halrun/halcmd).

##### Image Bit

La bascule *image\_bit* bascule entre deux images selon la position vraie ou fausse de halpin.

    <pyvcp>
        <image name='fwd' file='fwd.gif'/>
        <image name='rev' file='rev.gif'/>
        <vbox>
            <image_bit halpin='selectimage' images='fwd rev'/>
        </vbox>
    </pyvcp>

En utilisant les deux images fwd.gif et rev.gif. FWD est affiché quand *selectimage* est fausse et REV est affiché quand *selectimage* est vraie.

![images/pyvcp\_image01.png]()

Figure 6. selectimage fausse

![images/pyvcp\_image02.png]()

Figure 7. selectimage vraie

##### Image u32

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

![images/pyvcp\_image\_u32\_01.png]()

Figure 8. Halpin = 0

![images/pyvcp\_image01.png]()

Figure 9. Halpin = 1

![images/pyvcp\_image02.png]()

Figure 10. Halpin = 2

#### Barre de progression (bar)

Le widget barre de progression affiche la valeur d’un signal FLOAT, graphiquement dans une barre de progression et simultanément, en numérique.

    <bar>
        <halpin>"bar"</halpin>
        <min_>0</min_>
        <max_>123</max_>
        <bgcolor>"grey"</bgcolor>
        <fillcolor>"red"</fillcolor>
    </bar>

Le code ci-dessus donne ce résultat:

![images/pyvcp\_bar.png]()

#### Galvanomètre (meter)

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

![images/pyvcp\_meter.png]()

#### Boîte d’incrément (spinbox)

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

![images/pyvcp\_spinbox.png]()

#### Curseur (scale)

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

![images/pyvcp\_scale.png]()

Noter que par défaut c’est min qui est affiché même si il est supérieur à max, à moins que min ne soit négatif.

#### Bouton tournant (dial)

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

![images/pyvcp\_dial.png]()

#### Manivelle (jogwheel)

La manivelle imite le fonctionnement d’une vraie manivelle, en sortant sur une pin FLOAT la valeur sur laquelle est positionné le curseur, que ce soit en le faisant tourner avec un mouvement circulaire, ou en tournant la molette de la souris.

    <jogwheel>
        <halpin>"my-wheel"</halpin>
        <cpr>45</cpr>
        <size>250</size>
    </jogwheel>

Le code ci-dessus donne ce résultat:

![images/pyvcp\_jogwheel.png]()

### Documentation des containers de pyVCP

Les containers sont des widgets qui contiennent d’autres widgets.

#### Bordures

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

![images/pyvcp\_borders.png]()

#### Hbox

Utilisez une Hbox lorsque vous voulez aligner les widgets, horizontalement, les uns à côtés des autres.

    <hbox>
        <relief>RIDGE</relief>
        <bd>6</bd>
        <label><text>"a hbox:"</text></label>
        <led></led>
        <number></number>
        <bar></bar>
    </hbox>

![images/pyvcp\_hbox.png]()

Á l’intérieur d’une Hbox, il est possible d’utiliser les balises *&lt;boxfill fill=/&gt;*, *&lt;boxanchor anchor=/&gt;* et *&lt;boxexpand expand=/&gt;* pour choisir le comportement des éléments contenus dans la boîte, lors d’un redimensionnement de la fenêtre. Pour des détails sur le comportement de fill, anchor, et expand, référez vous au manuel du pack Tk, *pack(3tk)*. Valeurs par défaut, *fill='y'*, *anchor='center'*, *expand='yes'*.

#### Vbox

Utilisez une Vbox lorsque vous voulez aligner les widgets verticalement, les uns au dessus des autres.

    <vbox>
        <relief>RIDGE</relief>
        <bd>6</bd>
        <label><text>"a vbox:"</text></label>
        <led></led>
        <number></number>
        <bar></bar>
    </vbox>

![images/pyvcp\_vbox.png]()

Á l’intérieur d’une Vbox, vous pouvez utiliser les balises *&lt;boxfill fill=/&gt;*, *&lt;boxanchor anchor=/&gt;* et *&lt;boxexpand expand=/&gt;* pour choisir le comportement des éléments contenus dans la boîte, lors d’un redimensionnement de la fenêtre. Pour des détails sur le comportement de fill, anchor, et expand, référez vous au manuel du pack Tk, *pack(3tk)*. Valeurs par défaut, *fill='y'*, *anchor='center'*, *expand='yes'*.

#### Labelframe

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

![images/pyvcp\_labelframe\_fr1.png]()

#### Table

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

![images/pyvcp\_table.png]()

#### Onglets (Tabs)

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

![\] image::images/pyvcp\_tabs2.png\[\] image::images/pyvcp\_tabs3.png\[]()

Exemples d’utilisation de PyVCP
-------------------------------

### Panneau PyVCP dans AXIS

Procédure pour créer un panneau PyVCP et l’utiliser, attaché dans la partie droite de l’interface AXIS.

-   Créer un fichier .xml contenant la description du panneau et le placer dans le répertoire de la configuration.

-   Ajouter une entrée, avec le nom du fichier .xml, dans la section \[DISPLAY\] du fichier ini.

-   Ajouter une entrée POSTGUI\_HALFILE, avec le nom du fichier postgui HAL.

-   Ajouter les liens vers les pins de HAL pour le panneau dans le fichier postgui.hal pour "connecter" le panneau PyVCP avec Machinekit.

### Panneaux flottants

Pour créer des panneaux flottants PyVCP pouvant être utilisés avec n’importe quelle interface, suivre les points suivants:

-   Créer un fichier .xml contenant la description du panneau et le placer dans le répertoire de configuration.

-   Ajouter les lignes *loadusr* dans le fichier.hal pour charger chaque panneau.

-   Ajouter les liens vers les pins de HAl du panneau dans le fichier postgui.hal, pour "connecter" les panneaux PyVCP à Machinekit.

L’exemple suivant montre le chargement de deux panneaux PyVCP.

    loadusr -Wn btnpanel pyvcp -c btnpanel panel1.xml
    loadusr -Wn sppanel pyvcp -c sppanel panel2.xml

Les paramètres -Wn font que HAL **W**ait for **n**ame, attends le composant nommé *btnpanel*.

Les paramètres pyvcp -c font que PyVCP nomme le panneau.

Les pins de HAL de panel1.xml seront nommées *btnpanel.&lt;pin name&gt;*

Les pins de HAL de panel2.xml seront nommées *sppanel.&lt;pin name&gt;*

Bien s’assurer qu’aucune ligne *loadusr* ne fasse déjà appel à une de ces pins PyVCP.

### Boutons de Jog

Dans cet exemple nous allons créer un panneau PyVCP avec 3 boutons utilisables pour déplacer en manuel les axes X, Y et Z. Cette configuration sera réalisée avec l’assistant Stepconf qui générera la configuration de la machine. Premièrement, nous lançons l’assistant Stepconf et le configurons pour la machine, ensuite dans la page *Advanced Configuration Options* nous effectuons les sélections pour ajouter un panneau PyVCP vierge, comme indiqué sur l’image suivante. Pour cet exemple nous avons nommé la configuration *pyvcp\_xyz* sur la page *Basic Machine Information* de l’assistant Stepconf.

![images/xyz\_ACO.png]()

Figure 11. Assistant de configuration XYZ<span id="cap:XYZ-Wizard-Configuration"></span>

L’assistant Stepconf Wizard va créer plusieurs fichiers et les placer dans le répertoire */emc/configs/pyvcp\_xyz*. Si la case *Créer un lien* est cochée, un lien vers ces fichiers sera créé sur le bureau.

#### Créer les Widgets

Ouvrir le fichier custompanel.xml par un clic droit sur son nom, puis en sélectionnant *Ouvrir avec l'éditeur de texte*. Entre les balises *&lt;pyvcp&gt;* et *&lt;/pyvcp&gt;* nous ajouterons les widgets pour le panneau.

Tous les détails sur chacun des Widgets de PyVCP sont donnés dans la [documentation des widgets](#sec:Documentation-des-widgets).

Dans le fichier custompanel.xml nous ajoutons la description des widgets.

    <pyvcp>

        <labelframe text="Boutons de Jog">
           <font>("Helvetica",16)</font>

            <!-- le bouton de jog de l'axe X -->
            <hbox>
                <relief>RAISED</relief>
                <bd>3</bd>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"x-plus"</halpin>
                    <text>"X+"</text>
                </button>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"x-moins"</halpin>
                    <text>"X-"</text>
                </button>
            </hbox>

            <!-- le bouton de jog de l'axe Y -->
            <hbox>
                <relief>RAISED</relief>
                <bd>3</bd>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"y-plus"</halpin>
                    <text>"Y+"</text>
                </button>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"y-moins"</halpin>
                    <text>"Y-"</text>
                </button>
            </hbox>

            <!-- le bouton de jog de l'axe Z -->
            <hbox>
                <relief>RAISED</relief>
                <bd>3</bd>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"z-plus"</halpin>
                    <text>"Z+"</text>
                </button>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"z-moins"</halpin>
                    <text>"Z-"</text>
                </button>
            </hbox>

            <!-- le curseur de vitesse de jog -->
            <vbox>
                <relief>RAISED</relief>
                <bd>3</bd>
                <label>
                    <text>"Vitesse de Jog"</text>
                    <font>("Helvetica",16)</font>
                </label>
                <scale>
                    <font>("Helvetica",14)</font>
                    <halpin>"jog-speed"</halpin>
                    <resolution>1</resolution>
                    <orient>HORIZONTAL</orient>
                    <min_>0</min_>
                    <max_>80</max_>
                </scale>
            </vbox>
        </labelframe>
    </pyvcp>

Après les ajouts précédents, nous avons un panneau PyVCP tel que celui de l’image suivante, attaché à droite d’Axis. Il est beau mais ne fait rien tant que les boutons ne sont pas "connectés" à halui. Si, à ce stade, une erreur se produit lors du déplacement de la fenêtre vers le bas, c’est généralement dû à une erreur de syntaxe ou d'écriture, elle est donc dans cette partie qu’il conviendra tout d’abord de vérifier soigneusement.

![images/xyz\_buttons.png]()

Figure 12. Boutons de Jog<span id="cap:Jog-Buttons"></span>

#### Effectuer les connections

Pour effectuer les connections nécessaires, ouvrir le fichier custom\_postgui.hal et y ajouter le code suivant:

    # connecte les boutons PyVCP pour X
    net my-jogxmoins halui.jog.0.minus <= pyvcp.x-moins
    net my-jogxplus halui.jog.0.plus <= pyvcp.x-plus

    # connecte les boutons PyVCP pour Y
    net my-jogymoins halui.jog.1.minus <= pyvcp.y-moins
    net my-jogyplus halui.jog.1.plus <= pyvcp.y-plus

    # connecte les boutons PyVCP pour Z
    net my-jogzmoins halui.jog.2.minus <= pyvcp.z-moins
    net my-jogzplus halui.jog.2.plus <= pyvcp.z-plus

    # connecte le curseur de vitesse de jog PyVCP
    net my-jogspeed halui.jog-speed <= pyvcp.jog-speed-f

Après avoir désactivé l’A/U (E-Stop) et activé la marche machine en mode Jog, le déplacement du curseur du panneau PyVCP devrait agir dès qu’il est placé au delà de zéro et les boutons de jog devraient fonctionner. Il est impossible de jogger alors qu’un fichier G-code s’exécute ou pendant qu’il est en pause ni quand l’onglet *Données manuelles \[F5\]* du (MDI), est ouvert.

### Testeur de port

Cet exemple montre comment faire un simple testeur de port parallèle en utilisant PyVCP et HAL.

Premièrement, créer le fichier ptest.xml qui contiendra le code suivant pour créer la description du panneau.

    <!-- Panneau de test pour la config. du port parallèle -->
    <pyvcp>
      <hbox>
        <relief>RIDGE</relief>
        <bd>2</bd>
        <button>
          <halpin>"btn01"</halpin>
          <text>"Pin 01"</text>
        </button>
        <led>
          <halpin>"led-01"</halpin>
          <size>25</size>
          <on_color>"green"</on_color>
          <off_color>"red"</off_color>
        </led>
      </hbox>
      <hbox>
        <relief>RIDGE</relief>
        <bd>2</bd>
        <button>
          <halpin>"btn02"</halpin>
          <text>"Pin 02"</text>
        </button>
        <led>
          <halpin>"led-02"</halpin>
          <size>25</size>
          <on_color>"green"</on_color>
          <off_color>"red"</off_color>
        </led>
      </hbox>
      <hbox>
        <relief>RIDGE</relief>
        <bd>2</bd>
        <label>
          <text>"Pin 10"</text>
          <font>("Helvetica",14)</font>
        </label>
        <led>
          <halpin>"led-10"</halpin>
          <size>25</size>
          <on_color>"green"</on_color>
          <off_color>"red"</off_color>
        </led>
      </hbox>
      <hbox>
        <relief>RIDGE</relief>
        <bd>2</bd>
        <label>
          <text>"Pin 11"</text>
          <font>("Helvetica",14)</font>
        </label>
        <led>
          <halpin>"led-11"</halpin>
          <size>25</size>
          <on_color>"green"</on_color>
          <off_color>"red"</off_color>
        </led>
      </hbox>
    </pyvcp>

Le panneau flottant contenant deux pins de HAL d’entrée et deux pins de HAL de sortie.

![images/ptest.png]()

Figure 13. Panneau flottant testeur de port parallèle<span id="cap:Port-Tester-Panel"></span>

Pour lancer les commandes de HAL dont nous avons besoin et démarrer tout ce qi’il nous faut, nous avons mis le code suivant dans notre fichier ptest.hal.

    loadrt hal_parport cfg="0x378 out"
    loadusr -Wn ptest pyvcp -c ptest ptest.xml
    loadrt threads name1=porttest period1=1000000
    addf parport.0.read porttest
    addf parport.0.write porttest
    net pin01 ptest.btn01 parport.0.pin-01-out ptest.led-01
    net pin02 ptest.btn02 parport.0.pin-02-out ptest.led-02
    net pin10 parport.0.pin-10-in ptest.led-10
    net pin11 parport.0.pin-11-in ptest.led-11
    start

Pour lancer le fichier HAL, nous utilisons, dans un terminal, les commandes suivantes:

    ~$ halrun -I -f ptest.hal

La figure suivante montre à quoi ressemble le panneau complet.

![images/ptest-final.png]()

Figure 14. Testeur de port parallèle, complet<span id="cap:Port-Tester-Complete"></span>

Pour ajouter le reste des pins du port parallèle, il suffi de modifier les fichiers .xml et .hal.

Pour visualiser les pins après avoir lancé le script HAL, utiliser la commande suivante au prompt *halcmd:*

    halcmd: show pin
    Component Pins:
    Owner Type  Dir Value  Name
        2 bit   IN  FALSE  parport.0.pin-01-out <== pin01
        2 bit   IN  FALSE  parport.0.pin-02-out <== pin02
        2 bit   IN  FALSE  parport.0.pin-03-out
        2 bit   IN  FALSE  parport.0.pin-04-out
        2 bit   IN  FALSE  parport.0.pin-05-out
        2 bit   IN  FALSE  parport.0.pin-06-out
        2 bit   IN  FALSE  parport.0.pin-07-out
        2 bit   IN  FALSE  parport.0.pin-08-out
        2 bit   IN  FALSE  parport.0.pin-09-out
        2 bit   OUT TRUE   parport.0.pin-10-in ==> pin10
        2 bit   OUT FALSE  parport.0.pin-10-in-not
        2 bit   OUT TRUE   parport.0.pin-11-in ==> pin11
        2 bit   OUT FALSE  parport.0.pin-11-in-not
        2 bit   OUT TRUE   parport.0.pin-12-in
        2 bit   OUT FALSE  parport.0.pin-12-in-not
        2 bit   OUT TRUE   parport.0.pin-13-in
        2 bit   OUT FALSE  parport.0.pin-13-in-not
        2 bit   IN  FALSE  parport.0.pin-14-out
        2 bit   OUT TRUE   parport.0.pin-15-in
        2 bit   OUT FALSE  parport.0.pin-15-in-not
        2 bit   IN  FALSE  parport.0.pin-16-out
        2 bit   IN  FALSE  parport.0.pin-17-out
        4 bit   OUT FALSE  ptest.btn01 ==> pin01
        4 bit   OUT FALSE  ptest.btn02 ==> pin02
        4 bit   IN  FALSE  ptest.led-01 <== pin01
        4 bit   IN  FALSE  ptest.led-02 <== pin02
        4 bit   IN  TRUE   ptest.led-10 <== pin10
        4 bit   IN  TRUE   ptest.led-11 <== pin11

Cela montre quelles pins sont IN est lesquelles sont OUT, ainsi que toutes les connections.

### Compte tours pour GS2<span id="sec:Exemple-Compte-Tours-GS2"></span>

L’exemple suivant utilise un variateur de fréquence GS2 de la société Automation Direct. <span class="footnote">
\[ En Europe on trouve ce type de variateur sous la marque Omron.\]
</span> Il permet le pilotage du moteur, la visualisation de la vitesse ainsi que d’autres informations dans un panneau PyVCP. Cet exemple est basé sur un autre, relatif au variateur GS2 et se trouvant dans la section des exemples matériels de ce manuel. Ce dernier exemple s’appuie lui même sur la description du composant de HAL gs2\_vfd.

#### Le panneau

Pour créer le panneau nous ajoutons ce code au fichier .xml.

    <pyvcp>

        <!-- Compte tours -->
        <hbox>
            <relief>RAISED</relief>
            <bd>3</bd>
            <meter>
                <halpin>"spindle_rpm"</halpin>
                <text>"Broche"</text>
                <subtext>"tr/mn"</subtext>
                <size>200</size>
                <min_>0</min_>
                <max_>3000</max_>
                <majorscale>500</majorscale>
                <minorscale>100</minorscale>
                <region1>0,10,"yellow"</region1>
            </meter>
        </hbox>

        <!-- La Led On -->
        <hbox>
            <relief>RAISED</relief>
            <bd>3</bd>
            <vbox>
                <relief>RAISED</relief>
                <bd>2</bd>
                <label>
                    <text>"On"</text>
                    <font>("Helvetica",18)</font>
                </label>
                <width>5</width>
                <hbox>
                    <label width="2"/> <!-- utilisé pour centrer la Led -->
                    <rectled>
                        <halpin>"on-led"</halpin>
                        <height>"30"</height>
                        <width>"30"</width>
                        <on_color>"green"</on_color>
                        <off_color>"red"</off_color>
                    </rectled>
                </hbox>
            </vbox>

            <!-- La Led Sens horaire -->
            <vbox>
                <relief>RAISED</relief>
                <bd>2</bd>
                <label>
                    <text>"Sens horaire"</text>
                    <font>("Helvetica",18)</font>
                    <width>5</width>
                </label>
                <label width="2"/>
                <rectled>
                    <halpin>"fwd-led"</halpin>
                    <height>"30"</height>
                    <width>"30"</width>
                    <on_color>"green"</on_color>
                    <off_color>"red"</off_color>
                </rectled>
            </vbox>

            <!-- La Led Sens inverse -->
            <vbox>
                <relief>RAISED</relief>
                <bd>2</bd>
                <label>
                    <text>"Sens inverse"</text>
                    <font>("Helvetica",18)</font>
                    <width>5</width>
                </label>
                <label width="2"/>
                <rectled>
                    <halpin>"rev-led"</halpin>
                    <height>"30"</height>
                    <width>"30"</width>
                    <on_color>"red"</on_color>
                    <off_color>"green"</off_color>
                </rectled>
            </vbox>
        </hbox>
    </pyvcp>

L’image ci-dessous montre notre panneau PyVCP en fonctionnement.

![images/gs2\_panel.png]()

Figure 15. Panneau pour GS2<span id="cap:Panneau-GS2"></span>

#### Les connections

Pour qu’il fonctionne, il est nécessaire d’ajouter le code suivant au fichier custom\_postgui.hal, il réalise les connections entre PyVCP et Machinekit.

    # affiche le compte tours, calcul basé sur freq * rpm par hz
    loadrt mult2
    addf mult2.0 servo-thread
    setp mult2.0.in1 28.75
    net cypher_speed mult2.0.in0 <= spindle-vfd.frequency-out
    net speed_out pyvcp.spindle_rpm <= mult2.0.out

    # la led On
    net gs2-run => pyvcp.on-led

    # la led Sens horaire
    net gs2-fwd => pyvcp.fwd-led

    # la led Sens anti-horaire
    net running-rev spindle-vfd.spindle-rev => pyvcp.rev-led

Certaines lignes demandent quelques explications.

-   La ligne de la led Sens horaire utilise le signal créé dans le fichier custom.hal dans lequel la led Sens inverse doit utiliser le bit *spindle-rev*.

-   On ne *peut pas* lier deux fois le bit *spindle-fwd* pour utiliser le signal auquel il est déjà lié.

Création d’interfaces graphiques avec GladeVCP
----------------------------------------------

<span id="cha:GladeVCP"></span>

### Qu’est-ce que GladeVCP?

GladeVCP est un composant de Machinekit qui donne la possibilité d’ajouter de nouvelles interfaces graphiques utilisateur à Machinekit telles qu’Axis ou Touchy. À la différence de PyVCP, GladeVCP n’est pas limité à l’affichage et aux réglages des pins de HAL, toutes les actions peuvent être exécutées en code Python. En fait, une interface utilisateur Machinekit complète peut être construite avec GladeVCP et Python.

GladeVCP utilise l’environnement graphique et WYSIWYG [Glade](http://glade.gnome.org/) qui simplifie l'édition et la création visuelle de panneaux esthétiquement très réussis. Il s’appuie sur les liaisons entre [PyGTK](http://www.pygtk.org/) et le riche jeu de widgets [GTK+](http://www.gtk.org/), finalement, tous peuvent être utilisés dans une application GladeVCP et pas seulement les widgets spécialisés pour interagir avec HAL et Machinekit présentés ici.

#### PyVCP par rapport à GladeVCP

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

### Description du fonctionnement, avec un exemple de panneau

Une fenêtre de panneau GladeVCP peut démarrer avec trois différentes configuration:

-   Toujours visible, intégré dans Axis, du côté droit, exactement comme un panneau PyVCP.

-   Dans un onglet dans Axis ou Touchy; dans Axis un troisième onglet sera créé à côté des deux d’origine, ils doivent être choisis explicitement.

-   Comme une fenêtre indépendante, qui peut être iconisée ou agrandie, indépendamment de la fenêtre principale.

Lancer un panneau GladeVCP simple, intégré dans Axis comme PyVCP, taper les commandes suivantes:

    $ cd configs/sim/gladevcp

    $ machinekit gladevcp_panel.ini

![images/example-panel-small.png]()

Lancer le même panneau, mais dans un onglet d’Axis avec:

    $ cd configs/sim/gladevcp

    $ machinekit gladevcp_tab.ini

![images/example-tabbed-small.png]()

Pour lancer ce même panneau comme une fenêtre autonome à côté d’Axis, démarrer Axis en arrière plan puis démarrer gladevcp de la manière suivante:

    $ cd configs/sim/gladevcp

    $ machinekit axis.ini &

    $ gladevcp -c gladevcp -u ../gladevcp/hitcounter.py -H
    ../gladevcp/manual-example.hal ../gladevcp/manual-example.ui

![images/example-float-small.png]()

Pour lancer ce panneau dans *Touchy*:

    $ cd configs/sim

    $ machinekit gladevcp_touchy.ini

![images/touchy-tab-33.png]()

<span class="comment"> Ces deux derniers exemples ne fonctionnent pas pour le moment.</span>

Fonctionnellement, ces configurations sont identiques. La seule différence porte sur l'état et la visibilité de l'écran. Puisqu’il est possible de lancer plusieurs composants GladeVCP en parallèle (avec des noms de modules de HAL différents), le mélange des configurations est également possible. Par exemple, un panneau sur le côté droit et un ou plusieurs en onglets pour des parties d’interface moins souvent utilisées.

#### Description de l’exemple de panneau

Pendant qu’Axis est en marche, explorons *Afficher configuration de HAL* dans lequel nous trouvons le composant de HAL *gladevcp* et dont nous pouvons observer la valeur des pins pendant l’interaction avec les widgets du panneau. La configuration de HAL peut être trouvée dans *configs/gladevcp/manual-example.hal*.

Usage des deux cadres en partie basse. Le panneau est configuré pour que, quand l’Arrêt d’Urgence est désactivé, le cadre *Settings* s’active et mette la machine en marche, ce qui active à son tour le cadre *Commandes* du dessous. Les widgets de HAL du cadre *Settings* sont liés aux Leds et labels du cadre *Status* ainsi qu’au numéros de l’outil courant et à celui de l’outil préparé. Les utiliser pour bien voir leur effet. L’exécution des commandes *T&lt;numéro d’outil&gt;* et *M6* dans la fenêtre du MDI aura pour effet de changer les numéros de l’outil courant et de l’outil préparé dans les champs respectifs.

Les boutons du cadre *Commandes* sont des *widgets d’action MDI*. Les presser exécutera une commande MDI dans l’interpréteur. Le troisième bouton *Execute Oword subroutine* est un exemple avancé, il prends plusieurs pins de HAL du cadre *Settings* et leur passe comme paramètres, le *sous-programme Oword*. Les paramètres actuels reçus par la routine sont affichés par une commande *(DEBUG, )*. Voir *configs/gladevcp/nc\_files/oword.ngc* pour le corps du sous-programme.

Pour voir comment le panneau est intégré dans Axis, voir la déclaration de *\[DISPLAY\]GLADEVCP* dans gladevcp\_panel.ui, ainsi que les déclarations de *\[DISPLAY\]EMBED* et de *\[HAL\]POSTGUI\_HALFILE* dans *gladevcp\_tab.ini*, respectivement.

#### Description de l'éditeur de Glade

L’interface utilisateur est créée avec l'éditeur graphique de Glade. Pour l’essayer il faut avoir le pré-requis nécessaire, [que glade soit installé](#gladevcp:Pre-requis). Pour éditer l’interface utilisateur, lancer la commande:

    $ glade configs/gladevcp/manual-example.ui

La zone centrale de la fenêtre montre l’apparence de l’interface en création. Tous les objets de l’interface et les objets supportés se trouvent dans la partie haute à droite de la fenêtre, où il est possible de choisir un widget spécifique (ou en cliquant sur lui au centre de la fenêtre). Les propriétés du widget choisi sont affichées et peuvent être modifiées, dans le bas à droite de la fenêtre.

Pour voir comment les commandes MDI sont passées depuis les widgets d’action MDI, explorer la liste des widgets sous *Actions* en haut à droite de la fenêtre, et dans le bas à droite de la fenêtre, sous l’onglet *Général*, les propriétés des *commandes MDI*.

#### Explorer la fonction de rappel de Python

Voici comment une fonction de rappel Python est intégrée dans l’exemple:

-   Dans glade, regarder le label du widget `hits` (un widget GTK+).

-   Dans le widget `button1`, regarder dans l’onglet *Signaux* et trouver le signal *pressed* associé avec le gestionnaire *on\_button\_press*.

-   Dans ../gladevcp/hitcounter.py, regarder la méthode *on\_button\_press* et comment elle place la propriété du label dans l’objet *hits*.

C'était juste pour toucher le concept du doigt. Le mécanisme de fonction de rappel sera détaillé plus en détails dans la section [Programmation de GladeVCP](#gladevcp:GladeVCP_Programming).

### Créer et intégrer une interface utilisateur Glade

#### Pré-requis: Installation de Glade

Pour visualiser ou modifier les fichiers d’une interface Glade, Glade doit être installé. Ce n’est pas nécessaire pour seulement essayer un panneau GladeVCP. Si la commande *glade* est manquante, l’installer de la manière suivante:

    $ sudo apt-get install glade

Vérifier ensuite la version installée, qui doit être égale ou supérieure à 3.6.7:

    $ glade --version

**`glade3 3.6.7`**

#### Lancer Glade pour créer une nouvelle interface utilisateur

Cette section souligne juste les étapes initiales spécifiques à Machinekit. Pour plus d’informations et un tutoriel sur Glade, voir <http://glade.gnome.org>. Certains trucs & astuces sur Glade, peuvent aussi être trouvés sur [youtube](http://www.youtube.com).

Soit modifier une interface existante en lançant `glade <fichier>.ui` ou, démarrer une nouvelle en lançant juste la commande `glade` depuis un terminal.

-   Si Machinekit n’a pas été installé depuis un paquetage, l’environnement Machinekit du shell doit être configuré avec *. &lt;machinekitdir&gt;/scripts/rip-environment*, autrement Glade ne trouverait pas les widgets spécifiques à Machinekit.

-   Quand l'éditeur demande pour enregistrer les préférences, accepter ce qui est proposé par défaut et presser *Close*.

-   Depuis les *Niveaux supérieurs* (cadre de gauche), choisir *Fenêtre* (première icône) en haut des Niveaux supérieurs, par défaut cette fenêtre sera nommée *window1*. Ne pas changer ce nom, GladeVCP lui est relié.

-   Dans le bas des onglets de gauche, dérouler *HAL Python* et *Machinekit Actions*.

-   Ajouter au nouveau cadre, un conteneur comme une boîte HAL\_Box ou une HAL\_Table depuis *HAL Python*.

-   Pointer et placer dans un conteneur d’autres éléments, comme une LED, un bouton, etc.

Le résultat pourrait ressembler à cela:

![images/glade-manual-small.png]()

Glade a tendance à écrire beaucoup de messages dans la fenêtre du terminal, la plupart peuvent être ignorés. Sélectionner *Fichier → Enregistrer sous*, donner lui un nom comme *myui.ui* et bien vérifier qu’il sera enregistré comme un fichier *GtkBuilder* (bouton radio en bas à gauche du dialogue d’enregistrement). GladeVCP peut aussi traiter correctement l’ancien format *libglade* mais il n’y a aucune raison de l’utiliser. Par convention, l’extension des fichier GtkBuilder est *.ui*.

#### Tester un panneau

Vous êtes maintenant prêt à faire un essai (avec Machinekit, par exemple Axis en marche) faites:

    gladevcp myui.ui

GladeVCP crée le composant de HAL portant le nom qui a été donné au fichier, par exemple, le très original *myui.ui* dans notre cas, à moins qu’il n’ait été surchargé pat l’option `-c <nom du composant>`. Si Axis est en marche, essayer de trouver le composant dans *Afficher configuration de HAL* et inspecter ses pins.

Vous vous demandez peut être pourquoi les widgets conteneurs comme *HAL\_Hbox* ou *HAL\_Table* apparaissent grisés (inactifs). Les conteneurs HAL ont une pin de HAL associée qui est désactivée par défaut, c’est ce qui cause ce rendu grisé des widgets conteneurs inactifs. Un cas d’utilisation courante pourrait être pour associer les pins de HAL du conteneur `halui.machine.is-on` ou un des signaux `halui.mode.`, pour s’assurer que certains widgets n’apparaissent actifs que dans un certain état.

Pour activer un conteneur, exécuter la commande HAL `setp gladevcp.<nom-du-conteneur> 1`.

#### Préparer le fichier de commande HAL

La voie suggérée pour lier les pins de HAL dans un panneau GladeVCP consiste à les collecter dans un fichier séparé portant l’extension `.hal`. Ce fichier est passé via l’option `POSTGUI_HALFILE=`, dans la section `[HAL]` du fichier de configuration.

ATTENTION: Ne pas ajouter le fichier de commandes HAL de GladeVCP à la section ini d’Axis `[HAL]HALFILE=`, ça n’aurait pas l’effet souhaité. Voir les sections suivantes.

#### Intégration dans Axis, comme pour PyVCP

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

#### Intégration dans un nouvel onglet d’Axis, à la suite des autres

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

#### Intégration dans Touchy

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

### Options de GladeVCP en ligne de commande

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

### Références des Widgets HAL

GladeVcp inclus une collection de widgets Gtk qui ont des pins de HAL attachées, appelés widgets HAL, il sont destinés à contrôler, à afficher et à avoir d’autres interactions avec la couche HAL de Machinekit. Il sont destinés à être utilisés avec les interfaces créées par l'éditeur de Glade. Avec une installation correcte, les widgets HAL devraient être visibles, dans l'éditeur Glade, dans le groupe des Widgets *HAL Python*. Beaucoup de champs spécifiques à HAL dans l’onglet *Général* affichent une infobulle au survol de la souris.

Il y a deux variantes de signaux de HAL, bits et nombres. Les signaux bits sont les on/off. Les nombres peuvent être des "float", des "s32" ou des "u32". Pour plus d’informations sur les types de données de HAL, voir le manuel de HAL. Les widgets GladeVcp peuvent soit, afficher la valeur d’un signal avec un widget d’indication, soit, modifier la valeur d’un signal avec un widget de contrôle. Ainsi, il existe quatre classes de widgets gladvcp qui peuvent être connectés à un signal de HAL. Une autre classe de widgets d’aide permettent d’organiser et d'étiqueter les panneaux.

-   Widgets d’indications "bit" signals: [Led HAL](#gladevcp:HAL_LED)

-   Widgets de contrôle "bit" signals: [HAL Bouton](#gladevcp:HAL_Button), [HAL Bouton radio](#gladevcp:HAL_Button), [HAL Case à cocher](#gladevcp:HAL_Button)

-   Widgets d’indications "nombre" signals: [\[gladevcp:HAL\_Label\]](#gladevcp:HAL_Label), [HAL Barre de progression](#gladevcp:HAL_ProgressBar), [HAL HBar](#gladevcp:HAL_HBar), [HAL VBar](#gladevcp:HAL_HBar), [HAL Indicateur](#gladevcp:HAL_Meter)

-   Widgets de contrôle "nombre" signals: [boîte d’incrément](#gladevcp:HAL_SpinButton), [HAL HScale](#gladevcp:HAL_HScale), [HAL VScale](#gladevcp:HAL_HScale)

-   widgets d’aide: [HAL Table](#gladevcp:HAL_HBox), [HAL HBox](#gladevcp:HAL_HBox)

-   Tracé du parcours d’outil: [HAL Gremlin](#gladevcp:HAL_Gremlin)

Les widgets HAL héritent des méthodes, propriétés et signaux des widgets Gtk sous-jacents, il est donc utile de consulter le site du [GTK+](http://www.gtk.org/) ainsi que la documentation pour les liaisons avec [PyGTK](http://www.pygtk.org/).

#### Nommage des Widgets HAL et de leurs pins

La plupart des widgets HAL on une simple pin de HAL associée et portant le même nom que le widget (glade: Général→Nom).

Les exceptions à cette règle sont actuellement:

-   *HAL\_Spinbutton* et *HAL\_ComboBox*, qui ont deux pins: une pin
     `<nomwidget>-f` (float) et une pin `<nomwidget>-s` (s32)

-   *HAL\_ProgressBar*, qui a une pin d’entrée `<nomwidget>-value`, et une pin d’entrée `<nomwidget>-scale`.

#### Donner des valeurs aux Widgets HAL et à leurs pins

En règle générale, si une valeur doit être attribuée à la sortie d’un widget HAL depuis un code Python, le faire en appelant le *setter* Gtk sous-jacent (par exemple `set_active()`, `set_value()`), ne pas essayer de donner directement la valeur à la pin associée par un `halcomp[nompin] = value`, parce-que le widget ne verra jamais le changement!.

Il pourrait être tentant de *fixer une pin d’entrée de widget HAL* par programme. Noter que cela va à l’encontre du but premier d’une pin d’entrée. Elle devrait être attachée à un autre composant de HAL et réagir au signal qu’il génère. Bien qu’aucune protection, empêchant d'écrire sur les pins d’entrée HAL Python, ne soit présente actuellement, cela n’aurait aucun sens. Il faut utiliser `setp nompin valeur` dans un fichier Hal associé, pour les essais.

Il est par contre, parfaitement autorisé de mettre une valeur sur une pin de sortie de Hal avec `halcomp[nompin] = valeur` à condition que cette pin ne soit pas déjà associée avec un autre widget, ce qui aurait pu être créé par la méthode
 `hal_glib.GPin(halcomp.newpin(<nom>,<type>,<direction>)`. Voir la [programmation de GladeVCP](#gladevcp:GladeVCP_Programming) pour d’autres exemples.

#### Le signal *hal-pin-changed*

La programmation événementielle signifie que l’interface graphique indique au code quand "quelque chose se produit", grâce à une fonction de rappel, comme quand un bouton est pressé, la sortie du widget HAL (ceux qui affichent la valeur des pins de HAL) comme une LED, une barre, une VBar, un indicateur à aiguille etc, supportent le signal *hal-pin-changed* qui peut provoquer une fonction de rappel dans le code Python quand une pin de HAL change de valeur. Cela veut dire qu’il n’est plus nécessaire d’interroger en permanence les pins de HAL dans le code pour connaitre les changements, les widgets font ça en arrière plan et le font savoir.

Voici un exemple montrant comment régler un signal `hal-pin-changed` pour une Hal Led, dans l'éditeur de Glade:

![images/hal-pin-change-66.png]()

L’exemple dans `configs/gladevcp/examples/complex` montre comment c’est géré en Python.

#### Les boutons (HAL Button)

Ce groupe de widgets est dérivé de divers boutons Gtk, ce sont les widgets HAL\_Button, HAL\_ToggleButton, HAL\_RadioButton et CheckButton. Tous ont une seule pin de sortie BIT portant un nom identique au widget. Les boutons n’ont pas d’autres propriétés additionnelles, contrairement à leurs classes de base Gtk.

-   HAL\_Button: Action instantanée, ne retient pas l'état. Signal important: `pressed`.

-   HAL\_ToggleButton, HAL\_CheckButton: Retiennent l'état on/off. Signal important: `toggled`.

-   HAL\_RadioButton: Un parmi un groupe. Signal important: `toggled` (par bouton).

-   Importantes méthodes communes: `set_active()`, `get_active()`

-   Importantes propriétés: `label`, `image`

<span class="comment"> .Boutons</span>

Case à cocher: <span class="image"> ![images/checkbutton.png]() </span>

Boutons radio: <span class="image"> ![images/radiobutton.png]() </span>

Bouton à bascule: <span class="image"> ![images/button.png]() </span>

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

#### Les échelles (Scales)

HAL\_HScale et HAL\_VScale sont respectivement dérivées de GtkHScale et GtkVScale. Elles ont une pin de sortie FLOAT portant le même nom que le widget. Les échelles n’ont pas de propriété additionnelle.

Pour créer une échelle fonctionnelle dans Glade, ajouter un *Ajustement* (Général→Ajustement→Nouveau ou existant) et éditer l’objet ajustement. Il défini les valeurs défaut/min/max/incrément. Fixer la *Sensibilité de l’incrément* de l’ajustement sur automatique pour éviter les warnings.

Exemple d'échelle (HAL\_hscale): <span class="image"> ![images/hscale.png]() </span>

#### La boîte d’incrément (SpinButton)

La boîte d’incrément de HAL est dérivée de GtkSpinButton, elle a deux pins de sortie:

 &lt;nomwidget&gt;-f   
out FLOAT pin

 &lt;nomwidget&gt;-s   
out S32 pin

Pour être fonctionnelle, Spinbutton doit avoir une valeur d’ajustement comme l'échelle, vue précédemment.

Exemple de boîte d’incrément: <span class="image"> ![images/spinbutton.png]() </span>

#### Les labels

Le Label HAL est un simple widget basé sur GtkLabel qui représente la valeur d’une pin de HAL dans un format défini par l’utilisateur.

 HAL pin type   
Les pins de HAL sont des types (0:S32, 1:float ou 2:U32), voir aussi l’infobulle d’info sur *Général → HAL pin type*, (noter que c’est différent de PyVCP qui lui, a trois widgets label, un pour chaque type).

 text template   
Détermine le texte à afficher, une chaine au format Python pour convertir la valeur de la pin en texte. Par défauts, à `%s` (les valeurs sont converties par la fonction str()), mais peut contenir n’importe quel argument légal pour la méthode format() de Python. Exemple: `Distance: %.03f` va afficher le texte et la valeur de la pin avec 3 digits fractionnaires remplis avec des zéros pour une pin FLOAT.

#### Les conteneurs: HAL\_HBox et HAL\_Table

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

#### Les Leds

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

Exemple de Leds: <span class="image"> ![images/leds.png]() </span>

#### La barre de progression (ProgressBar)

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

Exemple de barre de progression: <span class="image"> ![images/progressbar2.png]() </span>

#### La boîte combinée (ComboBox)

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

#### Les barres

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

Barre horizontale: <span class="image"> ![images/hal\_hbar.png]() </span> Barre verticale: <span class="image"> ![images/vscale.png]() </span>

#### L’indicateur (HAL Meter)

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

![images/hal\_meter.png]()

#### Gremlin, visualiseur de parcours d’outil pour fichiers .ngc

Gremlin est un traceur de parcours d’outil similaire à celui d’Axis. Il demande un environnement Machinekit en fonctionnement, comme Axis ou Touchy. Pour se connecter à lui, inspecter la variable d’environnement INI\_FILE\_NAME. Gremlin affiche le fichiers .ngc courant. Si le fichier ngc est modifié, il doit être rechargé pour actualiser le tracé. Si il est lancé dans une application GladeVCP quand Machinekit n’est pas en marche, un message va être affiché parce-que le widget Gremlin ne trouve pas le statut de Machinekit, comme le nom du fichier courant.

Gremlin n’exporte aucune pin de HAL. Il a les propriétés suivantes:

 view   
Peut être la vue en *x*, *y*, *z*, *p* (perspective) . Par défaut, vue en *z*.

 enable\_dro   
Booléen; afficher une visu sur le tracé ou non. Par défaut,à *True*.

Exemple:

![images/gremlin.png]()

#### Fonction de diagrammes animés: Widgets HAL dans un bitmap

Pour certaines applications, il est intéressant d’avoir une image de fond, comme un diagramme fonctionnel et positionner les widgets aux endroits appropriés dans le diagramme. Une bonne combinaison consiste à placer une image de fond comme un fichier .png, mettre la fenêtre GladeVCP en taille fixe, et utiliser Glade pour fixer la position du widget sur cette image.

Le code pour l’exemple ci-dessus peut être trouvé dans `configs/gladevcp/animated-backdrop`:

![images/small-screenshot.png]()

### Références des Widgets Machinekit Action

GladeVcp inclus une collection d’actions préprogrammées appelées widgets *Machinekit Action* qui sont des Widgets pour l'éditeur Glade. À la différence des widgets HAL, qui interagissent avec les pins de HAL, les widgets Machinekit Actions, interagissent avec Machinekit et son interpréteur de G-code.

Les widgets Machinekit Action sont dérivés du widget Gtk.Action. Le widget Machinekit Action en quelques mots:

-   C’est un objet disponible dans l'éditeur Glade.

-   Il n’a pas d’apparence visuelle par lui-même.

-   Son but: associer à un composant d’interface visible, à un composant d’interface sensitif, comme un menu, un bouton outil, un bouton avec une commande. Voir les propriétés des widgets Action dans *Général → Related Action* de l'éditeur.

-   L’action préprogrammée sera exécutée quand l'état du composant associé basculera (bouton pressé, menu cliqué…)

-   Ils fournissent une voie facile pour exécuter des commandes sans avoir à faire appel à la programmation en Python.

L’apparence des Machinekit Actions dans Glade est approximativement la suivante:

![images/emc-actions.png]()

Le survol de la souris donne une infobulle.

#### Les widgets Machinekit Action

Les widgets Machinekit Action sont des widgets de type simple état. Ils implémentent une seule action par l’usage, d’un seul bouton, d’une option de menu, d’un bouton radio ou d’une case à cocher.

#### Les widgets Machinekit bascule action (ToggleAction)

Ce sont des widgets double état. Ils implémentent deux actions ou utilisent un second état (habituellement, *pressé*) pour indiquer qu’une action est actuellement en cours. Les bascules action sont prévues pour être utilisées avec les boutons à bascule (ToggleButtons) et les boutons à bascule d’outil (ToggleToolButtons) ou encore, pour basculer les items de menu. Un exemple simple est le bouton à bascule d’Arrêt d’Urgence (EStop).

Actuellement, les widgets suivants sont disponibles:

-   La bascule *d’Arrêt d’Urgence* (ESTOP) envoie la commande ESTOP ou ESTOP\_RESET à Machinekit, selon l'état courant.

-   La bascule *ON/OFF* envoie la commande STATE\_ON ou STATE\_OFF.

-   La bascule *Pause/Reprise* envoie la commande AUTO\_PAUSE ou AUTO\_RESUME.

Les bascules action suivantes ont seulement une commande associée et utilisent l'état *pressé* pour indiquer que l’opération demandée est lancée:

-   La bascule *Run* envoie la commande AUTO\_RUN et attends dans l'état pressé jusqu'à ce que l’interpréteur soit de nouveau au repos.

-   La bascule *Stop* est inactive jusqu'à ce que l’interpréteur passe à l'état actif (Un G-code est lancé) et permet alors à l’utilisateur d’envoyer la commande AUTO\_ABORT.

-   La bascule *MDI* envoie la commande passée dans le MDI et attends sa complétion dans l'état inactif *pressé*.

#### La bascule Action\_MDI et les widgets Action\_MDI

Ces widgets fournissent le moyen d’exécuter des commandes MDI. Le widget Action\_MDI n’attends pas la complétion de la commande, comme le fait la bascule Action\_MDI, qui reste elle, désactivée tant que la commande n’est pas terminée.

#### Un exemple simple: Exécuter une commande MDI lors de l’appui sur un bouton.

`configs/gladevcp/mdi-command-example/whoareyou.ui` est un fichier UI Glade qui transmet cette action basique:

L’ouvrir dans Glade et étudier comment il est fait. Lancer Axis puis dans un terminal faire: *+gladevcp whoareyou.ui+*. Voir l’action `hal_action_mdi1` et les propriétés de `MDI command` qui exécute juste `(MSG, "Hi, I’m an Machinekit_Action_MDI")` ce qui ouvre un popup de message dans Axis, comme ci-dessous:

![images/whoareyou.png]()

Noter que le bouton, associé à l’Action\_MDI, est grisé si la machine est arrêtée, en A/U ou si l’interpréteur est déjà en marche. Il deviendra automatiquement actif quand la machine sera mise en marche donc, sortie de l’A/U (E-Stop), et que le programme est au repos.

#### Paramètres passés avec les widgets Action\_MDI et ToggleAction\_MDI

Optionnellement, la chaine *MDI command* peut avoir des paramètres substitués avant d'être passée à l’interpréteur. Ces paramètres sont actuellement les noms des pins de HAL dans les composants GladeVCP. Voici comment cela fonctionne:

-   Supposons que nous avons une *SpinBox HAL* nommée `speed`, nous voulons passer sa valeur courante comme paramètre dans une commande MDI.

-   La SpinBox HAL aura une pin de HAL de type flottant, nommée speed-f (voir la description des Widgets Hal).

-   Pour substituer cette valeur dans la commande MDI, insérons le nom de la pin de HAL

-   Pour la spinbox HAL précédente, il aurait été possible d’utiliser

L’exemple de fichier UI est `configs/gladevcp/mdi-command-example/speed.ui`. Voici ce qui ce qui est obtenu en le lançant:

![images/speed.png]()

#### Un exemple plus avancé: Passer des paramètres à un sous-programme O-word

Il est parfaitement permis d’appeler un sous-programme O-word dans une commande MDI et passer la valeur des pins de HAL comme paramètres actuels. Un exemple de fichier UI est dans `configs/gladevcp/mdi-command-example/owordsub.ui`.

Placer `configs/gladevcp/nc_files/oword.ngc` de sorte qu’Axis puisse le trouver, et lancer *gladevcp owordsub.ui* depuis un terminal. Ce qui devrait ressembler à celà:

![images/oword.png]()

#### Préparation d’une Action\_MDI

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

#### Utiliser l’objet Machinekit Stat pour traiter les changements de statut

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

### Programmation de GladeVCP

#### Actions définies par l’utilisateur

La plupart des jeux de widgets, par le biais de l'éditeur Glade, supportent le concept de fonction de rappel, fonctions écrites par l’utilisateur, qui sont exécutées quand *quelque chose arrive* dans l’UI, événements tels que clics de souris, caractère tapé, mouvement de souris, événements d’horloge, fenêtre iconisée ou agrandie et ainsi de suite.

Les widgets de sortie HAL, typiquement, scrutent les événements de type *entrée*, tels qu’un bouton pressé, provoquant un changement de la valeur d’une pin HAL associée par le biais d’une telle fonction de rappel prédéfinie. Dans PyVCP, c’est réellement le seul type d'événement qui peut être défini à la main. Faire quelque chose de plus complexe, comme exécuter une commande MDI pour appeler un sous-programme G-code, n’est pas supporté.

Dans GladeVCP, les changement sur les pins de HAL sont juste un type de la classe générale d'événements (appelés signaux) dans GTK+. La plupart des widgets peuvent générer de tels signaux et l'éditeur de Glade supporte l’association de ces signaux avec une méthode Python ou nom de fonction.

Si vous décidez d’utiliser les actions définies par l’utilisateur, votre travail consistera à écrire un module Python dont la méthode, une fonction suffit dans les cas simples, peut être référencée à un gestionnaire d'événements dans Glade. GladeVCP fournit un moyen d’importer votre module au démarrage, il sera alors lié automatiquement au gestionnaire d'événements avec les signaux de widget comme un ensemble dans la description de l'éditeur Glade.

#### Un exemple: ajouter une fonction de rappel en Python

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

#### L'événement valeur de HAL modifiée

Les widgets d’entrée HAL, comme la Led, ont l'état de leur pin de HAL (on/off), automatiquement associé avec l’apparence optique du widget (Led allumée/éteinte).

Au delà de cette fonctionnalité primitive, on peut associer n’importe quelle pin de HAL avec une fonction de rappel, y compris les widgets de HAL prédéfinis. Cela correspond bien avec la structure événementielle de l’application typique du widget: chaque activité, qu’elle soit un simple clic de souris, une touche pressée, une horloge expirée ou le changement de valeur d’une pin de HAL, générera une fonction de rappel et sera gérée par le même mécanisme.

Pour les pins de HAL définies par l’utilisateur, non associées à un widget de HAL particulier, le nom du signal est *value-changed*. Voir la section [Ajouter des pins de HAL](#gladevcp:Adding_HAL_pins) pour plus de détails.

Les widgets HAL sont fournis avec un signal prédéfini appelé *hal-pin-changed*. Voir la section sur [les Widgets HAL](#gladevcp::hal-pin-changed_signal) pour d’autres détails.

#### Modèle de programmation

L’approche globale est la suivante:

-   Concevoir l’interface graphique avec Glade, fixer les gestionnaires de signaux associés aux widgets action.

-   Écrire un module Python qui contient des objets appelables (voir 'gestionnaire de modèles, plus loin)

-   Passer le chemin du modules à gladevcp avec l’option *-u &lt;module&gt;*.

-   gladevcp importe le module, inspecte les gestionnaires de signaux et les connecte à l’arbre des widgets.

-   La boucle principale d'événements est exécutée.

##### Modèle du gestionnaire simple

Pour des tâches simple, il est suffisant de définir des fonctions nommées après les gestionnaires de signaux de Glade. Elles seront appelées quand l'événement correspondant se produira dans l’arbre des widgets. Voici un exemple très simple, il suppose que le signal *pressed* d’un bouton Gtk ou d’un bouton HAL est lié à une fonction de rappel appelée *on\_button\_press*:

    nhits = 0
    def on_button_press(gtkobj,data=None):
        global nhits
        nhits += 1
        gtkobj.set_label("hits: %d" % nhits)

Ajouter cette fonction dans un fichier Python et le lancer avec:

    gladevcp -u <myhandler>.py mygui.ui

Noter que la communication entre les gestionnaires doit passer par des variables globales, qui s’adaptent mal est ne sont pas très "pythonique". C’est pourquoi nous en arrivons au gestionnaire de classes.

##### Modèle de gestionnaire basé sur les classes

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

##### Le protocole get\_handlers

Si durant l’inspection du module GladeVCP trouve une fonction *get\_handlers*, Il l’appelle de la manière suivante:

    get_handlers(halcomp,builder,useropts)

Les arguments sont:

-   halcomp - Se réfère au composant de HAL en construction.

-   builder - arbre du widget - résulte de la lecture de la définition de l’UI (soit, en référence à un objet de type GtkBuilder ou de type libglade).

-   useropts - Une liste de chaines collectée par l’option de la ligne de commande de gladevcp *-U &lt;useropts&gt;*.

GladeVCP inspecte alors la liste des instances de classe et récupère leurs noms. Les noms de méthode sont connectés à l’arbre des widgets comme gestionnaire de signaux. Seuls, les noms de méthode ne commençant pas par un **\_** (tiret bas) sont considérés.

Noter que peu importe si la libglade ou le nouveau format GtkBuilder est utilisé pour l’UI Glade, les widgets peuvent toujours être soumis au *builder.get\_object(&lt;nomwidget&gt;)*. En outre, la liste complète des widgets est disponible par *builder.get\_objects()*, indépendamment du format de l’UI.

#### Séquence d’initialisation

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

#### Multiple fonctions de rappel avec le même nom

Dans une classe, les noms de méthode doivent être unique. Cependant, il est permis d’avoir de multiples instances de classe passées à GladeVCP par get\_handlers() avec des méthodes portant le même nom. Lorsque le signal correspondant survient, les méthodes sont appelées dans l’ordre dans lequel elles ont été définies, module par module et dans un module, dans l’ordre des instances de classe retourné *get\_handlers()*.

#### Le drapeau GladeVCP **-U &lt;useropts&gt;**

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

#### Variables persistantes dans GladeVCP

Un aspect gênant de GladeVCP dans sa forme initiale avec pyvcp est le fait qu’on peut changer les valeurs des pins de HAL au travers du texte saisi, curseurs, bouton tournant, bouton à bascule etc, mais leurs paramètres ne sont pas enregistrés ni restaurés à la prochaine exécution de Machinekit. Ils commencent aux valeurs par défaut fixées dans le panneau ou la définition du widget.

GladeVCP dispose d’un mécanisme facile à utiliser pour enregistrer et restaurer l'état des widgets de HAL, ainsi que les variables du programme (en fait, n’importe quel attribut d’instance de type int, float, bool ou string).

Ce mécanisme utilise le format du populaire fichier *.ini* pour enregistrer et recharger les attributs persistants.

##### Examen de la persistance, de la version et de la signature du programme

Imaginons renommer, ajouter ou supprimer des widgets dans Glade: un fichier .ini qui traîne depuis une version précédente du programme, ou une interface utilisateur entièrement différente, ne serait pas en mesure de restaurer correctement l'état des variables et des types puisqu’ils ont changé depuis.

GladeVCP détecte cette situation par la signature qui dépends de tous les noms d’objets et de types qui ont été enregistrés et qui doivent être restaurés. Dans le cas de signatures incompatibles, un nouveau fichier .ini avec la configuration pas défaut est généré.

#### Utilisation des variables persistantes

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

#### Édition manuelle des fichiers .ini

Il est possible de faire cela, mais noter que les valeurs dans self.defaults écraseront votre édition si il y a erreur de frappe ou de syntaxe. Une erreur détectée, un message émis dans la console, donneront des indices sur ce qui s’est passé et le mauvais fichier ini sera renommé avec le suffixe .BAD. Après une mauvaise initialisation, les fichiers .BAD les plus anciens seront écrasés.

#### Ajouter des pins de HAL

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

#### Ajout de timers

Depuis que GladeVCP utilise les widgets Gtk qui se rattachent sur les classes de base [GObject](http://www.pygtk.org/pygtk2reference/gobject-functions.html), la totalité des fonctionnalités de la glib est disponible. Voici un exemple d' horloge de fonction de rappel:

    def _on_timer_tick(self,userdata=None):
        ...
        return True # pour relancer l'horloge; return False pour un monostable
    ...
    # démonstration d'une horloge lente en tâche de fond - la granularité est de une seconde
    # pour une horloge rapide (granularité 1 ms), utiliser cela:
    # glib.timeout_add(100, self._on_timer_tick,userdata) # 10Hz
    glib.timeout_add_seconds(1, self._on_timer_tick)

#### Exemples, et lancez votre propre application GladeVCP

Visiter `machinekit/configs/gladevcp` pour des exemples prêt à l’emploi et points de départ de vos propres projets.

### Questions & réponses

1.  *Je reçois un événement unmap inattendu dans ma fonction de gestionnaire juste après le démarrage, qu’est-ce que c’est?*

    C’est la conséquence d’avoir dans votre fichier d’UI Glade la propriété de la fenêtre window1 visible fixée à True, il y a changement de parents de la fenêtre GladeVCP dans Axis ou touchy. L’arbre de widget de GladeVCP est créé, incluant une fenêtre de niveau supérieur puis *re-aparenté dans Axis*, laissant trainer les orphelins de la fenêtre de niveau supérieur. Pour éviter d’avoir cette fenêtre vide qui traine, elle est unmapped (rendue invisible) et la cause du signal unmap que vous avez eux. Suggestion pour fixer le problème: fixer window1.visible à False et ignorer le message initial d'événement unmap.

2.  *Mon programme GladeVCP démarre, mais aucune fenêtre n’apparait alors qu’elle devrait.*

    La fenêtre allouée par Axis pour GladeVCP obtient la *taille naturelle* de tous ses enfants combinés. C’est au widget enfant a réclamer une taille (largeur et/ou hauteur). Cependant, toutes le fenêtres ne demandent pas une plus grande que 0, par exemple, le widget Graph dans sa forme courante. Si il y a un tel widget dans votre fichier Glade et que c’est lui qui défini la disposition vous devrez fixer sa largeur explicitement. Noter que la largeur et la hauteur de la fenêtre window1 dans Glade n’a pas de sens puisque cette fenêtre sera orpheline lors du changement de parent et donc sa géométrie n’aura aucun impact sur les mise en page (voir ci-dessus). La règle générale est la suivante: si vous exécutez manuellement un fichier UI avec *gladevcp &lt;fichierui&gt;* et que sa fenêtre a une géométrie raisonnable, elle devrait apparaitre correctement dans Axis.

3.  *Je veux une Led clignotante, alors j’ai coché une case pour la laisser clignoter avec un intervalle de 100ms. Elle devrait clignoter, mais je reçois un :Warning: value *0* le type *gint* est invalide ou hors de l'étendue pour les propriétés de *led-blink-rate*, c’est quoi le type gint?*

    Il semble qu’il s’agisse d’un bug de Glade. Il faut re-saisir une valeur sur le champ de la fréquence de clignotement et enregistrer à nouveau. Ça a marché pour moi.

4.  *Mon panneau gladevcp ne marche pas dans Axis, il n’enregistre pas les états quand je ferme Axis, j’ai pourtant défini un gestionnaire on\_destroy attaché au signal destroy de la fenêtre.*

    Ce gestionnaire est très probablement lié à window1, qui en raison du changement de parent ne peux pas assurer cette fonction. Attachez le gestionnaire on\_destroy handler au signal destroy d’une fenêtre intérieure. Par exemple: J’ai un notebook dans window1, attaché on\_destroy au signal destroy de notebooks et ça marche bien. Il ne marcherait pas pour window1.

### Troubleshooting

<span class="comment"> FIXME this is out of date</span>

-   make sure your have the development version of Machinekit installed. You don’t need the axisrc file any more, this was mentioned in the old GladeVcp wiki page.

-   run GladeVCP or Axis from a terminal window. If you get Python errors, check whether there’s still a `/usr/lib/python2.6/dist-packages/hal.so` file lying around besides the newer `/usr/lib/python2.6/dist-packages/_hal.so` (note underscore); if yes, remove the `hal.so` file. It has been superseded by hal.py in the same directory and confuses the import mechanism.

-   if you’re using run-in-place, do a *make clean* to remove any accidentally left over hal.so file, then *make*.

-   if you’re using *HAL\_table* or *HAL\_HBox* widgets, be aware they have an HAL pin associated with it which is off by default. This pin controls whether these container’s children are active or not.

### Notes d’implémentation: la gestion des touches dans Axis

Nous pensons que la gestion des touches fonctionne bien, mais comme c’est un nouveau code, nous devons vous informer à ce propos pour que vous puissiez surveiller ces problèmes; S’il vous plaît, faites nous savoir si vous connaissez des erreurs ou des choses bizarres. Voici l’histoire:

Axis utilise le jeu de widget de TkInter. L’application GladeVCP utilise les widgets Gtk et démarre dans un contexte de processus différent. Ils sont attachés dans Axis avec le protocole Xembed. Ce qui permet à une application enfant comme GladeVCP de bien tenir proprement dans la fenêtre d’un parent et, en théorie, d'être intégrée au gestionnaire d'événements.

Toutefois, cela suppose que parent et enfant supportent tous les deux proprement le protocole Xembed, c’est le cas avec Gtk, pas avec TkInter. Une conséquence de cela, c’est que certaines touches ne sont pas transmises correctement dans toutes les circonstances depuis un panneau GladeVCP vers Axis. Une d’elle est la touche *Entrée*. Ou quand le widget SpinButton a le focus, dans ce cas, par exemple la touche Échap n’est pas bien transmise à Axis et cause un abandon avec des conséquences potentiellement désastreuses.

Par conséquent, les événements touches dans GladeVCP, sont traités explicitement, et sélectivement transmises à Axis, pour assurer que de telles situations ne puissent pas survenir. Pour des détails, voir la fonction *keyboard\_forward()* dans la *lib/python/gladevcp/xembed.py*. :leveloffset: 0

Notions avancées
----------------

Python Interface
----------------

<span id="cha:python-interface"></span>

This is work in progress by Michael Haberler. Comments, fixes, and addenda are welcome, especially for PositionLogger (A bit of intent, purpose and usage would help here!)

### The machinekit Python module

User interfaces control Machinekit activity by sending NML messages to the Machinekit task controller, and monitor results by observing the Machinekit status structure, as well as the error reporting channel.

Programmatic access to NML is through a C++ API; however, the most important parts of the NML interface to Machinekit are also available to Python programs through the `machinekit` module.

Beyond the NML interface to the command, status and error channels, the `machinekit` module also contains:

-   support for reading values from ini files

-   support for position logging (???)

### Usage Patterns for the Machinekit NML interface

The general pattern for `machinekit` usage is roughly like this:

-   import the `machinekit` module

-   establish connections to the command, status and error NML channels as needed

-   poll the status channel, either periodically or as needed

-   before sending a command, determine from status whether it is in fact OK to do so (for instance, there is no point in sending a *Run* command if task is in the ESTOP state, or the interpreter is not idle)

-   send the command by using one of the `machinekit` command channel methods

To retrieve messages from the error channel, poll the error channel periodically, and process any messages retrieved.

-   poll the status channel, either periodically or as needed

-   print any error message FIXME: explore the exception code

`machinekit` also defines the `error` Python exception type to support error reporting.

### Reading Machinekit status

Here is a Python fragment to explore the contents of the `machinekit.stat` object which contains some 8ß0+ values (run while machinekit is running for typical values):

    import sys
    import machinekit
    try:
        s = machinekit.stat() # create a connection to the status channel
        s.poll() # get current values
    except machinekit.error, detail:
        print "error", detail
        sys.exit(1)
    for x in dir(s):
        if not x.startswith('_'):
            print x, getattr(s,x)

Machinekit uses the default compiled-in path to the NML configuration file unless overridden, see [Reading ini file values](#sec:Python-reading-ini-values) for an example.

#### machinekit.stat attributes

 **acceleration**   
*(returns float)* - default acceleration, reflects the ini entry \[TRAJ\] DEFAULT\_ACCELERATION.

 **active\_queue**   
*(returns int)* - number of motions blending.

 **actual\_position**   
*(returns tuple of floats)* - current trajectory position, (x y z a b c u v w) in machine units.

 **adaptive\_feed\_enabled**   
*(returns True/False)* - status of adaptive feedrate override (0/1).

 **ain**   
*(returns tuple of floats)* - current value of the analog input pins.

 **angular\_units**   
*(returns string)* - reflects \[TRAJ\] ANGULAR\_UNITS ini value.

 **aout**   
*(returns tuple of floats)* - current value of the analog output pins.

 **axes**   
*(returns string)* - reflects \[TRAJ\] AXES ini value.

 **axis**   
*(returns tuple of dicts)* - reflecting current axis values. See [The axis dictionary](#sec:The-Axis-dictionary).

 **axis\_mask**   
*(returns integer)* - mask of axis available as defined by \[TRAJ\] COORDINATES in the ini file. Returns the sum of the axes X=1, Y=2, Z=4, A=8, B=16, C=32, U=64, V=128, W=256.

 **block\_delete**   
*(returns integer)* - block delete currently on/off.

 **command**   
*(returns string)* - currently executing command.

 **current\_line**   
*(returns integer)* - currently executing line, int.

 **current\_vel**   
*(returns float)* - current velocity in Cartesian space.

 **cycle\_time**   
*(returns string)* - reflects \[TRAJ\] CYCLE\_TIME ini value (FIXME is this right?).

 **debug**   
*(returns integer)* - debug flag.

 **delay\_left**   
*(returns float)* - remaining time on dwell (G4) command, seconds.

 **din**   
*(returns tuple of integers)* - current value of the digital input pins.

 **distance\_to\_go**   
*(returns float)* - remaining distance of current move, as reported by trajectory planner, in Cartesian space.

 **dout**   
*(returns tuple of integers)* - current value of the digital output pins.

 **dtg**   
*(returns tuple of 9 floats)* - remaining distance of current move, as reported by trajectory planner.

 **echo\_serial\_number**   
*(returns integer)* - The serial number of the last completed command sent by a UI to task. All commands carry a serial number. Once the command has been executed, its serial number is reflected in `echo_serial_number`.

 **enabled**   
*(returns integer)* - trajectory planner enabled flag.

 **estop**   
*(returns integer)* - estop flag.

 **exec\_state**   
*(returns integer)* - task execution state. One of EXEC\_ERROR, EXEC\_DONE, EXEC\_WAITING\_FOR\_MOTION, EXEC\_WAITING\_FOR\_MOTION\_QUEUE, EXEC\_WAITING\_FOR\_PAUSE,EXEC\_WAITING\_FOR\_MOTION\_AND\_IO, EXEC\_WAITING\_FOR\_DELAY, EXEC\_WAITING\_FOR\_SYSTEM\_CMD.

 **feed\_hold\_enabled**   
*(returns integer)* - enable flag for feed hold.

 **feed\_override\_enabled**   
*(returns integer)* - enable flag for feed override.

 **feedrate**   
*(returns float)* - current feedrate override.

 **file**   
*(returns string)* - currently executing gcode file.

 **flood**   
*(returns integer)* - flood enabled.

 **g5x\_index**   
*(returns string)* - currently active coordinate system, G54=0, G55=1 etc.

 **g5x\_offset**   
*(returns tuple of floats)* - offset of the currently active coordinate system.

 **g92\_offset**   
*(returns tuple of floats)* - pose of the current g92 offset.

 **gcodes**   
*(returns tuple of 16 integers)* - currently active G-codes.

 **homed**   
*(returns integer)* - flag. 1 if homed.

 **id**   
*(returns integer)* - currently executing motion id.

 **inpos**   
*(returns integer)* - machine-in-position flag.

 **input\_timeout**   
*(returns integer)* - flag for M66 timer in progress.

 **interp\_state**   
*(returns integer)* - current state of RS274NGC interpreter. One of INTERP\_IDLE, INTERP\_READING, INTERP\_PAUSED, INTERP\_WAITING.

 **interpreter\_errcode**   
*(returns integer)* - current RS274NGC interpreter return code. One of INTERP\_OK, INTERP\_EXIT, INTERP\_EXECUTE\_FINISH, INTERP\_ENDFILE, INTERP\_FILE\_NOT\_OPEN, INTERP\_ERROR. see src/emc/nml\_intf/interp\_return.hh

 **joint\_actual\_position**   
*(returns tuple of floats)* - actual joint positions.

 **joint\_position**   
*(returns tuple of floats)* - Desired joint positions.

 **kinematics\_type**   
*(returns integer)* - identity=1, serial=2, parallel=3, custom=4 .

 **limit**   
*(returns tuple of integers)* - axis limit masks. minHardLimit=1, maxHardLimit=2, minSoftLimit=4, maxSoftLimit=8.

 **linear\_units**   
*(returns string)* - reflects \[TRAJ\]LINEAR\_UNITS ini value.

 **lube**   
*(returns integer)* - *lube on* flag.

 **lube\_level**   
*(returns integer)* - reflects *iocontrol.0.lube\_level*.

 **max\_acceleration**   
*(returns float)* - maximum acceleration. reflects \[TRAJ\] MAX\_ACCELERATION.

 **max\_velocity**   
*(returns float)* - maximum velocity. reflects \[TRAJ\] MAX\_VELOCITY.

 **mcodes**   
*(returns tuple of 10 integers)* - currently active M-codes.

 **mist**   
*(returns integer)* - *mist on* flag.

 **motion\_line**   
*(returns integer)* - source line number motion is currently executing. Relation to `id` unclear.

 **motion\_mode**   
*(returns integer)* - motion mode.

 **motion\_type**   
*(returns integer)* - trajectory planner mode. One of TRAJ\_MODE\_COORD, TRAJ\_MODE\_FREE, TRAJ\_MODE\_TELEOP.

 **optional\_stop**   
*(returns integer)* - option stop flag.

 **paused**   
*(returns integer)* - `motion paused` flag.

 **pocket\_prepped**   
*(returns integer)* - A Tx command completed, and this pocket is prepared. -1 if no prepared pocket.

 **poll()**   
- method to update current status attributes.

 **position**   
*(returns tuple of floats)* - trajectory position.

 **probe\_tripped**   
*(returns integer)* - flag, true if probe has tripped (latch)

 **probe\_val**   
*(returns integer)* - reflects value of the `motion.probe-input` pin.

 **probed\_position**   
*(returns tuple of floats)* - position where probe tripped.

 **probing**   
*(returns integer)* - flag, 1 if a probe operation is in progress.

 **program\_units**   
*(returns integer)* - one of CANON\_UNITS\_INCHES=1, CANON\_UNITS\_MM=2, CANON\_UNITS\_CM=3

 **queue**   
*(returns integer)* - current size of the trajectory planner queue.

 **queue\_full**   
*(returns integer)* - the trajectory planner queue is full.

 **read\_line**   
*(returns integer)* - line the RS274NGC interpreter is currently reading.

 **rotation\_xy**   
*(returns float)* - current XY rotation angle around Z axis.

 **settings**   
*(returns tuple of 3 floats)* - current interpreter settings. settings\[0\] = sequence number, settings\[1\] = feed rate, settings\[2\] = speed.

 **spindle\_brake**   
*(returns integer)* - value of the spindle brake flag.

 **spindle\_direction**   
*(returns integer)* - rotational direction of the spindle. forward=1, reverse=-1.

 **spindle\_enabled**   
*(returns integer)* - value of the spindle enabled flag.

 **spindle\_increasing**   
*(returns integer)* - unclear.

 **spindle\_override\_enabled**   
*(returns integer)* - value of the spindle override enabled flag.

 **spindle\_speed**   
*(returns float)* - spindle speed value, rpm, &gt; 0: clockwise, &lt; 0: counterclockwise.

 **spindlerate**   
*(returns float)* - spindle speed override scale.

 **state**   
*(returns integer)* - current command execution status. One of RCS\_DONE, RCS\_EXEC, RCS\_ERROR.

 **task\_mode**   
*(returns integer)* - current task mode. one of MODE\_MDI, MODE\_AUTO, MODE\_MANUAL.

 **task\_paused**   
*(returns integer)* - task paused flag.

 **task\_state**   
*(returns integer)* - current task state. one of STATE\_ESTOP, STATE\_ESTOP\_RESET, STATE\_ON, STATE\_OFF.

 **tool\_in\_spindle**   
*(returns integer)* - current tool number.

 **tool\_offset**   
*(returns tuple of floats)* - offset values of the current tool.

 **tool\_table**   
*(returns tuple of tool\_results)* - list of tool entries. Each entry is a sequence of the following fields: id, xoffset, yoffset, zoffset, aoffset, boffset, coffset, uoffset, voffset, woffset, diameter, frontangle, backangle, orientation. The id and orientation are integers and the rest are floats.

 **velocity**   
*(returns float)* - default velocity. reflects \[TRAJ\] DEFAULT\_VELOCITY.

#### The `axis` dictionary <span id="sec:The-Axis-dictionary"></span>

The axis configuration and status values are available through a list of per-axis dictionaries. Here’s an example how to access an attribute of a particular axis:

    import machinekit
    s = machinekit.stat()
    s.poll()
    print 'Axis 1 homed: ', s.axis[1]['homed']

For each axis, the following dictionary keys are available:

 **axisType**   
*(returns integer)* - type of axis configuration parameter, reflects \[AXIS\_x\]TYPE. LINEAR=1, ANGULAR=2. See [Axis ini configuration](#sub:AXIS-section) for details.

 **backlash**   
*(returns float)* - Backlash in machine units. configuration parameter, reflects \[AXIS\_x\]BACKLASH.

 **enabled**   
*(returns integer)* - non-zero means enabled.

 **fault**   
*(returns integer)* - non-zero means axis amp fault.

 **ferror\_current**   
*(returns float)* - current following error.

 **ferror\_highmark**   
*(returns float)* - magnitude of max following error.

 **homed**   
*(returns integer)* - non-zero means has been homed.

 **homing**   
*(returns integer)* - non-zero means homing in progress.

 **inpos**   
*(returns integer)* - non-zero means in position.

 **input**   
*(returns float)* - current input position.

 **max\_ferror**   
*(returns float)* - maximum following error. configuration parameter, reflects \[AXIS\_x\]FERROR.

 **max\_hard\_limit**   
*(returns integer)* - non-zero means max hard limit exceeded.

 **max\_position\_limit**   
*(returns float)* - maximum limit (soft limit) for axis motion, in machine units.configuration parameter, reflects \[AXIS\_x\]MAX\_LIMIT.

 **max\_soft\_limit**   
non-zero means `max_position_limit` was exceeded, int

 **min\_ferror**   
*(returns float)* - configuration parameter, reflects \[AXIS\_x\]MIN\_FERROR.

 **min\_hard\_limit**   
*(returns integer)* - non-zero means min hard limit exceeded.

 **min\_position\_limit**   
*(returns float)* - minimum limit (soft limit) for axis motion, in machine units.configuration parameter, reflects \[AXIS\_x\]MIN\_LIMIT.

 **min\_soft\_limit**   
*(returns integer)* - non-zero means `min_position_limit` was exceeded.

 **output**   
*(returns float)* - commanded output position.

 **override\_limits**   
*(returns integer)* - non-zero means limits are overridden.

 **units**   
*(returns float)* - units per mm, deg for linear, angular

 **velocity**   
*(returns float)* - current velocity.

### Preparing to send commands

Some commands can always be sent, regardless of mode and state; for instance, the `machinekit.command.abort()` method can always be called.

Other commands may be sent only in appropriate state, and those tests can be a bit tricky. For instance, an MDI command can be sent only if:

-   ESTOP has not been triggered, and

-   the machine is turned on and

-   the axes are homed and

-   the interpreter is not running and

-   the mode is set to `MDI mode`

so an appropriate test before sending an MDI command through `machinekit.command.mdi()` could be:

    import machinekit
    s = machinekit.stat()
    c = machinekit.command()

    def ok_for_mdi():
        s.poll()
        return not s.estop and s.enabled and s.homed and (s.interp_state == machinekit.INTERP_IDLE)

    if ok_for_mdi():
       c.mode(machinekit.MODE_MDI)
       c.wait_complete() # wait until mode switch executed
       c.mdi("G0 X10 Y20 Z30")

### Sending commands through `machinekit.command`

Before sending a command, initialize a command channel like so:

    import machinekit
    c = machinekit.command()

    # Usage examples for some of the commands listed below:
    c.abort()

    c.auto(machinekit.AUTO_RUN, program_start_line)
    c.auto(machinekit.AUTO_STEP)
    c.auto(machinekit.AUTO_PAUSE)
    c.auto(machinekit.AUTO_RESUME)

    c.brake(machinekit.BRAKE_ENGAGE)
    c.brake(machinekit.BRAKE_RELEASE)

    c.flood(machinekit.FLOOD_ON)
    c.flood(machinekit.FLOOD_OFF)

    c.home(2)

    c.jog(machinekit.JOG_STOP, axis)
    c.jog(machinekit.JOG_CONTINUOUS, axis, speed)
    c.jog(machinekit.JOG_INCREMENT, axis, speed, increment)

    c.load_tool_table()

    c.maxvel(200.0)

    c.mdi("G0 X10 Y20 Z30")

    c.mist(machinekit.MIST_ON)
    c.mist(machinekit.MIST_OFF)

    c.mode(machinekit.MODE_MDI)
    c.mode(machinekit.MODE_AUTO)
    c.mode(machinekit.MODE_MANUAL)

    c.override_limits()

    c.program_open("foo.ngc")
    c.reset_interpreter()

    c.set_home_parameters(jointnum, home_pos, home_offset, home_final_velocity, home_search_velocity, home_final_velocity, use_index, ignore_limits, is_shared, home_sequence, volatile_home, locking_indexer) )


    c.tool_offset(toolno, z_offset,  x_offset, diameter, frontangle, backangle, orientation)

#### `machinekit.command` attributes

 `serial`   
the current command serial number

#### `machinekit.command` methods:

 `abort()`   
send EMC\_TASK\_ABORT message.

 `auto(int[, int])`   
run, step, pause or resume a program.

 `brake(int)`   
engage or release spindle brake.

 `debug(int)`   
set debug level via EMC\_SET\_DEBUG message.

 `feedrate(float)`   
set the feedrate.

 `flood(int)`   
turn on/off flooding.

 `home(int)`   
home a given axis.

 `jog(int, int, [, int[,int]])`   
Syntax:
 jog(command, axis\[, velocity\[, distance\]\])
 jog(machinekit.JOG\_STOP, axis)
 jog(machinekit.JOG\_CONTINUOUS, axis, velocity)
 jog(machinekit.JOG\_INCREMENT, axis, velocity, distance)
 Constants:
 JOG\_STOP (0)
 JOG\_CONTINUOUS (1)
 JOG\_INCREMENT (2)

 `load_tool_table()`   
reload the tool table.

 `maxvel(float)`   
set maximum velocity

 `mdi(string)`   
send an MDI command. Maximum 255 chars.

 `mist(int)`   
turn on/off mist.
 Syntax:
 mist(command)
 mist(machinekit.MIST\_ON) \[(1)\]
 mist(machinekit.MIST\_OFF) \[(0)\]
 Constants:
 MIST\_ON (1)
 MIST\_OFF (0)

 `mode(int)`   
set mode (MODE\_MDI, MODE\_MANUAL, MODE\_AUTO).

 `override_limits()`   
set the override axis limits flag.

 `program_open(string)`   
open an NGC file.

 `reset_interpreter()`   
reset the RS274NGC interpreter

 `set_adaptive_feed(int)`   
set adaptive feed flag

 `set_analog_output(int, float)`   
set analog output pin to value

 `set_block_delete(int)`   
set block delete flag

 `set_digital_output(int, int)`   
set digital output pin to value

 `set_feed_hold(int)`   
set feed hold on/off

 `set_feed_override(int)`   
set feed override on/off

 `set_max_limit(int, float)`   
set max position limit for a given axis

 *set\_home\_parameters(int, float, float, float, float, float, int, int, int, int, int, int)*   
set home parameters for a given axis. All parameters must be passed to the function to succeed.

 `set_min_limit()`   
set min position limit for a given axis

 `set_optional_stop(int)`   
set optional stop on/off

 `set_spindle_override(int)`   
set spindle override flag

 `spindle(int)`   
set spindle direction. Argument one of SPINDLE\_FORWARD, SPINDLE\_REVERSE, SPINDLE\_OFF, SPINDLE\_INCREASE, SPINDLE\_DECREASE, or SPINDLE\_CONSTANT.

 `spindleoverride(float)`   
set spindle override factor

 `state(int)`   
set the machine state. Machine state should be STATE\_ESTOP, STATE\_ESTOP\_RESET, STATE\_ON, or STATE\_OFF

 `teleop_enable(int)`   
enable/disable teleop mode.

 `teleop_vector(float, float, float [,float, float, float])`   
set teleop destination vector

 `tool_offset(int, float, float, float, float, float, int)`   
set the tool offset. See usage example above.

 `traj_mode(int)`   
set trajectory mode. Mode is one of MODE\_FREE, MODE\_COORD, or MODE\_TELEOP.

 `unhome(int)`   
unhome a given axis.

 `wait_complete([float])`   
wait for completion of the last command sent. If timeout in seconds not specified, default is 1 second.

### Reading the error channel

To handle error messages, connect to the error channel and periodically poll() it.

Note that the NML channel for error messages has a queue (other than the command and status channels), which means that the first consumer of an error message deletes that message from the queue; whether your another error message consumer (e.g. Axis) will *see* the message is dependent on timing. It is recommended to have just one error channel reader task in a setup.

    import machinekit
    e = machinekit.error_channel()

    error = e.poll()

    if error:
        kind, text = error
        if kind in (machinekit.NML_ERROR, machinekit.OPERATOR_ERROR):
            typus = "error"
        else:
            typus = "info"
            print typus, text

### Reading ini file values <span id="sec:Python-reading-ini-values"></span>

Here’s an example for reading values from an ini file through the `machinekit.ini` object:

    # run as:
    # python ini-example.py ~/emc2-dev/configs/sim/axis/axis_mm.ini

    import sys
    import machinekit

    inifile = machinekit.ini(sys.argv[1])

    # inifile.find() returns None if the key wasnt found - the
    # following idiom is useful for setting a default value:

    machine_name = inifile.find('EMC', 'MACHINE') or "unknown"
    print "machine name: ", machine_name

    # inifile.findall() returns a list of matches, or an empty list
    # if the key wasnt found:

    extensions = inifile.findall("FILTER", "PROGRAM_EXTENSION")
    print "extensions: ", extensions

    # override default NML file by ini parameter if given
    nmlfile = inifile.find("EMC", "NML_FILE")
    if nmlfile:
        machinekit.nmlfile = os.path.join(os.path.dirname(sys.argv[1]), nmlfile)

### The `machinekit.positionlogger` type

Some usage hints can be gleaned from `src/emc/usr_intf/gremlin/gremlin.py`.

#### members

 `npts`   
number of points.

#### methods

 `start(float)`   
start the position logger and run every ARG seconds

 `clear()`   
clear the position logger

 `stop()`   
stop the position logger

 `call()`   
Plot the backplot now.

 `last([int])`   
Return the most recent point on the plot or None , :lang: fr :toc:

La cinématique dans Machinekit
------------------------------

<span id="cha:Cinematique"></span>

### Introduction

Habituellement quand nous parlons de machines CNC, nous pensons à des machines programmées pour effectuer certains mouvements et effectuer diverses tâches. Pour avoir une représentation unifiée dans l’espace de ces machines, nous la faisons correspondre à la vision humaine de l’espace en 3D, la plupart des machines (sinon toutes) utilisent un système de coordonnées courant, le système Cartésien.

Le système de coordonnées Cartésiennes est composé de 3 axes (X, Y, Z) chacun perpendiculaire aux 2 autres. <span class="footnote">
\[Le mot *axes* est aussi communément (et incorrectement) utilisé à propos des machines CNC, il fait référence aux directions des mouvements de la machine.\]
</span>

Quand nous parlons d’un programme G-code (RS274/NGC) nous parlons d’un certain nombre de commandes (G0, G1, etc.) qui ont comme paramètres (X- Y- Z-). Ces positions se référent exactement à des positions Cartésiennes. Une partie du contrôleur de mouvements de Machinekit est responsable de la translation entre ces positions et les positions correspondantes de la cinématique de la machine<span class="footnote">
\[Cinématique: une fonction à deux voies pour transformer un espace Cartésien en espace à articulations\]
</span>.

#### Les articulations par rapport aux axes

Une articulation, pour une machine CNC est un des degrés physiques de liberté de la machine. Elle peut être linéaire (vis à billes) ou rotative (table tournante, articulations d’un bras robotisé). Il peut y avoir n’importe quel nombre d’articulations sur une machine. Par exemple, un robot classique dispose de 6 articulations et une fraiseuse classique n’en a que 3.

Sur certaines machines, les articulations sont placées de manière à correspondre aux axes cinématiques (articulation 0 le long de l’axe X, articulation 1 le long de l’axe Y et articulation 2 le long de l’axe Z), ces machines sont appelées machines Cartésiennes (ou encore machines à cinématiques triviales . Ce sont les machines les plus courantes parmi les machines-outils mais elles ne sont pas courantes dans d’autres domaines comme les machines de soudage (ex: robots de soudage de type puma).

### Cinématiques triviales

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

### Cinématiques non triviales

Il peut y avoir un certain nombre de types de configurations de machine (robots: puma, scara; hexapodes etc.) Chacun d’eux est mis en place en utilisant des articulations linéaires et rotatives. Ces articulations ne correspondent pas habituellement avec les coordonnées Cartésiennes, cela nécessite une fonction cinématique qui fasse la conversion (en fait 2 fonctions: fonction en avant et inverse de la cinématique).

Pour illustrer ce qui précède, nous analyserons une simple cinématique appelée bipode (une version simplifiée du tripode, qui est déjà une version simplifiée de l’hexapode).

![images/bipod.png]()

Figure 16. Définir un bipode<span id="cap:Bipod-setup"></span>

Le bipode dont nous parlons est un appareil, composé de deux moteurs placés sur un mur, à cet appareil un mobile est suspendu par des fils. Les articulations dans ce cas sont les distances entre le mobile et les moteurs de l’appareil (nommées AD et BD sur la figure ci-dessus.

La position des moteurs est fixée par convention. Le moteur A est en (0,0), qui signifie que sa coordonnée X est 0 et sa coordonnée Y également 0. Le moteur B est placé en (Bx, 0), se qui veut dire que sa coordonnée X est Bx.

Notre pointe mobile se trouvera au point D défini par les distances AD et BD, et par les coordonnées Cartésiennes Dx, Dy.

La tâche de la cinématique consistera à transformer les longueurs des articulations en (AD, BD) en coordonnées Cartésiennes (Dx, Dy) et vice-versa.

#### Transformation avant<span id="sec:Forward-transformation"></span>

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

#### Transformation inverse<span id="sec:Inverse-transformation"></span>

La cinématique inverse est beaucoup plus simple dans notre exemple, de sorte que nous pouvons l'écrire directement:

**AD=sqrt(x<sup>2</sup>+y<sup>2</sup>)**

**BD=sqrt((Bx-x)<sup>2</sup>+y<sup>2</sup>)**

ou traduite en code:

    double x2 = pos->tran.x * pos->tran.x;
    double y2 = pos->tran.y * pos->tran.y;
    joints[0] = sqrt(x2 + y2);
    joints[1] = sqrt((Bx - pos->tran.x)*(Bx - pos->tran.x) + y2);
    return 0;

### Détails d’implémentation

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

Réglages des pas à pas
----------------------

<span id="cha:Reglages-de-pas-a-pas"></span>

### Obtenir le meilleur pilotage logiciel possible

Faire générer les impulsions de pas au logiciel présente un gros avantage, c’est gratuit. Quasiment chaque PC dispose d’un port parallèle capable de sortir sur ses broches les signaux de pas générés par le logiciel. Cependant, les générateurs d’impulsions logiciels ont aussi quelques inconvénients:

-   La fréquence maximum des impulsions est limitée.

-   Les impulsions générées sont irrégulières à cause du bruit.

-   Elles sont sujettes à la charge du CPU

Ce chapitre présente certaines mesures qui vous aideront à obtenir les meilleurs résultats du logiciel.

#### Effectuer un test de latence

Le CPU n’est pas le seul facteur déterminant la latence. Les cartes mères, cartes graphiques, ports USB et nombre d’autres choses peuvent la dégrader. La meilleure façon de savoir ce que vous pouvez attendre d’un PC consiste à exécuter le test de latence RTAI.

Lancer un test comme décrit [au chapitre sur le test](#cha:test-de-latence) de latence.

#### Connaître ce dont vos cartes de pilotage ont besoin

Les différente marques de cartes de pilotage de moteurs pas à pas demandent toutes des timing différents pour les impulsions de commande de pas et de direction. Aussi vous avez besoin d’accéder (ou Google) à la fiche des spécifications techniques de votre carte.

Par exemple, le manuel du Gecko G202 indique:

    Step Frequency: 0 to 200 kHz
    Step Pulse “0” Time: 0.5 µs min (Step on falling edge)
    Step Pulse “1” Time: 4.5 µs min
    Direction Setup: 1 µs min (20 µs min hold time after Step edge)

Les spécifications du Gecko G203V indiquent:

    Step Frequency: 0 to 333 kHz
    Step Pulse “0” Time: 2.0 µs min (Step on rising edge)
    Step Pulse “1” Time: 1.0 µs min

    Direction setup:
        200 ns (0.2µs) before step pulse rising edge
        200 ns (0.2µs) hold after step pulse rising edge

Un carte Xylotex donne dans ses données techniques un superbe graphe du timing nécessaire, il indique:

    Minimum DIR setup time before rising edge of STEP Pulse 200ns Minimum
    DIR hold time after rising edge of STEP pulse 200ns
    Minimum STEP pulse high time 2.0µs
    Minimum STEP pulse low time 1.0µs
    Step happens on rising edge

Notez les valeurs que vous trouvez, vous en aurez besoin pour la prochaine étape.

#### Choisir la valeur de BASE\_PERIOD

BASE\_PERIOD est l’horloge de votre Machinekit. A chaque période, le générateur d’impulsions de pas décide si il est temps pour une autre impulsion. Une période plus courte vous permettra de générer plus d’impulsions par seconde, dans les limites. Mais si vous la réglez trop bas, votre ordinateur va passer autant de temps à générer des impulsions de pas que pour exécuter tous le reste de ses tâches, il finira peut-être même par se bloquer. La latence et la génération de pas exigent d’affecter la plus courte période utilisable, comme nous le verrons un peu plus loin.

Regardons l’exemple du Gecko en premier. Le G202 peut gérer des impulsions restant à l'état bas pendant 0.5µs et à l'état haut pendant 4.5µs, il a besoin que la broche de direction soit stable 1µs avant le front descendant et qu’elle reste stable pendant 20µs après le front descendant. La plus longue durée est de 20µs, c’est le temps de maintien. Une approche simple consisterait à fixer la période à 20µs. Ce qui signifierait que tous les changements d'état des lignes STEP et DIR serait espacés de 20µs. C’est tout bon, non?

Faux! Si la latence était de zéro, et que tous les fronts soient espacés de 20µs, tout irait bien. Mais tous les ordinateurs ont une latence. Si l’ordinateur a 11µs de latence, celà signifie que, ce que l’ordinateur exécute aura parfois un retard de 11µs et la fois suivante pourra être juste à l’heure, le délai entre le premier et le second sera seulement de 9µs. Si le premier génére l’impulsion de pas et le second change la broche de direction, le timing de 20µs requis par le G202 sera tout simplement violé. Cela signifie que votre moteur aura peut être fait un pas dans la mauvaise direction et que votre pièce ne sera pas à la cote.

Le côté vraiment mauvais de ce problème est qu’il peut être très très rare. Les pires latences sont celles qui ne se produisent que quelques fois par minute. Les chances qu’une mauvaise latence de ce genre arrive juste quand le moteur est en train de changer de direction sont faibles. Ainsi, vous avez de très rares erreurs qui vous ruinent une pièce de temps en temps et qui sont impossibles à résoudre.

La façon la plus simple pour éviter ce problème est de choisir une BASE\_PERIOD qui soit la somme de la plus longue période requise par votre carte plus la durée de la pire latence de votre ordinateur. Si vous utilisez un Gecko avec un temps de maintien exigé de 20µs et que votre test de latence vous avait donné une latence maximum de 11µs, alors si vous définissez BASE\_PERIOD à 20+11 = 31µs (31000 nanosecondes dans le fichier ini), vous aurez la garantie de répondre aux exigences de votre carte de pilotage.

Mais c’est un compromis. Faire une impulsion de pas demande au moins deux périodes. Une pour débuter l’impulsion, et une pour y mettre fin. Etant donné que la période est de 31µs, il faut 2x31 = 62µs pour créer une impulsion de pas. Ce qui signifie que la fréquence de pas maximum sera seulement de 16129 pas par seconde. Pas très bon. (Mais n’abandonnez pas, nous avons encore quelques réglages à faire dans la section suivante.)

Pour la Xylotex, la configuration demande des temps de maintien très courts de 200ns chacun (0.2µs). Le temps le plus long est de 2µs. Si vous avez 11µs de latence, alors vous pouvez définir BASE\_PERIOD aussi bas que 11+2 = 13µs. Se débarrasser du long temps de maintien de 20µs aide vraiment. Avec une période de 13µs, un pas complet ne dure que 26µs = 2x13 et la fréquence maximum est de 38461 pas par seconde!

Mais ne commencez pas à célébrer celà. Notez que 13µs est une période très courte. Si vous essayez d’exécuter le générateur de pas toutes les 13µs, il ne restera peut-être pas assez de temps pour faire autre chose et votre ordinateur se bloquera. Si vous visez des périodes de moins de 25µs, vous devez commencer à 25µs ou plus, lancer Machinekit et voir comment les choses réagissent. Si tout va bien, vous pouvez réduire progressivement la période. Si le pointeur de la souris commence à être sacadé et que le reste du PC ralentit, votre période est un peu trop court. Retournez alors à la valeur précédente qui permettent le meilleur fonctionnement.

Dans ce cas, supposons que vous ayez commencé à 25µs, en essayant descendre à 13µs, vous trouvez que c’est autour de 16µs que se situe la limite la plus basse et qu’en dessous l’ordinateur ne répond plus très bien. Alors, vous utilisez 16µs. Avec une période à 16µs et une latence à 11µs, le temps de sortie le plus court sera de 16-11 = 5µs. La carte demande seulement 2µs, ainsi vous aurez une certaine marge. Il est bon d’avoir une marge si vous ne voulez pas perdre de pas parce que vous auriez réglé un timing trop court.

Quel est la fréquence de pas maximum? Rappelez-vous, deux périodes pour faire un pas. Vous avez réglé la période à 16µs alors qu’un pas prend 32µs. Il fonctionnera à 31250 pas par seconde, ce qui n’est pas mal.

#### Utiliser steplen, stepspace, dirsetup, et/ou dirhold

Dans la section précédente, nous avons utilisé la carte de puissance Xylotex pour piloter nos moteurs avec une période de 16µs ce qui nous a donné une fréquence de pas de 31250 pas par seconde maximum. Alors que la Gecko a été bloquée à 31µs avec une assez mauvaise fréquence de pas de 16129 pas par seconde. L’exemple de la Xylotex est au mieux de ce que nous puissions faire. Mais la Gecko peut être ameliorées.

Le problème avec le G202 est le temps de maintien demandé de 20µs. Ca plus la latence de 11µs nous oblige à utiliser une période longue de 31µs. Mais le générateur de pas logiciel de Machinekit a un certain nombre de paramètres qui permettent d’augmenter les différentes durées d’une période à plusieurs autres. Par exemple, si *steplen* passe de 1 à 2, alors il y aura deux périodes entre le début et la fin de l’impulsion. De même, si *dirhold* passe de 1 à 3, il y aura au moins trois périodes entre l’impulsion de pas et un changement d'état de la broche de direction.

Si nous pouvons utiliser *dirhold* pour le temps de maintien de 20µs demandé, alors le temps le plus long suivant sera de 4.5µs. Ajoutez les 11µs de latence à ces 4.5µs, et vous obtenez une période minimale de 15.5µs. Lorsque vous essayez 15.5µs, vous trouvez que l’ordinateur est très lent, donc vous régler sur 16µs. Si nous laissons *dirhold* à 1 (par défaut), alors le temps minimum entre le pas et la direction est de 16µs moins la période de latence de 11µs = 5µs, ce qui n’est pas suffisant. Nous avons besoin de 15 autres µs, puisque la période est de 16µs, nous avons besoin d’une période de plus. Nous allons donc passer *dirhold* de 1 à 2. Maintenant, le temps minimum entre la fin de l’impulsion et l’impulsion de changement de direction est de 5+16 = 21µs et nous n’avons pas à craindre que la Gecko parte dans la mauvaise direction en raison de la latence.

Si l’ordinateur a une latence de 11µs, alors la combinaison d’une période de base de 16µs et d’une valeur de *dirhold* de 2 garanti que nous serons toujours dans le respect des délais exigés par la Gecko. Pour les pas normaux (sans changement de direction), l’augmentation de la valeur de *dirhold* n’aura aucun effet. Il faudra deux périodes d’un total de 32µs pour faire un seul pas et nous avons la même fréquence de 31250 pas par seconde que nous avions eu avec la Xylotex.

Le temps de latence de 11µs utilisé dans cet exemple est très bon. Si vous travaillez par le biais de ces exemples avec des latences plus grandes, comme 20 ou 25µs, la fréquence de pas la plus grande à la fois pour la Xylotex et la Gecko sera plus faible. Mais les mêmes formules sont applicables pour calculer un BASE\_PERIOD optimal et pour régler *dirhold* ou d’autres paramètres du générateur de pas.

#### Pas de secret!

Pour un système à moteurs pas à pas avec générateur de pas logiciel rapide et fiable, vous ne pouvez pas deviner la période et les autres paramètres de configuration. Vous devez faire des mesures sur votre ordinateur et faire les calculs qui garantirons les meilleurs signaux dont les moteurs ont besoin.

Pour rendre le calcul plus facile, j’ai créé une feuille de calcul Open Office: [Step Timing Calculator (en) - Calculatrice Calendrier étape (fr)](http://wiki.machinekit.org/uploads/StepTimingCalculator.ods). Vous entrez les résultats du test de latence et les timing de votre carte de pilotage et la feuille calcule la meilleure BASE\_PERIOD. Ensuite, vous testez la période pour vous assurer que votre PC ne sera pas ralenti ou bloqué. Enfin, vous entrez dans la période actuelle et la feuille de calcul vous indiquera le réglage de stepgen nécessaire pour répondre aux exigences de votre carte de pilotage. Elle calcule aussi la fréquence de pas maximum que vous serez en mesure de générer.

J’ai ajouté quelques petites choses à la feuille de calcul pour calculer la fréquence maximum et quelques autres calculs.

Réglages d’une boucle PID
-------------------------

<span id="cha:Regulation-PID"></span>

### Régulation à PID

Un régulateur Proportionnel Intégral Dérivé (PID) est un organe de contrôle qui permet d’effectuer une régulation en boucle fermée d’un procédé.

Le régulateur compare une valeur mesurée sur le procédé avec une valeur de consigne. La différence entre ces deux valeurs (le signal d'*erreur*) est alors utilisée pour calculer une nouvelle valeur d’entrée du procédé tendant à réduire au maximum l'écart entre la mesure et la consigne (signal d’erreur le plus faible possible).

Contrairement aux algorithmes de régulation simples, le contrôle par PID peut ajuster les sorties du procédé, en fonction de l’amplitude du signal d’erreur, et en fonction du temps. Il donne des résultats plus précis et un contrôle plus stable. (Il est montré mathématiquement qu’une boucle PID donne un contrôle plus stable qu’un contrôle proportionnel seul et qu’il est plus précis que ce dernier qui laissera le procédé osciller).

#### Les bases du contrôle en boucle

Intuitivement, une boucle PID essaye d’automatiser ce que fait un opérateur muni d’un multimètre et d’un potentiomètre de contrôle. L’opérateur lit la mesure de sortie du procédé, affichée sur le multimètre et utilise le bouton du potentiomètre pour ajuster l’entrée du procédé (l'*action*) jusqu'à stabiliser la mesure de la sortie souhaitée, affichée sur le multimètre.

Un boucle de régulation est composée de trois parties:

1.  La mesure, effectuée par un capteur connecté à un procédé, par exemple un codeur.

2.  La décision, prise par les éléments du régulateur.

3.  L’action sur le dispositif de sortie, par exemple: un moteur.

Quand le régulateur lit le capteur, il soustrait la valeur lue à la valeur de la consigne et ainsi, obtient l'«erreur de mesure». Il peut alors utiliser cette erreur pour calculer une correction à appliquer sur la variable d’entrée du procédé (l'*action*) de sorte que cette correction tende à supprimer l’erreur mesurée en sortie de procédé.

Dans une boucle PID, la correction à partir de l’erreur est calculée de trois façons: P) l’erreur de mesure courante est soustraite directement (effet proportionnel), I) l’erreur est intégrée pendant un laps de temps (effet intégral), D) l’erreur est dérivée pendant un laps de temps (effet dérivé).

Une régulation à PID peut être utilisée dans n’importe quel procédé pour contrôler une variable mesurable, en manipulant d’autres variables de ce procédé. Par exemple, elle peut être utilisée pour contrôler: température, pression, débit, composition chimique, vitesse et autres variables.

Dans certains systèmes de régulation, les régulateurs sont placés en série ou en parallèle. Dans ces cas, le régulateur *maître* produit les signaux utilisés par les régulateurs *esclaves*. Une situation courante dans le contrôle des moteurs, la régulation de vitesse, qui peut demander que la vitesse du moteur soit contrôlée par un régulateur *esclave* (souvent intégré dans le variateur de fréquence du moteur) recevant en entrée une valeur proportionnelle à la vitesse. Cette entrée de l'*esclave* est alors fournie par la sortie du régulateur *maître*, lequel reçoit la variable de consigne.

#### Théorie

Le *PID* représente les abréviations des trois actions qu’il utilise pour effectuer ses corrections, ce sont des ajouts d’un signal à un autre. Tous agissent sur la quantité régulée. Les actions aboutissent finalement à des *soustractions* de l’erreur de mesure, parce que le signal proportionnel est habituellement négatif.

##### Action Proportionnelle

Pour cette action, l’erreur est multipliée par la constante P (pour Proportionnel) qui est négative, puis ajoutée (soustraction de l’erreur de mesure) à la quantité régulée. P est valide uniquement sur la bande dans laquelle le signal de sortie du régulateur est proportionnel à l’erreur du système. Noter que si l’erreur de mesure est égale à zéro, la partie proportionnelle de la sortie du régulateur est également à zéro.

##### Action Intégrale

L’action intégrale fait intervenir la notion de temps. Elle tire profit du signal d’erreur passé qui est intégré (additionné) pendant un laps de temps, puis multiplié par la constante I (négative) ce qui en fait une moyenne, elle est enfin additionnée (soustraction de l’erreur de mesure) à la quantité régulée. La moyenne de l’erreur de mesure permet de trouver l’erreur moyenne entre la sortie du régulateur et la valeur de la consigne. Un système seulement proportionnel oscille en plus et en moins autour de la consigne du fait qu’en arrivant vers la consigne, l’erreur est à zéro, il n’enlève alors plus rien est dépasse la consigne, ou oscille et/ou se stabilise à une valeur trop basse ou trop élevée. L’addition sur l’entrée d’une proportion négative (soustraction) de l’erreur de mesure moyennée, permet toujours de réduire l'écart moyen entre la mesure en sortie et la consigne. Donc finalement, une boucle PI bien réglée verra sa sortie redescendre lentement à la valeur de la consigne.

##### Action Dérivée

L’action dérivée utilise aussi la notion de temps. Elle cherche à anticiper l’erreur future. La dérivée première (la pente de l’erreur) est calculée pour un laps de temps et multipliée par la constante (négative) D, puis elle est additionnée (soustraction de l’erreur de mesure) à la quantité régulée. L’action dérivée de la régulation fourni une réponse aux perturbations agissant sur le système. Plus important est le terme dérivé, plus rapide sera la réponse en sortie à une perturbation sur l’entrée.

Plus techniquement, une boucle PID peut être caractérisée comme un filtre appliqué sur un système complexe d’un domaine fréquentiel. C’est utilisé pour calculer si le système atteindra une valeur stable. Si les valeurs sont choisies incorrectement, le procédé entrera en oscillation et sa sortie n’atteindra jamais la consigne.

#### Réglage d’une boucle

Régler une boucle de régulation consiste à agir sur les paramètres des différentes actions (gain du proportionnel, gain de l’intégral, gain de la dérivée) sur des valeurs optimales pour obtenir la réponse désirée sur la sortie du procédé. Le comportement des procédés varient selon les applications lors d’un changement de consigne. Certains procédés ne permettent aucun dépassement de la consigne. D’autres doivent minimiser l'énergie nécessaire pour atteindre un nouveau point de consigne. Généralement la stabilité de la réponse est requise, le procédé ne doit pas osciller quels que soient les conditions du procédé et le point de consigne.

Régler une boucle est rendu plus compliqué si le temps de réponse du procédé est long; il peut prendre plusieurs minutes, voir plusieurs heures pour qu’une modification de consigne produise un effet stable. Certains procédés ne sont pas linéaires et les paramètres qui fonctionnent bien à pleine charge ne marchent plus lors du démarrage hors charge du procédé. Cette section décrit quelques méthodes manuelles traditionnelles pour régler ces boucles.

Il existe plusieurs méthodes pour régler une boucle PID. Le choix de la méthode dépendra en grande partie de la possibilité ou non de mettre la boucle «hors production» pour la mise au point ainsi que de la vitesse de réponse du système. Si le système peut être mis hors production, la meilleure méthode de réglage consiste souvent à soumettre le système à un changement de consigne, à mesurer la réponse en fonction du temps et à l’aide de cette réponse à déterminer les paramètres de la régulation.

##### Méthode simple

Si le système doit rester en production, une méthode de réglage consiste à mettre les valeurs I et D à zéro. Augmenter ensuite le gain P jusqu'à ce que la sortie de la boucle oscille. Puis, augmenter le gain I jusqu'à ce que cesse l’oscillation. Enfin, augmenter le gain D jusqu'à ce que la boucle soit suffisamment rapide pour atteindre rapidement sa consigne. Le réglage d’une boucle PID rapide provoque habituellement un léger dépassement de consigne pour avoir une montée plus rapide, mais certains systèmes ne le permettent pas.

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Paramètre</th>
<th align="left">Temps de montée</th>
<th align="left">Dépassement</th>
<th align="left">Temps de réglage</th>
<th align="left">S.S. Error</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>P</p></td>
<td align="left"><p>Augmente</p></td>
<td align="left"><p>Augmente</p></td>
<td align="left"><p>Chang. faible</p></td>
<td align="left"><p>Diminue</p></td>
</tr>
<tr class="even">
<td align="left"><p>I</p></td>
<td align="left"><p>Diminue</p></td>
<td align="left"><p>Augmente</p></td>
<td align="left"><p>Augmente</p></td>
<td align="left"><p>Eliminate</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D</p></td>
<td align="left"><p>Chang. faible</p></td>
<td align="left"><p>Diminue</p></td>
<td align="left"><p>Diminue</p></td>
<td align="left"><p>Chang. faible</p></td>
</tr>
</tbody>
</table>

Effets de l’augmentation des paramètres

##### Méthode de Ziegler-Nichols

Une autre méthode de réglage est la méthode dite de "Ziegler-Nichols", introduite par John G. Ziegler et Nathaniel B. Nichols. Elle commence comme la méthode précédente: réglage des gains I et D à zéro et accroissement du gain P jusqu'à ce que la sortie du procédé commence à osciller. Noter alors le gain critique (K<sub>c</sub>) et la période d’oscillation de la sortie (P<sub>c</sub>). Ajuster alors les termes P, I et D de la boucle comme sur la table ci-dessous:

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Type de régulation</th>
<th align="left">P</th>
<th align="left">I</th>
<th align="left">D</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>P</p></td>
<td align="left"><p>.5K<sub>c</sub></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>PI</p></td>
<td align="left"><p>.45K<sub>c</sub></p></td>
<td align="left"><p>P<sub>c</sub>/1.2</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>PID</p></td>
<td align="left"><p>.6K<sub>c</sub></p></td>
<td align="left"><p>P<sub>c</sub>/2</p></td>
<td align="left"><p>P<sub>c</sub>/8</p></td>
</tr>
</tbody>
</table>

La logique Ladder
=================

La programmation en Ladder
--------------------------

<span id="cha:classicladder-introduction"></span>

### Introduction

La logique Ladder ou langage de programmation Ladder est une méthode pour tracer les schémas en logique électrique. Il s’agit maintenant d’un langage graphique vraiment populaire pour la programmation des automates programmables industriels (API). Il a été à l’origine inventé pour décrire la logique à relais. Son nom est fondé sur la constatation que les programmes dans cette langue ressemblent à une échelle (ladder), avec deux «rails» verticaux et, entre eux, une série «d'échelons». En Allemagne et ailleurs en Europe, le style consiste à placer les rails horizontaux, un en haut de la page et l’autre en bas avec les échelons verticaux dessinés séquentiellement de la gauche vers la droite.

Un programme en logique Ladder, également appelé schéma Ladder, est ressemblant au schéma d’un ensemble de circuits électriques à relais. C’est l’intérêt majeur du schéma Ladder de permettre à une large variété de personnels techniques, ingénieurs, techniciens électriciens, etc de le comprendre et de l’utiliser sans formation complémentaire grâce à cette ressemblance.

La logique Ladder est largement utilisée pour programmer les API, avec lesquels le contrôle séquentiel des processus de fabrication est requis. Le Ladder est utile pour les systèmes de contrôle simples mais critiques, ou pour reprendre d’anciens circuits à relais câblés. Comme les contrôleurs à logique programmable sont devenus plus sophistiqués, ils ont aussi été utilisés avec succès dans des systèmes d’automatisation très complexes.

Le langage Ladder peut être considéré comme un langage basé sur les règles, plutôt que comme un langage procédural. Un «échelon» en Ladder représente une règle. Quand elles sont mises en application avec des éléments électromécaniques, les diverses règles «s’exécutent» toutes simultanément et immédiatement. Quand elle sont mises en application dans la logique d’un automate programmable, les règles sont exécutées séquentiellement par le logiciel, dans une boucle. En exécutant la boucle assez rapidement, typiquement plusieurs fois par seconde, l’effet d’une exécution simultanée et immédiate est obtenu.

### Exemple

Les composants les plus communs du Ladder sont les contacts (entrées), ceux-ci sont habituellement NC (normalement clos) ou NO (normalement ouvert) et les bobines (sorties).

-   Le contact NO <span class="image"> ![images/ladder\_action\_load.png]() </span>

-   Le contact NC <span class="image"> ![images/ladder\_action\_loadbar.png]() </span>

-   La bobine (sortie) <span class="image"> ![images/ladder\_action\_out.png]() </span>

Bien sûr, il y a beaucoup plus de composants dans le langage Ladder complet, mais la compréhension de ceux-ci aidera à appréhender le concept global du langage.

L'échelle se compose d’un ou plusieurs échelons. Ces échelons sont tracés horizontalement, avec les composants placés sur eux (entrées, sorties et autres), les composants sont évalués de la gauche vers la droite.

Cet exemple est un simple échelon:

<span class="image"> ![images/example\_link\_contact\_coil.png]() </span>

L’entrée B0 sur la gauche et un contact normalement ouvert, il est connecté sur la sortie Q0 sur la droite. Imaginez maintenant qu’une tension soit appliquée à l’extrême gauche, dès que B0 devient vraie (par exemple: l’entrée est activée, ou l’utilisateur a pressé le contact NO), la tension atteint l’extrême droite en traversant la bobine Q0. Avec comme conséquence que la sortie Q0 passe de 0 à 1.

Classicladder Programming
-------------------------

<span id="cha:classicladder-programming"></span>

### Ladder Concepts

Classic Ladder is a type of programming language originally implemented on industrial PLCs (it’s called Ladder Programming). It is based on the concept of relay contacts and coils, and can be used to construct logic checks and functions in a manner that is familiar to many systems integrators. Ladder consists of rungs that may have branches and resembles an electrical circuit. It is important to know how ladder programs are evaluated when running.

It seems natural that each line would be evaluated left to right, then the next line down, etc., but it doesn’t work this way in ladder logic. Ladder logic *scans* the ladder rungs 3 times to change the state of the outputs.

-   the inputs are read and updated

-   the logic is figured out

-   the outputs are set

This can be confusing at first if the output of one line is read by the input of a another rung. There will be one scan before the second input becomes true after the output is set.

Another gotcha with ladder programming is the "Last One Wins" rule. If you have the same output in different locations of your ladder the state of the last one will be what the output is set to.

### Languages

The most common language used when working with Classic Ladder is *ladder*. Classic Ladder also supports Sequential Function Chart (Grafcet).

### Components

There are 2 components to Classic Ladder.

-   The real time module classicladder\_rt

-   The user space module (including a GUI) classicladder

#### Files

Typically classic ladder components are placed in the custom.hal file if your working from a Stepconf generated configuration. These must not be placed in the custom\_postgui.hal file or the Ladder Editor menu will be grayed out.

Ladder files (.clp) must not contain any blank spaces in the name.

#### Realtime Module

Loading the Classic Ladder real time module (classicladder\_rt) is possible from a HAL file, or directly using a halcmd instruction. The first line loads real time the Classic Ladder module. The second line adds the function classicladder.0.refresh to the servo thread. This line makes Classic Ladder update at the servo thread rate.

    loadrt classicladder_rt
    addf classicladder.0.refresh servo-thread

The speed of the thread that Classic Ladder is running in directly affects the responsiveness to inputs and outputs. If you can turn a switch on and off faster than Classic Ladder can notice it then you may need to speed up the thread. The fastest that Classic Ladder can update the rungs is one millisecond. You can put it in a faster thread but it will not update any faster. If you put it in a slower than one millisecond thread then Classic Ladder will update the rungs slower. The current scan time will be displayed on the section display, it is rounded to microseconds. If the scan time is longer than one millisecond you may want to shorten the ladder or put it in a slower thread.

#### Variables

It is possible to configure the number of each type of ladder object while loading the Classic Ladder real time module. If you do not configure the number of ladder objects Classic Ladder will use the default values.

<table>
<caption>Tableau 2. Default Variable Count<span id="cap:Default-Variable-Count"></span></caption>
<colgroup>
<col width="56%" />
<col width="28%" />
<col width="14%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Object Name</th>
<th align="left">Variable Name</th>
<th align="left">Default Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Number of rungs</p></td>
<td align="left"><p>(numRungs)</p></td>
<td align="left"><p>100</p></td>
</tr>
<tr class="even">
<td align="left"><p>Number of bits</p></td>
<td align="left"><p>(numBits)</p></td>
<td align="left"><p>20</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Number of word variables</p></td>
<td align="left"><p>(numWords)</p></td>
<td align="left"><p>20</p></td>
</tr>
<tr class="even">
<td align="left"><p>Number of timers</p></td>
<td align="left"><p>(numTimers)</p></td>
<td align="left"><p>10</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Number of timers IEC</p></td>
<td align="left"><p>(numTimersIec)</p></td>
<td align="left"><p>10</p></td>
</tr>
<tr class="even">
<td align="left"><p>Number of monostables</p></td>
<td align="left"><p>(numMonostables)</p></td>
<td align="left"><p>10</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Number of counters</p></td>
<td align="left"><p>(numCounters)</p></td>
<td align="left"><p>10</p></td>
</tr>
<tr class="even">
<td align="left"><p>Number of HAL inputs bit pins</p></td>
<td align="left"><p>(numPhysInputs)</p></td>
<td align="left"><p>15</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Number of HAL output bit pins</p></td>
<td align="left"><p>(numPhysOutputs)</p></td>
<td align="left"><p>15</p></td>
</tr>
<tr class="even">
<td align="left"><p>Number of arithmetic expressions</p></td>
<td align="left"><p>(numArithmExpr)</p></td>
<td align="left"><p>50</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Number of Sections</p></td>
<td align="left"><p>(numSections)</p></td>
<td align="left"><p>10</p></td>
</tr>
<tr class="even">
<td align="left"><p>Number of Symbols</p></td>
<td align="left"><p>(numSymbols)</p></td>
<td align="left"><p>Auto</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Number of S32 inputs</p></td>
<td align="left"><p>(numS32in)</p></td>
<td align="left"><p>10</p></td>
</tr>
<tr class="even">
<td align="left"><p>Number of S32 outputs</p></td>
<td align="left"><p>(numS32out)</p></td>
<td align="left"><p>10</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Number of Float inputs</p></td>
<td align="left"><p>(numFloatIn)</p></td>
<td align="left"><p>10</p></td>
</tr>
<tr class="even">
<td align="left"><p>Number of Float outputs</p></td>
<td align="left"><p>(numFloatOut)</p></td>
<td align="left"><p>10</p></td>
</tr>
</tbody>
</table>

Objects of most interest are numPhysInputs, numPhysOutputs, numS32in, and numS32out.

Changing these numbers will change the number of HAL bit pins available. numPhysInputs and numPhysOutputs control how many HAL bit (on/off) pins are available. numS32in and numS32out control how many HAL signed integers (+- integer range) pins are available.

For example (you don’t need all of these to change just a few):

    loadrt classicladder_rt numRungs=12 numBits=100 numWords=10
    numTimers=10 numMonostables=10 numCounters=10 numPhysInputs=10
    numPhysOutputs=10 numArithmExpr=100 numSections=4 numSymbols=200
    numS32in=5 numS32out=5

To load the default number of objects:

    loadrt classicladder_rt

### Loading the Classic Ladder user module

Classic Ladder HAL commands must executed before the GUI loads or the menu item Ladder Editor will not function. If you used the Stepper Config Wizard place any Classic Ladder HAL commands in the custom.hal file.

To load the user module:

    loadusr classicladder

To load a ladder file:

    loadusr classicladder myladder.clp

Classic Ladder Loading Options

-   *--nogui* - (loads without the ladder editor) normally used after debugging is finished.

-   *--modbus\_port=port* - (loads the modbus port number)

-   *--modmaster* - (initializes MODBUS master) should load the ladder program at the same time or the TCP is default port.

-   *--modslave* - (initializes MODBUS slave) only TCP

To use Classic Ladder with HAL without EMC:

    loadusr -w classicladder

The -w tells HAL not to close down the HAL environment until Classic Ladder is finished.

If you first load ladder program with the *--nogui* option then load Classic Ladder again with no options the GUI will display the last loaded ladder program.

In AXIS you can load the GUI from File/Ladder Editor…

### Classic Ladder GUI

If you load Classic Ladder with the GUI it will display two windows: section display, and section manager.

#### Sections Manager

When you first start up Classic Ladder you get an empty Sections Manager window.

![images/Default\_Sections\_Manager.png]()

Figure 17. Sections Manager Default Window<span id="cap:Sections-Manager-Default"></span>

This window allows you to name, create or delete sections and choose what language that section uses. This is also how you name a subroutine for call coils.

#### Section Display

When you first start up Classic Ladder you get an empty Section Display window. Displayed is one empty rung.

![images/Default\_Section\_Display.png]()

Figure 18. Section Display Default Window<span id="cap:Section-Display-Default"></span>

Most of the buttons are self explanatory:

The Vars button is for looking at variables, toggle it to display one, the other, both, then none of the windows.

The Config button is used for modbus and shows the max number of ladder elements that was loaded with the real time module.

The Symbols button will display an editable list of symbols for the variables (hint you can name the inputs, outputs, coils etc).

The Quit button will shut down the user program meaning Modbus and the display. The real time ladder program will still run in the background.

The check box at the top right allows you to select whether variable names or symbol names are displayed

You might notice that there is a line under the ladder program display that reads "Project failed to load…" That is the status bar that gives you info about elements of the ladder program that you click on in the display window. This status line will now display HAL signal names for variables %I, %Q and the first %W (in an equation) You might see some funny labels, such as (103) in the rungs. This is displayed (on purpose) because of an old bug- when erasing elements older versions sometimes didn’t erase the object with the right code. You might have noticed that the long horizontal connection button sometimes didn’t work in the older versions. This was because it looked for the *free* code but found something else. The number in the brackets is the unrecognized code. The ladder program will still work properly, to fix it erase the codes with the editor and save the program.

#### The Variable Windows

This are two variable windows: the Bit Status Window (boolean) and the Watch Window (signed integer). The Vars button is in the Section Display Window, toggle the Vars button to display one, the other, both, then none of the variable windows.

![images/Bit\_Status.png]()

Figure 19. Bit Status Window<span id="cap:Bit-Status-Window"></span>

The Bit Status Window displays some of the boolean (on/off) variable data. Notice all variables start with the % sign. The %I variables represent HAL input bit pins. The %Q represents the relay coil and HAL output bit pins. The %B represents an internal relay coil or internal contact. The three edit areas at the top allow you to select what 15 variables will be displayed in each column. For instance, if the %B Variable column were 15 entries high, and you entered 5 at the top of the column, variables %B5 to %B19 would be displayed. The check boxes allow you to set and unset %B variables manually as long as the ladder program isn’t setting them as outputs. Any Bits that are set as outputs by the program when Classic Ladder is running can not be changed and will be displayed as checked if on and unchecked if off.

![images/watch\_window.png]()

Figure 20. Watch Window<span id="cap:Watch-Window"></span>

The Watch Window displays variable status. The edit box beside it is the number stored in the variable and the drop-down box beside that allow you to choose whether the number to be displayed in hex, decimal or binary. If there are symbol names defined in the symbols window for the word variables showing and the *display symbols* checkbox is checked in the section display window, symbol names will be displayed. To change the variable displayed, type the variable number, e.g. %W2 (if the display symbols check box is not checked) or type the symbol name (if the display symbols checkbox is checked) over an existing variable number/name and press the Enter Key.

#### Symbol Window

![images/Default\_Symbols\_names.png]()

Figure 21. Symbol Names window<span id="cap:Symbol-Names-window"></span>

This is a list of *symbol* names to use instead of variable names to be displayed in the section window when the *display symbols* check box is checked. You add the variable name (remember the *%* symbol and capital letters), symbol name . If the variable can have a HAL signal connected to it (%I, %Q, and %W-if you have loaded s32 pin with the real time module) then the comment section will show the current HAL signal name or lack thereof. Symbol names should be kept short to display better. Keep in mind that you can display the longer HAL signal names of %I, %Q and %W variable by clicking on them in the section window. Between the two, one should be able to keep track of what the ladder program is connected to!

#### The Editor window

![images/Editor.png]()

Figure 22. Editor Window<span id="cap:Editor-Window"></span>

-   *Add* - adds a rung after the selected rung

-   *Insert* - inserts a rung before the selected rung

-   *Delete* - deletes the selected rung

-   *Modify* - opens the selected rung for editing

Starting from the top left image:

-   Object Selector, Eraser

-   N.O. Input, N.C. Input, Rising Edge Input , Falling Edge Input

-   Horizontal Connection, Vertical Connection , Long Horizontal Connection

-   Timer IEC Block, Counter Block, Compare Variable

-   Old Timer Block, Old Monostable Block (These have been replaced by the IEC Timer)

-   COILS - N.O. Output, N.C. Output, Set Output, Reset Output

-   Jump Coil, Call Coil, Variable Assignment

A short description of each of the buttons:

-   *Selector* - allows you to select existing objects and modify the information.

-   *Eraser* - erases an object.

-   *N.O. Contact* - creates a normally open contact. It can be an external HAL-pin (%I) input contact, an internal-bit coil (%B) contact or a external coil (%Q) contact. The HAL-pin input contact is closed when the HAL-pin is true. The coil contacts are closed when the corresponding coil is active (%Q2 contact closes when %Q2 coil is active).

-   *N.C. Contact* - creates a normally closed contact. It is the same as the N.O. contact except that the contact is open when the HAL-pin is true or the coil is active.

-   'Rising Edge Contact - creates a contact that is closed when the HAL-pin goes from False to true, or the coil from not-active to active.

-   *Falling Edge Contact* - creates a contact that is closed when the HAL-pin goes from true to false or the coil from active to not.

-   *Horizontal Connection* - creates a horizontal connection to objects.

-   *Vertical Connection* - creates a vertical connection to horizontal lines.

-   *Horizontal Running Connection* - creates a horizontal connection between two objects and is a quick way to connect objects that are more than one block apart.

-   *IEC Timer* - creates a timer and replaces the *Timer*.

-   *Timer* - creates a Timer Module (depreciated use IEC Timer instead).

-   *Monostable* - creates a one-shot monostable module

-   *Counter* - creates a counter module.

-   *Compare* - creates a compare block to compare variable to values or other variables. (eg %W1⇐5 or %W1=%W2) Compare cannot be placed in the right most side of the section display.

-   *Variable Assignment* - creates an assignment block so you to assign values to variables. (eg %W2=7 or %W1=%W2) ASSIGNMENT functions can only be placed at the right most side of the section display.

#### Config Window

The config window shows the current project status and has the Modbus setup tabs.

![images/Config.png]()

Figure 23. Config Window<span id="cap:Config-Window"></span>

### Ladder objects

#### CONTACTS

Represent switches or relay contacts. They are controlled by the variable letter and number assigned to them.

The variable letter can be B, I, or Q and the number can be up to a three digit number eg. %I2, %Q3, or %B123. Variable I is controlled by a HAL input pin with a corresponding number. Variable B is for internal contacts, controlled by a B coil with a corresponding number. Variable Q is controlled by a Q coil with a corresponding number. (like a relay with multiple contacts). E.g. if HAL pin classicladder.0.in-00 is true then %I0 N.O. contact would be on (closed, true, whatever you like to call it). If %B7 coil is *energized* (on, true, etc) then %B7 N.O. contact would be on. If %Q1 coil is *energized* then %Q1 N.O. contact would be on (and HAL pin classicladder.0.out-01 would be true.)

-   *N.O. Contact* - <span class="image"> ![images/ladder\_action\_load.png]() </span> (Normally Open) When the variable is false the switch is off.

-   *N.C. Contact* - <span class="image"> ![images/ladder\_action\_loadbar.png]() </span> (Normally Closed) When the variable is false the switch is on.

-   *Rising Edge Contact* - When the variable changes from false to true, the switch is PULSED on.

-   *Falling Edge Contact* - When the variable changes from true to false, the switch is PULSED on.

#### IEC TIMERS

Represent new count down timers. IEC Timers replace Timers and Monostables.

IEC Timers have 2 contacts.

-   *I* - input contact

-   *Q* - output contact

There are three modes - TON, TOF, TP.

-   *TON* - When timer input is true countdown begins and continues as long as input remains true. After countdown is done and as long as timer input is still true the output will be true.

-   *TOF* - When timer input is true, sets output true. When the input is false the timer counts down then sets output false.

-   *TP* - When timer input is pulsed true or held true timer sets output true till timer counts down. (one-shot)

The time intervals can be set in multiples of 100ms, seconds, or minutes.

There are also Variables for IEC timers that can be read and/or written to in compare or operate blocks.

-   *%TMxxx.Q* - timer done (Boolean, read write)

-   *%TMxxx.P* - timer preset (read write)

-   *%TMxxx.V* - timer value (read write)

#### TIMERS

Represent count down timers. This is deprecated and replaced by IEC Timers.

Timers have 4 contacts.

-   *E* - enable (input) starts timer when true, resets when goes false

-   *C* - control (input) must be on for the timer to run (usually connect to E)

-   *D* - done (output) true when timer times out and as long as E remains true

-   *R* - running (output) true when timer is running

The timer base can be multiples of milliseconds, seconds, or minutes.

There are also Variables for timers that can be read and/or written to in compare or operate blocks.

-   *%Txx.R* - Timer xx running (Boolean, read only)

-   *%Txx.D* - Timer xx done (Boolean, read only)

-   *%Txx.V* - Timer xx current value (integer, read only)

-   *%Txx.P* - Timer xx preset (integer, read or write)

#### MONOSTABLES

Represent the original one-shot timers. This is now deprecated and replaced by IEC Timers.

Monostables have 2 contacts, I and R.

-   *I* - input (input) will start the mono timer running.

-   *R* - running (output) will be true while timer is running.

The I contact is rising edge sensitive meaning it starts the timer only when changing from false to true (or off to on). While the timer is running the I contact can change with no effect to the running timer. R will be true and stay true till the timer finishes counting to zero. The timer base can be multiples of milliseconds, seconds, or minutes.

There are also Variables for monostables that can be read and/or written to in compare or operate blocks.

-   *%Mxx.R* - Monostable xx running (Boolean, read only)

-   *%Mxx.V* - Monostable xx current value (integer, read only)

-   *%Mxx.P* - Monostable xx preset (integer, read or write)

#### COUNTERS

Represent up/down counters.

There are 7 contacts:

-   *R* - reset (input) will reset the count to 0.

-   *P* - preset (input) will set the count to the preset number assigned from the edit menu.

-   *U* - up count (input) will add one to the count.

-   *D* - down count (input) will subtract one from the count.

-   *E* - under flow (output) will be true when the count rolls over from 0 to 9999.

-   *D* - done (output) will be true when the count equals the preset.

-   *F* - overflow (output) will be true when the count rolls over from 9999 to 0.

The up and down count contacts are edge sensitive meaning they only count when the contact changes from false to true (or off to on if you prefer).

The range is 0 to 9999.

There are also Variables for counters that can be read and/or written to in compare or operate blocks.

-   *%Cxx.D* - Counter xx done (Boolean, read only)

-   *%Cxx.E* - Counter xx empty overflow (Boolean, read only)

-   *%Cxx.F* - Counter xx full overflow (Boolean, read only)

-   *%Cxx.V* - Counter xx current value (integer, read or write)

-   *%Cxx.P* - Counter xx preset (integer, read or write)

#### COMPARE

For arithmetic comparison. Is variable %XXX = to this number (or evaluated number)

The compare block will be true when comparison is true. you can use most math symbols:

-   +, - ,\* , /, = (standard math symbols)

-   &lt; (less than), &gt; (greater than), ⇐ (less or equal), &gt;= (greater or equal), &lt;&gt; (not equal)

-   (, ) grouping

-   ^ (exponent),% (modulus),& (and),| (or),. -

-   ABS (absolute), MOY (French for average) ,AVG (average)

For example ABS(%W2)=1, MOY(%W1,%W2)&lt;3.

No spaces are allowed in the comparison equation. For example %C0.V&gt;%C0.P is a valid comparison expression while %C0.V &gt; %CO.P is not a valid expression.

There is a list of Variables down the page that can be used for reading from and writing to ladder objects. When a new compare block is opened be sure and delete the \# symbol when you enter a compare.

To find out if word variable \#1 is less than 2 times the current value of counter \#0 the syntax would be:

    %W1<2*%C0.V

To find out if S32in bit 2 is equal to 10 the syntax would be:

    %IW2=10

Note: Compare uses the arithmetic equals not the double equals that programmers are used to.

#### VARIABLE ASSIGNMENT

For variable assignment, e.g. assign this number (or evaluated number) to this variable %xxx, there are two math functions MINI and MAXI that check a variable for maximum (0x80000000) and minimum values (0x07FFFFFFF) (think signed values) and keeps them from going beyond.

When a new variable assignment block is opened be sure to delete the \# symbol when you enter an assignment.

To assign a value of 10 to the timer preset of IEC Timer 0 the syntax would be:

    %TM0.P=10

To assign the value of 12 to s32out bit 3 the syntax would be:

    %QW3=12

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
<td align="left">When you assign a value to a variable with the variable assignment block the value is retained until you assign a new value using the variable assignment block. The last value assigned will be restored when Machinekit is started.</td>
</tr>
</tbody>
</table>

The following figure shows an Assignment and a Comparison Example. %QW0 is a S32out bit and %IW0 is a S32in bit. In this case the HAL pin classicladder.0.s32out-00 will be set to a value of 5 and when the HAL pin classicladder.0.s32in-00 is 0 the HAL pin classicladder.0.out-00 will be set to True.

![images/AssignCompare-Ladder.png]()

Figure 24. Assign/Compare Example<span id="cap:Assign/Compare-Example"></span>

![images/Assignment\_Expression.png]()

![images/Comparison\_Expression.png]()

#### COILS

Coils represent relay coils. They are controlled by the variable letter and number assigned to them.

The variable letter can be B or Q and the number can be up to a three digit number eg. %Q3, or %B123. Q coils control HAL out pins, e.g. if %Q15 is energized then HAL pin classicladder.0.out-15 will be true. B coils are internal coils used to control program flow.

-   *N.O. COIL* - (a relay coil.) When coil is energized it’s N.O. contact will be closed (on, true, etc)

-   *N.C. COIL* - (a relay coil that inverses its contacts.) When coil is energized it"s N.O. contact will be open (off, false, etc)

-   *SET COIL* - (a relay coil with latching contacts) When coil is energized it’s N.O. contact will be latched closed.

-   *RESET COIL* - (a relay coil with latching contacts) When coil is energized It’s N.0. contact will be latched open.

-   *JUMP COIL* - (a *goto* coil) when coil is energized ladder program jumps to a rung (in the CURRENT section) -jump points are designated by a rung label. (Add rung labels in the section display, top left label box)

-   *CALL COIL* - (a *gosub* coil) when coil is energized program jumps to a subroutine section designated by a subroutine number -subroutines are designated SR0 to SR9 (designate them in the section manager)

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
<td align="left">If you use a N.C. contact with a N.C. coil the logic will work (when the coil is energized the contact will be closed) but that is really hard to follow!</td>
</tr>
</tbody>
</table>

##### JUMP COIL

A JUMP COIL is used to *JUMP* to another section, like a goto in BASIC programming language.

If you look at the top left of the sections display window you will see a small label box and a longer comment box beside it. Now go to Editor→Modify then go back to the little box, type in a name.

Go ahead and add a comment in the comment section. This label name is the name of this rung only and is used by the JUMP COIL to identify where to go.

When placing a JUMP COIL, add it in the rightmost position and change the label to the rung you want to JUMP to.

##### CALL COIL

A CALL COIL is used to go to a subroutine section then return, like a gosub in BASIC programming language.

If you go to the sections manager window hit the add section button. You can name this section, select what language it will use (ladder or sequential), and select what type (main or subroutine).

Select a subroutine number (SR0 for example). An empty section will be displayed and you can build your subroutine.

When you’ve done that, go back to the section manager and click on the your main section (default name prog1).

Now you can add a CALL COIL to your program. CALL COILs are to be placed at the rightmost position in the rung.

Remember to change the label to the subroutine number you chose before.

### Classic Ladder Variables

These Variables are used in COMPARE or OPERATE to get information about, or change specs of, ladder objects such as changing a counter preset, or seeing if a timer is done running.

List of variables :

-   *%Bxxx* - Bit memory xxx (Boolean)

-   *%Wxxx* - Word memory xxx (32 bits signed integer)

-   *%IWxxx* - Word memory xxx (S32 in pin)

-   *%QWxxx* - Word memory xxx (S32 out pin)

-   *%IFxx* - Word memory xx (Float in pin) (**converted to S32 in Classic Ladder**)

-   *%QFxx* - Word memory xx (Float out pin) (**converted to S32 in Classic Ladder**)

-   *%Txx.R* - Timer xx running (Boolean, user read only)

-   *%Txx.D* - Timer xx done (Boolean, user read only)

-   *%Txx.V* - Timer xx current value (integer, user read only)

-   *%Txx.P* - Timer xx preset (integer)

-   *%TMxxx.Q* - Timer xxx done (Boolean, read write)

-   *%TMxxx.P* - Timer xxx preset (integer, read write)

-   *%TMxxx.V* - Timer xxx value (integer, read write)

-   *%Mxx.R* - Monostable xx running (Boolean)

-   *%Mxx.V* - Monostable xx current value (integer, user read only)

-   *%Mxx.P* - Monostable xx preset (integer)

-   *%Cxx.D* - Counter xx done (Boolean, user read only)

-   *%Cxx.E* - Counter xx empty overflow (Boolean, user read only)

-   *%Cxx.F* - Counter xx full overflow (Boolean, user read only)

-   *%Cxx.V* - Counter xx current value (integer)

-   *%Cxx.P* - Counter xx preset (integer)

-   *%Ixxx* - Physical input xxx (Boolean) (HAL input bit)

-   *%Qxxx* - Physical output xxx (Boolean) (HAL output bit)

-   *%Xxxx* - Activity of step xxx (sequential language)

-   *%Xxxx.V* - Time of activity in seconds of step xxx (sequential language)

-   *%Exx* - Errors (Boolean, read write(will be overwritten))

-   *Indexed or vectored variables* - These are variables indexed by another variable. Some might call this vectored variables. Example: %W0\[%W4\] ⇒ if %W4 equals 23 it corresponds to %W23

### GRAFCET Programming

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
<td align="left">This is probably the least used and most poorly understood feature of Classic Ladder. Sequential programming is used to make sure a series of ladder events always happen in a prescribed order. Sequential programs do not work alone. There is always a ladder program as well that controls the variables. Here are the basic rules governing sequential programs:</td>
</tr>
</tbody>
</table>

-   Rule 1 : Initial situation - The initial situation is characterized by the initial steps which are by definition in the active state at the beginning of the operation.There shall be at least one initial step.

-   Rule 2 : R2, Clearing of a transition - A transition is either enabled or disabled. It is said to be enabled when all immediately preceding steps linked to its corresponding transition symbol are active, otherwise it is disabled. A transition cannot be cleared unless it is enabled, and its associated transition condition is true.

-   Rule 3 : R3, Evolution of active steps - The clearing of a transition simultaneously leads to the active state of the immediately following step(s) and to the inactive state of the immediately preceding step(s).

-   Rule 4 : R4, Simultaneous clearing of transitions - All simultaneous cleared transitions are simultaneously cleared.

-   Rule 5 : R5, Simultaneous activation and deactivation of a step - If during operation, a step is simultaneously activated and deactivated, priority is given to the activation.

This is the SEQUENTIAL editor window Starting from the top left image: Selector arrow , Eraser Ordinary step , Initial (Starting) step Transition , Step and Transition Transition Link-Downside , Transition Link-Upside Pass-through Link-Downside , Pass-through Link-Upside Jump Link Comment Box \[show sequential program\]

-   *ORDINARY STEP* - has a unique number for each one

-   *STARTING STEP* - a sequential program must have one. This is where the program will start.

-   *TRANSITION* - This shows the variable that must be true for control to pass through to the next step.

-   *STEP AND TRANSITION* - Combined for convenience

-   *TRANSITION LINK-DOWNSIDE* - splits the logic flow to one of two possible lines based on which of the next steps is true first (Think OR logic)

-   *TRANSITION LINK=UPSIDE* - combines two (OR) logic lines back in to one

-   *PASS-THROUGH LINK-DOWNSIDE* - splits the logic flow to two lines that BOTH must be true to continue (Think AND logic)

-   *PASS-THROUGH LINK-UPSIDE* - combines two concurrent (AND logic) logic lines back together

-   *JUMP LINK* - connects steps that are not underneath each other such as connecting the last step to the first

-   *COMMENT BOX* - used to add comments

To use links, you must have steps already placed. Select the type of link, then select the two steps or transactions one at a time. It takes practice!

With sequential programming: The variable %Xxxx (eg. %X5) is used to see if a step is active. The variable %Xxxx.V (eg. %X5.V) is used to see how long the step has been active. The %X and %X.v variables are use in LADDER logic. The variables assigned to the transitions (eg. %B) control whether the logic will pass to the next step. After a step has become active the transition variable that caused it to become active has no control of it anymore. The last step has to JUMP LINK back only to the beginning step.

### Modbus

Things to consider:

-   Modbus is a userspace program so it might have latency issues on a heavily laden computer.

-   Modbus is not really suited to Hard real time events such as position control of motors or to control E-stop.

-   The Classic Ladder GUI must be running for Modbus to be running.

-   Modbus is not fully finished so it does not do all modbus functions.

To get MODBUS to initialize you must specify that when loading the Classic Ladder userspace program.

Loading Modbus

    loadusr -w classicladder --modmaster myprogram.clp

The -w makes HAL wait until you close Classic Ladder before closing realtime session. Classic Ladder also loads a TCP modbus slave if you add *--modserver* on command line.

Modbus Functions

-   *1* - read coils

-   *2* - read inputs

-   *3* - read holding registers

-   *4* - read input registers

-   *5* - write single coils

-   *6* - write single register

-   *8* - echo test

-   *15* - write multiple coils

-   *16* - write multiple registers

If you do not specify a *-- modmaster* when loading the Classic Ladder user program this page will not be displayed.

![images/Config-io.png]()

Figure 25. Config I/O<span id="cap:Config-I/O"></span>

![images/Config-com.png]()

Figure 26. Config Coms<span id="cap:Config-Coms"></span>

-   *SERIAL PORT* - For IP blank. For serial the location/name of serial driver eg. /dev/ttyS0 ( or /dev/ttyUSB0 for a USB-to-serial converter).

-   *SERIAL SPEED* - Should be set to speed the slave is set for - 300, 600, 1200, 2400, 4800, 9600, 19200, 38400, 57600, 115200 are supported.

-   *PAUSE AFTER TRANSMIT* - Pause (milliseconds) after transmit and before receiving answer, some devices need more time (e.g., USB-to-serial converters).

-   *PAUSE INTER-FRAME* - Pause (milliseconds) after receiving answer from slave. This sets the duty cycle of requests (it’s a pause for EACH request).

-   *REQUEST TIMEOUT LENGTH* - Length (milliseconds) of time before we decide that the slave didn’t answer.

-   *MODBUS ELEMENT OFFSET* - used to offset the element numbers by 1 (for manufacturers numbering differences).

-   *DEBUG LEVEL* - Set this to 0-3 (0 to stop printing debug info besides no-response errors).

-   *READ COILS/INPUTS MAP TO* - Select what variables that read coils/inputs will update. (B or Q).

-   *WRITE COILS MAP TO* - Select what variables that write coils will updated.from (B,Q,or I).

-   *READ REGISTERS/HOLDING* - Select what variables that read registers will update. (W or QW).

-   *WRITE REGISTERS MAP TO* - Select what variables that read registers will updated from. (W, QW, or IW).

-   *SLAVE ADDRESS* - For serial the slaves ID number usually settable on the slave device (usually 1-256) For IP the slave IP address plus optionally the port number.

-   *TYPE ACCESS* - This selects the MODBUS function code to send to the slave (eg what type of request).

-   *COILS / INPUTS* - Inputs and Coils (bits) are read from/written to I, B, or Q variables (user selects).

-   *REGISTERS (WORDS)* - Registers (Words/Numbers) map to IW, W, or QW variables (user selects).

-   *1st MODBUS ELEMENT* - The address (or register number) of the first element in a group. (remember to set MODBUS ELEMENT OFFSET properly).

-   *NUMBER OF ELEMENTS* - The number of elements in this group.

-   *LOGIC* - You can invert the logic here.

-   *1st%I%Q IQ WQ MAPPED* - This is the starting number of %B, %I, %Q, %W, %IW, or %QW variables that are mapped onto/from the modbus element group (starting at the first modbus element number).

In the example above: Port number - for my computer /dev/ttyS0 was my serial port.

The serial speed is set to 9600 baud.

Slave address is set to 12 (on my VFD I can set this from 1-31, meaning I can talk to 31 VFDs maximum on one system).

The first line is set up for 8 input bits starting at the first register number (register 1). So register numbers 1-8 are mapped onto Classic Ladder’s %B variables starting at %B1 and ending at %B8.

The second line is set for 2 output bits starting at the ninth register number (register 9) so register numbers 9-10 are mapped onto Classic Ladder’s %Q variables starting at %Q9 ending at %Q10.

The third line is set to write 2 registers (16 bits each) starting at the 0th register number (register 0) so register numbers 0-1 are mapped onto Classic Ladder’s %W variables starting at %W0 ending at %W1.

It’s easy to make an off-by-one error as sometimes the modbus elements are referenced starting at one rather then 0 (actually by the standard that is the way it’s supposed to be!) You can use the modbus element offset radio button to help with this.

The documents for your modbus slave device will tell you how the registers are set up- there is no standard way.

The SERIAL PORT, PORT SPEED, PAUSE, and DEBUG level are editable for changes (when you close the config window values are applied, though Radio buttons apply immediately).

To use the echo function select the echo function and add the slave number you wish to test. You don’t need to specify any variables.

The number 257 will be sent to the slave number you specified and the slave should send it back. you will need to have Classic Ladder running in a terminal to see the message.

#### MODBUS Settings

Serial:

-   Classic Ladder uses RTU protocol (not ASCII).

-   8 data bits, No parity is used, and 1 stop bit is also known as 8-N-1.

-   Baud rate must be the same for slave and master. Classic Ladder can only have one baud rate so all the slaves must be set to the same rate.

-   Pause inter frame is the time to pause after receiving an answer.

-   MODBUS\_TIME\_AFTER\_TRANSMIT is the length of pause after sending a request and before receiving an answer (this apparently helps with USB converters which are slow).

#### MODBUS Info

-   Classic Ladder can use distributed inputs/outputs on modules using the modbus protocol ("master": polling slaves).

-   The slaves and theirs I/O can be configured in the config window.

-   2 exclusive modes are available : ethernet using Modbus/TCP and serial using Modbus/RTU.

-   No parity is used.

-   If no port name for serial is set, TCP/IP mode will be used…

-   The slave address is the slave address (Modbus/RTU) or the IP address.

-   The IP address can be followed per the port number to use (xx.xx.xx.xx:pppp) else the port 9502 will be used per default.

-   2 products have been used for tests: a Modbus/TCP one (Adam-6051, <http://www.advantech.com>) and a serial Modbus/RTU one (<http://www.ipac.ws>).

-   See examples: adam-6051 and modbus\_rtu\_serial.

-   Web links: <http://www.modbus.org> and this interesting one: <http://www.iatips.com/modbus.html>

-   MODBUS TCP SERVER INCLUDED

-   Classic Ladder has a Modbus/TCP server integrated. Default port is 9502. (the previous standard 502 requires that the application must be launched with root privileges).

-   List of Modbus functions code supported are: 1, 2, 3, 4, 5, 6, 15 and 16.

-   Modbus bits and words correspondence table is actually not parametric and correspond directly to the %B and %W variables.

More information on modbus protocol is available on the internet.

<http://www.modbus.org/>

#### Communication Errors

If there is a communication error, a warning window will pop up (if the GUI is running) and %E0 will be true. Modbus will continue to try to communicate. The %E0 could be used to make a decision based on the error. A timer could be used to stop the machine if timed out, etc.

#### MODBUS Bugs

-   In compare blocks the function %W=ABS(%W1-%W2) is accepted but does not compute properly. only %W0=ABS(%W1) is currently legal.

-   When loading a ladder program it will load Modbus info but will not tell Classic Ladder to initialize Modbus. You must initialize Modbus when you first load the GUI by adding *--modmaster*.

-   If the section manager is placed on top of the section display, across the scroll bar and exit is clicked the user program crashes.

-   When using *--modmaster* you must load the ladder program at the same time or else only TCP will work.

-   reading/writing multiple registers in Modbus has checksum errors.

### Setting up Classic Ladder

In this section we will cover the steps needed to add Classic Ladder to a Stepconf Wizard generated config. On the advanced Configuration Options page of Stepconf Wizard check off "Include Classic Ladder PLC".

![images/stepconf\_ladder.png]()

Figure 27. Stepconf Classic Ladder<span id="cap:Stepconf-Classicladder"></span>

#### Add the Modules

If you used the Stepconf Wizard to add Classic Ladder you can skip this step.

To manually add Classic Ladder you must first add the modules. This is done by adding a couple of lines to the custom.hal file.

This line loads the real time module:

    loadrt classicladder_rt

This line adds the Classic Ladder function to the servo thread:

    addf classicladder.0.refresh servo-thread

#### Adding Ladder Logic

Now start up your config and select "File/Ladder Editor" to open up the Classic Ladder GUI. You should see a blank Section Display and Sections Manager window as shown above. In the Section Display window open the Editor. In the Editor window select Modify. Now a Properties window pops up and the Section Display shows a grid. The grid is one rung of ladder. The rung can contain branches. A simple rung has one input, a connector line and one output. A rung can have up to six horizontal branches. While it is possible to have more than one circuit in a run the results are not predictable.

![images/Section\_Display\_Grid.png]()

Figure 28. Section Display with Grid<span id="cap:Section-Display-with-Grid"></span>

Now click on the N.O. Input in the Editor Window.

![images/Editor\_NO\_Input.png]()

Figure 29. Editor Window<span id="cap:Editor-Window-NO"></span>

Now click in the upper left grid to place the N.O. Input into the ladder.

![images/Section\_Display\_Build01.png]()

Figure 30. Section Display with Input<span id="cap:Section-Display-with-Input"></span>

Repeat the above steps to add a N.O. Output to the upper right grid and use the Horizontal Connection to connect the two. It should look like the following. If not, use the Eraser to remove unwanted sections.

![images/Section\_Display\_Build02.png]()

Figure 31. Section Display with Rung<span id="cap:Section-Display-with-Rung"></span>

Now click on the OK button in the Editor window. Now your Section Display should look like this.

![images/Section\_Display\_Build03.png]()

Figure 32. Section Display Finished<span id="cap:Section-Display-Finished"></span>

To save the new file select Save As and give it a name. The .clp extension will be added automatically. It should default to the running config directory as the place to save it.

![images/SaveAs.png]()

Figure 33. Save As Dialog<span id="cap:Save-As-Dialog"></span>

Again if you used the Stepconf Wizard to add Classic Ladder you can skip this step.

To manually add a ladder you need to add add a line to your custom.hal file that will load your ladder file. Close your Machinekit session and add this line to your custom.hal file.

    loadusr -w classicladder --nogui MyLadder.clp

Now if you start up your Machinekit config your ladder program will be running as well. If you select "File/Ladder Editor", the program you created will show up in the Section Display window.

### Ladder Examples

<span id="cha:classicladder-examples"></span>

#### Wrapping Counter

To have a counter that "wraps around" you have to use the preset pin and the reset pin. When you create the counter set the preset at the number you wish to reach before wrapping around to 0. The logic is if the counter value is over the preset then reset the counter and if the underflow is on then set the counter value to the preset value. As you can see in the example when the counter value is greater than the counter preset the counter reset is triggered and the value is now 0. The underflow output %Q2 will set the counter value at the preset when counting backwards.

![images/wrapping-counter.png]()

Figure 34. Wrapping Counter<span id="fig:Wrapping-Counter"></span>

#### Reject Extra Pulses

This example shows you how to reject extra pulses from an input. Suppose the input pulse %I0 has an annoying habit of giving an extra pulse that spoils our logic. The TOF (Timer Off Delay) prevents the extra pulse from reaching our cleaned up output %Q0. How this works is when the timer gets an input the output of the timer is on for the duration of the time setting. Using a normally closed contact %TM0.Q the output of the timer blocks any further inputs from reaching our output until it times out.

![images/extra-pulse-reject.png]()

Figure 35. Reject Extra Pulse<span id="fig:Reject-Extra-Pulse"></span>

#### External E-Stop

The External E-Stop example is in the /config/classicladder/cl-estop folder. It uses a pyVCP panel to simulate the external components.

To interface an external E-Stop to Machinekit and have the external E-Stop work together with the internal E-Stop requires a couple of connections through Classic Ladder.

First we have to open the E-Stop loop in the main HAL file by commenting out by adding the pound sign as shown or removing the following lines.

    # net estop-out <= iocontrol.0.user-enable-out
    # net estop-out => iocontrol.0.emc-enable-in

Next we add Classic Ladder to our custom.hal file by adding these two lines:

    loadrt classicladder_rt
    addf classicladder.0.refresh servo-thread

Next we run our config and build the ladder as shown here.

![images/EStop\_Section\_Display.png]()

Figure 36. E-Stop Section Display<span id="cap:E-Stop-Section-Display"></span>

After building the ladder select Save As and save the ladder as estop.clp

Now add the following line to your custom.hal file.

    # Load the ladder
    loadusr classicladder --nogui estop.clp

I/O assignments

-   %I0 = Input from the pyVCP panel simulated E-Stop (the checkbox)

-   %I1 = Input from Machinekit’s E-Stop

-   %I2 = Input from Machinekit’s E-Stop Reset Pulse

-   %I3 = Input from the pyVCP panel reset button

-   %Q0 = Ouput to Machinekit to enable

-   %Q1 = Output to external driver board enable pin (use a N/C output if your board had a disable pin)

Next we add the following lines to the custom\_postgui.hal file

    # E-Stop example using pyVCP buttons to simulate external components

    # The pyVCP checkbutton simulates a normally closed external E-Stop
    net ext-estop classicladder.0.in-00 <= pyvcp.py-estop

    # Request E-Stop Enable from Machinekit
    net estop-all-ok iocontrol.0.emc-enable-in <= classicladder.0.out-00

    # Request E-Stop Enable from pyVCP or external source
    net ext-estop-reset classicladder.0.in-03 <= pyvcp.py-reset

    # This line resets the E-Stop from Machinekit
     net emc-reset-estop iocontrol.0.user-request-enable =>
    classicladder.0.in-02

    # This line enables Machinekit to unlatch the E-Stop in Classic Ladder
    net emc-estop iocontrol.0.user-enable-out => classicladder.0.in-01

    # This line turns on the green indicator when out of E-Stop
    net estop-all-ok => pyvcp.py-es-status

Next we add the following lines to the panel.xml file. Note you have to open it with the text editor not the default html viewer.

        <pyvcp>
        <vbox>
        <label><text>"E-Stop Demo"</text></label>
        <led>
        <halpin>"py-es-status"</halpin>
        <size>50</size>
        <on_color>"green"</on_color>
        <off_color>"red"</off_color>
        </led>
        <checkbutton>
        <halpin>"py-estop"</halpin>
        <text>"E-Stop"</text>
        </checkbutton>
        </vbox>
        <button>
        <halpin>"py-reset"</halpin>
        <text>"Reset"</text>
        </button>
        </pyvcp>

Now start up your config and it should look like this.

![images/axis\_cl-estop.png]()

Figure 37. AXIS E-Stop<span id="cap:AXIS-E-Stop"></span>

Note that in this example like in real life you must clear the remote E-Stop (simulated by the checkbox) before the AXIS E-Stop or the external Reset will put you in OFF mode. If the E-Stop in the AXIS screen was pressed, you must press it again to clear it. You cannot reset from the external after you do an E-Stop in AXIS.

#### Timer/Operate Example

In this example we are using the Operate block to assign a value to the timer preset based on if an input is on or off.

![images/Tmr\_Section\_Display.png]()

Figure 38. Timer/Operate Example<span id="cap:Timer/Operate-Example"></span>

In this case %I0 is true so the timer preset value is 10. If %I0 was false the timer preset would be 5.

#### Tool Turret

-   This Example is not complete yet.

This is a program for one type of tool turret. The turret has a home switch at tool position 1 and another switch to tell you when the turret is in a lockable position. To keep track of the actual tool number one must count how many positions past home you are. We will use Classic Ladder’s counter block *$CO*.The counter is preset to 1 when RESET is true. The counter is increased by one on the rising edge of INDEX. We then *COMPARE* the counter value (%C0.V) to the tool number we want (in the example only checks for tool 1 and 2 are shown). We also *OPERATE* the counter value to a word variable (%W0) that (you can assume) is mapped on to a s32 out HAL pin so you can let some other HAL component know what the current tool number is. In the real world another s32 (in) pin would be used to get the requested tool number from Machinekit.You would have to load Classic Ladder’s real time module specifying that you want s32 in and out pins. See *loading options* above. \[display turret sample\]

#### Sequential Example

-   This Example is not complete yet.

This is a sequential program.
 When the program is first started step one is active.
 Then when %B0 is true, steps 2 and 3 are then active and step one is inactive.
 Then when %B1 and/or %B2 are true, step 4 and/or 5 are active and step 2 and/or 3 are inactive.
 Then when either %B3 OR %B4 are true, step 6 is true and steps 4 and 5 are inactive.
 Then when %B5 is true step 1 is active and step 6 is inactive and it all starts again.

As shown, the sequence has been:
 %B0 was true making step 2 and 3 active, then %B1 became true
 (and still is-see the pink line through %B1)
 making step 4 active and step 2 inactive.
 Step 3 is active and waiting for %B2 to be true.
 Step 4 is active and is waiting for %B3 to be true.
 WOW, that was quite a mouthful!!

Exemples d’utilisation
======================

Deuxième port parallèle sur port PCI
------------------------------------

Lors de l’ajout d’un deuxième port parallèle placé dans un slot PCI il est indispensable de connaître son adresse avant de pouvoir l’utiliser avec Machinekit. Pour trouver l’adresse de ce port, ouvrez un terminal et tapez:

    lspci -v

Vous devriez voir quelques choses comme ci-dessous parmi la liste du matériel installé en PCI:

    Communication controller: NetMos Technology PCI 1 port parallel adapter (rev 01)
    LSI Logic / Symbios Logic: Unknown device 0010
        medium devsel, IRQ 11
        ports at a800 [size=8]
        ports at ac00 [size=8]
        ports at b000 [size=8]
        ports at b400 [size=8]
        ports at b800 [size=8]
        ports at bc00 [size=16]

Dans notre cas, l’adresse était la première, nous avons donc modifié le fichier .hal pour passer de

    loadrt hal_parport cfg=0x378

à

    loadrt hal_parport cfg="0x378 0xa800 in"

Noter les guillemets obligatoires encadrant les adresses.

Nous avons également ajouté:

    addf parport.1.read base-thread
    addf parport.1.write base-thread

pour que le second port parallèle soit lu et écrit.

Par défaut les 8 premières broches des ports parallèles sont des sorties. Le drapeau *in* situé derrière l’adresse d’un port permet de les positionner comme étant 8 entrées sur ce port.

Contrôle de la broche
---------------------

<span id="cha:Controle-broche"></span>

### Vitesse broche en 0-10V

Si la vitesse de la broche est contrôlée par un variateur de fréquence avec une consigne vitesse en 0 à 10V et qu’une carte de conversion (DAC) comme la m5i20 est utilisée pour sortir le signal.

Premièrement il faut calculer le facteur d'échelle entre la vitesse broche et la tension de commande. Dans cet exemple, la vitesse maximale de la broche sera de 5000 tr/mn pour une tension de commande de 10V. 10 Volts/5000 tr/mn = 0.002 Volts par tr/mn notre facteur d'échelle sera donc de 0.002.

Si la carte DAC ne dispose pas d’une fonction échelle, il est nécessaire d’ajouter un composant *scale* (echelle) au fichier hal pour calibrer *motion.spindle-speed-out* entre 0 et 10 comme demandé par le variateur de fréquence.

    loadrt scale count=1
    addf scale.0 servo-thread
    setp scale.0.gain 0.002
    net spindle-speed-scale motion.spindle-speed-out => scale.0.in
    net spindle-speed-DAC scale.0.out => «le nom de la sortie de votre DAC»

### Vitesse de broche en PWM

Si la vitesse de la broche peut être contrôlée par un signal de PWM, utiliser le composant *pwmgen* pour créer le signal:

    loadrt pwmgen output_type=0
    addf pwmgen.update servo-thread
    addf pwmgen.make-pulses base-thread
    net spindle-speed-cmd motion.spindle-speed-out => pwmgen.0.value
    net spindle-on motion.spindle-on => pwmgen.0.enable
    net spindle-pwm pwmgen.0.pwm => parport.0.pin-09-out
    # Adapter selon la vitesse maximale de la broche en tr/mn
    setp pwmgen.0.scale 1800

La réponse du contrôleur de PWM est simple: 0% donne 0tr/mn, 10% donnent 180 tr/mn… 100% donnent 1800 tr/mn. Si un minimum est nécessaire pour faire tourner la broche, suivre l’exemple *nist-lathe* fourni dans les exemples de configuration pour ajouter un composant d'échelle.

### Marche broche

Si un signal de marche broche reliant *motion.spindle-on* à une broche de sortie physique est envisagé. Pour relier ces pins à une broche du port parallèle, ajouter une ligne comme la suivante dans le fichier .hal, il faut bien sûr qu’elle soit câblée à l’interface de contrôle.

    net spindle-enable motion.spindle-on => parport.0.pin-14-out

### Sens de rotation de la broche

Pour contrôler le sens de rotation de la broche, les pins de HAL *motion.spindle-forward* et *motion.spindle-reverse* étant contrôlées par M3 et M4, peuvent être mise à une valeur positive différente de zéro pour que M3/4 inverse le sens de la broche.

Pour relier ces pins à des broches du port parallèle utiliser, par exemple, les lignes suivantes dans le fichier .hal, bien sûr ces broches doivent être câblées à l’interface de contrôle.

    net spindle-fwd motion.spindle-forward -> parport.0.pin-16-out
    net spindle-rev motion.spindle-reverse => parport.0.pin-17-out

### Démarrage en rampe

Si la broche doit démarrer en rampe et que le contrôleur n’a pas cette possibilité, HAL pourra le faire. Il faut premièrement détourner la sortie de *motion.spindle-speed-out* et la faire transiter par un composant *limit2* dont l'échelle est ajustée de sorte que la consigne suive une rampe entre *motion.spindle-speed-out* et le périphérique recevant la consigne de vitesse. Ensuite, il faut faire connaitre à Machinekit le moment ou la broche a atteint sa vitesse pour que les mouvements puissent commencer.

Reprenant dans l’exemple en 0-10 V, la ligne:

    net spindle-speed-scale motion.spindle-speed-out => scale.0.in

sera modifiée, comme indiqué dans l’exemple ci-dessous:

Introduction aux composants de HAL *limit2* et *near*:

Au cas ou vous ne les auriez jamais rencontré auparavant, voici une rapide introduction à ces deux composants de HAL utilisés dans l’exemple suivant.

-   Le composant de HAL *limit2* est un composant qui reçoit une valeur sur une entrée et fourni une valeur en sortie, limitée entre les seuils min et max et *également*, limitée pour ne pas dépasser l’amortissement spécifié. En d’autres termes, les fluctuations de la valeur de sortie seront toujours lentes et cette lenteur est ajustable.

-   Le composant de HAL *near* est un composant à deux entrées et une sortie binaire qui indique quand les deux entrées sont approximativement égales.

Voir le manuel de HAL ou les man pages, taper juste *man limit2* ou *man near*.

    # charge un composant temps réel limit2 et un near avec des noms aisés à suivre
    loadrt limit2 names=spindle-ramp
    loadrt near names=spindle-at-speed

    # ajoute les fonctions au thread
    addf spindle-ramp servo-thread
    addf spindle-at-speed servo-thread

    # fixe le paramètre max pour l'amortissement
    # (accélération/décélération de la broche en unités par seconde)
    setp spindle-ramp.maxv 60

    # détourne la sortie vitesse broche et l'envoie à l'entrée de la rampe
    net spindle-cmd <= motion.spindle-speed-out => spindle-ramp.in

    # la sortie de la rampe est envoyée à l'entrée de l'échelle
    net spindle-ramped <= spindle-ramp.out => scale.0.in

    # pour connaitre quand commencer le mouvement on envoie la vitesse de broche
    # commandée à une entrée du composant spindle-at-speed (qui est un composant near).
    # on envoie également le signal de fin de rampe (vitesse actuelle)
    # sur l'autre entrée de spindle-at-speed
    net spindle-cmd => spindle-at-speed.in1
    net spindle-ramped => spindle-at-speed.in2

    # la sortie de spindle-at-speed est envoyée à motion.spindle-at-speed
    # et quand elle devient TRUE, les mouvements peuvent commencer
    net spindle-ready <= spindle-at-speed.out => motion.spindle-at-speed

### Vitesse de broche avec signal de retour

Une information de retour est nécessaire pour que Machinekit puisse réaliser des mouvements synchronisés avec la broche comme le filetage ou la vitesse de coupe constante. L’assistant de configuration StepConf peut réaliser les connections lui même si les signaux *Canal A codeur broche* et *Index codeur broche* sont choisis parmi les entrées.

Matériel supposé présent:

-   Un codeur est monté sur la broche et délivre 100 impulsions par tour sur son canal A.

-   Ce canal A est raccordé à la broche 10 du port parallèle.

-   L’index de ce codeur est connecté à la broche 11 du port parallèle.

Configuration de base pour ajouter ces composants:

    loadrt encoder num_chan=1
    addf encoder.update-counters base-thread
    addf encoder.capture-position servo-thread
    setp encoder.0.position-scale 100
    setp encoder.0.counter-mode 1
    net spindle-position encoder.0.position => motion.spindle-revs
    net spindle-velocity encoder.0.velocity => motion.spindle-speed-in
    net spindle-index-enable encoder.0.index-enable <=> motion.spindle-index-enable
    net spindle-phase-a encoder.0.phase-A
    net spindle-phase-b encoder.0.phase-B
    net spindle-index encoder.0.phase-Z
    net spindle-phase-a <= parport.0.pin-10-in
    net spindle-index <= parport.0.pin-11-in

### Vitesse broche atteinte

Si le moteur de broche possède un retour d’information de vitesse provenant d’un codeur, il est alors possible d’utiliser la variable *motion.spindle-at-speed* pour permettre à Machinekit d’attendre que la broche ait atteint sa vitesse de consigne avant d’effectuer tout mouvement. Cette variable passe à TRUE quand la vitesse commandée est atteinte. Comme le retour vitesse est la vitesse de consigne ne sont jamais *exactement* identiques, il faut utiliser le composant *near* qui indique quand les deux composantes sont suffisamment proches l’une de l’autre.

Il est nécessaire de connecter la commande de vitesse broche sur near.n.in1 et le signal de retour vitesse du codeur sur near.n.in2. La sortie near.n.out est connectée à motion.spindle-at-speed. Le paramètre near.n.scale doit être ajusté pour indiquer dans quelle mesure les deux valeurs sont suffisamment proches pour passer activer la sortie. Selon le matériel utilisé, il pourra être utile d’ajuster l'échelle.

Les éléments suivants sont à ajouter au fichier HAL pour activer *Spindle At Speed*. Si near est déjà présent dans le fichier HAL, augmenter le numéro de composant et adapter le code suivant en conséquence. S’assurer que le nom du signal est bien le même dans le fichier HAL.

    # charger un composant near et l'attacher à un thread
    loadrt near
    addf near.0 servo-thread

    # connecter une entrée à la vitesse de broche commandée
    net spindle-cmd => near.0.in1

    # connecter une entrée à la mesure de vitesse broche du codeur
    net spindle-velocity => near.0.in2

    # connecter la sortie sur l'entrée spindle-at-speed
    net spindle-at-speed motion.spindle-at-speed <= near.0.out

    # Ajuster les entrées de vitesse de broche pour être dans une fourchette de 1%
    setp near.0.scale 1.01

Utilisation d’une manivelle
---------------------------

<span id="cha:Manivelle-Electronique"></span>

Cet exemple explique comment relier une manivelle, facile à trouver aujourd’hui sur le marché. Cet exemple utilisera la manivelle MPG3 avec une carte d’interface C22 de chez CNC4PC et un second port parallèle placé sur un slot PCI. Cet exemple fournira trois axes avec chacun trois incréments de pas: 0.1, 0.01, 0.001.

Dans le fichier *custom.hal* ou dans un fichier jog.hal, ajouter ce qui suit en vérifiant bien que les composants *mux4* ou *encoder* ne soient pas déjà utilisés. Si c'était le cas il faudrait en augmenter le nombre en incrémentant la valeur du *count* de la commande *loadrt*. Ajuster également le numéro de référence. Les composants mux4 et encoder sont décrits dans le manuel de HAL et dans les man pages.

Manivelle de jog

    loadrt encoder num_chan=1
    loadrt mux4 count=1
    addf encoder.capture-position servo-thread
    addf encoder.update-counters base-thread
    addf mux4.0 servo-thread

    # Mode position
    # Chaque cran de manivelle provoque un pas calibré,
    # la durée du mouvement total peut dépasser la durée de rotation de la manivelle.
    # C'est le mode par défaut.

    setp axis.N.jog-vel-mode 0

    # Mode vitesse
    # L'axe s'arrête quand la manivelle s'arrête, même si la pas de jog est incomplet.
    # Décommenter la ligne suivante pour obtenir ce mode de fonctionnement,
    # et commenter le mode position.

    setp axis.N.jog-vel-mode 1

    # Chaque axe est ajusté indépendemment des autres.

    # Tailles des pas de jog

    setp mux4.0.in0 0.1
    setp mux4.0.in1 0.01
    setp mux4.0.in2 0.001

    # Sélecteur de taille des pas du jog

    net scale1 mux4.0.sel0 <= parport.1.pin-09-in
    net scale2 mux4.0.sel1 <= parport.1.pin-10-in

    net pend-scale axis.0.jog-scale <= mux4.0.out
    net pend-scale axis.1.jog-scale
    net pend-scale axis.2.jog-scale

    # Signaux du codeur

    net mpg-a encoder.0.phase-A <= parport.1.pin-02-in
    net mpg-b encoder.0.phase-B <= parport.1.pin-03-in

    # Sélecteur d'axe

    net mpg-x axis.0.jog-enable <= parport.1.pin-04-in
    net mpg-y axis.1.jog-enable <= parport.1.pin-05-in
    net mpg-z axis.2.jog-enable <= parport.1.pin-06-in

    net pend-counts axis.0.jog-counts <= encoder.0.counts
    net pend-counts axis.1.jog-counts
    net pend-counts axis.2.jog-counts

Broche avec variateur GS2
-------------------------

### Exemple

Cet exemple montre les connexions demandées pour utiliser un variateur de fréquence fourni par la société Automation Direct pour piloter une broche. <span class="footnote">
\[En Europe on trouve une gamme de produits identiques sous la marque Omron.\]
</span> La direction de la broche et sa vitesse seront contrôlées par Machinekit.

L’utilisation du composant GS2 est très simple à régler. Une configuration complète peut être réalisée par l’assistant Stepconf. Bien vérifier que les pins *Spindle CW* et *Spindle PWM* sont inutilisées sur la page de réglage du port parallèle.

Placer les lignes suivantes dans le fichier custom.hal pour connecter Machinekit au GS2 et avoir le contrôle de celui-ci depuis l’interface utilisateur.

    # Charger le composant utilisateur pour le variateur variateur
    loadusr -Wn spindle-vfd gs2_vfd -n spindle-vfd

    # connecter la pin de direction au variateur
    net gs2-fwd spindle-vfd.spindle-fwd <= motion.spindle-forward

    # connecter la pin de Marche/Arrêt au variateur
    net gs2-run spindle-vfd.spindle-on <= motion.spindle-on

    # connecter la pin indiquant que la consigne vitesse est atteinte
    net gs2-at-speed motion.spindle-at-speed <= spindle-vfd.at-speed

    # connecter la commande de vitesse au variateur
    net gs2-RPM spindle-vfd.speed-command <= motion.spindle-speed-out

Sur le variateur de fréquence lui même, il est nécessaire de fixer quelques paramètres avant de pouvoir communiquer avec lui par Modbus. D’autres seront ajustés selon les caractéristiques demandées par le système. Ces réglages sortent, pour la plupart, du cadre de cet exemple, ils sont tous expliqués dans le manuel du variateur qu’il est impératif de consulter.

-   Les switches de communication doivent être positionnés sur RS-232C.

-   Les paramètres moteur doivent être ajustés en fonction du moteur.

-   P3.00 (Source des commandes de marche) doit être ajusté sur *Opérations déterminées par l’interface RS-485*, *03* ou *04*

-   P4.00 (Source des commandes de fréquence) doit être ajusté sur *Fréquence déterminée par l’interface RS232C/RS485*, *05*

-   P9.02 (Protocole de communication) doit être ajusté sur *Modbus RTU mode, 8 bits de donnée, pas de parity, 2 bits de stop*, *03*

Un panneau de commande virtuel PyVCP, basé sur cet exemple, est donné dans l'[exemple avec un variateur](#sec:Exemple-Compte-Tours-GS2).

Diagnostics & FAQ
=================

Moteurs pas à pas
-----------------

<span id="cha:stepper-diagnostics"></span>

Si ce que vous obtenez ne correspond pas à ce que vous espériez, la plupart du temps c’est juste un petit manque d’expérience. Accroître son expérience permet souvent une meilleure compréhension globale. Porter un diagnostic sur plusieurs problèmes est toujours plus facile en les prenant séparément, de même qu’une équation dont on a réduit le nombre de variables est toujours plus rapide à résoudre. Dans le monde réel ce n’est pas toujours le cas mais c’est une bonne voie à suivre.

### Problèmes communs

#### Le moteur n’avance que d’un pas

La raison la plus fréquente dans une nouvelle installation pour que le moteur ne bouge pas est l’interversion entre le signal de pas et le signal de direction. Si, quand vous pressez le bouton de jog dans un sens puis dans l’autre, le moteur n’avance que d’un pas à chaque fois et toujours dans la même direction, vous êtes dans ce cas.

#### Le moteur ne bouge pas

Certaine interfaces de pilotage de moteurs ont une broche d’activation (enable) ou demandent un signal de pompe de charge pour activer leurs sorties.

#### Distance incorrecte

Si vous commandez une distance de déplacement précise sur un axe et que le déplacement réel ne correspond pas, alors l'échelle de l’axe n’est pas bonne.

### Messages d’erreur

#### Erreur de suivi

Le concept d’erreur de suivi est étrange quand il s’agit de moteurs pas à pas. Etant un système en boucle ouverte, aucune contre réaction ne permet de savoir si le suivi est correct ou non. Machinekit calcule si il peut maintenir le suivi demandé par une commande, si ce n’est pas possible il stoppe le mouvement et affiche une erreur de suivi. Les erreurs de suivi sur les systèmes pas à pas sont habituellement les suivantes:

-   FERROR to small - (FERROR trop petit)

-   MIN\_FERROR to small - (MIN\_FERROR trop petit)

-   MAX\_VELOCITY to fast - (MAX\_VELOCITY trop rapide)

-   MAX\_ACCELERATION to fast - (MAX\_ACCELERATION trop rapide)

-   BASE\_PERIOD set to long - (BASE\_PERIOD trop longue)

-   Backlash ajouté à un axe (rattrapage de jeu)

Toutes ces erreurs se produisent lorsque l’horloge temps réel n’est pas capable de fournir le nombre de pas nécessaire pour maintenir la vitesse requise par le réglage de la variable BASE\_PERIOD. Ce qui peut se produire, par exemple après un test de latence trop bref pour obtenir un valeur fiable, dans ce cas, revenir à une valeur plus proche de ce qu’elle était et réessayez. C’est également le cas quand les valeurs de vitesse maximum et d’accélération maximum sont trop élevées.

Si un backlash a été ajouté, il est nécessaire d’augmenter STEPGEN\_MAXACCEL aux environs du double de MAX\_ACCELERATION dans la section \[AXIS\] du fichier INI et ce, pour chacun des axes sur lesquels un backlash a été ajouté. Machinekit utilise une *extra accélération* au moment de l’inversion de sens pour reprendre le jeu. Sans correction du backlash, l’accélération pour le générateur de pas peut être juste un peu plus basse que celle du planificateur de mouvements.

#### Erreur de RTAPI

Quand vous rencontrez cette erreur:

    RTAPI: ERROR: Unexpected realtime delay on task n

C’est généralement que la variable BASE\_PERIOD dans la section \[EMCMOT\] du fichier ini a une valeur trop petite. Vous devez lancer un *Latency Test* pendant une durée plus longue pour voir si vous n’avez pas un délai excessif quelque part, responsable de ce problème. Si c’est le cas réajuster alors BASE\_PERIOD avec la nouvelle valeur obtenue.

Machinekit vérifie le nombre de cycles du CPU entre les invocations du thread temps réel. Si certains éléments de votre matériel provoquent un délai excessif ou que les threads sont ajustés à des valeurs trop rapides, vous rencontrerez cette erreur.

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
<td align="left">Cette erreur n’est affichée qu’une seule fois par session. En effet, si votre BASE_PERIOD était trop basse vous pourriez avoir des centaines de milliers de messages d’erreur par seconde si plus d’un était affiché.</td>
</tr>
</tbody>
</table>

Plus d’informations [sur le test de latence.](#cha:test-de-latence)

### Tester

#### Tester le timing des pas

Si un de vos axes vibre, grogne ou fait des petits mouvements dans toutes les directions, c’est révélateur d’un mauvais timing d’impulsions de pas de ce moteur. Les paramètres du pilote matériel sont a vérifier et a ajuster. Il peut aussi y avoir des pertes de pas aux changements de direction. Si le moteur cale complétement, il est aussi possible que les paramètres MAX\_ACCELERATION ou MAX\_VELOCITY aient des valeurs trop élevées.

Le programme suivant vérifie que la configuration de l’axe Z est correcte. Copiez le programme dans le répertoire de votre machinekit/nc\_files nommez le *TestZ.ngc* ou similaire. Initialisez votre machine avec Z = 0.000 sur le dessus de la table. Chargez et lancez le programme. Il va effectuer 200 mouvements d’aller et retour entre 10.00 et 30.00mm. Si vous avez un problème de configuration, la position de l’axe Z affichée à la fin du programme, soit 10.00mm, ne correspondra pas à la position mesurée. Pour tester un autre axe remplacez simplement le Z des G0 par le nouvel axe.

    ( Faire Z=0 au dessus de la table avant de démarrer! )
    ( Ce programme teste les pertes de position en Z )
    ( msg, test 1 de la configuration de l'axe Z )
    G21 #1000=100 ( boucle 100 fois )
    ( cette boucle comporte un délai après chaque mouvement )
    ( test des réglages d'accélération et de vitesse )
    o100 while [#1000]
       G0 Z30.000
       G4 P0.250
       G0 Z10.000
       G4 P0.250
       #1000 = [#1000 - 1]
    o100 endwhile
    ( msg, test 2 de la configuration de l'axe Z, pressez S pour continuer)
    M1 (un arrêt ici)
    #1000=100 ( boucle 100 fois )
    ( Les boucles suivantes n'ont plus de délai en fin de mouvements )
    ( test des réglages des temps de maintien des pilotes)
    ( et les réglages d'accélération max )
    o101 while [#1000]
       G0 Z30.000 .
       G0 Z10.000
       #1000 = [#1000 - 1]
    o101 endwhile
    ( msg, Fin Z doit être à 10mm au dessus de la table )
    M2

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

<span class="comment"> vim: set syntax=asciidoc:</span>

Glossary
--------

A listing of terms and what they mean. Some terms have a general meaning and several additional meanings for users, installers, and developers.

 Acme Screw   
<span id="glo:AcmeScrew"></span> A type of lead-screw that uses an Acme thread form. Acme threads have somewhat lower friction and wear than simple triangular threads, but ball-screws are lower yet. Most manual machine tools use acme lead-screws.

 Axis   
<span id="glo:Axis"></span> One of the computer controlled movable parts of the machine. For a typical vertical mill, the table is the X axis, the saddle is the Y axis, and the quill or knee is the Z axis. Angular axes like rotary tables are referred to as A, B, and C. Additional linear axes relative to the tool are called U, V, and W respectively.

 Axis(GUI)   
One of the Graphical User Interfaces available to users of Machinekit. It features the modern use of menus and mouse buttons while automating and hiding some of the more traditional Machinekit controls. It is the only open-source interface that displays the entire tool path as soon as a file is opened.

 Backlash   
<span id="glo:Backlash"></span> The amount of "play" or lost motion that occurs when direction is reversed in a lead screw. or other mechanical motion driving system. It can result from nuts that are loose on leadscrews, slippage in belts, cable slack, "wind-up" in rotary couplings, and other places where the mechanical system is not "tight". Backlash will result in inaccurate motion, or in the case of motion caused by external forces (think cutting tool pulling on the work piece) the result can be broken cutting tools. This can happen because of the sudden increase in chip load on the cutter as the work piece is pulled across the backlash distance by the cutting tool.

 Backlash Compensation   
Any technique that attempts to reduce the effect of backlash without actually removing it from the mechanical system. This is typically done in software in the controller. This can correct the final resting place of the part in motion but fails to solve problems related to direction changes while in motion (think circular interpolation) and motion that is caused when external forces (think cutting tool pulling on the work piece) are the source of the motion.

 Ball Screw   
<span id="glo:Ballscrew"></span> A type of lead-screw that uses small hardened steel balls between the nut and screw to reduce friction. Ball-screws have very low friction and backlash, but are usually quite expensive.

 Ball Nut   
<span id="glo:Ballnut"></span> A special nut designed for use with a ball-screw. It contains an internal passage to re-circulate the balls from one end of the screw to the other.

 CNC   
<span id="glo:CNC"></span> Computer Numerical Control. The general term used to refer to computer control of machinery. Instead of a human operator turning cranks to move a cutting tool, CNC uses a computer and motors to move the tool, based on a part program.

 Comp   
<span id="glo:comp"></span> A tool used to build, compile and install Machinekit HAL components.

 Configuration(n)   
A directory containing a set of configuration files. Custom configurations are normally saved in the users home/Machinekit/configs directory. These files include Machinekit’s traditional INI file and HAL files. A configuration may also contain several general files that describe tools, parameters, and NML connections.

 Configuration(v)   
The task of setting up Machinekit so that it matches the hardware on a machine tool.

 Coordinate Measuring Machine   
A Coordinate Measuring Machine is used to make many accurate measurements on parts. These machines can be used to create CAD data for parts where no drawings can be found, when a hand-made prototype needs to be digitized for moldmaking, or to check the accuracy of machined or molded parts.

 Display units   
The linear and angular units used for onscreen display.

 DRO   
A Digital Read Out is a system of position-measuring devices attached to the slides of a machine tool, which are connected to a numeric display showing the current location of the tool with respect to some reference position. DROs are very popular on hand-operated machine tools because they measure the true tool position without backlash, even if the machine has very loose Acme screws. Some DROs use linear quadrature encoders to pick up position information from the machine, and some use methods similar to a resolver which keeps rolling over.

 EDM   
EDM is a method of removing metal in hard or difficult to machine or tough metals, or where rotating tools would not be able to produce the desired shape in a cost-effective manner. An excellent example is rectangular punch dies, where sharp internal corners are desired. Milling operations can not give sharp internal corners with finite diameter tools. A *wire* EDM machine can make internal corners with a radius only slightly larger than the wire’s radius. A *sinker* EDM can make internal corners with a radius only slightly larger than the radius on the corner of the sinking electrode.

 Machinekit   
<span id="glo:Machinekit"></span> The Enhanced Machine Controller. Initially a NIST project. Machinekit is able to run a wide range of motion devices.

 MachinekitIO   
<span id="glo:MachinekitIO"></span> The module within Machinekit that handles general purpose I/O, unrelated to the actual motion of the axes.

 MachinekitMOT   
<span id="glo:MachinekitMOT"></span> The module within Machinekit that handles the actual motion of the cutting tool. It runs as a real-time program and directly controls the motors.

 Encoder   
<span id="glo:Encoder"></span> A device to measure position. Usually a mechanical-optical device, which outputs a quadrature signal. The signal can be counted by special hardware, or directly by the parport with Machinekit.

 Feed   
<span id="glo:Feed"></span> Relatively slow, controlled motion of the tool used when making a cut.

 Feed rate   
The speed at which a cutting motion occurs. In auto or mdi mode, feed rate is commanded using an F word. F10 would mean ten machine units per minute.

 Feedback   
<span id="glo:Feedback"></span> A method (e.g., quadrature encoder signals) by which Machinekit receives information about the position of motors

 Feedrate Override   
<span id="glo:FeedrateOveride"></span> A manual, operator controlled change in the rate at which the tool moves while cutting. Often used to allow the operator to adjust for tools that are a little dull, or anything else that requires the feed rate to be “tweaked”.

 Floating Point Number   
A number that has a decimal point. (12.300) In HAL it is known as float.

 G-Code   
<span id="glo:G-Code"></span> The generic term used to refer to the most common part programming language. There are several dialects of G-code, Machinekit uses RS274/NGC.

 GUI   
<span id="glo:GUI"></span> Graphical User Interface.

 General   
A type of interface that allows communications between a computer and a human (in most cases) via the manipulation of icons and other elements (widgets) on a computer screen.

 Machinekit   
An application that presents a graphical screen to the machine operator allowing manipulation of the machine and the corresponding controlling program.

 HAL   
<span id="glo:HAL"></span> Hardware Abstraction Layer. At the highest level, it is simply a way to allow a number of building blocks to be loaded and interconnected to assemble a complex system. Many of the building blocks are drivers for hardware devices. However, HAL can do more than just configure hardware drivers.

 Home   
<span id="glo:Home"></span> A specific location in the machine’s work envelope that is used to make sure the computer and the actual machine both agree on the tool position.

 ini file   
<span id="glo:inifile"></span> A text file that contains most of the information that configures Machinekit for a particular machine

 Instance   
<span id="glo:Instance"></span> One can have an instance of a class or a particular object. The instance is the actual object created at runtime. In programmer jargon, the Lassie object is an instance of the Dog class.

 Joint Coordinates   
<span id="glo:Joint_Coordinates"></span> These specify the angles between the individual joints of the machine. See also Kinematics

 Jog   
Manually moving an axis of a machine. Jogging either moves the axis a fixed amount for each key-press, or moves the axis at a constant speed as long as you hold down the key. In manual mode, jog speed can be set from the graphical interface.

 kernel-space   
<span id="glo:kernel-space"></span> See real-time.

 Kinematics   
<span id="glo:Kinematics"></span> The position relationship between world coordinates and joint coordinates of a machine. There are two types of kinematics. Forward kinematics is used to calculate world coordinates from joint coordinates. Inverse kinematics is used for exactly the opposite purpose. Note that kinematics does not take into account, the forces, moments etc. on the machine. It is for positioning only.

 Lead-screw   
<span id="glo:Leadscrew"></span> An screw that is rotated by a motor to move a table or other part of a machine. Lead-screws are usually either ball-screws or acme screws, although conventional triangular threaded screws may be used where accuracy and long life are not as important as low cost.

 Machine units   
The linear and angular units used for machine configuration. These units are specified and used in the ini file. HAL pins and parameters are also generally in machine units.

 MDI   
<span id="glo:MDI"></span> Manual Data Input. This is a mode of operation where the controller executes single lines of G-code as they are typed by the operator.

 NIST   
<span id="glo:NIST"></span> National Institute of Standards and Technology. An agency of the Department of Commerce in the United States.

 Offsets   
<span id="glo:Offsets"></span> An arbitrary amount, added to the value of something to make it equal to some desired value. For example, gcode programs are often written around some convenient point, such as X0, Y0. Fixture offsets can be used to shift the actual execution point of that gcode program to properly fit the true location of the vise and jaws. Tool offsets can be used to shift the "uncorrected" length of a tool to equal that tool’s actual length.

 Part Program   
<span id="glo:PartProgram"></span> A description of a part, in a language that the controller can understand. For Machinekit, that language is RS-274/NGC, commonly known as G-code.

 Program Units   
The linear and angular units used in a part program. The linear program units do not have to be the same as the linear machine units. See G20 and G21 for more information. The angular program units are always measured in degrees.

 Python   
General-purpose, very high-level programming language. Used in Machinekit for the Axis GUI, the Stepconf configuration tool, and several G-code programming scripts.

 Rapid   
<span id="glo:Rapid"></span> Fast, possibly less precise motion of the tool, commonly used to move between cuts. If the tool meets the workpiece or the fixturing during a rapid, it is probably a bad thing!

 Rapid rate   
<span id="glo:RapidRate"></span>The speed at which a rapid motion occurs. In auto or mdi mode, rapid rate is usually the maximum speed of the machine. It is often desirable to limit the rapid rate when testing a g-code program for the first time.

 Real-time   
<span id="glo:real-time"></span> Software that is intended to meet very strict timing deadlines. Under Linux, in order to meet these requirements it is necessary to install RTAI or RTLINUX and build the software to run in those special environments. For this reason real-time software runs in kernel-space.

 RTAI   
<span id="glo:RTAI"></span> Real Time Application Interface, see <https://www.rtai.org/>, one of two real-time extensions for Linux that Machinekit can use to achieve real-time performance.

 RTLINUX   
<span id="glo:RTLINUX"></span> See <http://www.rtlinux.org>, one of two real-time extensions for Linux that Machinekit can use to achieve real-time performance.

 RTAPI   
A portable interface to real-time operating systems including RTAI and RTLINUX

 RS-274/NGC   
<span id="glo:RS274NGC"></span> The formal name for the language used by Machinekit part programs.

 Servo Motor   
<span id="glo:ServoMotor"></span> Generally, any motor that is used with error-sensing feedback to correct the position of an actuator. Also, a motor which is specially-designed to provide improved performance in such applications.

 Servo Loop   
<span id="glo:ServoLoop"></span> A control loop used to control position or velocity of an motor equipped with a feedback device.

 Signed Integer   
A whole number that can have a positive or negative sign. In HAL it is known as s32. (A signed 32-bit integer has a usable range of -2,147,483,647 to +2,147,483,647.)

 Spindle   
<span id="glo:Spindle"></span> The part of a machine tool that spins to do the cutting. On a mill or drill, the spindle holds the cutting tool. On a lathe, the spindle holds the workpiece.

 Spindle Speed Override   
A manual, operator controlled change in the rate at which the tool rotates while cutting. Often used to allow the operator to adjust for chatter caused by the cutter’s teeth. Spindle Speed Override assumes that the Machinekit software has been configured to control spindle speed.

 Stepconf   
An Machinekit configuration wizard. It is able to handle many step-and-direction motion command based machines. It writes a full configuration after the user answers a few questions about the computer and machine that Machinekit is to run on.

 Stepper Motor   
<span id="glo:StepperMotor"></span> A type of motor that turns in fixed steps. By counting steps, it is possible to determine how far the motor has turned. If the load exceeds the torque capability of the motor, it will skip one or more steps, causing position errors.

 TASK   
<span id="glo:TASK"></span> The module within Machinekit that coordinates the overall execution and interprets the part program.

 Tcl/Tk   
<span id="glo:Tcl/Tk"></span> A scripting language and graphical widget toolkit with which several of Machinekits GUIs and selection wizards were written.

 Traverse Move   
A move in a straight line from the start point to the end point.

 Units   
See "Machine Units", "Display Units", or "Program Units".

 Unsigned Integer   
A whole number that has no sign. In HAL it is known as u32. (An unsigned 32-bit integer has a usable range of zero to 4,294,967,296.)

 World Coordinates   
<span id="glo:World_Coordinates"></span> This is the absolute frame of reference. It gives coordinates in terms of a fixed reference frame that is attached to some point (generally the base) of the machine tool.

Legal Section
-------------

### Copyright Terms

Copyright (c) 2000-2013 Machinekit.org

Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.1 or any later version published by the Free Software Foundation; with no Invariant Sections, no Front-Cover Texts, and one Back-Cover Text: "This Machinekit Handbook is the product of several authors writing for machinekit.io As you find it to be of value in your work, we invite you to contribute to its revision and growth." A copy of the license is included in the section entitled "GNU Free Documentation License". If you do not find the license you may order a copy from Free Software Foundation, Inc. 59 Temple Place, Suite 330, Boston, MA 02111-1307

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

You may copy and distribute a Modified Version of the Document under the conditions of sections 2 and 3 above, provided that you release the Modified Version under precisely this License, with the Modified Version filling the role of the Document, thus licensing distribution and modification of the Modified Version to whoever possesses a copy of it. In addition, you must do these things in the Modified Version:

1.  Use in the Title Page (and on the covers, if any) a title distinct from that of the Document, and from those of previous versions (which should, if there were any, be listed in the History section of the Document). You may use the same title as a previous version if the original publisher of that version gives permission. B. List on the Title Page, as authors, one or more persons or entities responsible for authorship of the modifications in the Modified Version, together with at least five of the principal authors of the Document (all of its principal authors, if it has less than five). C. State on the Title page the name of the publisher of the Modified Version, as the publisher. D. Preserve all the copyright notices of the Document. E. Add an appropriate copyright notice for your modifications adjacent to the other copyright notices. F. Include, immediately after the copyright notices, a license notice giving the public permission to use the Modified Version under the terms of this License, in the form shown in the Addendum below. G. Preserve in that license notice the full lists of Invariant Sections and required Cover Texts given in the Document’s license notice. H. Include an unaltered copy of this License. I. Preserve the section entitled "History", and its title, and add to it an item stating at least the title, year, new authors, and publisher of the Modified Version as given on the Title Page. If there is no section entitled "History" in the Document, create one stating the title, year, authors, and publisher of the Document as given on its Title Page, then add an item describing the Modified Version as stated in the previous sentence. J. Preserve the network location, if any, given in the Document for public access to a Transparent copy of the Document, and likewise the network locations given in the Document for previous versions it was based on. These may be placed in the "History" section. You may omit a network location for a work that was published at least four years before the Document itself, or if the original publisher of the version it refers to gives permission. K. In any section entitled "Acknowledgements" or "Dedications", preserve the section’s title, and preserve in the section all the substance and tone of each of the contributor acknowledgements and/or dedications given therein. L. Preserve all the Invariant Sections of the Document, unaltered in their text and in their titles. Section numbers or the equivalent are not considered part of the section titles. M. Delete any section entitled "Endorsements". Such a section may not be included in the Modified Version. N. Do not retitle any existing section as "Endorsements" or to conflict in title with any Invariant Section.

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

<span class="comment"> = Index</span>

<span class="comment"> vim: set syntax=asciidoc:</span>

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


