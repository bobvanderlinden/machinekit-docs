Réglages d’une boucle PID
=========================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Regulation-PID"></span>

Régulation à PID
----------------

Un régulateur Proportionnel Intégral Dérivé (PID) est un organe de contrôle qui permet d’effectuer une régulation en boucle fermée d’un procédé.

Le régulateur compare une valeur mesurée sur le procédé avec une valeur de consigne. La différence entre ces deux valeurs (le signal d'*erreur*) est alors utilisée pour calculer une nouvelle valeur d’entrée du procédé tendant à réduire au maximum l'écart entre la mesure et la consigne (signal d’erreur le plus faible possible).

Contrairement aux algorithmes de régulation simples, le contrôle par PID peut ajuster les sorties du procédé, en fonction de l’amplitude du signal d’erreur, et en fonction du temps. Il donne des résultats plus précis et un contrôle plus stable. (Il est montré mathématiquement qu’une boucle PID donne un contrôle plus stable qu’un contrôle proportionnel seul et qu’il est plus précis que ce dernier qui laissera le procédé osciller).

### Les bases du contrôle en boucle

Intuitivement, une boucle PID essaye d’automatiser ce que fait un opérateur muni d’un multimètre et d’un potentiomètre de contrôle. L’opérateur lit la mesure de sortie du procédé, affichée sur le multimètre et utilise le bouton du potentiomètre pour ajuster l’entrée du procédé (l'*action*) jusqu'à stabiliser la mesure de la sortie souhaitée, affichée sur le multimètre.

Un boucle de régulation est composée de trois parties:

1.  La mesure, effectuée par un capteur connecté à un procédé, par exemple un codeur.

2.  La décision, prise par les éléments du régulateur.

3.  L’action sur le dispositif de sortie, par exemple: un moteur.

Quand le régulateur lit le capteur, il soustrait la valeur lue à la valeur de la consigne et ainsi, obtient l'«erreur de mesure». Il peut alors utiliser cette erreur pour calculer une correction à appliquer sur la variable d’entrée du procédé (l'*action*) de sorte que cette correction tende à supprimer l’erreur mesurée en sortie de procédé.

Dans une boucle PID, la correction à partir de l’erreur est calculée de trois façons: P) l’erreur de mesure courante est soustraite directement (effet proportionnel), I) l’erreur est intégrée pendant un laps de temps (effet intégral), D) l’erreur est dérivée pendant un laps de temps (effet dérivé).

Une régulation à PID peut être utilisée dans n’importe quel procédé pour contrôler une variable mesurable, en manipulant d’autres variables de ce procédé. Par exemple, elle peut être utilisée pour contrôler: température, pression, débit, composition chimique, vitesse et autres variables.

Dans certains systèmes de régulation, les régulateurs sont placés en série ou en parallèle. Dans ces cas, le régulateur *maître* produit les signaux utilisés par les régulateurs *esclaves*. Une situation courante dans le contrôle des moteurs, la régulation de vitesse, qui peut demander que la vitesse du moteur soit contrôlée par un régulateur *esclave* (souvent intégré dans le variateur de fréquence du moteur) recevant en entrée une valeur proportionnelle à la vitesse. Cette entrée de l'*esclave* est alors fournie par la sortie du régulateur *maître*, lequel reçoit la variable de consigne.

### Théorie

Le *PID* représente les abréviations des trois actions qu’il utilise pour effectuer ses corrections, ce sont des ajouts d’un signal à un autre. Tous agissent sur la quantité régulée. Les actions aboutissent finalement à des *soustractions* de l’erreur de mesure, parce que le signal proportionnel est habituellement négatif.

#### Action Proportionnelle

Pour cette action, l’erreur est multipliée par la constante P (pour Proportionnel) qui est négative, puis ajoutée (soustraction de l’erreur de mesure) à la quantité régulée. P est valide uniquement sur la bande dans laquelle le signal de sortie du régulateur est proportionnel à l’erreur du système. Noter que si l’erreur de mesure est égale à zéro, la partie proportionnelle de la sortie du régulateur est également à zéro.

#### Action Intégrale

L’action intégrale fait intervenir la notion de temps. Elle tire profit du signal d’erreur passé qui est intégré (additionné) pendant un laps de temps, puis multiplié par la constante I (négative) ce qui en fait une moyenne, elle est enfin additionnée (soustraction de l’erreur de mesure) à la quantité régulée. La moyenne de l’erreur de mesure permet de trouver l’erreur moyenne entre la sortie du régulateur et la valeur de la consigne. Un système seulement proportionnel oscille en plus et en moins autour de la consigne du fait qu’en arrivant vers la consigne, l’erreur est à zéro, il n’enlève alors plus rien est dépasse la consigne, ou oscille et/ou se stabilise à une valeur trop basse ou trop élevée. L’addition sur l’entrée d’une proportion négative (soustraction) de l’erreur de mesure moyennée, permet toujours de réduire l'écart moyen entre la mesure en sortie et la consigne. Donc finalement, une boucle PI bien réglée verra sa sortie redescendre lentement à la valeur de la consigne.

#### Action Dérivée

L’action dérivée utilise aussi la notion de temps. Elle cherche à anticiper l’erreur future. La dérivée première (la pente de l’erreur) est calculée pour un laps de temps et multipliée par la constante (négative) D, puis elle est additionnée (soustraction de l’erreur de mesure) à la quantité régulée. L’action dérivée de la régulation fourni une réponse aux perturbations agissant sur le système. Plus important est le terme dérivé, plus rapide sera la réponse en sortie à une perturbation sur l’entrée.

Plus techniquement, une boucle PID peut être caractérisée comme un filtre appliqué sur un système complexe d’un domaine fréquentiel. C’est utilisé pour calculer si le système atteindra une valeur stable. Si les valeurs sont choisies incorrectement, le procédé entrera en oscillation et sa sortie n’atteindra jamais la consigne.

### Réglage d’une boucle

Régler une boucle de régulation consiste à agir sur les paramètres des différentes actions (gain du proportionnel, gain de l’intégral, gain de la dérivée) sur des valeurs optimales pour obtenir la réponse désirée sur la sortie du procédé. Le comportement des procédés varient selon les applications lors d’un changement de consigne. Certains procédés ne permettent aucun dépassement de la consigne. D’autres doivent minimiser l'énergie nécessaire pour atteindre un nouveau point de consigne. Généralement la stabilité de la réponse est requise, le procédé ne doit pas osciller quels que soient les conditions du procédé et le point de consigne.

Régler une boucle est rendu plus compliqué si le temps de réponse du procédé est long; il peut prendre plusieurs minutes, voir plusieurs heures pour qu’une modification de consigne produise un effet stable. Certains procédés ne sont pas linéaires et les paramètres qui fonctionnent bien à pleine charge ne marchent plus lors du démarrage hors charge du procédé. Cette section décrit quelques méthodes manuelles traditionnelles pour régler ces boucles.

Il existe plusieurs méthodes pour régler une boucle PID. Le choix de la méthode dépendra en grande partie de la possibilité ou non de mettre la boucle «hors production» pour la mise au point ainsi que de la vitesse de réponse du système. Si le système peut être mis hors production, la meilleure méthode de réglage consiste souvent à soumettre le système à un changement de consigne, à mesurer la réponse en fonction du temps et à l’aide de cette réponse à déterminer les paramètres de la régulation.

#### Méthode simple

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

#### Méthode de Ziegler-Nichols

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

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


