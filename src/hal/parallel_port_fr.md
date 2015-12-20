Port parallèle
==============

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Parport"></span>

Parport
-------

Parport est un pilote pour le port parallèle traditionnel des PC. Le port dispose d’un total de 17 broches physiques. Le port parallèle originel a divisé ces broches en trois groupes: données, contrôles et états. Le groupe *données* consiste en 8 broches de sortie, le groupe *contrôles* consiste en 4 broches et le groupe *états* consiste en 5 broches d’entrée.

Au début des années 1990, le port parallèle bidirectionnel est arrivé, ce qui a permis à l’utilisateur d’ajuster le groupe des données comme étant des sorties ou comme étant des entrées. Le pilote de HAL supporte le port bidirectionnel et permet à l’utilisateur de configurer le groupe des données en entrées ou en sorties. Si il est configuré en sorties, un port fournit un total de 12 sorties et 5 entrées. Si il est configuré en entrées, il fournit 4 sorties et 13 entrées.

Dans certains ports parallèles, les broches du groupe contrôle sont des collecteurs ouverts, ils peuvent aussi être mis à l'état bas par une porte extérieure. Sur une carte avec les broches de contrôle en collecteurs ouverts, le mode *x* de HAL permet un usage plus flexible avec 8 sorties dédiées, 5 entrées dédiées et 4 broches en collecteurs ouverts. Dans d’autres ports parallèles, les broches du groupe contrôles sont en push-pull et ne peuvent pas être utilisées comme des entrées.

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
HAL et les collecteurs ouverts
</div>
<div class="paragraph">
<p>HAL ne peut pas déterminer automatiquement si les broches en mode bidirectionnel <em>x</em> sont effectivement en collecteurs ouverts. Si elles n’y sont pas, elles ne peuvent pas être utilisées comme des entrées. Essayer de les passer à l'état BAS par une source extérieure peut détériorer le matériel.</p>
</div>
<div class="paragraph">
<p>Pour déterminer si un port a des broches de contrôle en <em>collecteur ouvert</em>, charger hal_parport en mode <em>x</em>, positionner les broches de contrôle à une valeur HAUTE. HAL doit lire des pins à l'état VRAI. Ensuite, insérer une résistance de 470Ω entre une des broches de contrôle et GND du port parallèle. Si la tension de cette broche de contrôle est maintenant proche de 0V et que HAL la lit comme une pin FAUSSE, alors vous avez un port OC. Si la tension résultante est loin de 0V ou que HAL ne la lit pas comme étant FAUSSE, votre port ne ne peut pas être utilisé en mode <em>x</em>.</p>
</div>
<div class="paragraph">
<p>Le matériel extérieur qui pilote les broches de contrôle devrait également utiliser des portes en collecteur ouvert (ex: 74LS05…). Généralement, une pin de HAL -out devrait être VRAIE quand la pin physique est utilisée comme une entrée.</p>
</div>
<div class="paragraph">
<p>Sur certaines machines, les paramètres du BIOS peuvent affecter la possibilité d’utiliser le mode <em>x</em>. Le mode <em>SPP</em> est le mode qui fonctionne le plus fréquemment.</p>
</div></td>
</tr>
</tbody>
</table>

Aucune autre combinaison n’est supportée. Un port ne peut plus être modifié pour passer d’entrées en sorties une fois le pilote installé. La figure [des diagrammes blocs](#fig:Parport-block-diag) affiche deux diagrammes, un montre le pilote quand le groupe de données est configuré en sorties et le second le montre configuré en entrées.

Le pilote *parport* peut contrôler au maximum 8 ports (définis par MAX\_PORTS dans le fichier hal\_parport.c). Les ports sont numérotés à partir de zéro.

### Chargement de hal\_parport

    loadrt hal_parport cfg="<config-string>"

### Utiliser l’index du port

Les adresses d’E/S inférieures à 16 sont traitées comme les index de port. C’est la manière la plus simple d’installer le pilote *parport* en coopération avec le pilote de Linux parport\_pc si il est chargé.

    loadrt hal_parport cfg="0"

Utilisera l’adresse que Linux a détecté pour parport0.

### Utiliser l’adresse du port

La chaine config-string représente l’adresse hexadécimale du port, suivie optionnellement par une direction, le tout répété pour chaque port. Les directions sont *in*, *out*, ou *x*, elles déterminent la direction des broches physiques 2 à 9 et s’il y a lieu de créer des pins d’entrée de HAL pour les broches de contrôle physiques. Si la direction n’est pas précisée, le groupe données sera par défaut configuré en sorties. Par exemple:

    loadrt hal_parport cfg="0x278 0x378 in 0x20A0 out"

Cet exemple installe les pilotes pour un port 0x0278, avec les broches 2 à 9 en sorties (par défaut, puisque ni *in*, ni *out* n’est spécifié), un port 0x0378, avec les broches 2 à 9 en entrées et un port 0x20A0, avec les broches 2 à 9 explicitement spécifiées en sorties. Notez que vous devez connaître l’adresse de base des ports parallèles pour configurer correctement les pilotes. Pour les ports sur bus ISA, ce n’est généralement pas un problème, étant donné que les ports sont presque toujours à une adresse *bien connue*, comme 0x278 ou 0x378 qui sont typiquement configurées dans le BIOS. Les adresses des cartes sur bus PCI sont habituellement trouvées avec *lspci -v* dans une ligne *I/O ports*, ou dans un message du noayau après l’exécution de *sudo modprobe -a parport\_pc*. Il n’y a pas d’adresse par défaut, si &lt;config-string&gt; ne contient pas au moins une adresse, c’est une erreur.

![](images/parport-block-diag.png)

Figure 1. Diagrammes blocs de parport

### Pins

-   *(bit) parport.&lt;portnum&gt;.pin-&lt;pinnum&gt;-out* — Pilote une broche de sortie physique. output pin.

-   *(bit) parport.&lt;portnum&gt;.pin-&lt;pinnum&gt;-in* — Suit une broche d’entrée physique. pin.

-   *(bit) parport.&lt;portnum&gt;.pin-&lt;pinnum&gt;-in-not* — Suit une pin d’entrée physique, mais inversée. Pour chaque pin, *&lt;portnum&gt;* est le numéro du port et *&lt;pinnum&gt;* est le numéro de la broche physique du connecteur DB-25.

Pour chaque broche de sortie physique, le pilote crée une simple pin de HAL, par exemple parport.0.pin-14-out. Les pins 2 jusqu'à 9 font partie du groupe *données*, elles sont des pins de sortie si le port est défini comme un port de sortie (par défaut, port de sortie). Les broches 1, 14, 16 et 17 sont des sorties dans tous les modes. Ces pins de HAL contrôlent l'état des pins physiques correspondantes.

Pour chaque pin d’entrée physique, le pilote crée deux pins de HAL, par exemple: parport.0.pin-12-in et parport.0.pin-12-in-not. Les pins 10, 11, 12, 13 et 15 sont toujours des sorties. Les pins 2 jusqu'à 9 sont des pins d’entrée seulement si le port est défini comme un port d’entrée. Une pin de HAL -in est VRAIE si la pin physique est haute et FAUSSE si la pin physique est basse. Une pin de HAL -in-not est inversée, elle est FAUSSE si la pin physique est haute. En connectant un signal à l’une ou à l’autre, l’utilisateur peut décider de la logique de l’entrée. En mode *x*, les pins 1, 14, 16 et 17 sont également des pins d’entrée.

### Paramètres

-   *(bit) parport.&lt;portnum&gt;.pin-&lt;pinnum&gt;-out-invert* — Inverse une pin de sortie.

-   *(bit) parport.&lt;portnum&gt;.pin-&lt;pinnum&gt;-out-reset* — (seulement pour les pins 2..9) VRAIE si cette pin doit être réinitialisée quand la fonction de réinitialisation est exécutée.

-   *(U32) parport.&lt;portnum&gt;.reset-time* — Le temps (en nanosecondes) entre le moment ou la broche est écrite et le moment ou elle est réinitialisée par les fonctions de réinitialisation de HAL.

Le paramètre *-invert* détermine si une pin de sortie est active haute ou active basse. Si *-invert* est FAUX, mettre la pin HAL -out VRAIE, placera la pin physique à l'état haut et mettre la pin HAL FAUSSE, placera la pin physique à l'état bas. Si *-invert* est VRAI, mettre la pin HAL -out VRAIE, va mettre la pin physique à l'état bas. Si *-reset* est VRAI, la fonction de réinitialisation va passer la pin à la valeur de *-out-invert*. Ceci peut être utilisé en conjonction avec *stepgen doublefreq* pour produire un pas par période.

### Fonctions

-   *(funct) parport.&lt;portnum&gt;.read*-- Lit les pins physiques du port &lt;portnum&gt; et met à jour les pins de HAL -in et -in-not.

-   *(funct) parport.read-all* — Lit les pins physiques de tous les ports et met à jour les pins de HAL -in et -in-not.

-   *(funct) parport.&lt;portnum&gt;.write* — Lit les pins de HAL -out du port &lt;portnum&gt; et met à jour les pins de sortie physiques correspondantes.

-   *(funct) parport.write-all* — Lit les pins de HAL -out de tous les ports et met à jour toutes les pins de sortie physiques.

-   *(funct) parport.&lt;portnum&gt;.reset* — Attends que le délai de mise à jour *reset-time* soit écoulé depuis la dernière écriture associée *write* puis remet à jour les pins aux valeurs indiquées par *-out-invert* et les paramètres de *-out-invert*. La réinitialisation doit être plus tard dans le même thread que l'écriture.

Les différentes fonctions individuelles sont prévues pour les situations où un port doit être mis à jour dans un thread très rapide, mais d’autres ports peuvent être mis à jour dans un thread plus lent pour gagner du temps CPU. Ce n’est probablement pas une bonne idée d’utiliser en même temps, les fonctions -all et une fonction individuelle.

### Problème courant

Si, au chargement du module un message du genre suivant apparait:

    insmod: error inserting '/home/jepler/machinekit/rtlib/hal_parport.ko':
    -1 Device or resource busy

s’assurer que le module du kernel standard, parport\_pc, n’est pas chargé et qu’aucun périphérique dans le système ne revendique les ports concernés. <span class="footnote">
\[Dans le paquetage Machinekit pour Debian, le fichier /etc/modprobe.d/machinekit empêche normalement que *parport\_pc* soit chargé automatiquement.\]
</span>

Si le module est chargé mais ne semble pas fonctionner, l’adresse du port est incorrecte ou le module *probe\_parport* est revendiqué par un autre périphérique.

### Utiliser DoubleStep

Pour activer DoubleStep sur un port parallèle, il faut ajouter la fonction *parport.n.reset* après *parport.n.write* et configurer *stepspace* à *0* ainsi que le *reset-time* souhaité. Alors ce pas pourra être positionné à chaque période dans HAL, puis voir son état basculé par *parport* après été positionné pendant le temps spécifié par parport.n.reset-time.

Par exemple:

    loadrt hal_parport cfg="0x378 out"
    setp parport.0.reset-time 5000
    loadrt stepgen step_type=0,0,0
    addf parport.0.read base-thread
    addf stepgen.make-pulses base-thread
    addf parport.0.write base-thread
    addf parport.0.reset base-thread
    addf stepgen.capture-position servo-thread
    ...
    setp stepgen.0.steplen 1
    setp stepgen.0.stepspace 0

probe\_parport<span id="sec:probe_parport"></span>
--------------------------------------------------

Dans les PC actuels, les ports parallèles peuvent requérir une configuration plug and play (PNP) avant qu’ils ne puissent être utilisés. Le module de noyau *probe\_parport* effectue la configuration de tous les port PNP présents. Il doit être chargé avant *hal\_parport*. Sur les machines sans port PNP, il peut être chargé mais restera sans effet.

### Installer probe\_parport

    loadrt probe_parport
    loadrt hal_parport ...

Si le kernel Linux affiche un message similaire à:

`parport: PnPBIOS parport detected.`

Quand le module parport\_pc est chargé, avec la commande: *sudo modprobe -a parport\_pc; sudo rmmod parport\_pc*, l’utilisation de ce module sera probablement nécessaire.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


