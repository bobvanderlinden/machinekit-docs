Moteurs pas à pas
=================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:stepper-diagnostics"></span>

Si ce que vous obtenez ne correspond pas à ce que vous espériez, la plupart du temps c’est juste un petit manque d’expérience. Accroître son expérience permet souvent une meilleure compréhension globale. Porter un diagnostic sur plusieurs problèmes est toujours plus facile en les prenant séparément, de même qu’une équation dont on a réduit le nombre de variables est toujours plus rapide à résoudre. Dans le monde réel ce n’est pas toujours le cas mais c’est une bonne voie à suivre.

Problèmes communs
-----------------

### Le moteur n’avance que d’un pas

La raison la plus fréquente dans une nouvelle installation pour que le moteur ne bouge pas est l’interversion entre le signal de pas et le signal de direction. Si, quand vous pressez le bouton de jog dans un sens puis dans l’autre, le moteur n’avance que d’un pas à chaque fois et toujours dans la même direction, vous êtes dans ce cas.

### Le moteur ne bouge pas

Certaine interfaces de pilotage de moteurs ont une broche d’activation (enable) ou demandent un signal de pompe de charge pour activer leurs sorties.

### Distance incorrecte

Si vous commandez une distance de déplacement précise sur un axe et que le déplacement réel ne correspond pas, alors l'échelle de l’axe n’est pas bonne.

Messages d’erreur
-----------------

### Erreur de suivi

Le concept d’erreur de suivi est étrange quand il s’agit de moteurs pas à pas. Etant un système en boucle ouverte, aucune contre réaction ne permet de savoir si le suivi est correct ou non. Machinekit calcule si il peut maintenir le suivi demandé par une commande, si ce n’est pas possible il stoppe le mouvement et affiche une erreur de suivi. Les erreurs de suivi sur les systèmes pas à pas sont habituellement les suivantes:

-   FERROR to small - (FERROR trop petit)

-   MIN\_FERROR to small - (MIN\_FERROR trop petit)

-   MAX\_VELOCITY to fast - (MAX\_VELOCITY trop rapide)

-   MAX\_ACCELERATION to fast - (MAX\_ACCELERATION trop rapide)

-   BASE\_PERIOD set to long - (BASE\_PERIOD trop longue)

-   Backlash ajouté à un axe (rattrapage de jeu)

Toutes ces erreurs se produisent lorsque l’horloge temps réel n’est pas capable de fournir le nombre de pas nécessaire pour maintenir la vitesse requise par le réglage de la variable BASE\_PERIOD. Ce qui peut se produire, par exemple après un test de latence trop bref pour obtenir un valeur fiable, dans ce cas, revenir à une valeur plus proche de ce qu’elle était et réessayez. C’est également le cas quand les valeurs de vitesse maximum et d’accélération maximum sont trop élevées.

Si un backlash a été ajouté, il est nécessaire d’augmenter STEPGEN\_MAXACCEL aux environs du double de MAX\_ACCELERATION dans la section \[AXIS\] du fichier INI et ce, pour chacun des axes sur lesquels un backlash a été ajouté. Machinekit utilise une *extra accélération* au moment de l’inversion de sens pour reprendre le jeu. Sans correction du backlash, l’accélération pour le générateur de pas peut être juste un peu plus basse que celle du planificateur de mouvements.

### Erreur de RTAPI

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

Tester
------

### Tester le timing des pas

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

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


