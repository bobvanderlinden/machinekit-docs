Créer des composants de l’espace utilisateur
============================================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

Utilisation de base en Python
-----------------------------

Un composant de l’espace utilisateur commence par créer ses pins et ses paramètres, puis il entre dans une boucle dans laquelle il va positionner périodiquement toutes ses sorties en fonction de ses entrées. Le composant suivant, un passe-tout, copie la valeur vue sur ses pins d’entrée (*passe\_tout.in*) vers ses pins de sortie (*passe\_tout.out*) approximativement une fois par seconde.

    #!/usr/bin/python
    import hal, time
    h = hal.component("passe_tout")
    h.newpin("in", hal.HAL_FLOAT, hal.HAL_IN)
    h.newpin("out", hal.HAL_FLOAT, hal.HAL_OUT)
    h.ready()
    try:
        while 1:
            time.sleep(1)
            h['out'] = h['in']
    except KeyboardInterrupt:
        raise SystemExit

Copier le listing précédent dans un fichier nommé *passe\_tout*, le rendre exécutable par un *chmod +x* et le placer dans son *$PATH*. On peut alors l’essayer en faisant:

`halrun`

`halcmd: loadusr passe_tout`

`halcmd: show pin`

    Component Pins:
    Owner Type  Dir     Value  Name
     03   float IN          0  passe_tout.in
     03   float OUT         0  passe_tout.out

`halcmd: setp passe_tout.in 3.14`

`halcmd: show pin`

    Component Pins:
    Owner Type  Dir     Value  Name
     03   float IN       3.14  passe_tout.in
     03   float OUT      3.14  passe_tout.out

Composants de l’espace utilisateur et délais
--------------------------------------------

Si vous tapez rapidement *show pin*, vous pourrez voir que *passe\_tout.out* conserve un moment son ancienne valeur de 0. Ceci est dû à l’appel de la fonction *time.sleep(1)*, qui fait que les pins de sortie changent d'état, au plus, une fois par seconde. Parce-que ce composant appartient à l’espace utilisateur, ce délai peut apparaître plus long, par exemple si la mémoire utilisée par le composant pass\_tout est échangée avec le disque dur, le délai peut être allongé jusqu’au rafraîchissement de la mémoire.

Ces composants de l’espace utilisateur conviennent parfaitement pour des éléments tels que des panneaux de contrôle pour lesquels des délais de l’ordre de quelques millisecondes sont imperceptibles. Ils ne conviennent pas, en revanche, pour envoyer des impulsions de pas vers une carte de pilotage de périphériques pour lesquelles les délais doivent rester de l’ordre de quelques microsecondes, dans tous les cas.

Créer les pins et les paramètres
--------------------------------

`h = hal.component("passe_tout")`

Le composant lui-même est créé par l’appel du constructeur *hal.component*. Les arguments sont le nom du composant HAL et optionnellement, le préfixe utilisé pour les noms de pin et de paramètre. Si le préfixe n’est pas spécifié, le nom du composant est utilisé.

`h.newpin("in", hal.HAL_FLOAT, hal.HAL_IN)`

Puis les pins sont créées par appels des méthodes sur l’objet composant. Les arguments sont: pin nom suffixe, type de pin et direction de la pin. Pour les paramètres, les arguments sont: paramètre nom suffixe, type de paramètre et direction du paramètre.

<table>
<caption>Tableau 1. Noms des options de HAL</caption>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Types de Pin et Paramètre:</strong></th>
<th align="left">HAL_BIT</th>
<th align="left">HAL_FLOAT</th>
<th align="left">HAL_S32</th>
<th align="left">HAL_U32</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>Directions des pins:</strong></p></td>
<td align="left"><p>HAL_IN</p></td>
<td align="left"><p>HAL_OUT</p></td>
<td align="left"><p>HAL_IO</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>Directions des paramètres:</strong></p></td>
<td align="left"><p>HAL_RO</p></td>
<td align="left"><p>HAL_RW</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

Le nom complet d’une pin ou d’un paramètre est formé en joignant le préfixe avec le suffixe par un **.**, comme dans l’exemple où la pin créée est appelée *passe\_tout.in*.

`h.ready()`

Une fois toutes les pins et les paramètres créés, la méthode *.ready()* est appelée.

### Changer le préfixe

Le préfixe peut être changé en appelant la méthode *.setprefix()*. Le préfixe courant peut être retrouvé en appelant la méthode *.getprefix()*.

Lire et écrire les pins et les paramètres
-----------------------------------------

Pour les pins et les paramètres qui sont aussi des identifiants Python, la valeur est accessible ou ajustable en utilisant la syntaxe des attributs suivante:

`h.out = h.in`

Pour les pins et les paramètres qui sont aussi des identifiants Python, la valeur est accessible ou ajustable en utilisant la syntaxe de sous-script suivante:

`h[out] = h[in]`

### Pilotage des pins de sortie (HAL\_OUT)

Périodiquement, habituellement dans le temps de réponse de l’horloge, toutes les pins HAL\_OUT doivent être *pilotées* en leur assignant une nouvelle valeur. Ceci doit être fait que la valeur soit différente ou non de la valeur précédemment assignée. Quand la pin est connectée au signal, l’ancienne valeur de sortie n’est pas copiée vers le signal, la valeur correcte n’apparaîtra donc sur le signal qu’une fois que le composant lui aura assigné une nouvelle valeur.

### Pilotage des pins bidirectionnelles (HAL\_IO)

La règle mentionnée ci-dessus ne s’applique pas aux pins bidirectionnelles. Au lieux de cela, une pin bidirectionnelle doit seulement être pilotée par le composant et quand le composant souhaite changer sa valeur. Par exemple, dans l’interface codeur, le composant codeur positionne seulement la pin *index-enable* à *FALSE* quand une impulsion d’index est vue et que l’ancienne valeur est *TRUE*, mais ne la positionne jamais à *TRUE*. Positionner de manière répétitive la pin à *FALSE* pourrait faire qu’un autre composant connecté agisse comme si une nouvelle impulsion d’index avait été vue.

Quitter
-------

Une requête *halcmd unload* pour le composant est délivrée comme une exception *KeyboardInterrupt* . Quand une requête de déchargement arrive, le processus doit quitter dans un court laps de temps ou appeler la méthode *.exit()* sur le composant si un travail substantiel, comme la lecture ou l'écriture de fichiers, doit être fourni pour terminer le processus d’arrêt.

Idées de projets
----------------

-   Créer un panneau de contrôle extérieur avec boutons poussoirs, interrupteurs et voyants. Connecter le tout à un microcontrôleur et raccorder le microcontrôleur à un PC en utilisant une liaison série. Python est vraiment capable d’interfacer une liaison série grâce à son module [pyserial](http://pyserial.sourceforge.net/) (Paquet *python-serial*, dans les dépôts universe d’Debian)

-   Relier un module d’affichage à LCD [LCDProc](http://lcdproc.omnipotent.net/) et l’utiliser pour afficher les informations de votre choix (Paquet *lcdproc*, dans les dépôts universe d’Debian)

-   Créer un panneau de contrôle virtuel utilisant n’importe quelle librairie d’interface graphique supportée par Python (gtk, qt, wxwindows, etc)

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


