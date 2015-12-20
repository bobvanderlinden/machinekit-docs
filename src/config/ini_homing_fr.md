Prise d’origine
===============

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="sec:Prises-d-origine"></span>

La prise d’origine
------------------

La prise d’origine semble assez simple, il suffit de déplacer chaque axe à un emplacement connu et de positionner l’ensemble des variables internes de Machinekit en conséquence. Toutefois, les machines étant différentes les unes des autres, la prise d’origine est maintenant devenue assez complexe.

Séquences de prise d’origine
----------------------------

Il existe quatre séquences de prise d’origine possibles. Elles sont définies par le signe des variables SEARCH\_VEL et LATCH\_VEL ainsi que les paramètres de configuration associés, la figure suivante donne le détail de ces séquences.

![](images/machinekit-motion-homing-diag_fr.png)

Figure 1. Les séquences de POM possibles

1.  Comme on le voit sur la figure, les deux conditions de base sont les suivantes:

    1.  La direction de recherche (SEARCH\_VEL) et la direction de détection (LATCH\_VEL) sont de même signe.

    2.  La direction de recherche (SEARCH\_VEL) et la direction de détection (LATCH\_VEL) sont de signe opposé.

Configuration
-------------

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

### Vitesse de recherche (HOME\_SEARCH\_VEL)

Vitesse de la phase initiale de prise d’origine, pendant la recherche du contact d’origine machine. Une valeur différente de zéro indique à Machinekit la présence d’un contact d’origine machine. Machinekit va alors commencer par vérifier si ce contact est déjà attaqué. Si oui, il le dégagera à la vitesse établie par *HOME\_SEARCH\_VEL*, la direction du dégagement sera de signe opposé à celui de *HOME\_SEARCH\_VEL*. Puis, il va revenir vers le contact en se déplaçant dans la direction spécifiée par le signe de *HOME\_SEARCH\_VEL* et à la vitesse déterminée par sa valeur absolue. Quand le contact d’origine machine est détecté, le mobile s’arrête aussi vite que possible, il y aura cependant toujours un certain dépassement dû à l’inertie et dépendant de la vitesse. Si celle-ci est trop élevée, le mobile peut dépasser suffisamment le contact pour aller attaquer un fin de course de limite d’axe, voir même aller se crasher dans une butée mécanique. À l’opposé, si *HOME\_SEARCH\_VEL* est trop basse, la prise d’origine peut durer très longtemps.

Une valeur égale à zéro indique qu’il n’y a pas de contact d’origine machine, dans ce cas, les phases de recherche de ce contact seront occultées. La valeur par défaut est zéro.

### Vitesse de détection (HOME\_LATCH\_VEL)

Spécifie la vitesse et la direction utilisée par le mobile pendant la dernière phase de la prise d’origine, c’est la recherche précise du contact d’origine machine, si il existe et de l’emplacement de l’impulsion d’index, si elle est présente. Cette vitesse est plus lente que celle de la phase de recherche initiale, afin d’améliorer la précision. Si *HOME\_SEARCH\_VEL* et *HOME\_LATCH\_VEL* sont de mêmes signes, la phase de recherche précise s’effectuera dans le même sens que la phase de recherche initiale. Dans ce cas, le mobile dégagera d’abord le contact en sens inverse avant de revenir vers lui à la vitesse définie ici. L’acquisition de l’origine machine se fera sur la première impulsion de changement d'état du contact. Si *HOME\_SEARCH\_VEL* et *HOME\_LATCH\_VEL* sont de signes opposés, la phase de recherche précise s’effectuera dans le sens opposé à celui de la recherche initiale. Dans ce cas, Machinekit dégagera le contact à la vitesse définie ici. L’acquisition de l’origine machine se fera sur la première impulsion de changement d'état du contact lors de son dégagement. Si *HOME\_SEARCH\_VEL* est à zéro, signifiant qu’il n’y a pas de contact et que *HOME\_LATCH\_VEL* et différente de zéro, le mobile continuera jusqu'à la prochaine impulsion d’index, l’acquisition de l’origine machine se fera à cet position. Si *HOME\_SEARCH\_VEL* est différent de zéro et que *HOME\_LATCH\_VEL* est égale à zéro, c’est une cause d’erreur, l’opération de prise d’origine échouera. La valeur par défaut est zéro.

### HOME\_IGNORE\_LIMITS

Peut contenir les valeurs YES ou NO. Cette variable détermine si Machinekit doit ignorer les fins de course de limites d’axe. Certaines machines n’utilisent pas un contact d’origine séparé, à la place, elles utilisent un des interrupteurs de fin de course comme contact d’origine. Dans ce cas, Machinekit doit ignorer l’activation de cette limite de course pendant la séquence de prise d’origine. La valeur par défaut de ce paramètre est NO.

### HOME\_USE\_INDEX

Spécifie si une impulsion d’index doit être prise en compte (cas de règles de mesure ou de codeurs de positions). Si cette variable est vraie (HOME\_USE\_INDEX = YES), Machinekit fera l’acquisition de l’origine machine sur le premier front de l’impulsion d’index. Si elle est fausse (=NO), Machinekit fera l’acquisition de l’origine sur le premier front produit par le contact d’origine (dépendra des signes de *HOME\_SEARCH\_VEL* et *HOME\_LATCH\_VEL*). La valeur par défaut est NO.

### HOME\_OFFSET

Contient l’emplacement du point d’origine ou de l’impulsion d’index, en coordonnées relatives. Il peut aussi être traité comme le décalage entre le point d’origine machine et le zéro de l’axe. A la détection du point d’origine ou de l’impulsion d’origine, Machinekit ajuste les coordonnées de l’axe à la valeur de *HOME\_OFFSET*. La valeur par défaut est zéro.

### Position de l’origine (HOME)

C’est la position sur laquelle ira le mobile à la fin de la séquence de prise d’origine machine. Après avoir détecté le contact d’origine, avoir ajusté les coordonnées de ce point à la valeur de *HOME\_OFFSET*, le mobile va se déplacer sur la valeur de *HOME*, c’est le point final de la séquence de prise d’origine. La valeur par défaut est zéro. Notez que même si ce paramètre est égal à la valeur de *HOME\_OFFSET*, le mobile dépassera très légérement la position du point d’aquisition de l’origine machine avant de s’arrêter. Donc il y aura toujours un petit mouvement à ce moment là (sauf bien sûr si *HOME\_SEARCH\_VEL* est à zéro, et que toute la séquence de POM a été sautée). Ce mouvement final s’effectue en vitesse de déplacement rapide. Puisque l’axe est maintenant référencé, il n’y a plus de risque pour la machine, un mouvement rapide est donc la façon la plus rapide de finir la séquence de prise d’origine.

### HOME\_IS\_SHARED

Si cet axe n’a pas un contact d’origine séparé des autres, mais plusieurs contacts câblés sur la même broche d’entrée, mettre cette valeur à 1 pour éviter de commencer la prise d’origine si un de ces contacts partagés est déjà activé. Mettez cette valeur à 0 pour permettre la prise d’origine même si un contact est déjà attaqué.

### HOME\_SEQUENCE

Utilisé pour définir l’ordre des séquences *HOME\_ALL* de prise d’origine des différents axes (exemple: la POM de l’axe X ne pourra se faire qu’après celle de Z). La POM d’un axe ne pourra se faire qu’après tous les autres en ayant la valeur la plus petite de *HOME\_SEQUENCE* et après qu’ils soient déjà tous à *HOME\_OFFSET*. Si deux axes ont la même valeur de *HOME\_SEQUENCE*, leurs POM s’effectueront simultanément. Si *HOME\_SEQUENCE* est égale à -1 ou n’est pas spécifiée, l’axe ne sera pas compris dans la séquence *HOME\_ALL*. Les valeurs de *HOME\_SEQUENCE* débutent à 0, il ne peut pas y avoir de valeur inutilisée.

### VOLATILE\_HOME

Si ce paramètre est vrai, l’origine machine de cet axe sera effacée chaque fois que la machine sera mise à l’arrêt. Cette variable est appropriée pour les axes ne maintenant pas la position si le moteur est désactivé (gravité de la broche par exemple). Certains moteurs pas à pas, en particulier fonctionnant en micropas, peuvent se comporter de la sorte.

### LOCKING\_INDEXER

Si cet axe comporte un verrouillage d’indexeur rotatif, celui-ci sera déverrouillé avant le début de la séquence de prise d’origine, et verrouillé à la fin.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


