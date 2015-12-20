Les M-codes
===========

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:M-codes"></span>

Table des M-codes
-----------------

<table>
<colgroup>
<col width="28%" />
<col width="71%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Code</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="#sec:M0-M1">M0 M1</a></p></td>
<td align="left"><p>Pause dans le programme</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M2-M30">M2 M30</a></p></td>
<td align="left"><p>Fin de programme</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M60">M60</a></p></td>
<td align="left"><p>Pause pour déchargement pièce</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M3-M4-M5">M3 M4 M5</a></p></td>
<td align="left"><p>Contrôle de la broche</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M6-Appel-Outil">M6 Tn</a></p></td>
<td align="left"><p>Appel d’outil n=numéro d’outil</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M7-M8-M9">M7 M8 M9</a></p></td>
<td align="left"><p>Contrôle des arrosages</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M19">M19</a></p></td>
<td align="left"><p>Orientation de la broche</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M48-M49">M48 M49</a></p></td>
<td align="left"><p>Contrôle des correcteurs de vitesse</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M50-Controle-Correcteur-Vitesse-Travail">M50</a></p></td>
<td align="left"><p>Contrôle du correcteur de vitesse travail</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M51-Controle-Correcteur-Vitesse-Broche">M51</a></p></td>
<td align="left"><p>Contrôle du correcteur de vitesse de broche</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M52-Controle-Vitesse-Adaptative">M52</a></p></td>
<td align="left"><p>Correcteur dynamique de vitesse d’avance</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M53-Controle-Coupure-Vitesse">M53</a></p></td>
<td align="left"><p>Contrôle de la coupure de vitesse</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M61-Correction-Numero-Outil-Courant">M61</a></p></td>
<td align="left"><p>Correction du numéro de l’outil courant</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M62-a-M65-Ctrl-Sortie-Numerique">M62 à M65</a></p></td>
<td align="left"><p>Contrôle de sortie numérique</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M66-Ctrl-Entree-Numerique-Et-Analogique">M66</a></p></td>
<td align="left"><p>Contrôle d’entrée numérique et analogique</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M67-Ctrl-Sortie-Analogique-Synchro">M67</a></p></td>
<td align="left"><p>Contrôle sortie analogique synchronisée</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M68-Ctrl-Sortie-Analogique-Directe">M68</a></p></td>
<td align="left"><p>Contrôle sortie analogique directe</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M70-Save-Modal-State">M70</a></p></td>
<td align="left"><p>Enregistre l'état modal</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M71-Invalidate-Stored-Modal-State">M71</a></p></td>
<td align="left"><p>Invalide l'état modal enregistré</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M72-Restore-Modal-State">M72</a></p></td>
<td align="left"><p>Restaure l'état modal</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M73-Save-Autorestore-Modal-State">M73</a></p></td>
<td align="left"><p>Enregistrement/auto-restauration de l'état modal</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M100-a-M199">M100 à M199</a></p></td>
<td align="left"><p>M-codes définis par l’utilisateur</p></td>
</tr>
</tbody>
</table>

M0, M1, pause dans le programme
-------------------------------

-   *M0* - Effectue une pause temporaire dans le programme en cours (quelle que soit la position du bouton d’arrêt facultatif). Machinekit reste en mode automatique afin que le MDI ou d’autres actions manuelles ne puissent pas être activés. Presser le départ cycle après cette commande relance le programme à la ligne suivante.

-   *M1* - Stoppe temporairement le programme en cours (mais seulement si le bouton d’arrêt optionnel est activé). Machinekit reste en mode automatique afin que le MDI ou d’autres actions manuelles ne puissent pas être activés. Presser le départ cycle après cette commande relance le programme à la ligne suivante.

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
<td align="left">Il est permis de programmer <em>M0</em> et <em>M1</em> en mode données manuelles (MDI), mais l’effet ne sera probablement pas perceptible, puisque le comportement normal en mode MDI est de s’arrêter, de toute façon, à la fin de chaque ligne.</td>
</tr>
</tbody>
</table>

M2, M30, fin de programme
-------------------------

-   *M2* - Indique la fin du programme. Presser le départ cycle après cette commande relance le programme au début du fichier.

-   *M30* - Décharge le porte-pièce du chargeur et termine le programme. Presser le départ cycle après cette commande relance le programme au début du fichier.

Les deux commandes précédentes produisent les effets suivants:

1.  Changement du mode automatique au mode MDI.

2.  Les décalages d’axes sont mis aux valeurs par défaut (comme avec *G54*).

3.  Le plan de travail actif devient XY (comme avec *G17*).

4.  Le mode de déplacement devient absolu (comme avec *G90*).

5.  La vitesse travail passe en unités par minute (comme avec *G94*).

6.  Les correcteurs de vitesse sont activés (comme avec *M48*).

7.  Les compensations d’outil sont désactivées (comme avec *G40*).

8.  La broche est arrêtée (comme avec *M5*).

9.  Le mode mouvement courant devient *G1* (comme avec *G1*).

10. L’arrosage est arrêté (comme avec *M9*).

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
<td align="left">Les lignes de code placées après un <em>M2</em> ou un <em>M30</em> ne sont pas exécutées.</td>
</tr>
</tbody>
</table>

M60, pause pour déchargement pièce
----------------------------------

-   *M60* - Procède au changement de porte-pièce avec le chargeur de pièces et effectue une pause dans le programme en cours (quel que soit le réglage du bouton d’arrêt facultatif). Presser ensuite le bouton de départ cycle pour relancer le programme à la ligne suivante.

M3, M4, M5 Contrôle de la broche
--------------------------------

-   *M3 Snnnnn* - Démarre la broche en sens horaire à la vitesse **nnnnn**.

-   *M4 Snnnnn* - Démarre la broche en sens anti-horaire à la vitesse **nnnnn**.

-   *M5* - Arrête la rotation de la broche.

Il est permis d’utiliser *M3* ou *M4* si la vitesse de broche est à zéro. Si cela est fait (ou si le bouton du correcteur de vitesse est activé mais mis à zéro), la broche ne tournera pas. Si, plus tard la vitesse de broche est augmentée (ou que le correcteur de vitesse est augmenté), la broche va se mettre en rotation. Il est permis d’utiliser *M3* ou *M4* quand la broche est déjà en rotation ou d’utiliser *M5* quand la broche est déjà arrêtée.

M6 Appel d’outil
----------------

### Changement d’outil manuel

Si le composant de HAL, hal\_manualtoolchange est chargé, *M6* va arrêter la broche et inviter l’utilisateur à changer l’outil. Pour plus d’informations sur hal\_manualtoolchange voir la section [sur le changement manuel d’outil](#sec:Changement-D-Outil-Manuel).

### Changement d’outil

Pour changer l’outil, actuellement dans la broche, par un autre, nouvellement sélectionné en utilisant le mot T, voir la section [sur le choix de l’outil](#sec:T-Choix-Outil), programmer *M6*. Un changement d’outil complet donnera:

-   La rotation de la broche est arrêtée.

-   L’outil qui a été sélectionné (par le mot T sur la même ligne ou sur n’importe quelle ligne après le changement d’outil précédent), sera placé dans la broche. Le mot **T** est un nombre entier indiquant le numéro de poche d’outil dans le carrousel (non son index).

-   Si l’outil sélectionné n’est pas déjà dans la broche avant le changement d’outil, l’outil qui était dans la broche (s’il y en avait un) va être replacé dans son emplacement dans le chargeur.

-   Les coordonnées des axes seront arrêtées dans les mêmes positions absolues qu’elles avaient avant le changement d’outil (mais la broche devra peut-être être réorientée).

-   Aucune autre modification ne sera apportée. Par exemple, l’arrosage continue à couler durant le changement d’outil à moins qu’il ne soit arrêté par *M9*.

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
<td align="left">La longueur d’outil n’est pas modifié par <em>M6</em>, utilisez un <em>G43</em> après le <em>M6</em> pour changer la longueur d’outil.</td>
</tr>
</tbody>
</table>

Le changement d’outil peut inclure des mouvements d’axes pendant son exécution. Il est permis (mais pas utile) de programmer un changement d’outil avec le même outil que celui qui est déjà dans la broche. Il est permis également, si il n’y a pas d’outil dans le slot sélectionné, dans ce cas, la broche sera vide après le changement d’outil. Si le slot zéro a été le dernier sélectionné, il n’y aura pas d’outil dans la broche après le changement.

M7, M8, M9 Contrôle de l’arrosage
---------------------------------

-   *M7* - Active l’arrosage par gouttelettes.

-   *M8* - Active l’arrosage fluide.

-   *M9* - Arrête tous les arrosages.

Il est toujours permis d’utiliser une de ces commandes, que les arrosages soient arrêtés ou non.

M19 Orientation de la broche
----------------------------

-   *M19 R- Q- \[P-\]*

-   *R* - Position à atteindre à partir de 0, cette valeur doit être comprise entre 0 et 360 degrés.

-   *Q* - Durée d’attente en secondes pour compléter l’orientation. Si *motion.spindle.is\_oriented* n’est pas devenue vraie dans le temps imparti par Q, une erreur de timeout se produira.

-   *P* - Direction de rotation vers la position cible.

    -   *0* - rotation pour petit mouvement angulaire (défaut)

    -   *1* - rotation toujours en sens horaire (même direction qu’avec M3)

    -   *2* - rotation toujours en sens anti-horaire (même direction qu’avec M4)

M19 est révoqué par M3,M4 ou M5.

L’orientation de la broche nécessite un codeur de position avec index, indiquant la position de la broche ainsi que sa direction de rotation.

Paramètres de réglage de la section \[RS274NGC\].

ORIENT\_OFFSET = 0 à 360 (offset fixe en degrés, ajouté au mot R de M19)

Broches de HAL

-   *motion.spindle-orient-angle* (sortie float) Orientation souhaitée pour M19. Valeur du paramètre R de M19 plus la valeur du paramètre d’ini \[RS274NGC\]ORIENT\_OFFSET.

-   *motion.spindle-orient-mode* (sortie s32) Mode de rotation de la broche souhaité. Reflète le mot P de M19, Défaut = 0

-   *motion.spindle-orient* (sortie bit) Indique le début du cycle d’orientation de la broche. Positionné par M19. Remis à zéro par M3,M4 ou M5. Si *spindle-orient-fault* n’est pas à zéro alors que *spindle-orient* est vraie la commande M19 échoue avec un message d’erreur.

-   *motion.spindle-is-oriented* (entrée bit) Pin de confirmation de l’orientation de la broche. Termine le cycle d’orientation. Si *spindle-orient* est vraie quand *spindle-is-oriented* est activée, la pin *spindle-orient* est mise à zéro et la pin *spindle-locked* est activée. La pin *spindle-brake* est également activée.

-   *motion.spindle-orient-fault* (entrée s32) Entrée de code d’erreur pour le cycle d’orientation. Toute valeur, autre que zéro, provoquera l’abandon du cycle d’orientation.

-   *motion.spindle-locked* (sortie bit) Pin indiquant que le cycle de rotation est terminé. Désactivée par M3,M4 ou M5.

M48, M49 Contrôle des correcteurs de vitesse
--------------------------------------------

-   *M48* - Autorise les curseurs de corrections de vitesses de broche et celui de vitesse d’avance travail.

-   *M49* - Inhibe les deux curseurs.

Il est permis d’autoriser ou d’inhiber ces curseurs quand ils sont déjà autorisés ou inhibés. Ils peuvent aussi être activés individuellement en utilisant les commandes *M50* et *M51*, voir ci-dessous.

M50 Contrôle du correcteur de vitesse travail
---------------------------------------------

-   *M50 &lt;P1&gt;* - Autorise le curseur de correction de vitesse d’avance travail. Le paramètre *P1* est optionnel.

-   *M50 P0* - Inhibe le curseur de correction d’avance travail.

Quand il est inhibé, le curseur de correction de vitesse n’a plus aucune influence et les mouvements seront exécutés à la vitesse d’avance travail programmée. (à moins que ne soit actif un correcteur de vitesse adaptative).

M51 Contrôle du correcteur de vitesse broche
--------------------------------------------

-   *M51 &lt;P1&gt;* - Autorise le curseur de correction de vitesse de la broche. Le paramètre *P1* est optionnel.

-   *M51 P0* - Inhibe le curseur de correction de vitesse de broche.

Quand il est inhibé, le curseur de correction de vitesse de broche n’a plus aucune influence, et la broche tournera à la vitesse programmée, en utilisant le mot *S* comme décrit dans la section [sur le réglage de la vitesse de broche](#sec:S-Broche).

M52 Contrôle de vitesse adaptative
----------------------------------

-   *M52 P1* - Utilise une vitesse adaptative. Le paramètre *P1* est optionnel.

-   *M52 P0* - Cesse l’utilisation d’une vitesse adaptative.

Quand la vitesse adaptative est utilisée, certaines valeurs externes sont utilisées avec les correcteurs de vitesse de l’interface utilisateur et les vitesses programmées pour obtenir la vitesse travail. Dans Machinekit, la HAL pin *motion.adaptive-feed* est utilisée dans ce but. Les valeurs de *motion.adaptive-feed* doivent être dans comprises entre 0 (vitesse nulle) et 1 (pleine vitesse).

M53 Contrôle de la coupure de vitesse
-------------------------------------

-   *M53 P1* - Autorise le bouton de coupure de vitesse. Le paramètre *P1* est optionnel. Autoriser la coupure de vitesse permet d’interrompre les mouvements par le biais d’une coupure de vitesse. Dans Machinekit, la HAL pin *motion.feed-hold* est utilisée pour cette fonctionnalité. Une valeur de 1 provoque un arrêt des mouvements quand *M53* est actif.

-   *M53 P0* - Inhibe le bouton de coupure de vitesse. L'état de *motion.feed-hold* est sans effet sur la vitesse quand *M53* est inhibé.

M61 Correction du numéro de l’outil courant
-------------------------------------------

-   'M61 Q ' - Corrige le numéro de l’outil courant, en mode MDI ou après un changement manuel d’outil dans la fenêtre de données manuelles. Au démarrage de Machinekit avec un outil dans la broche, il est possible ainsi d’ajuster le numéro de l’outil courant sans faire de changement d’outil.

C’est une erreur si:

-   Q n’est pas égal où supérieur à 0

M62 à M65 Contrôle de bits de sortie numérique
----------------------------------------------

-   *M62 P* - Active un bit de sortie numérique en synchronisme avec un mouvement.

-   *M63 P* - Désactive un bit de sortie numérique en synchronisme avec un mouvement.

-   *M64 P* - Active immédiatement un bit de sortie numérique.

-   *M65 P* - Désactive immédiatement un bit de sortie numérique.

Le mot *P* spécifie le numéro du bit de sortie numérique. Le mot P doit être compris entre 0 et une valeur par défaut de 3. Si nécessaire, le nombre des entrées/sorties peut être augmenté en utilisant le paramètre *num\_dio* lors du chargement du contrôleur de mouvement. Voir le manuel de l’intégrateur et section "Machinekit et HAL", pour plus d’informations.

Les commandes *M62* et *M63* seront mises en file d’attente. Toute nouvelle commande, destinée à un bit de sortie écrasera l’ancien réglage de ce bit. Plusieurs bits peuvent changer d'état simultanément par l’envoi de plusieurs commandes M62/M63.

Les nouveaux changements d'état des bits de sortie spécifiés, seront effectifs au début du prochain mouvement commandé. S’il n’y a pas de commande de mouvement ultérieur, les changements en attente n’auront pas lieu. Il est préférable de toujours programmer un G-code de mouvement (G0, G1, etc) juste après les M62/63.

*M64* et *M65* produisent leur effet immédiatement après être reçus par le contrôleur de mouvement. Ils ne sont pas synchronisés avec un mouvement.

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
<td align="left">M62 à M66 ne seront opérationnels que si les pins <em>motion.digital-out-nn</em> appropriées sont connectées aux sorties dans le fichier HAL.</td>
</tr>
</tbody>
</table>

M66 Contrôle d’entrée numérique et analogique
---------------------------------------------

    M66 P- | E- <L-> <Q->

-   *P* - Spécifie le numéro d’un bit d’entrée numérique entre 0 et 3.

-   *E* - Spécifie le numéro d’un bit d’entrée analogique entre 0 et 3.

-   *L* - Spécifie le mode d’attente.

    -   Mode 0: *IMMEDIATE* - pas d’attente, retour immédiat, la valeur courante de l’entrée est stockée dans le paramètre \#5399

    -   Mode 1: *RISE* attente d’un front montant sur l’entrée.

    -   Mode 2: *FALL* attente d’un front descendant sur l’entrée.

    -   Mode 3: *HIGH* attente d’un état logique HAUT sur l’entrée.

    -   Mode 4: *LOW* attente d’un état logique BAS sur l’entrée.

-   *Q* - Spécifie le timeout pour l’attente, en secondes. Si le timeout est dépassé, l’attente est interrompue et la variable \#5399 positionnée à -1.

-   Le mode *0* est le seul autorisé pour une entrée analogique.

Exemple de ligne avec M66

    M66 P0 L3 Q5 (attend jusqu'à 5 secondes la montée de l'entrée numérique 0)

-   *M66* attend un nouvel événement sur une entrée ou la fin de l’exécution du programme, jusqu'à ce que l'événement sélectionné (ou le timeout programmé) ne survienne. C’est également une erreur de programmer *M66* avec les deux mots, un mot P- et un mot E- (ce qui reviendrait à sélectionner à la fois une entrée analogique et une numérique).

Si nécessaire, le nombre des entrées/sorties peut être augmenté en utilisant les paramètres *num\_dio* ou *num\_aio* lors du chargement du contrôleur de mouvement. Voir le Manuel de l’intégrateur pour plus d’informations, section des configurations, paragraphes "Machinekit et HAL".

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
<td align="left">M66 ne sera opérationnel que si les pins motion.digital-in-nn ou motion.analog-in-nn appropriées sont connectées aux entrées dans le fichier HAL.</td>
</tr>
</tbody>
</table>

M67 Contrôle de sortie analogique
---------------------------------

    M67 E- Q-

-   *M67* - Contrôle une sortie analogique synchronisée avec un mouvement.

-   *E* - Spécifie le numéro de la sortie, doit être compris entre 0 et 3.

-   *Q* - Spécifie la valeur à appliquer sur la sortie.

Les changements de valeur spécifiés, seront effectifs au début du prochain mouvement commandé. S’il n’y a pas de commande de mouvement ultérieur, les changements en attente n’auront pas lieu. Il est préférable de toujours programmer un G-code de mouvement (G0, G1, etc) juste après les M67. M67 fonctionne comme M62 à M63.

Le nombre d’entrées/sorties peut être augmenté en utilisant le paramètre *num\_aio* au chargement du contrôleur de mouvement. Voir les chapitres "Machinekit et HAL" dans la section configuration du Manuel de l’intégrateur pour plus d’informations sur le contrôleur de mouvement.

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
<td align="left">M67 ne sera opérationnel que si les pins motion.analog-out-nn appropriées sont connectées aux sorties dans le fichier HAL.</td>
</tr>
</tbody>
</table>

M68 Contrôle de sortie analogique directe
-----------------------------------------

    M68 E- Q-

-   *M68* - Contrôle directement une sortie analogique.

-   *E* - Spécifie le numéro de la sortie, doit être compris entre 0 et 3.

-   *Q* - Spécifie la valeur à appliquer sur la sortie.

M68 produit son effet immédiatement après être reçu par le contrôleur de mouvement. Il n’est pas synchronisé avec un mouvement. M68 fonctionne comme M64 à M65.

Le nombre d’entrées/sorties peut être augmenté en utilisant le paramètre *num\_aio* au chargement du contrôleur de mouvement. Voir le chapitre "Machinekit et HAL" dans le Manuel de l’intégrateur pour plus d’informations sur le contrôleur de mouvement.

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
<td align="left">M68 ne sera opérationnel que si les pins <em>motion.analog-out-nn</em> appropriées sont connectées aux sorties dans le fichier HAL.</td>
</tr>
</tbody>
</table>

M70 Enregistrement de l'état modal
----------------------------------

Pour enregistrer explicitement l'état modal au niveau de l’appel courant, programmer *M70*. Une fois l'état modal enregistré avec *M70*, il peut être restauré exactement dans le même état en exécutant un *M72*.

Une paire d’instructions *M70* et *M72* est typiquement utilisée pour protéger un programme contre d'éventuels changements modaux pouvant se produire dans les sous-programmes.

Les états enregistrés sont les suivants:

-   unités machine courantes G20/G21 (po/mm)

-   plan de travail courant (G17/G18/G19 G17.1,G18.1,G19.1)

-   statut de la compensation de rayon d’outil (G40,G41,G42,G41.1,G42,1)

-   mode de déplacement - relatif/absolu (G90/G91)

-   mode de vitesse (G93/G94,G95)

-   coordonnées système courantes (G54-G59.3)

-   statut de la compensation de longueur d’outil (G43,G43.1,G49)

-   options du plan de retrait (G98,G99)

-   mode de contrôle de broche (G96-css ou G97-RPM)

-   mode de déplacement en arc (G90.1, G91.1)

-   mode diamètre/rayon des tours (G7,G8)

-   mode de contrôle de trajectoire (G61, G61.1, G64)

-   avance et vitesse broche courantes (valeurs *F* et *S*)

-   statut de la broche (M3,M4,M5) - on/off et direction

-   statut de l’arrosage (M7) et (M8)

-   réglages des correcteurs de vitesse broche (M51) et du correcteur de vitesse travail (M50)

-   réglage du contrôle de vitesse adaptative (M52)

-   réglage du contrôle de la coupure de vitesse (M53)

Noter qu’en particulier, les modes de mouvement (G1 etc) ne sont *PAS* restaurés.

*Le niveau de l’appel courant* signifie:

-   Exécution dans le programme principal. Il n’y a qu’un seul emplacement de stockage pour l'état modal au niveau du programme principal; si plusieurs instructions *M70* sont exécutées tour à tour, seul l'état enregistré le plus récent est restauré quand un *M72* est exécuté.

-   Exécution dans un sous-programme G-code. L'état enregistré par *M70* dans un sous-programme se comporte exactement comme un paramètre nommé local - on ne peut s’y référer qu'à l’intérieur du sous-programme en invoquant un *M72*, à la sortie du sous-programme, le paramètre disparaît.

Une invocation récursive d’un sous-programme introduit un nouveau niveau d’appel.

M71 Invalidation de l'état modal enregistré
-------------------------------------------

[L'état modal enregistré par *M70*](#saved_state_by_M70) ou par [*M73*](#sec:M73-Save-Autorestore-Modal-State) au niveau de l’appel courant est invalidé (ne peut plus être restauré nulle part).

Un appel ultérieur à *M72* sur le même niveau d’appel, échouera.

Si il est exécuté dans un sous-programme qui protège l'état modal par un *M73*, un *return* ou *endsub* ultérieur ne restaurera *PAS* l'état modal.

L’utilité de ce dispositif est douteuse. Il ne devrait pas être invoqué quand il peut disparaître.

M72 Restauration de l'état modal
--------------------------------

[L'état modal enregistré par un *M70*](#saved_state_by_M70) peut être restauré en exécutant un *M72*.

La gestion de G20/G21 reçoit un traitement particulier car les avances sont interprétées différemment selon G20/G21: si les unités de longueur (mm/po) doivent être modifiées par une opération de restauration, *M72* va restaurer le mode distance en premier, puis ensuite tous les autres états, y compris les avances pour être sure que les valeurs d’avance soient interprétées selon un réglage d’unités correct.

C’est une erreur d’exécuter *M72* sans enregistrement précédent avec *M70* à ce niveau.

L’exemple suivant montre l’enregistrement puis la restauration de l'état modal autour de l’appel d’un sous-programme utilisant *M70* et *M72*. Noter que le sous-programme *imperialsub* n’est pas "au courant" des caractéristiques de M7x et peut être utilisé non modifié:

M73 Enregistrement et auto-restauration de l'état modal
-------------------------------------------------------

Pour enregistrer l'état modal à l’intérieur d’un sous-programme et restaurer cet état lors d’un *endsub* ou autre *return*, programmer *M73*.

En cas d’abandon d’un programme en cours d’exécution dans un sous-programme traitant un *M73*, l'état ne sera **PAS** restauré.

En outre, la fin normale (*M2*) d’un programme principal contenant un *M73* ne restaurera **pas** l'état.

L’utilisation suggérée consiste à placer au début d’un sous-programme, un O-code de sous-programme comme dans l’exemple ci-dessous. En utilisant *M73*, cette manière valide le design des sous-programmes qui doivent modifier l'état modal mais qui protège le programme appelant contre tout changement inopiné de l'état modal. Noter l’usage de [paramètres nommés](#sec:Parametres-Nommes) dans le sous-programme *showstate*.

Restauration sélective de l'état modal par le test de paramètres prédéfinis<span id="sec:Selectively-restoring-modal-state"></span>
-----------------------------------------------------------------------------------------------------------------------------------

Exécuter un *M72* ou au retour d’un sous-programme contenant un 'M73\_ pour restaurer [**tout** l'état modal enregistré](#saved_state_by_M70).

Si seulement certains aspects de l'état modal doivent être préservés, une alternative consiste a utiliser les [paramètres nommés prédéfinis](#sec:Predefined-Named-Parameters), paramètres locaux et états conditionnels. L’idée est de rappeler les modes à restaurer au début du sous-programme et de restaurer ceux-ci avant de quitter. Voici un exemple, basé sur le programme *nc\_files/tool-length-probe.ngc*:

M100 à M199 Commandes définies par l’utilisateur
------------------------------------------------

    M1-- <P- Q->

-   *M1 --* - Un entier compris entre 100 et 199.

-   *P* - Un nombre passé comme premier argument au programme externe.

-   *Q* - Un nombre passé comme second argument au programme externe.

Le programme externe, nommé *M100* à *M199*, (avec un *M* majuscule et aucune extension) qui doit se trouver dans le répertoire pointé par la variable *\[DISPLAY\] PROGRAM\_PREFIX* du fichier ini, sera exécuté avec les valeurs *P-* et *Q-* comme étant ses deux arguments. L’exécution du fichier G-code courant passera en pause jusqu'à ce que le programme invoqué soit terminé. Tout fichier exécutable valide peut être utilisé. Le fichier doit se trouver dans le chemin spécifié dans le fichier ini de configuration. Voir la section sur le fichier de configuration dans le manuel de l’intégrateur.

Après la création d’un nouveau programme M1nn, l’interface graphique doit être redémarrée pour que le nouveau programme soit pris en compte, autrement une erreur *M-code inconnu* surviendra.

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
<td align="left">Ne pas utiliser un traitement de texte pour créer ou éditer ces fichiers. Un traitement de texte ajoute des caractères invisibles qui causent des problèmes et empêchent les scripts bash ou Python de fonctionner. Pour ces raisons, utiliser un éditeur de texte tel que <em>Gedit</em> dans Debian ou le Notepad++ dans un autre OS.</td>
</tr>
</tbody>
</table>

Le message d’erreur *M-code inconnu* signifie que:

-   La commande utilisateur spécifiée n’existe pas.

-   Le fichier n’a pas été rendu exécutable.

-   Le nom du fichier comporte une extension.

-   Le nom du fichier ne suis pas le format suivant: M1nn où nn = 00 à 99.

-   Le nom de fichier utilise un *m* minuscule.

Exemple d’utilisation, dans un programme G-code, on doit ouvrir et fermer un mandrin automatique via une broche du port parallèle, on appellera respectivement M101 pour ouvrir le mandrin et M102 pour le fermer. Les deux scripts bash correspondants, appelés M101 et M102 seront créés avant le lancement de Machinekit puis rendus exécutables, par exemple par un clic droit puis *propriétés → permissions → Exécution*. S’assurer que cette broche du port parallèle n’est pas déjà utilisée dans un fichier de HAL.

Exemple de fichier pour M101

    #!/bin/bash
    # ce fichier met la broche 14 du port à 1 pour ouvrir le mandrin automatique
    halcmd setp parport.0.pin-14-out True
    exit 0

Exemple de fichier pour M102

    #!/bin/bash
    # ce fichier met la broche 14 du port à 0 pour fermer le mandrin automatique
    halcmd setp parport.0.pin-14-out False
    exit 0

Pour passer des variables à un fichier M1nn, utiliser les mots facultatifs P et Q de cette façon:

    M100 P123.456 Q321.654

Exemple pour M100

    #!/bin/bash
    tension=$1
    vitesse=$2
    halcmd setp thc.voltage $tension
    halcmd setp thc.feedrate $vitesse
    exit 0

Pour ouvrir un message graphique et passer en pause jusqu'à ce que la fenêtre du message soit fermée, utiliser un programme comme *Eye of Gnome* pour afficher le fichier graphique. Quand la fenêtre sera fermée, le programme reprendra.

Exemple pour M110, affichage d’un graphique avec passage en pause

    #!/bin/bash
    eog /home/robert/machinekit/nc_files/message.png
    exit 0

Pour afficher un message graphique en continuant le traitement du fichier G-code, ajouter un caractère esperluette à la commande.

Exemple pour M110, affichage d’un graphique sans passer en pause

    #!/bin/bash
    eog /home/robert/machinekit/nc_files/message.png &
    exit 0

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


