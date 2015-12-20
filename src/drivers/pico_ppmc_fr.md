Pico PPMC
=========

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:pico-drivers"></span>

Le *Pico Systems* est une famille de cartes pour contrôler les servos analogiques, les moteurs pas à pas et les servos numériques pilotés en PWM. Les cartes se connectent sur le PC par le port parallèle configuré en mode EPP. Bien que la plupart des utilisateurs ne connectent qu’une seule carte par port parallèle, en théorie toutes les combinaisons de cartes entre 8 et 16 peuvent être utilisées sur un seul port parallèle. Un pilote servant pour tous les types de cartes. La combinaison finale d’entrées/sorties dépends du nombre de cartes installées. Le pilote ne distingue pas entre les cartes, il s’agit simplement d’un numéro de canal d’entrées/sorties (codeur, etc) commençant à 0 sur la première carte.

Installation:

        loadrt hal_ppmc port_addr=<addr1>[,<addr2>[,<addr3>...]]

Le paramètres *port\_addr* indique au pilote quel port parallèle utiliser. Par défaut, *&lt;addr1&gt;* est en 0x0378, *&lt;addr2&gt;* et les suivantes ne sont pas utilisées. Le pilote cherche sur l’espace entier de l’adresse du port parallèle étendu (EPP) indiquée par *port\_addr*, scrutant pour toute carte(s) de la famille PPMC. Il exporte ensuite les pins de HAL de tout ce qu’il a trouvé. Durant le chargement, ou la tentative de chargement, le pilote affiche tous les messages de débogage utiles dans le log du noyau, qui pourra être visualisé avec *dmesg*.

Un maximum de 3 bus parport peuvent êtres utilisés, et chaque bus peut recevoir un maximum de 8 périphériques.

Pins
----

Dans ce qui suit, pour les pins, les paramètres et les fonctions, &lt;board&gt; représente l’ID de la carte. Selon nos conventions de nommage, la première carte devrait toujours avoir l’ID zéro. Toutefois, le driver fixera l’ID en se basant sur les switches de la carte, de sorte qu’il peut être différent de zéro même si il n’y a qu’une seule carte.

 *(All s32 output) ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.count*   
Position codeur, en nombre de top comptés.

 *(all s32 output) ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.delta*   
Différence de top comptés depuis la dernière lecture, en unités brutes de comptage codeur.

 *(All float output) ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.velocity*   
Vitesse mise à l'échelle en unités utilisateur par seconde. Sur PPMC et USC ces valeurs sont dérivées du nombre de top codeur par période servo, elle est donc affectée par la granularité du codeur. Sur les cartes UPC avec les micro-logiciels du 21/08/09 et suivants, la vitesse est estimée par timestamping sur le comptage du codeur, ce qui peut être utilisé pour accroitre la finesse de cette sortie vitesse. Cela peut être régulé par un composant PID de HAL pour produire une réponse servo plus stable. Cette fonction doit être validée dans la ligne de commande HAL qui démarre le pilote PPMC, avec une option *timestamp=0x00*.

 *(All float output) ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.position*   
Position codeur, en unités utilisateur.

 *(All bit bidir) ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.index-enable*   
Connecte l’index *axis.\#.index-enable* pour *home-to-index*. C’et un signal de HAL bi-directionnel. Le fixer à TRUE, causera une remise à zéro hardware du codeur sur la prochaine impulsion d’index du codeur. Le pilote détectera cela et remettra le signal sur FALSE.

 *(UPC bit input) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.enable*   
Active un générateur de PWM.

 *(UPC float input) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.value*   
Valeur qui détermine le rapport cyclique de l’onde PWM. La valeur est divisée par *pwm.&lt;channel&gt;.scale*, par exemple, si le résultat est 0.6, le rapport cyclique sera de 60%, et ainsi de suite. Les valeurs de rapport cyclique négatives finiront en valeurs absolues, la pin de direction sera positionnée pour indiquer ce négatif.

 *(USC bit input) ppmc.&lt;port&gt;.stepgen.&lt;channel&gt;.enable*   
Active un générateur d’impulsions de pas.

 *(USC float input) ppmc.&lt;port&gt;.stepgen.&lt;channel&gt;.velocity*   
Valeur qui détermine la fréquence des pas. La valeur est multipliée par *stepgen.&lt;channel&gt;.scale* et le résultat est la fréquence, en pas par seconde. Des valeurs négatives résultera une fréquence basée sur une valeur absolue, la pin de direction sera positionnée pour indiquer ce négatif.

 *(All bit output) ppmc.&lt;port&gt;.in-&lt;channel&gt;*   
État d’une pin d’entrée numérique, voir l’entrée numérique canonique.

 *(All bit input) ppmc.&lt;port&gt;.in.&lt;channel&gt;-not*   
État inversé d’une pin d’entrée numérique, voir l’entrée numérique canonique.

 *(All bit output) ppmc.&lt;port&gt;.out-&lt;channel&gt;*   
Valeur à écrire sur une sortie numérique, voir la sortie numérique canonique.

 *(Option float output) ppmc.&lt;port&gt;.DAC8-&lt;channel&gt;.value*   
Valeur à écrire sur une sortie analogique, étendue entre 0 et 255. Ce qui envoie 8 bits de sortie sur J8, sur lequel doit être connectée une carte DAC de broche. 0 corresponds à zéro Volts, 255 corresponds à 10 Volts. La polarité de la sortie peut être fixée toujours négative, toujours positive, ou elle peut être contrôlée par l'état de SSR1 (positive quand *on*) et SSR2 (négative quand *on*). Vous devez spécifier *extradac = 0x00* sur la ligne de commande HAL qui charge le pilote PPMC pour valider cette fonction sur la première carte USC ou UPC.

 *(Option bit input) ppmc.&lt;port&gt;.dout-&lt;channel&gt;.out*   
Valeur à écrire sur une des 8 pins de sorties extra numériques de J8. Vous devez spécifier *extradout = 0x00* sur la ligne de commande HAL qui charge le pilote PPMC pour valider cette fonction sur la première carte USC ou UPC. *extradac* et *extradout* sont des caractéristiques mutuellement exclusives comme elles utilisent les mêmes lignes de signal à des fins différentes.Ces pins de sortie seront énumérées après les sorties numériques standards de la carte.

Paramètres
----------

 *(All float) ppmc.&lt;port&gt;.enc.&lt;channel&gt;.scale*   
Nombre de tops codeur par unité utilisateur (pour les conversions depuis le nombre d’unités).

 *(UPC float) ppmc.&lt;port&gt;.pwm.&lt;channel-range&gt;.freq*   
Fréquence porteuse de la PWM, en Hz. S’applique à un groupe de quatre générateurs de PWM consécutifs, indiqués par *&lt;channel-range&gt;*. Le minimum est de 610Hz, le maximum est de 500KHz.

 *(UPC float) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.scale*   
Échelle pour générateur de PWM. Si *scale* vaut X, alors le rapport cyclique sera de 100% quand *value* de la pin vaudra X (ou -X).

 *(UPC float) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.max-dc*   
Rapport cyclique maximum, compris entre 0.0 et 1.0.

 *(UPC float) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.min-dc*   
Rapport cyclique minimum, compris entre 0.0 et 1.0.

 *(UPC float) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.duty-cycle*   
Rapport cyclique actuel (utilisé surtout pour la maintenance)

 *(UPC bit) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.bootstrap*   
Si true, le générateur de PWM générera une courte séquence d’impulsions dans les deux polarités quand l’Arrêt d’Urgence sera activé, pour charger les capacités de bootstrap utilisées par certains pilotes à portes MOSFET.

 *(USC u32) ppmc.&lt;port&gt;.stepgen.&lt;channel-range&gt;.setup-time*   
Fixe le temps minimum, entre l’impulsion de changement de direction et l’impulsion de pas, en unités de 100ns. S’applique à un groupe de quatre générateurs de PWM consécutifs, comme indiqué par *&lt;channel-range&gt;*.

 *(USC u32) ppmc.&lt;port&gt;.stepgen.&lt;channel-range&gt;.pulse-width*   
Fixe la largeur des impulsions de pas, en unité de 100ns. S’applique à un groupe de quatre générateurs de PWM consécutifs, comme indiqué par *&lt;channel-range&gt;*.

 *(USC u32) ppmc.&lt;port&gt;.stepgen.&lt;channel-range&gt;.pulse-space-min*   
Fixe le temps minimum entre les impulsions, en unité de 100ns. S’applique à un groupe de quatre générateurs de PWM consécutifs, comme indiqué par *&lt;channel-range&gt;*. Le ratio maximum est: **`1 / (100ns * (pulse-width + pulse-space-min))`**

 *(USC float) ppmc.&lt;port&gt;.stepgen.&lt;channel&gt;.scale*   
Échelle pour générateur d’impulsions de pas. La fréquence des pas est en Hz, c’est la valeur absolue de *vitesse* \* *échelle*.

 *(USC float) ppmc.&lt;port&gt;.stepgen.&lt;channel&gt;.max-vel*   
La valeur maximum de *velocity*. Les consignes supérieures à *max-vel*, lui seront clampées. S’applique également aux valeurs négatives. (La valeur absolue est clampée.)

 *(USC float) ppmc.&lt;port&gt;.stepgen.&lt;channel&gt;.frequency*   
Fréquence de pas actuelle en Hz (utilisé principalement pour la maintenance)

 *(Option float) ppmc.&lt;port&gt;.DAC8.&lt;channel&gt;.scale*   
Fixe l'échelle d’une sortie extra DAC, de sorte qu’une valeur de sortie égale à l'échelle fournisse une amplitude de sortie de 10.0 V. (Le signe de la sortie est fixé par cavaliers et/ou une autre sortie numérique)

 *(Option bit) ppmc.&lt;port&gt;.out.&lt;channel&gt;-invert*   
Inverse une sortie numérique, voir la sortie numérique canonique.

 *(Option bit) ppmc.&lt;port&gt;.dout.&lt;channel&gt;-invert*   
Inverse une sortie numérique de J8, voir la sortie numérique canonique.

Fonctions
---------

 *(All funct) ppmc.&lt;port&gt;.read*   
Lit toutes les entrées (entrées numériques et top de codeurs) sur un port. Ces lectures sont organisées par blocs de registres contigus, pour éviter au maximum de charger le CPU.

 *(All funct) ppmc.&lt;port&gt;.write*   
Écrit toutes les sorties (sorties numériques, générateurs de pas et de PWM) sur un port. Ces lectures sont organisées par blocs de registres contigus, pour éviter au maximum de charger le CPU.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


