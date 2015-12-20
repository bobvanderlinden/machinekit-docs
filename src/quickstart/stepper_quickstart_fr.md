Configuration rapide pour moteurs pas à pas
===========================================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:stepper-quickstart"></span>

Cette section suppose qu’une installation du logiciel à partir du CD Live a été faite. Après cette installation et avant de continuer, il est recommandé de connecter le PC sur Internet pour y faire les dernières mises à jour. Pour les installations plus complexes se référer au Manuel de l’intégrateur.

Test de latence (Latency Test)
------------------------------

Le test de latence détermine la capacité du processeur à répondre aux requêtes qui lui sont faites. Certains matériels peuvent interrompre ce processus, causant des pertes de pas lorsque le PC pilote une machine CNC. Ce test est la toute première chose à faire pour valider un PC. Pour le lancer, suivre les instructions de la section [sur le test de latence](#cha:test-de-latence).

Sherline
--------

Si vous avez une machine Sherline plusieurs configurations prédéfinies sont fournies. Au premier démarrage de Machinekit, le sélecteur de configuration s’ouvre, sélectionnez alors le modèle correspondant à votre machine *Sherline*, puis acceptez d’enregistrer une copie.

Xylotex
-------

Si vous avez une machine *Xylotex* vous pouvez utiliser l’assistant graphique de configuration fourni avec Machinekit et créer rapidement votre configuration personnalisée [avec l’assistant Stepconf](#cha:Assistant-graphique-StepConf).

Informations relatives à la machine
-----------------------------------

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

Informations relatives au brochage
----------------------------------

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

Informations relatives à la mécanique
-------------------------------------

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

Assistants de configuration graphique
-------------------------------------

-   Pour les moteurs pas à pas, voir la documentation de l’assistant graphique Stepconf au chapitre [concernant cet assistant.](#cha:Assistant-graphique-StepConf)

-   Pour les servomoteurs et les moteurs pas à pas, voir la documentation de l’assistant graphique PNCconf au chapitre [relatif à cet assistant](#cha:Assistant-graphique-PNCConf)

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


