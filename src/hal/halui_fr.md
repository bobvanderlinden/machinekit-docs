L’interface Halui
=================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Halui"></span>

Introduction<span id="sec:HaluiIntroduction"></span>
----------------------------------------------------

Halui est une interface utilisateur pour Machinekit s’appuyant sur HAL, elle connecte les pins de HAL à des commandes NML. La plupart des fonctionnalités (boutons, indicateurs etc.) utilisées par les interfaces graphiques traditionnelles (mini, Axis, etc.), sont fournies par des pins de HAL dans Halui.

La façon la plus facile pour utiliser halui est de modifier votre dossier d’ini pour inclure

    HALUI = halui

dans la section \[HAL\].

Une solution alternative pour l’invoquer (surtout si vous générez la config avec stepconf) est d’inclure

    loadusr halui -ini /path/to/inifile.ini

dans votre fichier custom.hal.

Nomenclature des pins d’Halui<span id="sec:Halui-pin-reference"></span>
-----------------------------------------------------------------------

 Abandon   
(abort)

-   halui.abort (bit, in) - pin de requête d’abandon (efface les erreurs)

 Axes   
(axis)

-   halui.axis.n.pos-commanded (float, out) - Position de l’axe commandée, en coordonnées machine

-   halui.axis.n.pos-feedback (float, out) - Position de l’axe lue, en coordonnées machine

-   halui.axis.n.pos-relative (float, out) - Position de l’axe, en coordonnées relatives

 Arrêt d’urgence   
(E-Stop)

-   halui.estop.activate (bit, in) - pin de requête d’arrêt d’urgence (E-Stop)

-   halui.estop.is-activated (bit, out) - indique si l’arrêt d’urgence est actif

-   halui.estop.reset (bit, in) - pin de requête de relâchement de l’arrêt d’urgence (E-Stop reset)

 Correcteur de vitesse d’avance   
(Feed override)

-   halui.feed-override.count-enable (bit, in) - doit être vraie pour que *counts* ou *direct-value* soient opérationnels.

-   halui.feed-override.counts (s32, in) - *counts* \* scale = FO pourcent. Peut être utilisé avec un codeur ou *direct-value*.

-   halui.feed-override.decrease (bit, in) - pin pour diminuer la correction (-=scale)

-   halui.feed-override.increase (bit, in) - pin pour augmenter la correction (+=scale)

-   halui.feed-override.direct-value (bit, in) - fausse lors de l’utilisation un codeur pour changer counts, vraie pour ajuster counts directement. La pin *count-enable* doit être vraie.

-   halui.feed-override.scale (float, in) - pin pour positionner l'échelle pour accroître ou décroître la correction de vitesse d’avance.

-   halui.feed-override.value (float, out) - Valeur de la correction courante de vitesse d’avance

 Arrosage par gouttelettes   
(Mist)

-   halui.mist.is-on (bit, out) - indique si l’arrosage par gouttelettes est actif

-   halui.mist.off (bit, in) - pin de requête d’arrêt de l’arrosage par gouttelettes

-   halui.mist.on (bit, in) - pin de requête de l’arrosage par gouttelettes

 Arrosage fluide   
(Flood)

-   halui.flood.is-on (bit, out) - indique si l’arrosage fluide est actif

-   halui.flood.off (bit, in) - pin de requête d’arrêt d’arrosage fluide

-   halui.flood.on (bit, in) - pin de requête d’arrosage fluide

 Prise d’origine machine de tous les axes   
(Homing)

-   halui.home-all (bit, in) - pin de requête de prise d’origine machine de tous les axes. Cette pin sera présente seulement si HOME\_SEQUENCE est fixée dans le fichier ini.

 Jog   
(Manivelle) &lt;n&gt; est un nombre compris entre 0 et 8, ou &lt;selected&gt;.

-   halui.jog-deadband (float, in)- bande morte pour le jogging analogique (les petites vitesses de jog sont sans effet)

-   halui.jog.speed (float, in) - positionne la vitesse de jog

-   halui.jog.&lt;n&gt;.analog (float, in) - entrée analogique de vitesse de jog (utilisé avec les joysticks ou autres matériels analogiques)

-   halui.jog.&lt;n&gt;.minus (bit, in) - jog en direction négative

-   halui.jog.&lt;n&gt;.plus (bit, in) - jog en direction positive

-   halui.jog.&lt;selected&gt;.minus (bit, in) - jog l’axe &lt;selected&gt; en direction négative et à la vitesse de halui.jog.speed velocity

-   halui.jog.&lt;selected&gt;.plus (bit, in) - jog l’axe &lt;selected&gt; en direction positive et à la vitesse de halui.jog.speed velocity

 Articulations   
(Joints) &lt;n&gt; est un nombre compris entre 0 et 8, ou &lt;selected&gt;.

-   halui.joint.&lt;n&gt;.has-fault (bit, out) - pin de status indiquant que l’articulation est en défaut

-   halui.joint.&lt;n&gt;.home (bit, in) - pin pour la prise d’origine d’une articulation spécifique

-   halui.joint.&lt;n&gt;.is-homed (bit, out) - pin de status indiquant que l’articulation est référencée

-   halui.joint.&lt;n&gt;.is-selected (bit, out) - pin indiquant que l’articulation est &lt;selected&gt; - interne à halui

-   halui.joint.&lt;n&gt;.on-hard-max-limit (bit, out) - pin de status indiquant que le joint est sur son fin de course de limite positive

-   halui.joint.&lt;n&gt;.on-hard-min-limit (bit, out) - pin de status indiquant que le joint est sur son fin de course de limite négative

-   halui.joint.&lt;n&gt;.on-soft-max-limit (bit, out) - pin de status indiquant que le joint est sur sa limite logicielle positive

-   halui.joint.&lt;n&gt;.on-soft-min-limit (bit, out) - pin de status indiquant que le joint est sur sa limite logicielle négative

-   halui.joint.&lt;n&gt;.select (bit, in) - select joint (0..8) - interne à halui

-   halui.joint.&lt;n&gt;.unhome (bit, in) - unhomes this joint

-   halui.joint.selected (u32, out) - selected joint (0..8) - interne à halui

-   halui.joint.selected.has-fault (bit, out) - pin de status indiquant que le joint &lt;n&gt; est en défaut

-   halui.joint.selected.home (bit, in) - pin pour la prise d’origine de l’articulation &lt;selected&gt;

-   halui.joint.selected.is-homed (bit, out) - pin de status indiquant que le joint &lt;selected&gt; est référencé

-   halui.joint.selected.on-hard-max-limit (bit, out) - pin de status indiquant que le joint &lt;selected&gt; est sur son fin de course de limite positive

-   halui.joint.selected.on-hard-min-limit (bit, out) - pin de status indiquant que le joint &lt;selected&gt; est sur son fin de course de limite négative

-   halui.joint.selected.on-soft-max-limit (bit, out) - pin de status indiquant que le joint &lt;selected&gt; est sur sa limite logicielle positive

-   halui.joint.selected.on-soft-min-limit (bit, out) - pin de status indiquant que le joint &lt;selected&gt; est sur sa limite logicielle négative

-   halui.joint.selected.unhome (bit, in) - pin for unhoming l’articulation selected.

 Graissage centralisé   
(Lube)

-   halui.lube.is-on (bit, out) - indique si le graissage est actif

-   halui.lube.off (bit, in) - pin de requête d’arrêt du graissage

-   halui.lube.on (bit, in) - pin de requête de graissage

 Machine   
(Marche / Arrêt)

-   halui.machine.is-on (bit, out) - indique que la machine est en marche

-   halui.machine.off (bit, in) - pin de requête d’arrêt machine

-   halui.machine.on (bit, in) - pin de requête de marche machine

 Vitesse maximum   
La vitesse linéaire maximum peut être ajustée entre 0 et la valeur de la variable MAX\_VELOCITY dans la section \[TRAJ\] du fichier ini.

-   halui.max-velocity.count-enable (bit, in) - Doit être vraie pour que les *counts* ou *direct-value* soit opérationnels.

-   halui.max-velocity.counts (s32, in) - counts \* scale = MV pourcent. Utilisable avec un codeur ou *direct-value*.

-   halui.max-velocity.direct-value (bit, in) - faux quand un codeur est utilisé pour modifier *counts*, vraie pour ajuster *counts* directement. La pin *count-enable* doit être vraie.

-   halui.max-velocity.decrease (bit, in) - pin pour diminuer la vitesse max

-   halui.max-velocity.increase (bit, in) - pin pour augmenter la vitesse max

-   halui.max-velocity.scale (float, in) - Valeur appliquée sur le nombre de fronts montants des pins increase ou decrease en unités machine par seconde.

-   halui.max-velocity.value (float, out) - Valeur de la vitesse linéaire maximum en unités machine par seconde.

 Données manuelles   
<span id="sub:MDI"></span> Il arrive que l’utilisateur veuille ajouter des tâches plus complexes devant être effectuées par l’activation d’une pin de HAL. C’est possible en utilisant le schéma de commande en données manuelles (MDI) suivant:

-   Une MDI\_COMMAND est ajoutée dans la section \[HALUI\] du fichier ini, par exemple:

        [HALUI]
        MDI_COMMAND = G0 X0

-   Quand halui démarre il va lire/détecter le champ MDI\_COMMAND dans le fichier ini et exporter les pins de type (bit) halui.mdi-command-&lt;nr&gt;, &lt;nr&gt; est un nombre compris entre 00 et le nombre de MDI\_COMMAND trouvées dans le fichier ini, avec un maximum de 64 commandes.

-   Quand la pin halui.mdi-command-&lt;nr&gt; est activée, halui va essayer d’envoyer au MDI la commande définie dans le fichier ini. Ca ne fonctionnera pas dans tous les modes de fonctionnement où se trouve Machinekit, par exemple, tant qu’il est en AUTO halui ne peut pas envoyer de commande MDI.

     Sélection d’une articulation   
    (Joint Selection)

-   halui.joint.select (u32, in) - sélectionne l’articulation (0..7) - internal halui

-   halui.joint.selected (u32, out) - articulation (0..7) sélectionnée - internal halui

-   halui.joint.x.select bit (bit, in) - pins pour sélectinner une articulation - internal halui

-   halui.joint.x.is-selected bit (bit, out) - pin de status indiquant une articulation sélectionné - internal halui

     Mode de fonctionnement   
    (Mode)

-   halui.mode.auto (bit, in) - pin de requête du mode auto

-   halui.mode.is\_auto (bit, out)- indique si le mode auto est actif

-   halui.mode.is-joint (bit, out) - indique si le mode articulation par articulation est actif

-   halui.mode.is\_manual (bit, out) - indique si le mode manuel est actif

-   halui.mode.is\_mdi (bit, out) - indique si le mode données manuelles est actif

-   halui.mode.is-teleop (bit, out) - indique que le mode jog coordonné est actif

-   halui.mode.joint (bit, in) - pin de requête du mode jog articulation par articulation

-   halui.mode.manual (bit, in) - pin de requête du mode manuel

-   halui.mode.mdi (bit, in) - pin de requête du mode données manuelles

-   halui.mode.teleop (bit, in) - pin de requête du mode jog coordonné

     Programme   
    (Program)

-   halui.program.block-delete.is-on (bit, out) - status pin telling that block delete is on

-   halui.program.block-delete.off (bit, in) - pin for requesting that block delete is off

-   halui.program.block-delete.on (bit, in) - pin for requesting that block delete is on

-   halui.program.is-idle (bit, out) - pin de status indiquant qu’aucun programme n’est lancé

-   halui.program.is-paused (bit, out) - pin de status indiquant qu’un programme est en pause

-   halui.program.is-running (bit, out) - pin de status indiquant qu’un programme est lancé

-   halui.program.optional-stop.is-on (bit, out) - status pin telling that the optional stop is on

-   halui.program.optional-stop.off (bit, in) - pin requesting that the optional stop is off

-   halui.program.optional-stop.on (bit, in) - pin requesting that the optional stop is on

-   halui.program.pause (bit, in) - pin pour passer un programme en pause

-   halui.program.resume (bit, in) - pin pour lancer la reprise d’un programme

-   halui.program.run (bit, in) - pin de lancement d’un programme

-   halui.program.step (bit, in) - pin pour avancer d’une ligne de programme

-   halui.program.stop (bit, in) - pin pour stopper un programme

     Correcteur de vitesse de broche   
    (Spindle Override)

-   halui.spindle-override.count-enable (bit, in) - doit être vraie pour que *counts* ou *direct-value* soient opérationnels.

-   halui.spindle-override.counts (s32, in) - comptage depuis un codeur, par exemple pour modifier la correction de vitesse de broche

-   halui.spindle-override.decrease (bit, in) - pin pour diminuer la correction de vitesse de broche (-=scale)

-   halui.spindle-override.direct-value (bit, in) - fausse en utilisant un codeur pour modifier *counts* directement. La pin *count-enable* doit être vraie.

-   halui.spindle-override.increase (bit, in) - pin pour augmenter la correction de vitesse de broche (+=scale)

-   halui.spindle-override.scale (float, in) - pin pour positionner l'échelle des corrections de vitesse de broche possibles

-   halui.spindle-override.value (float, out) - Valeur courante de la correction de vitesse de broche

     Broche   
    (Spindle)

-   halui.spindle.brake-is-on (bit, out) - indique si le frein est actif

-   halui.spindle.brake-off (bit, in) - pin de désactivation du frein de broche

-   halui.spindle.brake-on (bit, in) - pin d’activation du frein de broche

-   halui.spindle.decrease (bit, in) - Diminue la vitesse de broche

-   halui.spindle.forward (bit, in) - Marche broche en sens horaire

-   halui.spindle.increase (bit, in) - Augmente la vitesse de broche

-   halui.spindle.is-on (bit, out) - indique la broche est en marche (les deux sens)

-   halui.spindle.reverse (bit, in) - Marche broche en sens anti-horaire

-   halui.spindle.runs-backward (bit, out) - indique la broche est en marche et en sens inverse

-   halui.spindle.runs-forward (bit, out) - indique la broche est en marche et en marche avant

-   halui.spindle.start (bit, in) - Marche de la broche

-   halui.spindle.stop (bit, in) - Arrêt de la broche

     Outil   
    (Tool)

-   halui.tool.length-offset (float, out) - indique la correction de longueur d’outil appliquée

-   halui.tool.number (u32, out) - indique l’outil courant sélectionné

Exemples de programme avec Halui
--------------------------------

Pour que ces exemples fonctionnent, il faut ajouter la ligne suivante dans la section \[HAL\] du fichier ini.

    HALUI = halui

### Démarrage à distance

Pour connecter un bouton de démarrage à distance à Machinekit il faut utiliser la pin *halui.program.run* et la pin *halui.mode.auto*.

Il faut s’assurer qu’il est possible de démarrer en utilisant la pin *halui.mode.is-auto*. On peut faire cela avec un composant de HAL *and2*. La figure suivante montre comment faire.

Quand le bouton de commande à distance est pressé, il est connecté à *halui.mode.auto* et à l’entrée *and2.0.in0*. Si le le mode auto est activé, la pin *halui.mode.is-auto* sera TRUE.

Si les deux entrées du composant *and2.0* sont TRUE, la sortie *and2.0.out* sera TRUE également et le programme sera démarré.

![](images/remote-start.png)

Figure 1. Exemple de commande distante

Les commandes de Hal pour accomplir ces actions sont les suivantes:

    net program-start-btn halui.mode.auto and2.0.in0 <= <la pin d'entrée>
    net program-run-ok and2.0.in1 <= halui.mode.is-auto
    net remote-program-run halui.program.run <= and2.0.out

Noter que sur la première ligne il y a deux pins en lecture, ce qui pourrait aussi se séparer en deux lignes comme ceci:

    net program-start-btn halui.mode.auto <= <la pin d'entrée>
    net program-start-btn and2.0.in0

### Pause et Reprise

Cet exemple a été developpé pour permettre à Machinekit de déplacer un axe rotatif selon un signal provenant d’une machine extérieure. La coordination entre les deux systèmes est assurée par deux composants de Halui:

-   halui.program.is-paused

-   halui.program.resume

Dans le fichier *custom.hal*, ajoutez les deux lignes suivantes qui seront connectées à vos entrées/sorties pour mettre le programme en pause ou pour le reprendre quand l’autre système veut qu’Machinekit soit relancé.

    net ispaused halui.program.is paused => "la pin de sortie"
    net resume halui.program.resume <= "la pin d'entrée"

Les pins d’entrée et de sortie, correspondent à celles qui sont câblées vers l’autre contrôleur. Elles peuvent être des broches du port parallèle ou toutes autres broches auquelles nous avons accès.

Le fonctionnement est le suivant, quand un M0 est rencontré dans le programme G-code, *halui.program.is-paused* devient TRUE. Ce qui rend la broche de sortie également TRUE de sorte que l’autre contrôleur sait que Machinekit est arrêté.

Pour reprendre l’exécution du G-code, l’autre contrôleur devra rendre l’entrée TRUE. Ce qui relancera Machinekit jusqu’au prochain M0.

Difficultés de timing

-   Le signal de reprise ne doit pas être plus long que le temps nécessaire pour exécuter le G-code.

-   Le signal *Is Paused* ne doit plus être actif quand le signal suivant de reprise arrive.

Ces problèmes de timming pourraient être évités, en utilisant ClassicLadder pour activer le signal *is paused* avec une tempo et le désactiver en fin de tempo. La reprise pourrait également être fournie par un signal monostable très court.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


