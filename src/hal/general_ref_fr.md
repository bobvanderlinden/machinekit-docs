Conventions générales
=====================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:References-generales"></span>

Les noms
--------

Toutes les entités de HAL sont accessibles et manipulées par leurs noms, donc, documenter les noms des pins, signaux, paramètres, etc, est très important. Les noms dans HAL ont un maximum de 41 caractères de long (comme défini par HAL\_NAME\_LEN dans hal.h). De nombreux noms seront présentés dans la forme générale, avec un texte mis en forme *&lt;comme-cela&gt;* représentant les champs de valeurs diverses.

Quand les pins, signaux, ou paramètres sont décrits pour la première fois, leur nom sera précédé par leur type entre parenthèses (*float*) et suivi d’une brève description. Les définitions typiques de pins ressemblent à ces exemples:

 (bit) parport.&lt;portnum&gt;.pin-&lt;pinnum&gt;-in   
La HAL pin associée avec la broche physique d’entrée *&lt;pinnum&gt;* du connecteur db25.

 (float) pid.&lt;loopnum&gt;.output   
La sortie de la boucle PID.

De temps en temps, une version abrégée du nom peut être utilisée, par exemple la deuxième pin ci-dessus pourrait être appelée simplement avec *.output* quand cela peut être fait sans prêter à confusion.

Conventions générales de nommage<span id="sec:GR-Conventions-nommage"></span>
-----------------------------------------------------------------------------

Le but des conventions de nommage est de rendre l’utilisation de HAL plus facile. Par exemple, si plusieurs interfaces de codeur fournissent le même jeu de pins et qu’elles sont nommées de la même façon, il serait facile de changer l’interface d’un codeur à un autre. Malheureusement, comme tout projet open-source, HAL est la combinaison de choses diversement conçues et comme les choses simples évoluent. Il en résulte de nombreuses incohérences. Cette section vise à remédier à ce problème en définissant certaines conventions, mais il faudra certainement un certain temps avant que tous les modules soient convertis pour les suivre.

Halcmd et d’autres utilitaires HAL de bas niveau, traitent les noms HAL comme de simples entités, sans structure. Toutefois, la plupart des modules ont une certaine structure implicite. Par exemple, une carte fournit plusieurs blocs fonctionnels, chaque bloc peut avoir plusieurs canaux et chaque canal, une ou plusieurs broches. La structure qui en résulte ressemble à une arborescence de répertoires. Même si halcmd ne reconnait pas la structure arborescente, la convention de nommage est un bon choix, elles lui permettra de regrouper ensemble, les items du même groupe, car il trie les noms. En outre, les outils de haut niveau peuvent être conçus pour reconnaitre de telles structures si les noms fournissent les informations nécessaires. Pour cela, tous les modules de HAL devraient suivrent les règles suivantes:

-   Les points (*.*) séparent les niveaux hiérarchiques. C’est analogue à la barre de fraction (*/*) dans les noms de fichiers.

-   Le tiret (*-*) sépare les mots ou les champs dans la même hiérarchie.

-   Les modules HAL ne doivent pas utiliser le caractère souligné ou les casses mélangées. <span class="footnote">
    \[Les caractères souslignés ont été enlevés, mais il reste quelques cas de mélange de casses, par exemple *pid.0.Pgain* au lieux de *pid.0.p-gain*.\]
    </span>

-   Utiliser seulement des caractères minuscules, lettres et chiffres.

Conventions de nommage des pilotes de matériels<span id="sec:GR-Nommage-pilotes-materiel"></span>
-------------------------------------------------------------------------------------------------

<span class="footnote">
\[La plupart des pilotes ne suivent pas ces conventions dans la version 2.0. Ce chapitre est réellement un guide pour les développements futurs.\]
</span>

### Noms de pin/paramètre

Les pilotes matériels devraient utiliser cinq champs (sur trois niveaux) pour obtenir un nom de pin ou de paramètre, comme le suivant:

    <device-name>.<device-num>.<io-type>.<chan-num>.<specific-name>

Les champs individuels sont:

 &lt;device-name&gt;   
Le matériel avec lequel le pilote est sensé travailler. Il s’agit le plus souvent d’une carte d’interface d’un certain type, mais il existe d’autres possibilités.

 &lt;device-num&gt;   
Il est possible d’installer plusieurs cartes servo, ports parallèles ou autre périphérique matériel dans un ordinateur. Le numéro du périphérique identifie un périphérique spécifique. Les numéros de périphériques commencent à 0 et s’incrémentent.<span class="footnote">
\[Certains matériels utilisent des cavaliers ou d’autres dispositifs pour définir une identification spécifique à chacun. Idéalement, le pilote fournit une manière à l’utilisateur de dire, le *device-num 0 est spécifique au périphérique qui a l’ID XXX*, ses sous-ensembles porterons tous un numéro commençant par 0. Mais à l’heure actuelle, certains pilotes utilisent l’ID directement comme numéro de périphérique. Ce qui signifie qu’il est possible d’avoir un périphérique Numéro 2, sans en avoir en Numéro 0. C’est un bug qui devrait disparaître en version 2.1.\]
</span>

 &lt;io-type&gt;   
La plupart des périphériques fournissent plus d’un type d’I/O. Même le simple port parallèle a, à la fois plusieurs entrées et plusieurs sorties numériques. Les cartes commerciales plus complexes peuvent avoir des entrées et des sorties numériques, des compteurs de codeurs, des générateurs d’impulsions de pas ou de PWM, des convertisseurs numérique/analogique, analogique/numérique et d’autres possibilités plus spécifiques. Le *I/O type* est utilisé pour identifier le type d’I/O avec lequel la pin ou le paramètre est associé. Idéalement, les pilotes qui implémentent les mêmes type d’I/O, même sur des dispositifs très différents, devraient fournir un jeu de pins et de paramètres cohérents et de comportements identiques. Par exemple, toutes les entrées numériques doivent se comporter de la même manière quand elles sont vues de l’intérieur de HAL, indépendamment du périphérique.

 &lt;chan-num&gt;   
Quasiment tous les périphériques d’I/O ont plusieurs canaux, le numéro de canal *chan-num* identifie un de ceux ci. Comme les numéros de périphériques *device-num*, les numéros de canaux, *chan-num*, commencent à zéro et s’incrémentent.<span class="footnote">
\[Une exception à la règle du *numéro de canal commençant à zéro* est le port parallèle. Ses *HAL pins* sont numérotées avec le numéro de la broche correspondante du connecteur DB-25. C’est plus pratique pour le câblage, mais non cohérent avec les autres pilotes. Il y a débat pour savoir si c’est un bogue ou une fonctionnalité.\]
</span> Si plusieurs périphériques sont installés, les numéro de canaux des périphériques supplémentaires recommencent à zéro. Comme il est possible d’avoir un numéro de canal supérieur à 9, les numéros de canaux doivent avoir deux chiffres, avec un zéro en tête pour les nombres inférieur à 10 pour préserver l’ordre des tris. Certains modules ont des pins et/ou des paramètres qui affectent plusieurs canaux. Par exemple un générateur de PWM peut avoir quatre canaux avec quatre entrées *duty-cycle* indépendantes, mais un seul paramètre *frequency* qui contrôle les quatres canaux (à cause de limitations matérielles). Le paramètre *frequency* doit utiliser les numéros de canaux de *00-03*.

 &lt;specific-name&gt;   
Un canal individuel d’I/O peut avoir une seule HAL pin associée avec lui, mais la plupart en ont plus. Par exemple, une entrée numérique a deux pins, une qui est l'état de la broche physique, l’autre qui est la même chose mais inversée. Cela permet au configurateur de choisir entre les deux états de l’entrée, active haute ou active basse. Pour la plupart des types d' entrée/sortie, il existe un jeu standard de broches et de paramètres, (appelé l'*interface canonique*) que le pilote doit implémenter. Les interfaces canoniques sont décrites [dans ce chapitre qui leur est dédié](#sec:Peripheriques-canoniques).

#### Exemples

 motenc.0.encoder.2.position   
La sortie position du troisième canal codeur sur la première carte Motenc.

 stg.0.din.03.in   
L'état de la quatrième entrée numérique sur la première carte Servo-to-Go.

 ppmc.0.pwm.00-03.frequency   
La fréquence porteuse utilisée sur les canaux PWM de 0 à 3 sur la première carte Pico Systems ppmc.

### Noms des fonctions

Les pilotes matériels ont généralement seulement deux types de fonctions HAL, une qui lit l'état du matériel et met à jour les pins HAL, l’autre qui écrit sur le matériel en utilisant les données fournies sur les pins HAL. Ce qui devrait être nommé de la façon suivante:

    <device-name>-<device-num>.<io-type>-<chan-num-range>.read|write

 &lt;device-name&gt;   
Le même que celui utilisé pour les pins et les paramètres.

 &lt;device-num&gt;   
Le périphérique spécifique auquel la fonction aura accès.

 &lt;io-type&gt;   
Optionnel. Une fonction peut accéder à toutes les d’entrées/sorties d’une carte ou, elle peut accéder seulement à un certain type. Par exemple, il peut y avoir des fonctions indépendantes pour lire les compteurs de codeurs et lire les entrées/sorties numériques. Si de telles fonctions indépendantes existent, le champ &lt;io-type&gt; identifie le type d’I/O auxquelles elles auront accès. Si une simple fonction lit toutes les entrés/sorties fournies par la carte, &lt;io-type&gt; n’est pas utilisé.<span class="footnote">
\[Note aux programmeurs de pilotes: ne PAS implémenter des fonctions séparées pour différents types d’I/O à moins qu’elles ne soient interruptibles et puissent marcher dans des threads indépendants. Si l’interruption de la lecture d’un codeur pour lire des entrées numériques, puis reprendre la lecture du codeur peut poser problème, alors implémentez une fonction unique qui fera tout.\]
</span>

 &lt;chan-num-range&gt;   
Optionnel. Utilisé seulement si l’entrée/sortie &lt;io-type&gt; est cassée dans des groupes et est accédée par différentes fonctions.

 read|write   
Indique si la fonction lit le matériel ou lui écrit.

#### Exemples

 motenc.0.encoder.read   
Lit tous les codeurs sur la première carte motenc.

 generic8255.0.din.09-15.read   
Lit le deuxième port 8 bits sur la première carte d’entrées/sorties à base de 8255.

 ppmc.0.write   
Écrit toutes les sorties (générateur de pas, pwm, DAC et ADC) sur la première carte Pico Systems ppmc.

Périphériques d’interfaces canoniques
-------------------------------------

Les sections qui suivent expliquent les pins, paramètres et fonctions qui sont fournies par les *périphériques canoniques*. Tous les pilotes de périphériques HAL devraient fournir les mêmes pins et paramètres et implémenter les mêmes comportements.

Noter que seuls les champs *&lt;io-type&gt;* et *&lt;specific-name&gt;* sont définis pour un périphérique canonique. Les champs *&lt;device-name&gt;*, *&lt;device-num&gt;* et *&lt;chan-num&gt;* sont définis en fonction des caractéristiques du périphérique réel.

Entrée numérique (Digital Input)<span id="sec:CanonDigIn"></span>
-----------------------------------------------------------------

L’entrée numérique canonique (I/O type: *digin*) est assez simple.

### Pins

 (bit) *in*   
État de l’entrée matérielle.

 (bit) *in-not*   
État inversé de l’entrée matérielle.

### Paramètres

Aucun

### Fonctions

 (funct) *read*   
Lire le matériel et ajuster les HAL pins *in* et *in-not*.

Sortie numérique (Digital Output)<span id="sec:CanonDigOut"></span>
-------------------------------------------------------------------

La sortie numérique canonique est également très simple (I/O type: *digout*).

### Pins

 (bit) *out*   
Valeur à écrire (éventuellement inversée) sur une sortie matérielle.

### Paramètres

 (bit) *invert*   
Si TRUE, *out* est inversée avant écriture sur la sortie matérielle.

### Fonctions

 (funct) *write*   
Lit *out* et *invert* et ajuste la sortie en conséquence.

Entrée analogique (Analog Input)
--------------------------------

L’entrée analogique canonique (I/O type: *adcin* ). Devrait être utilisée pour les convertisseurs analogiques/numériques, qui convertissent par exemple, les tensions en une échelle continue de valeurs.

### Pins

 (float) *value*   
Lecture du matériel, avec mise à l'échelle ajustée par les paramètres *scale* et *offset*. *Value* = ((lecture entrée, en unités dépendantes du matériel) x *scale*) - *offset*

### Paramètres

 (float) *scale*   
La tension d’entrée (ou l’intensité) sera multipliée par *scale* avant d'être placée dans *value*.

 (float) *offset*   
Sera soustrait à la tension d’entrée (ou l’intensité) après que la mise à l'échelle par scale ait été appliquée.

 (float) *bit\_weight*   
Valeur du bit le moins significatif (LSB). C’est effectivement, la granularité de lecture en entrée.

 (float) *hw\_offset*   
Valeur présente sur l’entrée quand aucune tension n’est appliquée sur la pin.

### Fonctions

 (funct) *read*   
Lit les valeurs de ce canal d’entrée analogique. Peut être utilisé pour lire un canal individuellement, ou pour lire tous les canaux à la fois.

Sortie analogique (Analog Output)
---------------------------------

La sortie analogique canonique (I/O Type: *adcout* ). Elle est destinée à tout type de matériel capable de sortir une échelle plus ou moins étendue de valeurs. Comme par exemple les convertisseurs numérique/analogique ou les générateurs de PWM.

### Pins

 (float) *value*   
La valeur à écrire. La valeur réelle sur la sortie matérielle dépends de la mise à l'échelle des paramètres d’offset.

 (bit) *enable*   
Si fausse, la sortie matérielle passera à 0, indépendamment de la pin *value*.

### Paramètres

 (float) *offset*   
Sera ajouté à *value* avant l’actualisation du matériel.

 (float) *scale*   
Doit être défini de sorte qu’une entrée avec 1 dans *value* produira 1V

 (float) *high\_limit*   
(optionnel) Quand la valeur en sortie matérielle est calculée, si *value* + *offset* est plus grande que *high\_limit*, alors *high\_limit* lui sera substitué.

 (float) *low\_limit*   
(optionnel) Quand la valeur en sortie matérielle est calculée, si *value* + *offset* est plus petite que *low\_limit*, alors *low\_limit* lui sera substitué.

 (float) *bit\_weight*   
(optionnel) La valeur du bit le moins significatif (LSB), en Volts (ou mA, pour les sorties courant)

 (float) *hw\_offset*   
(optionnel) La tension actuelle (ou l’intensité) présente sur la sortie quand 0 est écrit sur le matériel.

### Fonctions

 (funct) *write*   
Ecrit la valeur calculée sur la sortie matérielle. Si enable est FALSE, la sortie passera à 0, indépendamment des valeurs de *value*, *scale* et *offset* . La signification de *0* dépend du matériel. Par exemple, un convertisseur A/D 12 bits peut vouloir écrire 0x1FF (milieu d'échelle) alors que le convertisseur D/A reçoit 0 Volt de la broche matérielle. Si enable est TRUE, l'échelle, l’offset et la valeur sont traités et (*scale* \_ *value*) + *offset* sont envoyés à la sortie du DAC . Si enable est FALSE, la sortie passe à 0.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


