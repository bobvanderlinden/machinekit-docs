Les composants temps réel
=========================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Composants-temps-reel"></span>

Stepgen
-------

Ce composant fournit un générateur logiciel d’impulsions de pas répondant aux commandes de position ou de vitesse. En mode position, il contient une boucle de position pré-réglée, de sorte que les réglages de PID ne sont pas nécessaires. En mode vitesse, il pilote un moteur à la vitesse commandée, tout en obéissant aux limites de vitesse et d’accélération. C’est un composant uniquement temps réel, dépendant de plusieurs facteurs comme la vitesse du CPU, etc, il est capable de fournir des fréquences de pas maximum comprise entre 10kHz et 50kHz. La figure [ci-dessous](#fig:Diagramme-bloc-stepgen) montre trois schémas fonctionnels, chacun est un simple générateur d’impulsions de pas. Le premier diagramme est pour le type *0*, (pas et direction). Le second est pour le type *1* (up/down, ou pseudo-PWM) et le troisième est pour les types 2 jusqu'à 14 (les différentes séquences de pas). Les deux premiers diagrammes montrent le mode de commande *position* et le troisième montre le mode *vitesse*. Le mode de commande et le type de pas, se règlent indépendamment et n’importe quelle combinaison peut être choisie.

Diagramme bloc du générateur de pas stepgen

![](images/stepgen-block-diag.png)

### L’installer

    halcmd: loadrt stepgen step_type=<type-array> [ctrl_type=<ctrl_array>]

*&lt;type-array&gt;* est une série d’entiers décimaux séparés par des virgules. Chaque chiffre provoquera le chargement d’un simple générateur d’impulsions de pas, la valeur de ce chiffre déterminera le type de pas. *&lt;ctrl\_array&gt;* est une série de lettres *p* ou *v* séparées par des virgules, qui spécifient le mode pas ou le mode vitesse. *ctrl\_type* est optionnel, si il est omis, tous les générateurs de pas seront en mode position. Par exemple, la commande:

    halcmd: loadrt stepgen step_type=0,0,2 ctrl_type=p,p,v

va installer trois générateurs de pas. Les deux premiers utilisent le type de pas *0* (pas et direction) et fonctionnent en mode position. Le dernier utilise le type de pas *2* (quadrature) et fonctionne en mode vitesse. La valeur par défaut de *&lt;config-array&gt;* est *0,0,0* qui va installer trois générateurs de type *0* (step/dir). Le nombre maximum de générateurs de pas est de 8 (comme définit par MAX\_CHAN dans stepgen.c). Chaque générateur est indépendant, mais tous sont actualisés par la même fonction(s), au même instant. Dans les descriptions qui suivent, *&lt;chan&gt;* est le nombre de générateurs spécifiques. La numérotation des générateurs commence à 0.

### Le désinstaller

    halcmd: unloadrt stepgen

### Pins

Chaque générateur d’impulsions de pas n’aura que certaines de ces pins, selon le type de pas et le mode de contrôle sélectionné.

-   *(float) stepgen.&lt;chan&gt;.position-cmd* — Position désirée du moteur, en unités de longueur (mode position seulement).

-   *(float) stepgen.&lt;chan&gt;.velocity-cmd* — Vitesse désirée du moteur, en unités de longueur par seconde (mode vitesse seulement).

-   *(s32) stepgen.&lt;chan&gt;.counts* — Rétroaction de la position en unités de comptage, actualisée par la fonction *capture\_position()*.

-   *(float) stepgen.&lt;chan&gt;.position-fb* — Rétroaction de la position en unités de longueur, actualisée par la fonction *capture\_position()*.

-   *(bit) stepgen.&lt;chan&gt;.step* — Sortie des impulsions de pas (type de pas 0 seulement).

-   *(bit) stepgen.&lt;chan&gt;.dir* — Sortie direction (type de pas 0 seulement).

-   *(bit) stepgen.&lt;chan&gt;.up* — Sortie UP en pseudo-PWM (type de pas 1 seulement).

-   *(bit) stepgen.&lt;chan&gt;.down* — Sortie DOWN en pseudo-PWM (type de pas 1 seulement).

-   *(bit) stepgen.&lt;chan&gt;.phase-A* — Sortie phase A (séquences de pas 2 à 14 seulement).

-   *(bit) stepgen.&lt;chan&gt;.phase-B* — Sortie phase B (séquences de pas 2 à 14 seulement).

-   *(bit) stepgen.&lt;chan&gt;.phase-C* — Sortie phase C (séquences de pas 3 à 14 seulement).

-   *(bit) stepgen.&lt;chan&gt;.phase-D* — Sortie phase D (séquences de pas 5 à 14 seulement).

-   *(bit) stepgen.&lt;chan&gt;.phase-E* — Sortie phase E (séquences de pas 11 à 14 seulement).

### Paramètres

-   *(float) stepgen.&lt;chan&gt;.position-scale* — Pas par unité de longueur. Ce paramètre est utilisé pour les sorties et les rétroactions.

-   *(float) stepgen.&lt;chan&gt;.maxvel* — Vitesse maximale, en unités de longueur par seconde. Si égal à 0.0, n’a aucun effet.

-   *(float) stepgen.&lt;chan&gt;.maxaccel* — Valeur maximale d’accélération, en unités de longueur par seconde au carré. Si égal à 0.0, n’a aucun effet.

-   *(float) stepgen.&lt;chan&gt;.frequency* — Fréquence des pas, en pas par seconde.

-   *(float) stepgen.&lt;chan&gt;.steplen* — Durée de l’impulsion de pas (types de pas 0 et 1) ou durée minimum dans un état donné (séquences de pas 2 à 14), en nanosecondes.

-   *(float) stepgen.&lt;chan&gt;.stepspace* — Espace minimum entre deux impulsions de pas (types de pas 0 et 1 seulement), en nanosecondes.

-   *(float) stepgen.&lt;chan&gt;.dirsetup* — Durée minimale entre un changement de direction et le début de la prochaine impulsion de pas (type de pas 0 seulement), en nanosecondes.

-   *(float) stepgen.&lt;chan&gt;.dirhold* — Durée minimale entre la fin d’une impulsion de pas et un changement de direction (type de pas 0 seulement), en nanosecondes.

-   *(float) stepgen.&lt;chan&gt;.dirdelay* — Durée minimale entre un pas dans une direction et un pas dans la direction opposée (séquences de pas 1 à 14 seulement), en nanosecondes.

-   *(s32) stepgen.&lt;chan&gt;.rawcounts* — Valeur de comptage brute (count) de la rétroaction, réactualisée par la fonction *make\_pulses()*.

En mode position, les valeurs de maxvel et de maxaccel sont utilisées par la boucle de position interne pour éviter de générer des trains d’impulsions de pas que le moteur ne peut pas suivre. Lorsqu’elles sont réglées sur des valeurs appropriées pour le moteur, même un grand changement instantané dans la position commandée produira un mouvement trapézoïdal en douceur vers la nouvelle position. L’algorithme fonctionne en mesurant à la fois, l’erreur de position et l’erreur de vitesse, puis en calculant une accélération qui tende à réduire vers zéro, les deux en même temps. Pour plus de détails, y compris les contenus de la boîte *d'équation de contrôle*, consulter le code source.

En mode vitesse, maxvel est une simple limite qui est appliquée à la vitesse commandée, maxaccel est utilisé pour créer une rampe avec la fréquence actuelle, si la vitesse commandée change brutalement. Comme dans le mode position, des valeurs appropriées de ces paramètres assurent que le moteur pourra suivre le train d’impulsions généré.

### Séquences de pas

Le générateur de pas supporte 15 différentes *séquences de pas*. Le type de pas 0 est le plus familier, c’est le standard pas et direction (step/dir). Quand stepgen est configuré pour le type 0, il y a quatre paramètres supplémentaires qui déterminent le timing exact des signaux de pas et de direction. Voir la figure [ci-dessous](#fig:StepDir-timing) pour la signification de ces paramètres. Les paramètres sont en nanosecondes, mais ils doivent être arrondis à un entier, multiple de la période du thread qui appelle *make\_pulses()*. Par exemple, si *make\_pulses()* est appelée toutes les 16µs et que *steplen* est à 20000, alors l’impulsion de pas aura une durée de 2 x 16 = 32µs. La valeur par défaut de ces quatre paramètres est de 1ns, mais l’arrondi automatique prendra effet au premier lancement du code. Puisqu’un pas exige d'être haut pendant *steplen* ns et bas pendant *stepspace* ns, la fréquence maximale est 1.000.000.000 divisé par *(steplen+stepspace)*. Si *maxfreq* est réglé plus haut que cette limite, il sera abaissé automatiquement. Si *maxfreq* est à zéro, il restera à zéro, mais la fréquence de sortie sera toujours limitée.

Timing pas et direction

![](images/stepgen-type0.png)

Le type de pas 1 a deux sorties, up et down. Les impulsions apparaissent sur l’une ou l’autre, selon la direction du déplacement. Chaque impulsion a une durée de *steplen* ns et les impulsions sont séparées de *stepspace* ns. La fréquence maximale est la même que pour le type 0. Si *maxfreq* est réglé plus haut que cette limite il sera abaissé automatiquement. Si *maxfreq* est à zéro, il restera à zéro, mais la fréquence de sortie sera toujours limitée.

Les séquences 2 jusqu'à 14 sont basées sur les états et ont entre deux et cinq sorties. Pour chaque pas, un compteur d'état est incrémenté ou décrémenté. Les figures suivantes:

-   [Trois phases en quadrature](#fig:Trois-phases-quadrature),

-   [Quatre phases](#fig:Quatre-phases),

-   [Cinq phases](#fig:Cinq-phases)

montrent les différentes séquences des sorties en fonction de l'état du compteur. La fréquence maximale est 1.000.000.000 (1\*10<sup>9</sup>) divisé par *steplen* et comme dans les autres séquences, *maxfreq* sera abaissé si il est au dessus de cette limite.

Séquences de pas à trois phases

![](images/stepgen-type2-4.png)

Séquences de pas à quatre phases

![](images/stepgen-type5-10.png)

Séquence de pas à cinq phases

![](images/stepgen-type11-14.png)

### Fonctions

Le composant exporte trois fonctions. Chaque fonction agit sur tous les générateurs d’impulsions de pas. Lancer différents générateurs dans différents threads n’est pas supporté.

-   *(funct) stepgen.make-pulses* — Fonction haute vitesse de génération et de comptage des impulsions (non flottant).

-   *(funct) stepgen.update-freq* — Fonction basse vitesse de conversion de position en vitesse, mise à l'échelle et traitement des limitations.

-   *(funct) stepgen.capture-position* — Fonction basse vitesse pour la rétroaction, met à jour les latches et les mesures de position.

La fonction à grande vitesse *stepgen.make-pulses* devrait être lancée dans un thread très rapide, entre 10 et 50us selon les capacités de l’ordinateur. C’est la période de ce thread qui détermine la fréquence maximale des pas, de *steplen*, *stepspace*, *dirsetup*, *dirhold* et *dirdelay*, tous sont arrondis au multiple entier de la période du thread en nanosecondes. Les deux autres fonctions peuvent être appelées beaucoup plus lentement.

PWMgen
------

Ce composant fournit un générateur logiciel de PWM (modulation de largeur d’impulsions) et PDM (modulation de densité d’impulsions). C’est un composant temps réel uniquement, dépendant de plusieurs facteurs comme la vitesse du CPU, etc, Il est capable de générer des fréquences PWM de quelques centaines de Hertz en assez bonne résolution, à peut-être 10kHz avec une résolution limitée.

### L’installer

    halcmd: loadrt pwmgen output_type=<config-array>

*&lt;config-array&gt;* est une série d’entiers décimaux séparés par des virgules. Chaque chiffre provoquera le chargement d’un simple générateur de PWM, la valeur de ce chiffre determinera le type de sortie.

Exemple avec pwmgen

    halcmd: loadrt pwmgen output_type=0,1,2

va installer trois générateurs de PWM. Le premier utilisera une sortie de type *0* (PWM seule), le suivant utilisera une sortie de type 1 (PWM et direction) et le troisième utilisera une sortie de type 2 (UP et DOWN). Il n’y a pas de valeur par défaut, si *&lt;config-array&gt;* n’est pas spécifié, aucun générateur de PWM ne sera installé. Le nombre maximum de générateurs de fréquences est de 8 (comme définit par MAX\_CHAN dans pwmgen.c). Chaque générateur est indépendant, mais tous sont mis à jour par la même fonction(s), au même instant. Dans les descriptions qui suivent, *&lt;chan&gt;* est le nombre de générateurs spécifiques. La numérotation des générateurs de PWM commence à 0.

### Le désinstaller

    halcmd: unloadrt pwmgen

### Pins

Chaque générateur de PWM aura les pins suivantes:

-   *(float) pwmgen.&lt;chan&gt;.value* — Valeur commandée, en unités arbitraires. Sera mise à l'échelle par le paramètre d'échelle (voir ci-dessous).

-   *(bit) pwmgen.&lt;chan&gt;.enable* — Active ou désactive les sorties du générateur de PWM.

Chaque générateur de PWM aura également certaines de ces pins, selon le type de sortie choisi:

-   *(bit) pwmgen.&lt;chan&gt;.pwm* — Sortie PWM (ou PDM), (types de sortie 0 et 1 seulement).

-   *(bit) pwmgen.&lt;chan&gt;.dir* — Sortie direction (type de sortie 1 seulement).

-   *(bit) pwmgen.&lt;chan&gt;.up* — Sortie PWM/PDM pour une valeur positive en entrée (type de sortie 2 seulement).

-   *(bit) pwmgen.&lt;chan&gt;.down* — Sortie PWM/PDM pour une valeur négative en entrée (type de sortie 2 seulement).

### Paramètres

-   *(float) pwmgen.&lt;chan&gt;.scale* — Facteur d'échelle pour convertir les valeurs en unités arbitraires, en coefficients de facteur cyclique.

-   *(float) pwmgen.&lt;chan&gt;.pwm-freq* — Fréquence de PWM désirée, en Hz. Si égale à 0.0, la modulation sera PDM au lieu de PWM. Si elle est réglée plus haute que les limites internes, au prochain appel de la fonction *update\_freq()* elle sera ramenée aux limites internes. Si elle est différente de zéro et si *le lissage* est faux, au prochain appel de la fonction *update\_freq()* elle sera réglée au plus proche entier multiple de la période de la fonction *make\_pulses()*.

-   *(bit) pwmgen.&lt;chan&gt;.dither-pwm* — Si vrai, active le lissage pour affiner la fréquence PWM ou le rapport cyclique qui ne pourraient pas être obtenus avec une pure PWM. Si faux, la fréquence PWM et le rapport cyclique seront tous les deux arrondis aux valeurs pouvant être atteintes exactement.

-   *(float) pwmgen.&lt;chan&gt;.min-dc* — Rapport cyclique minimum compris entre 0.0 et 1.0 (Le rapport cyclique sera à zéro quand il est désactivé, indépendamment de ce paramètre).

-   *(float) pwmgen.&lt;chan&gt;.max-dc* — Rapport cyclique maximum compris entre 0.0 et 1.0.

-   *(float) pwmgen.&lt;chan&gt;.curr-dc* — Rapport cyclique courant, après toutes les limitations et les arrondis (lecture seule).

### Types de sortie

Le générateur de PWM supporte trois *types de sortie*.

-   Le *type 0* - A une seule pin de sortie. Seules, les commandes positives sont acceptées, les valeurs négatives sont traitées comme zéro (elle seront affectées par le paramètre *min-dc* si il est différent de zéro).

-   Le *type 1* - A deux pins de sortie, une pour le signal PWM/PDM et une pour la direction. Le rapport cyclique d’une pin PWM est basé sur la valeur absolue de la commande, de sorte que les valeurs négatives sont acceptables. La pin de direction est fausse pour les commandes positives et vraie pour les commandes négatives.

-   Le *type 2* - A également deux sorties, appelées *up* et *down*. Pour les commandes positives, le signal PWM apparaît sur la sortie *up* et la sortie *down* reste fausse. Pour les commandes négatives, le signal PWM apparaît sur la sortie *down* et la sortie *up* reste fausse. Les sorties de type 2 sont appropriées pour piloter la plupart des ponts en H.

### Fonctions

Le composant exporte deux fonctions. Chaque fonction agit sur tous les générateurs de PWM, lancer différents générateurs dans différents threads n’est pas supporté.

-   *(funct) pwmgen.make-pulses* — Fonction haute vitesse de génération de fréquences PWM (non flottant).

-   *(funct) pwmgen.update* — Fonction basse vitesse de mise à l'échelle, limitation des valeurs et traitement d’autres paramètres.

La fonction haute vitesse *pwmgen.make-pulses* devrait être lancée dans un thread très rapide, entre 10 et 50 us selon les capacités de l’ordinateur. C’est la période de ce thread qui détermine la fréquence maximale de la porteuse PWM, ainsi que la résolution des signaux PWM ou PDM. L’autre fonction peut être appelée beaucoup plus lentement.

Codeur
------

Ce composant fournit, en logiciel, le comptage des signaux provenant d’encodeurs en quadrature. Il s’agit d’un composant temps réel uniquement, il est dépendant de divers facteurs comme la vitesse du CPU, etc, il est capable de compter des signaux de fréquences comprises entre 10kHz à peut être 50kHz. La figure ci-dessous représente le diagramme bloc d’une voie de comptage de codeur.

Diagramme bloc du codeur

![](images/encoder-block-diag.png)

### L’installer

    halcmd: loadrt encoder [num_chan=<counters>]

*&lt;counters&gt;* est le nombre de compteurs de codeur à installer. Si *numchan* n’est pas spécifié, trois compteurs seront installés. Le nombre maximum de compteurs est de 8 (comme définit par MAX\_CHAN dans encoder.c). Chaque compteur est indépendant, mais tous sont mis à jour par la même fonction(s) au même instant. Dans les descriptions qui suivent, *&lt;chan&gt;* est le nombre de compteurs spécifiques. La numérotation des compteurs commence à 0.

### Le désinstaller

    halcmd: unloadrt encoder

### Pins

-   *Encodeur &lt;chan&gt; counter-mode* (bit, I/O) (par défaut: FALSE) — Permet le mode compteur. Lorsque TRUE, le compteur compte chaque front montant de l’entrée phase-A, ignorant la valeur de la phase-B. Ceci est utile pour compter la sortie d’un capteur simple canal (pas de quadrature). Si FALSE, il compte en mode quadrature.

-   *encoder.&lt;chan&gt;.counts* (s32, Out) — Position en comptage du codeur.

-   *encoder.&lt;chan&gt;.counts-latched* (s32, Out) — Non utilisé à ce moment.

-   *encoder.&lt;chan&gt; index-enable* (bit, I/O) — Si TRUE, *counts* et *position* sont remis à zéro au prochain front montant de la phase Z. En même temps, *index-enable* est remis à zéro pour indiquer que le front montant est survenu. La broche *index-enable* est bi-directionnelle. Si *index-enable* est FALSE, la phase Z du codeur sera ignorée et le compteur comptera normalement. Le pilote du codeur ne doit jamais mettre *index-enable* TRUE. Cependant, d’autres composants peuvent le faire.

-   *encoder.&lt;chan&gt;.latch-falling* (bit, In) (par défaut: TRUE) — Non utilisé à ce moment.

-   *encoder.&lt;chan&gt;.latch-input* (bit, In) (par défaut: TRUE) — Non utilisé à ce moment.

-   *encoder.&lt;chan&gt;.latch-rising* (bit, In) — Non utilisé à ce moment.

-   *encoder.&lt;chan&gt;.min-speed-estimate* (Float, In) — Effectue une estimation de la vitesse minimale réelle, à partir de laquelle, la vitesse sera estimée comme non nulle et la position interpolées, comme étant interpolée. Les unités de vitesse *min-speed-estimate* sont les mêmes que les unités de *velocity*. Le facteur d'échelle, en compte par unité de longueur. Régler ce paramètre trop bas, fera prendre beaucoup de temps pour que la vitesse arrive à 0 après que les impulsions du codeur aient cessé d’arriver.

-   *encoder.&lt;chan&gt;.phase-A* (bit, In) — Signal de la phase A du codeur en quadrature.

-   *encoder.&lt;chan&gt;.phase-B* (bit, In) — Signal de la phase B du codeur en quadrature.

-   *encoder.&lt;chan&gt;.phase-Z* (bit, In) — Signal de la phase Z (impulsion d’index) du codeur en quadrature.

-   *encoder.&lt;chan&gt;.position* (float, Out) - Position en unités mises à l'échelle (voir *position* échelle).

-   *encoder.&lt;chan&gt;.position-interpolated* (float, Out) - Position en unités mises à l'échelle, interpolées entre les comptes du codeur. *position-interpolated* tente d’interpoler entre les comptes du codeur, basée sur la mesure de vitesse la plus récente. Valable uniquement lorsque la vitesse est approximativement constante et supérieure à *min-speed-estimate*. Ne pas utiliser pour le contrôle de position, puisque sa valeur est incorrecte en basse vitesse, lors des inversions de direction et pendant les changements de vitesse. Toutefois, il permet à un codeur à PPR faible (y compris les codeur à une impulsion par tour) d'être utilisé pour du filetage sur tour et peut aussi avoir d’autres usages.

-   *encoder.&lt;chan&gt;.position-latched* (float, Out) — Non utilisé à ce moment.

-   *encoder.&lt;chan&gt;.position-scale* (float, I/O) — Le facteur d'échelle, en comptes par unité de longueur. Par exemple, si *position-scale* est à 500, alors à 1000 comptes codeur, la position sera donnée à 2,0 unités.

-   *encoder.&lt;chan&gt;.rawcounts* (s32, In) — Le compte brut, tel que déterminé par \_update-counters. Cette valeur est mise à jour plus fréquemment que compte et position. Il n’est également pas affecté par le reset ou l’impulsion d’index.

-   *encoder.&lt;chan&gt;.reset* (bit, In) — Si TRUE, force *counts* et *position* immédiatement à zéro.

-   *encoder.&lt;chan&gt;.velocity* (float, Out) — Vitesse en unités mises à l'échelle par secondes. *encoder* utilise un algorithme qui réduit considérablement la quantification du bruit comparé à simplement différencier la sortie *position*. Lorsque la magnitude de la vitesse réelle est inférieure à *min-speed-estimate*, la sortie *velocity* est à 0.

-   *encoder.&lt;chan&gt;.x4-mode* (bit, I/O) (par défaut: TRUE) — Permet le mode x4. Lorsqu’il est TRUE, le compteur compte chaque front de l’onde en quadrature (quatre compte par cycle complet). Si FALSE, il ne compte qu’une seule fois par cycle complet. En mode compteur, ce paramètre est ignoré. Le mode 1x est utile pour certaines manivelles électroniques.

### Paramètres

-   *encoder.&lt;chan&gt;.capture-position.time (s32, RO)*

-   *encoder.&lt;chan&gt;.capture-position.tmax (s32, RW)*

-   *encoder.&lt;chan&gt;.update-counters.time (s32, RO)*

-   *encoder.&lt;chan&gt;.update-counter.tmax (s32, RW)*

### Fonctions

Le composant exporte deux fonctions. Chaque fonction agit sur tous les compteurs de codeur, lancer différents compteurs de codeur dans différents threads n’est pas supporté.

-   *(funct) encoder.update-counters* — Fonction haute vitesse de comptage d’impulsions (non flottant).

-   *(funct) encoder.capture-position* — Fonction basse vitesse d’actualisation des latches et mise à l'échelle de la position.

PID
---

Ce composant fournit une boucle de contrôle Proportionnelle/Intégrale/Dérivée. C’est un composant temps réel uniquement. Par souci de simplicité, cette discussion suppose que nous parlons de boucles de position, mais ce composant peut aussi être utilisé pour implémenter d’autres boucles de rétroaction telles que vitesse, hauteur de torche, température, etc. La figure [ci-dessous](#fig:Diagramme-bloc-PID) est le schéma fonctionnel d’une simple boucle PID.

Diagramme bloc d’une boucle PID

![](images/pid-block-diag.png)

### L’installer

    halcmd: loadrt pid [num_chan=<loops>] [debug=1]

*&lt;loops&gt;* est le nombre de boucles PID à installer. Si *numchan* n’est pas spécifié, une seule boucle sera installée. Le nombre maximum de boucles est de 16 (comme définit par MAX\_CHAN dans pid.c). Chaque boucle est complétement indépendante. Dans les descriptions qui suivent, *&lt;loopnum&gt;* est le nombre de boucles spécifiques. La numérotation des boucle PID commence à 0.

Si *debug=1* est spécifié, le composant exporte quelques paramètres destinés au débogage et aux réglages. Par défaut, ces paramètres ne sont pas exportés, pour économiser la mémoire partagée et éviter d’encombrer la liste des paramètres.

### Le désinstaller

    halcmd: unloadrt pid

### Pins

Les trois principales pins sont:

-   *(float) pid.&lt;loopnum&gt;.command* — La position désirée (consigne), telle que commandée par un autre composant système.

-   *(float) pid.&lt;loopnum&gt;.feedback* — La position actuelle (mesure), telle que mesurée par un organe de rétroaction comme un codeur de position.

-   *(float) pid.&lt;loopnum&gt;.output* — Une commande de vitesse qui tend à aller de la position actuelle à la position désirée.

Pour une boucle de position, *command* et *feedback* sont en unités de longueur. Pour un axe linéaire, cela pourrait être des pouces, mm, mètres, ou tout autre unité pertinente. De même pour un axe angulaire, ils pourraient être des degrés, radians, etc. Les unités sur la pin *output* représentent l'écart nécessaire pour que la rétroaction coïncide avec la commande. Pour une boucle de position, *output* est une vitesse exprimée en pouces/seconde, mm/seconde, degrés/seconde, etc. Les unités de temps sont toujours des secondes et les unités de vitesses restent cohérentes avec les unités de longueur. Si la commande et la rétroaction sont en mètres, la sortie sera en mètres par seconde.

Chaque boucle PID a deux autres pins qui sont utilisées pour surveiller ou contrôler le fonctionnement général du composant.

-   *(float) pid.&lt;loopnum&gt;.error* — Egal à *.command* moins *.feedback*. (consigne - mesure)

-   *(bit) pid.&lt;loopnum&gt;.enable* — Un bit qui active la boucle. Si *.enable* est faux, tous les intégrateurs sont remis à zéro et les sorties sont forcées à zéro. Si *.enable* est vrai, la boucle opère normalement.

Pins utilisé pour signaler la saturation. La saturation se produit lorsque la sortie de le bloc PID est à son maximum ou limiter au minimum.

-   *(bit) pid.&lt;loopnum&gt;.saturated* — True lorsque la sortie est saturée.

-   *(float) pid.&lt;loopnum&gt;.saturated\_s* — Le temps de la sortie a été saturé.

-   *(s32) pid.&lt;loopnum&gt;.saturated\_count* — Le temps de la sortie a été saturé.

### Paramètres

Le gain PID, les limites et autres caractéristiques *accordables* de la boucle sont implémentés comme des paramètres.

-   *(float) pid.&lt;loopnum&gt;.Pgain* — Gain de la composante proportionnelle.

-   *(float) pid.&lt;loopnum&gt;.Igain* — Gain de la composante intégrale.

-   *(float) pid.&lt;loopnum&gt;.Dgain* — Gain de la composante dérivée.

-   *(float) pid.&lt;loopnum&gt;.bias* — Constante du décalage de sortie.

-   *(float) pid.&lt;loopnum&gt;.FF0* — Correcteur prédictif d’ordre zéro (retour vitesse) sortie proportionnelle à la commande (position).

-   *(float) pid.&lt;loopnum&gt;.FF1* — Correcteur prédictif de premier ordre (retour vitesse) sortie proportionnelle à la dérivée de la commande (vitesse).

-   *(float) pid.&lt;loopnum&gt;.FF2* — Correcteur prédictif de second ordre (retour vitesse) sortie proportionnelle à la dérivée seconde de la commande (accélération). <span class="footnote">
    \[FF2 n’est actuellement pas implémenté, mais il pourrait l'être. Considérez cette note comme un “FIXME” dans le code.\]
    </span>

-   *(float) pid.&lt;loopnum&gt;.deadband* — Définit la bande morte tolérable.

-   *(float) pid.&lt;loopnum&gt;.maxerror* — Limite d’erreur.

-   *(float) pid.&lt;loopnum&gt;.maxerrorI* — Limite d’erreur intégrale.

-   *(float) pid.&lt;loopnum&gt;.maxerrorD* — Limite d’erreur dérivée.

-   *(float) pid.&lt;loopnum&gt;.maxcmdD* — Limite de la commande dérivée.

-   *(float) pid.&lt;loopnum&gt;.maxcmdDD* — Limite de la commande dérivée seconde.

-   *(float) pid.&lt;loopnum&gt;.maxoutput* — Limite de la valeur de sortie.

Toutes les limites *max???,* sont implémentées de sorte que si la valeur de ce paramètre est à zéro, il n’y a pas de limite.

Si *debug=1* est spécifié quand le composant est installé, quatre paramètres supplémentaires seront exportés:

-   *(float) pid.&lt;loopnum&gt;.errorI* — Intégrale de l’erreur.

-   *(float) pid.&lt;loopnum&gt;.errorD* — Dérivée de l’erreur.

-   *(float) pid.&lt;loopnum&gt;.commandD* — Dérivée de la commande.

-   *(float) pid.&lt;loopnum&gt;.commandDD* — Dérivée seconde de la commande.

### Fonctions

Le composant exporte une fonction pour chaque boucle PID. Cette fonction exécute tous les calculs nécessaires à la boucle. Puisque chaque boucle a sa propre fonction, les différentes boucles peuvent être incluses dans les différents threads et exécutées à différents rythmes.

-   *(funct) pid.&lt;loopnum&gt;.do\_pid\_calcs* — Exécute tous les calculs d’une seule boucle PID.

Si vous voulez comprendre exactement l’algorithme utilisé pour calculer la sortie d’une boucle PID, référez vous à la figure [PID](#fig:Diagramme-bloc-PID), les commentaires au début du source *machinekit/src/hal/components/pid.c* et bien sûr, au code lui même. Les calculs de boucle sont dans la fonction C *calc\_pid()*.

Codeur simulé
-------------

Le codeur simulé est exactement la même chose qu’un codeur. Il produit des impulsions en quadrature avec une impulsion d’index, à une vitesse contrôlée par une pin de HAL. Surtout utile pour les essais.

### L’installer

    halcmd: loadrt sim-encoder num_chan=<number>

*&lt;number&gt;* est le nombre de canaux à simuler. Si rien n’est spécifié, un seul canal sera installé. Le nombre maximum de canaux est de 8 (comme défini par MAX\_CHAN dans sim\_encoder.c).

### Le désinstaller

    halcmd: unloadrt sim-encoder

### Pins

-   *(float) sim-encoder.&lt;chan-num&gt;.speed* — La vitesse commandée pour l’arbre simulé.

-   *(bit) sim-encoder.&lt;chan-num&gt;.phase-A* — Sortie en quadrature.

-   *(bit) sim-encoder.&lt;chan-num&gt;.phase-B* — Sortie en quadrature.

-   *(bit) sim-encoder.&lt;chan-num&gt;.phase-Z* — Sortie de l’impulsion d’index.

Quand *.speed* est positive, *.phase-A* mène *.phase-B*.

### Paramètres

-   *(u32) sim-encoder.&lt;chan-num&gt;.ppr* — Impulsions par tour d’arbre.

-   *(float) sim-encoder.&lt;chan-num&gt;.scale* — Facteur d'échelle pour *speed*. Par défaut est de 1.0, ce qui signifie que *speed* est en tours par seconde. Passer l'échelle à 60 pour des tours par minute, la passer à 360 pour des degrés par seconde, à 6.283185 pour des radians par seconde, etc.

Noter que les impulsions par tour ne sont pas identiques aux valeurs de comptage par tour (counts). Une impulsion est un cycle complet de quadrature. La plupart des codeurs comptent quatre fois pendant un cycle complet.

### Fonctions

Le composant exporte deux fonctions. Chaque fonction affecte tous les codeurs simulés.

-   *(funct) sim-encoder.make-pulses* — Fonction haute vitesse de génération d’impulsions en quadrature (non flottant).

-   *(funct) sim-encoder.update-speed* — Fonction basse vitesse de lecture de *speed*, de mise à l'échelle et d’activation de *make-pulses*.

Anti-rebond
-----------

L’anti-rebond est un composant temps réel capable de filtrer les rebonds créés par les contacts mécaniques. Il est également très utile dans d’autres applications, où des impulsions très courtes doivent être supprimées.

### L’installer

    halcmd: loadrt debounce cfg=<config-string>

*&lt;config-string&gt;* est une série d’entiers décimaux séparés par des espaces. Chaque chiffre installe un groupe de filtres anti-rebond identiques, le chiffre détermine le nombre de filtres dans le groupe. Par exemple:

    halcmd: loadrt debounce cfg=1,4,2

va installer trois groupes de filtres. Le groupe 0 contient un filtre, le groupe 1 en contient quatre et le groupe 2 en contient deux. La valeur par défaut de *&lt;config-string&gt;* est *1* qui installe un seul groupe contenant un seul filtre. Le nombre maximum de groupes est de 8 (comme définit par MAX\_GROUPS dans debounce.c). Le nombre maximum de filtres dans un groupe est limité seulement par l’espace de la mémoire partagée. Chaque groupe est complétement indépendant. Tous les filtres dans un même groupe sont identiques et ils sont tous mis à jour par la même fonction, au même instant. Dans les descriptions qui suivent, *&lt;G&gt;* est le numéro du groupe et *&lt;F&gt;* est le numéro du filtre dans le groupe. Le premier filtre est le filtre 0 dans le groupe 0.

### Le désinstaller

    halcmd: unloadrt debounce

### Pins

Chaque filtre individuel a deux pins.

-   *(bit) debounce.&lt;G&gt;.&lt;F&gt;.in* — Entrée du filtre *&lt;F&gt;* du groupe *&lt;G&gt;*.

-   *(bit) debounce.&lt;G&gt;.&lt;F&gt;.out* — Sortie du filtre *&lt;F&gt;* du groupe *&lt;G&gt;*.

### Paramètres

Chaque groupe de filtre a un paramètre. <span class="footnote">
\[Chaque filtre individuel a également une variable d'état interne. C’est un switch du compilateur qui peut exporter cette variable comme un paramètre. Ceci est prévu pour des essais et devrait juste être un gaspillage de mémoire partagée dans des circonstances normales.\]
</span>

-   *(s32) debounce.&lt;G&gt;.delay* — Délai de filtrage pour tous les filtres du groupe *&lt;G&gt;*.

Le délai du filtre est dans l’unité de la période du thread. Le délai minimum est de zéro. La sortie d’un filtre avec un délai de zéro, suit exactement son entrée, il ne filtre rien. Plus le délai augmente, plus larges seront les impulsions rejetées. Si le délai est de 4, toutes les impulsions égales ou inférieures à quatre périodes du thread, seront rejetées.

### Fonctions

Chaque groupe de filtres exporte une fonction qui met à jour tous les filtres de ce groupe *simultanément*. Différents groupes de filtres peuvent être mis à jour dans différents threads et à différentes périodes.

-   *(funct) debounce.&lt;G&gt;* — Met à jour tous les filtres du groupe *&lt;G&gt;*.

Siggen
------

Siggen est un composant temps réel qui génère des signaux carrés, triangulaires et sinusoïdaux. Il est principalement utilisé pour les essais.

### L’installer

    halcmd: loadrt siggen [num_chan=<chans>]

*&lt;chans&gt;* est le nombre de générateurs de signaux à installer. Si *numchan* n’est pas spécifié, un seul générateur de signaux sera installé. Le nombre maximum de générateurs est de 16 (comme définit par MAX\_CHAN dans siggen.c). Chaque générateur est complétement indépendant. Dans les descriptions qui suivent, *&lt;chan&gt;* est le numéro d’un générateur spécifique. Les numéros de générateur commencent à 0.

### Le désinstaller

    halcmd: unloadrt siggen

### Pins

Chaque générateur a cinq pins de sortie.

-   *(float) siggen.&lt;chan&gt;.sine* — Sortie de l’onde sinusoïdale.

-   *(float) siggen.&lt;chan&gt;.cosine* — Sortie de l’onde cosinusoïdale.

-   *(float) siggen.&lt;chan&gt;.sawtooth* — Sortie de l’onde en dents de scie.

-   *(float) siggen.&lt;chan&gt;.triangle* — Sortie de l’onde triangulaire.

-   *(float) siggen.&lt;chan&gt;.square* — Sortie de l’onde carrée.

Les cinq sorties ont les mêmes fréquence, amplitude et offset.

Trois pins de contrôle s’ajoutent aux pins de sortie:

-   *(float) siggen.&lt;chan&gt;.frequency* — Réglage de la fréquence en Hertz, par défaut la valeur est de 1 Hz.

-   *(float) siggen.&lt;chan&gt;.amplitude* — Réglage de l’amplitude de pic des signaux de sortie, par défaut, est à 1.

-   *(float) siggen.&lt;chan&gt;.offset* — Réglage de la composante continue des signaux de sortie, par défaut, est à 0.

Par exemple, si *siggen.0.amplitude* est à 1.0 et *siggen.0.offset* est à 0.0, les sorties oscilleront entre -1.0 et +1.0. Si *siggen.0.amplitude* est à 2.5 et *siggen.0.offset* est à 10.0, les sorties oscilleront entre 7.5 et 12.5.

### Paramètres

Aucun. <span class="footnote">
\[Dans les versions antérieures à la 2.1, fréquence, amplitude et offset étaient des paramètres. Ils ont été modifiés en pins pour permettre le contrôle par d’autres composants.\]
</span>

### Fonctions

-   *(funct) siggen.&lt;chan&gt;.update* — Calcule les nouvelles valeurs pour les cinq sorties.

lut5
----

Le composant lut5 est un composant de logique à 5 entrées basé sur une table de vérité.

-   *lut5* ne requiert pas un thread à virgule flottante.

Installation

    loadrt lut5 [count=N|names=name1[,name2...]]
    addf lut5.N servo-thread | base-thread
    setp lut5.N.function 0xN

Calcul de la valeur de la fonction

Pour calculer la valeur hexadécimale de la fonction, démarrer par le haut et entrer un 1 où un 0 pour indiquer si cette colonne devra être vraie où fausse. Ensuite écrire les valeurs en dessous, d’abord dans la colonne de sortie en commençant par le haut puis en écrivant les valeurs correspondantes de la droite vers la gauche. Le nombre binaire sera celui contenu dans la colonne de sortie. Utiliser une calculette comme celle fournie sous Debian, entrer ce nombre binaire et le convertir en hexadécimal pour obtenir la valeur pour la fonction.

<table>
<caption>Tableau 1. Table de vérité</caption>
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
<th align="left">Bit 4</th>
<th align="left">Bit 3</th>
<th align="left">Bit 2</th>
<th align="left">Bit 1</th>
<th align="left">Bit 0</th>
<th align="left">Output</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

Un exemple de fonction.

Dans la table suivante nous avons sélectionné l'état de sortie pour chaque ligne que nous souhaitons vraie.

<table>
<caption>Tableau 2. Table de vérité</caption>
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
<th align="left">Bit 4</th>
<th align="left">Bit 3</th>
<th align="left">Bit 2</th>
<th align="left">Bit 1</th>
<th align="left">Bit 0</th>
<th align="left">Output</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
</tr>
</tbody>
</table>

En regardant la colonne de sortie de notre exemple, nous voulons que la sortie soit active quand le bit 0 OU le bit 0 ET le bit 1 soient actifs et rien d’autre. Le nombre binaire est *b1010* (rotation de la sortie de 90° en sens horaire). Entrer ce nombre dans une calculette, le convertir en hexadécimal et le nombre demandé pour cette fonction est *0xa*. Le préfixe *0x* étant celui des nombres hexadécimaux.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


