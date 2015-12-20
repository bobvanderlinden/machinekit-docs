Exemples pour HAL
=================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="Exemples-pour-HAL"></span>

Tous ces exemples s’appuient sur une configuration créée par Stepconf, elle a deux threads, *base-thread* et *servo-thread*.

L’assistant de configuration Stepconf aura créé le fichier vide *custom.hal* et le fichier *custom\_postgui.hal*.

Le fichier custom.hal sera chargé après le fichier de configuration de HAL, le fichier custom\_postgui.hal sera chargé après que l’interface graphique ne le soit.

Changement d’outil manuel
-------------------------

Dans cet exemple, il est supposé que la configuration a été réalisée et qu’il faut lui ajouter la fenêtre de changement d’outil de HAL. Le composant de changement d’outil manuel de HAL est surtout intéressant si les outils sont mesurables avec précision en longueur et que les offsets sont stockés dans la table d’outils. Si il est nécessaire de faire un *Toucher* pour chaque outil, il sera préférable de scinder le programme G-code en plusieurs tronçons. Pour utiliser la fenêtre de changement manuel d’outil de HAL, il faut d’abord charger le composant *hal\_manualtoolchange* puis envoyer l’ordre *iocontrol tool change* vers le *change* de hal\_manualtoolchange ainsi qu’envoyer le *changed* de hal\_manualtoolchange en retour sur le iocontrol tool changed.

Voici un exemple *avec* utilisation du composant de HAL, pour clarifier tout cela:

    loadusr -W hal_manualtoolchange
    net tool-change iocontrol.0.tool-change => hal_manualtoolchange.change
    net tool-changed iocontrol.0.tool-changed <= hal_manualtoolchange.changed
    net tool-number iocontrol.0.tool-prep-number => hal_manualtoolchange.number
    net tool-prepare-loopback iocontrol.0.tool-prepare => iocontrol.0.tool-prepared

Et voici un exemple *sans* le composant de changement manuel:

    net tool-number <= iocontrol.0.tool-prep-number
    net tool-change-loopback iocontrol.0.tool.-change => iocontrol.0.tool-changed
    net tool-prepare-loopback iocontrol.0.tool-prepare => iocontrol.0.tool-prepared

Calcul de vitesse
-----------------

Cet exemple utilise *ddt*, *mult2* et *abs* pour calculer la vitesse de déplacement sur un axe. Pour plus de détails, voir la section [sur les composants de HAL](#cha:Composants-de-HAL).

En premier il convient de vérifier si la configuration contient déjà des composants temps réel. Il est possible de le vérifier en ouvrant la fenêtre de Halshow et en cherchant les composants dans la section des pins. Si il y en a déjà en activité, il faudra augmenter leur nombre et ajuster l’instance de ces composants à la valeur correcte. Ajouter le code suivant dans le fichier *custom.hal*.

Charger les composants temps réel.

    loadrt ddt count=1
    loadrt mult2 count=1
    loadrt abs count=1

Ajouter les fonctions au thread pour qu’elles soient rafraîchies.

    addf ddt.0 servo-thread
    addf mult2.0 servo-thread
    addf abs.0 servo-thread

Faire les connections.

    setp mult2.in1 60
    net xpos-cmd ddt.0.in
    net X-IPS mult2.0.in0 <= ddt.0.out
    net X-ABS abs.0.in <= mult2.0.out
    net X-IPM abs.0.out

Dans la dernière section, nous avons fixé le *mult2.0.in1* à 60 pour convertir les unités par seconde en unités par minute (dans cet exemple, des pouces/mn), nous l’obtenons sur la sortie *ddt.0.out*.

La commande *xpos-cmd* envoie la position commandée à l’entrée *ddt.0.in*. Le *ddt* calcule la dérivée de la variation du signal sur son entrée.

La sortie ddt2.0.out est multipliée par 60 pour obtenir des unités par minute.

La sortie mult2.0.out est envoyée au composant *abs* pour obtenir la valeur absolue.

La figure suivante montre le résultat quand l’axe X se déplace à 15 unités/mn dans la direction négative. Noter que la valeur absolue peut être prise sur la pin *abs.0.out* ou le signal X-IPM.

Exemple avec la vitesse

![](images/velocity-01.png)

Amortissement d’un signal
-------------------------

Cette exemple montre comment les composants de HAL *lowpass*, *limit2* ou *limit3* peuvent être utilisés pour amortir de brusques changements d’un signal.

Nous sommes sur un tour dont la broche est pilotée par un servomoteur. Si nous envoyions directement la consigne de vitesse de broche sur le servo, celui-ci chercherait, à partir de la vitesse courante, à atteindre la vitesse commandée le plus vite possible. Cette situation est problématique et peut détériorer le matériel. Pour amortir ce changement de vitesse, nous pouvons faire passer la sortie *motion.spindle-speed-out* à travers un limiteur avant d’aller au PID, de sorte que la valeur de commande du PID soit amortie.

Les trois composants intégrés pour amortir le signal seront:

 limit2   
-   Limite le signal de sortie pour qu’il soit entre min et max.

-   Limite sa vitesse de montée à moins de MaxV par seconde. (dérivée première)

 limit3   
-   Limite le signal de sortie pour qu’il soit entre min et max.

-   Limite sa vitesse de montée à moins de MaxV par seconde. (dérivée première)

-   Limite sa vitesse de montée à moins de MaxV par seconde<sup>2</sup>. (dérivée seconde)

 lowpass   
-   Filtre passe-bas.

Pour plus de détails voir [les composants de HAL](#cha:Composants-de-HAL) ou les man pages des composants concernés.

Placer le code suivant dans un fichier appelé *softstart.hal*.

    loadrt threads period1=1000000 name1=thread
    loadrt siggen
    loadrt lowpass
    loadrt limit2
    loadrt limit3
    net square siggen.0.square => lowpass.0.in limit2.0.in limit3.0.in
    net lowpass <= lowpass.0.out
    net limit2 <= limit2.0.out
    net limit3 <= limit3.0.out
    setp siggen.0.frequency .1
    setp lowpass.0.gain .01
    setp limit2.0.maxv 2
    setp limit3.0.maxv 2
    setp limit3.0.maxa 10
    addf siggen.0.update thread
    addf lowpass.0 thread
    addf limit2.0 thread
    addf limit3.0 thread
    start
    loadusr halscope

Ouvrir un terminal et et lancer le fichier avec la commande suivante:

    halrun -I softstart.hal

Pour démarrer l’oscilloscope de HAL pour la première fois, cliquer *OK* pour accepter le thread par défaut.

Ensuite, il faut ajouter les signaux à suivre aux canaux du scope. Cliquer sur le canal *1* puis sélectionner *square* depuis l’onglet *Signaux*. Répéter pour les canaux suivants en ajoutant *lowpass*, *limit2* et *limit3*.

Ensuite, pour régler le signal du déclencheur cliquer sur le bouton *Source* est sélectionner *square*. Le bouton devrait changer pour *Source Canal 1*.

Puis, cliquer sur *Simple* dans le groupe *Mode Run*. L’oscillo devrait faire un balayage puis, afficher les traces.

Pour séparer les signaux et mieux les visualiser, cliquer sur un canal et utiliser le curseur de position verticale pour positionner les traces.

![](images/softstart-scope_fr.png)

Figure 1. Amortissement d’un signal carré<span id="cap:Softstart"></span>

Pour voir l’effet d’un changement du point de réglage des valeurs des composants, il est possible de passer des commandes depuis le terminal. Par exemple,pour voir différentes valeurs de gain pour le passe-bas, taper la commande suivante, puis essayer différentes valeurs:

    setp lowpass.0.gain .01

Après un changement de réglage, relancer Halscope pour visualiser l’effet.

Pour terminer, taper *exit* dans le terminal pour fermer halrun et halscope. Ne pas refermer le terminal avec halrun en marche, la mémoire ne serait pas vidée proprement, ce qui pourrait empêcher Machinekit de se charger.

Pour tout savoir sur Halscope et Halrun [voir le tutoriel de HAL](#sec:Intro-tutoriel).

HAL en autonome
---------------

Dans certains cas il peut être utile de lancer un écran GladeVCP avec juste HAL. Par exemple, nous avons un moteur pas à pas a piloter et tout ce qu’il nous faut pour notre application est une simple interface avec *Marche/Arrêt* plutôt que charger une application de CNC complète.

Dans l’exemple qui suit, nous allons créer ce simple panneau GladeVCP.

Syntaxe de base

    # charge l'interface graphique winder.glade et la nome winder
    loadusr -Wn winder gladevcp -c winder -u handler.py winder.glade

    # charge les composants temps réel
    loadrt threads name1=fast period1=50000 fp1=0 name2=slow period2=1000000
    loadrt stepgen step_type=0 ctrl_type=v
    loadrt hal_parport cfg="0x378 out"

    # ajoute les fonctions aux threads
    addf stepgen.make-pulses fast
    addf stepgen.update-freq slow
    addf stepgen.capture-position slow
    addf parport.0.read fast
    addf parport.0.write fast

    # effectue les connections de hal
    net winder-step parport.0.pin-02-out <= stepgen.0.step
    net winder-dir parport.0.pin-03-out <= stepgen.0.dir
    net run-stepgen stepgen.0.enable <= winder.start_button



    # démarre les threads
    start

    # commenter la ligne suivante pendant les essais et utiliser le mode interactif
    pour voir les pins etc.
    # option halrun -I -f start.hal

    # attends que la GUI gladevcp nommée winder soit terminée
    waitusr winder

    # arrête tous les threads
    stop

    # décharge tous les composants de HAL avant de quitter
    unloadrt all

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


