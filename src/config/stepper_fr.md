Configuration d’un système pas/direction (dir/step)
===================================================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:config-steppers"></span>

Introduction
------------

Ce chapitre décrit quelques uns des réglages les plus fréquents, sur lesquels l’utilisateur aura à agir lors de la mise au point de Machinekit. En raison de l’adaptabilité de Machinekit, il serait très difficile de les documenter tous en gardant ce document relativement concis.

Le système rencontré le plus fréquemment chez les utilisateurs de Machinekit est un système à moteurs pas à pas. Les interfaces de pilotage de ces moteurs recoivent de Machinekit des signaux de pas et de direction.

C’est le système le plus simple à mettre en oeuvre parce que les moteurs fonctionnent en boucle ouverte (pas d’information de retour des moteurs), le système nécessite donc d'être configuré correctement pour que les moteurs ne perdent pas de pas et ne calent pas.

Ce chapitre s’appuie sur la configuration fournie d’origine avec Machinekit appelée *stepper* et qui se trouve habituellement dans */etc/machinekit/sample-configs/stepper*.

Fréquence de pas maximale
-------------------------

Avec la génération logicielle des pas la fréquence maximale en sortie, pour les impulsions de pas et de direction, est de une impulsion pour deux BASE\_PERIOD. La fréquence de pas maximale accessible pour un axe est le produit de MAX\_VELOCITY et de INPUT\_SCALE. Si la fréquence demandée est excessive, une erreur de suivi se produira (following error), particulièrement pendant les jog rapides et les mouvements en G0.

Si votre interface de pilotage des moteurs accepte des signaux d’entrée en quadrature, utilisez ce mode. Avec un signal en quadrature, un pas est possible pour chaque BASE\_PERIOD, ce qui double la fréquence maximum admissible.

Les autres remèdes consistent à diminuer une ou plusieurs variables: BASE\_PERIOD (une valeur trop faible peux causer un bloquage du PC), INPUT\_SCALE (s’il est possible sur l’interface de pilotage de sélectionner une taille de pas différente, de changer le rapport des poulies ou le pas de la vis mère), ou enfin MAX\_VELOCITY et STEPGEN\_MAXVEL.

Si aucune combinaison entre BASE\_PERIOD, INPUT\_SCALE et MAX\_VELOCITY n’est fonctionnelle, il faut alors envisager un générateur de pas externe (parmis les contrôleurs de moteurs pas à pas universels supportés par Machinekit)

Brochage
--------

Machinekit est très flexible et grâce à la couche d’abstraction de HAL (Hardware Abstraction Layer) il est facile de spécifier que tel signal ira sur telle broche.

Voir le tutoriel de HAL pour plus d’informations.

Comme décrit dans l’introduction et la manuel de HAL, il comporte des composants dont il fourni les signaux, les pins et les paramètres.

Les premiers signaux et pins relatifs au brochage sont:<span class="footnote">
\[Note: pour rester concis, nous ne présenterons qu’un seul axe, tous les autres sont similaires.\]
</span>

    signaux: Xstep, Xdir et Xen
    pins: parport.0.pin-XX-out & parport.0.pin-XX-in

Pour configurer le fichier ini, il est possible de choisir entre les deux brochages les plus fréquents, devenus des standards de fait, le brochage standard\_pinout.hal ou le brochage xylotex\_pinout.hal. Ces deux fichiers indiquent à HAL comment raccorder les différents signaux aux différentes pins. Dans la suite, nous nous concentrerons sur le brochage *standard\_pinout.hal*.

Le fichier standard\_pinout.hal<span id="sec:standard-pinout.hal"></span>
-------------------------------------------------------------------------

Ce fichier contient certaines commandes de HAL et habituellement ressemble à celà:

    # standard pinout config file for 3-axis steppers
    # using a parport for I/O
    #

    # first load the parport driver
    loadrt hal_parport cfg="0x0378"

    #
    # next connect the parport functions to threads
    # read inputs first
    addf parport.0.read base-thread 1

    # write outputs last
    addf parport.0.write base-thread -1

    #
    # finally connect physical pins to the signals
    net Xstep => parport.0.pin-03-out
    net Xdir  => parport.0.pin-02-out
    net Ystep => parport.0.pin-05-out
    net Ydir  => parport.0.pin-04-out
    net Zstep => parport.0.pin-07-out
    net Zdir  => parport.0.pin-06-out

    # create a signal for the estop loopback
    net estop-loop iocontrol.0.user-enable-out iocontrol.0.emc-enable-in

    # create signals for tool loading loopback
    net tool-prep-loop iocontrol.0.tool-prepare iocontrol.0.tool-prepared
    net tool-change-loop iocontrol.0.tool-change iocontrol.0.tool-changed

    # connect "spindle on" motion controller pin to a physical pin
    net spindle-on motion.spindle-on => parport.0.pin-09-out

    ###
    ### You might use something like this to enable chopper drives when machine ON
    ### the Xen signal is defined in core_stepper.hal
    ###
    # net Xen => parport.0.pin-01-out

    ###
    ### If you want active low for this pin, invert it like this:
    ###
    # setp parport.0.pin-01-out-invert 1

    ###
    ### A sample home switch on the X axis (axis 0).  make a signal,
    ### link the incoming parport pin to the signal, then link the signal
    ### to Machinekit's axis 0 home switch input pin
    ###
    # net Xhome parport.0.pin-10-in => axis.0.home-sw-in

    ###
    ### Shared home switches all on one parallel port pin?
    ### that's ok, hook the same signal to all the axes, but be sure to
    ### set HOME_IS_SHARED and HOME_SEQUENCE in the ini file.  See the
    ### user manual!
    ###
    # net homeswitches <= parport.0.pin-10-in
    # net homeswitches => axis.0.home-sw-in
    # net homeswitches => axis.1.home-sw-in
    # net homeswitches => axis.2.home-sw-in

    ###
    ### Sample separate limit switches on the X axis (axis 0)
    ###
    # net X-neg-limit parport.0.pin-11-in => axis.0.neg-lim-sw-in
    # net X-pos-limit parport.0.pin-12-in => axis.0.pos-lim-sw-in

    ###
    ### Just like the shared home switches example, you can wire together
    ### limit switches.  Beware if you hit one, Machinekit will stop but can't tell
    ### you which switch/axis has faulted.  Use caution when recovering from this.
    ###
    # net Xlimits parport.0.pin-13-in => axis.0.neg-lim-sw-in axis.0.pos-lim-sw-in

Les lignes commençant par **\#** sont des commentaires, aident à la lecture du fichier.

Vue d’ensemble du fichier standard\_pinout.hal
----------------------------------------------

Voici les opérations qui sont exécutées quand le fichier standard\_pinout.hal est lu par l’interpréteur:

1.  Le pilote du port parallèle est chargé (voir le Parport section de le Manuel de HAL pour plus de détails)

2.  Les fonctions de lecture/écriture du pilote sont assignée au thread «Base thread» <span class="footnote">
    \[Le thread le plus rapide parmis les réglages de Machinekit, habituellement il n’y a que quelques microsecondes entre les exécutions de ce code.\]
    </span>

3.  Les signaux du générateur de pas et de direction des axes X,Y,Z… sont raccordés aux broches du port parallèle

4.  D’autres signaux d’entrées/sorties sont connectés (boucle d’arrêt d’urgence, boucle du changeur d’outil…)

5.  Un signal de marche broche est défini et raccordé à une broche du port parallèle

Modifier le fichier standard\_pinout.hal
----------------------------------------

Pour modifier le fichier standard\_pinout.hal, il suffit de l’ouvrir dans un éditeur de texte puis d’y localiser les parties à modifier.

Si vous voulez par exemple, modifier les broches de pas et de direction de l’axe X, il vous suffit de modifier le numéro de la variable nommée *parport.0.pin-XX-out*:

    net Xstep parport.0.pin-03-out
    net Xdir  parport.0.pin-02-out

peut être modifiée pour devenir:

    net Xstep parport.0.pin-02-out
    net Xdir  parport.0.pin-03-out

ou de manière générale n’importe quel numéro que vous souhaiteriez.

Attention: il faut être certain de n’avoir qu’un seul signal connecté à une broche.

Modifier la polarité d’un signal
--------------------------------

Si une interface attends un signal *actif bas*, ajouter une ligne avec le paramètre d’inversion de la sortie, *-invert*. Par exemple, pour inverser le signal de rotation de la broche:

    setp parport.0.pin-09-invert TRUE

Ajouter le contrôle de vitesse broche en PWM
--------------------------------------------

Si votre vitesse de broche peut être contrôlée par un signal de PWM, utilisez le composant *pwmgen* pour créer ce signal:

    loadrt pwmgen output_type=0
    addf pwmgen.update servo-thread
    addf pwmgen.make-pulses base-thread
    net spindle-speed-cmd motion.spindle-speed-out => pwmgen.0.value
    net spindle-on motion.spindle-on => pwmgen.0.enable
    net spindle-pwm pwmgen.0.pwm => parport.0.pin-09-out
    setp pwmgen.0.scale 1800 # Change to your spindle’s top speed in RPM

Ce qui donnera le fonctionnement suivant, pour un signal PWM à: 0% donnera une vitesse de 0tr/mn, 10% une vitesse de 180tr/mn, etc. Si un signal PWM supérieur à 0% est requis pour que la broche commence à tourner, suivez l’exemple du fichier de configuration *nist-lathe* qui utilise un composant d'échelle (*scale*).

Ajouter un signal de validation **enable**
------------------------------------------

Certains pilotes de moteurs requiert un signal de validation *enable* avant d’autoriser tout mouvement du moteur. Pour celà des signaux sont déjà définis et appelés *Xen*, *Yen*, *Zen*.

Pour les connecter vous pouvez utilisez l’exemple suivant:

    net Xen parport.0.pin-08-out

Il est possible d’avoir une seule pin de validation pour l’ensemble des pilotes, ou plusieurs selon la configuration que vous voulez. Notez toutefois qu’habituellement quand un axe est en défaut, tous les autres sont invalidés aussi de sorte que, n’avoir qu’un seul signal/pin de validation pour l’ensemble est parfaitement sécurisé.

Ajouter un bouton d’Arrêt d’Urgence externe
-------------------------------------------

Comme vous pouvez [le voir ici](#sec:standard-pinout.hal), par défaut la configuration standard n’utilise pas de bouton d’Arrêt d’Urgence externe. footnote:\[Une explication complète sur la manière de gérer les circuiteries d’Arrêt d’Urgence se trouve sur le [wiki(en)](http://wiki.machinekit.org/) et dans le Manuel de l’intégrateur.

Pour ajouter un simple bouton d’AU externe (ou plusieurs en série) vous devez remplacer la ligne suivante:

    net estop-loop iocontrol.0.user-enable-out iocontrol.0.emc-enable-in

par

    net estop-loop parport.0.pin-01-in iocontrol.0.emc-enable-in

Ce qui implique qu’un bouton d’Arrêt d’Urgence soit connecté sur la broche 01 du port parallèle. Tant que le bouton est enfoncé (le contact ouvert)<span class="footnote">
\[Utiliser exclusivement des contacts normalement fermés pour les A/U.\]
</span>, Machinekit restera dans l'état *Arrêt d’Urgence* (ESTOP). Quand le bouton externe sera relâché, Machinekit passera immédiatement dans l'état *Arrêt d’Urgence Relâché* (ESTOP-RESET) vous pourrez ensuite mettre la machine en marche en pressant le bouton *Marche machine* et vous êtes alors prêt à continuer votre travail avec Machinekit.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


