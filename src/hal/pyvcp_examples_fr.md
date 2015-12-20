Exemples d’utilisation de PyVCP
===============================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

Panneau PyVCP dans AXIS
-----------------------

Procédure pour créer un panneau PyVCP et l’utiliser, attaché dans la partie droite de l’interface AXIS.

-   Créer un fichier .xml contenant la description du panneau et le placer dans le répertoire de la configuration.

-   Ajouter une entrée, avec le nom du fichier .xml, dans la section \[DISPLAY\] du fichier ini.

-   Ajouter une entrée POSTGUI\_HALFILE, avec le nom du fichier postgui HAL.

-   Ajouter les liens vers les pins de HAL pour le panneau dans le fichier postgui.hal pour "connecter" le panneau PyVCP avec Machinekit.

Panneaux flottants
------------------

Pour créer des panneaux flottants PyVCP pouvant être utilisés avec n’importe quelle interface, suivre les points suivants:

-   Créer un fichier .xml contenant la description du panneau et le placer dans le répertoire de configuration.

-   Ajouter les lignes *loadusr* dans le fichier.hal pour charger chaque panneau.

-   Ajouter les liens vers les pins de HAl du panneau dans le fichier postgui.hal, pour "connecter" les panneaux PyVCP à Machinekit.

L’exemple suivant montre le chargement de deux panneaux PyVCP.

    loadusr -Wn btnpanel pyvcp -c btnpanel panel1.xml
    loadusr -Wn sppanel pyvcp -c sppanel panel2.xml

Les paramètres -Wn font que HAL **W**ait for **n**ame, attends le composant nommé *btnpanel*.

Les paramètres pyvcp -c font que PyVCP nomme le panneau.

Les pins de HAL de panel1.xml seront nommées *btnpanel.&lt;pin name&gt;*

Les pins de HAL de panel2.xml seront nommées *sppanel.&lt;pin name&gt;*

Bien s’assurer qu’aucune ligne *loadusr* ne fasse déjà appel à une de ces pins PyVCP.

Boutons de Jog
--------------

Dans cet exemple nous allons créer un panneau PyVCP avec 3 boutons utilisables pour déplacer en manuel les axes X, Y et Z. Cette configuration sera réalisée avec l’assistant Stepconf qui générera la configuration de la machine. Premièrement, nous lançons l’assistant Stepconf et le configurons pour la machine, ensuite dans la page *Advanced Configuration Options* nous effectuons les sélections pour ajouter un panneau PyVCP vierge, comme indiqué sur l’image suivante. Pour cet exemple nous avons nommé la configuration *pyvcp\_xyz* sur la page *Basic Machine Information* de l’assistant Stepconf.

![](images/xyz_ACO.png)

Figure 1. Assistant de configuration XYZ<span id="cap:XYZ-Wizard-Configuration"></span>

L’assistant Stepconf Wizard va créer plusieurs fichiers et les placer dans le répertoire */emc/configs/pyvcp\_xyz*. Si la case *Créer un lien* est cochée, un lien vers ces fichiers sera créé sur le bureau.

### Créer les Widgets

Ouvrir le fichier custompanel.xml par un clic droit sur son nom, puis en sélectionnant *Ouvrir avec l'éditeur de texte*. Entre les balises *&lt;pyvcp&gt;* et *&lt;/pyvcp&gt;* nous ajouterons les widgets pour le panneau.

Tous les détails sur chacun des Widgets de PyVCP sont donnés dans la [documentation des widgets](#sec:Documentation-des-widgets).

Dans le fichier custompanel.xml nous ajoutons la description des widgets.

    <pyvcp>

        <labelframe text="Boutons de Jog">
           <font>("Helvetica",16)</font>

            <!-- le bouton de jog de l'axe X -->
            <hbox>
                <relief>RAISED</relief>
                <bd>3</bd>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"x-plus"</halpin>
                    <text>"X+"</text>
                </button>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"x-moins"</halpin>
                    <text>"X-"</text>
                </button>
            </hbox>

            <!-- le bouton de jog de l'axe Y -->
            <hbox>
                <relief>RAISED</relief>
                <bd>3</bd>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"y-plus"</halpin>
                    <text>"Y+"</text>
                </button>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"y-moins"</halpin>
                    <text>"Y-"</text>
                </button>
            </hbox>

            <!-- le bouton de jog de l'axe Z -->
            <hbox>
                <relief>RAISED</relief>
                <bd>3</bd>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"z-plus"</halpin>
                    <text>"Z+"</text>
                </button>
                <button>
                    <font>("Helvetica",20)</font>
                    <width>3</width>
                    <halpin>"z-moins"</halpin>
                    <text>"Z-"</text>
                </button>
            </hbox>

            <!-- le curseur de vitesse de jog -->
            <vbox>
                <relief>RAISED</relief>
                <bd>3</bd>
                <label>
                    <text>"Vitesse de Jog"</text>
                    <font>("Helvetica",16)</font>
                </label>
                <scale>
                    <font>("Helvetica",14)</font>
                    <halpin>"jog-speed"</halpin>
                    <resolution>1</resolution>
                    <orient>HORIZONTAL</orient>
                    <min_>0</min_>
                    <max_>80</max_>
                </scale>
            </vbox>
        </labelframe>
    </pyvcp>

Après les ajouts précédents, nous avons un panneau PyVCP tel que celui de l’image suivante, attaché à droite d’Axis. Il est beau mais ne fait rien tant que les boutons ne sont pas "connectés" à halui. Si, à ce stade, une erreur se produit lors du déplacement de la fenêtre vers le bas, c’est généralement dû à une erreur de syntaxe ou d'écriture, elle est donc dans cette partie qu’il conviendra tout d’abord de vérifier soigneusement.

![](images/xyz_buttons.png)

Figure 2. Boutons de Jog<span id="cap:Jog-Buttons"></span>

### Effectuer les connections

Pour effectuer les connections nécessaires, ouvrir le fichier custom\_postgui.hal et y ajouter le code suivant:

    # connecte les boutons PyVCP pour X
    net my-jogxmoins halui.jog.0.minus <= pyvcp.x-moins
    net my-jogxplus halui.jog.0.plus <= pyvcp.x-plus

    # connecte les boutons PyVCP pour Y
    net my-jogymoins halui.jog.1.minus <= pyvcp.y-moins
    net my-jogyplus halui.jog.1.plus <= pyvcp.y-plus

    # connecte les boutons PyVCP pour Z
    net my-jogzmoins halui.jog.2.minus <= pyvcp.z-moins
    net my-jogzplus halui.jog.2.plus <= pyvcp.z-plus

    # connecte le curseur de vitesse de jog PyVCP
    net my-jogspeed halui.jog-speed <= pyvcp.jog-speed-f

Après avoir désactivé l’A/U (E-Stop) et activé la marche machine en mode Jog, le déplacement du curseur du panneau PyVCP devrait agir dès qu’il est placé au delà de zéro et les boutons de jog devraient fonctionner. Il est impossible de jogger alors qu’un fichier G-code s’exécute ou pendant qu’il est en pause ni quand l’onglet *Données manuelles \[F5\]* du (MDI), est ouvert.

Testeur de port
---------------

Cet exemple montre comment faire un simple testeur de port parallèle en utilisant PyVCP et HAL.

Premièrement, créer le fichier ptest.xml qui contiendra le code suivant pour créer la description du panneau.

    <!-- Panneau de test pour la config. du port parallèle -->
    <pyvcp>
      <hbox>
        <relief>RIDGE</relief>
        <bd>2</bd>
        <button>
          <halpin>"btn01"</halpin>
          <text>"Pin 01"</text>
        </button>
        <led>
          <halpin>"led-01"</halpin>
          <size>25</size>
          <on_color>"green"</on_color>
          <off_color>"red"</off_color>
        </led>
      </hbox>
      <hbox>
        <relief>RIDGE</relief>
        <bd>2</bd>
        <button>
          <halpin>"btn02"</halpin>
          <text>"Pin 02"</text>
        </button>
        <led>
          <halpin>"led-02"</halpin>
          <size>25</size>
          <on_color>"green"</on_color>
          <off_color>"red"</off_color>
        </led>
      </hbox>
      <hbox>
        <relief>RIDGE</relief>
        <bd>2</bd>
        <label>
          <text>"Pin 10"</text>
          <font>("Helvetica",14)</font>
        </label>
        <led>
          <halpin>"led-10"</halpin>
          <size>25</size>
          <on_color>"green"</on_color>
          <off_color>"red"</off_color>
        </led>
      </hbox>
      <hbox>
        <relief>RIDGE</relief>
        <bd>2</bd>
        <label>
          <text>"Pin 11"</text>
          <font>("Helvetica",14)</font>
        </label>
        <led>
          <halpin>"led-11"</halpin>
          <size>25</size>
          <on_color>"green"</on_color>
          <off_color>"red"</off_color>
        </led>
      </hbox>
    </pyvcp>

Le panneau flottant contenant deux pins de HAL d’entrée et deux pins de HAL de sortie.

![](images/ptest.png)

Figure 3. Panneau flottant testeur de port parallèle<span id="cap:Port-Tester-Panel"></span>

Pour lancer les commandes de HAL dont nous avons besoin et démarrer tout ce qi’il nous faut, nous avons mis le code suivant dans notre fichier ptest.hal.

    loadrt hal_parport cfg="0x378 out"
    loadusr -Wn ptest pyvcp -c ptest ptest.xml
    loadrt threads name1=porttest period1=1000000
    addf parport.0.read porttest
    addf parport.0.write porttest
    net pin01 ptest.btn01 parport.0.pin-01-out ptest.led-01
    net pin02 ptest.btn02 parport.0.pin-02-out ptest.led-02
    net pin10 parport.0.pin-10-in ptest.led-10
    net pin11 parport.0.pin-11-in ptest.led-11
    start

Pour lancer le fichier HAL, nous utilisons, dans un terminal, les commandes suivantes:

    ~$ halrun -I -f ptest.hal

La figure suivante montre à quoi ressemble le panneau complet.

![](images/ptest-final.png)

Figure 4. Testeur de port parallèle, complet<span id="cap:Port-Tester-Complete"></span>

Pour ajouter le reste des pins du port parallèle, il suffi de modifier les fichiers .xml et .hal.

Pour visualiser les pins après avoir lancé le script HAL, utiliser la commande suivante au prompt *halcmd:*

    halcmd: show pin
    Component Pins:
    Owner Type  Dir Value  Name
        2 bit   IN  FALSE  parport.0.pin-01-out <== pin01
        2 bit   IN  FALSE  parport.0.pin-02-out <== pin02
        2 bit   IN  FALSE  parport.0.pin-03-out
        2 bit   IN  FALSE  parport.0.pin-04-out
        2 bit   IN  FALSE  parport.0.pin-05-out
        2 bit   IN  FALSE  parport.0.pin-06-out
        2 bit   IN  FALSE  parport.0.pin-07-out
        2 bit   IN  FALSE  parport.0.pin-08-out
        2 bit   IN  FALSE  parport.0.pin-09-out
        2 bit   OUT TRUE   parport.0.pin-10-in ==> pin10
        2 bit   OUT FALSE  parport.0.pin-10-in-not
        2 bit   OUT TRUE   parport.0.pin-11-in ==> pin11
        2 bit   OUT FALSE  parport.0.pin-11-in-not
        2 bit   OUT TRUE   parport.0.pin-12-in
        2 bit   OUT FALSE  parport.0.pin-12-in-not
        2 bit   OUT TRUE   parport.0.pin-13-in
        2 bit   OUT FALSE  parport.0.pin-13-in-not
        2 bit   IN  FALSE  parport.0.pin-14-out
        2 bit   OUT TRUE   parport.0.pin-15-in
        2 bit   OUT FALSE  parport.0.pin-15-in-not
        2 bit   IN  FALSE  parport.0.pin-16-out
        2 bit   IN  FALSE  parport.0.pin-17-out
        4 bit   OUT FALSE  ptest.btn01 ==> pin01
        4 bit   OUT FALSE  ptest.btn02 ==> pin02
        4 bit   IN  FALSE  ptest.led-01 <== pin01
        4 bit   IN  FALSE  ptest.led-02 <== pin02
        4 bit   IN  TRUE   ptest.led-10 <== pin10
        4 bit   IN  TRUE   ptest.led-11 <== pin11

Cela montre quelles pins sont IN est lesquelles sont OUT, ainsi que toutes les connections.

Compte tours pour GS2<span id="sec:Exemple-Compte-Tours-GS2"></span>
--------------------------------------------------------------------

L’exemple suivant utilise un variateur de fréquence GS2 de la société Automation Direct. <span class="footnote">
\[ En Europe on trouve ce type de variateur sous la marque Omron.\]
</span> Il permet le pilotage du moteur, la visualisation de la vitesse ainsi que d’autres informations dans un panneau PyVCP. Cet exemple est basé sur un autre, relatif au variateur GS2 et se trouvant dans la section des exemples matériels de ce manuel. Ce dernier exemple s’appuie lui même sur la description du composant de HAL gs2\_vfd.

### Le panneau

Pour créer le panneau nous ajoutons ce code au fichier .xml.

    <pyvcp>

        <!-- Compte tours -->
        <hbox>
            <relief>RAISED</relief>
            <bd>3</bd>
            <meter>
                <halpin>"spindle_rpm"</halpin>
                <text>"Broche"</text>
                <subtext>"tr/mn"</subtext>
                <size>200</size>
                <min_>0</min_>
                <max_>3000</max_>
                <majorscale>500</majorscale>
                <minorscale>100</minorscale>
                <region1>0,10,"yellow"</region1>
            </meter>
        </hbox>

        <!-- La Led On -->
        <hbox>
            <relief>RAISED</relief>
            <bd>3</bd>
            <vbox>
                <relief>RAISED</relief>
                <bd>2</bd>
                <label>
                    <text>"On"</text>
                    <font>("Helvetica",18)</font>
                </label>
                <width>5</width>
                <hbox>
                    <label width="2"/> <!-- utilisé pour centrer la Led -->
                    <rectled>
                        <halpin>"on-led"</halpin>
                        <height>"30"</height>
                        <width>"30"</width>
                        <on_color>"green"</on_color>
                        <off_color>"red"</off_color>
                    </rectled>
                </hbox>
            </vbox>

            <!-- La Led Sens horaire -->
            <vbox>
                <relief>RAISED</relief>
                <bd>2</bd>
                <label>
                    <text>"Sens horaire"</text>
                    <font>("Helvetica",18)</font>
                    <width>5</width>
                </label>
                <label width="2"/>
                <rectled>
                    <halpin>"fwd-led"</halpin>
                    <height>"30"</height>
                    <width>"30"</width>
                    <on_color>"green"</on_color>
                    <off_color>"red"</off_color>
                </rectled>
            </vbox>

            <!-- La Led Sens inverse -->
            <vbox>
                <relief>RAISED</relief>
                <bd>2</bd>
                <label>
                    <text>"Sens inverse"</text>
                    <font>("Helvetica",18)</font>
                    <width>5</width>
                </label>
                <label width="2"/>
                <rectled>
                    <halpin>"rev-led"</halpin>
                    <height>"30"</height>
                    <width>"30"</width>
                    <on_color>"red"</on_color>
                    <off_color>"green"</off_color>
                </rectled>
            </vbox>
        </hbox>
    </pyvcp>

L’image ci-dessous montre notre panneau PyVCP en fonctionnement.

![](images/gs2_panel.png)

Figure 5. Panneau pour GS2<span id="cap:Panneau-GS2"></span>

### Les connections

Pour qu’il fonctionne, il est nécessaire d’ajouter le code suivant au fichier custom\_postgui.hal, il réalise les connections entre PyVCP et Machinekit.

    # affiche le compte tours, calcul basé sur freq * rpm par hz
    loadrt mult2
    addf mult2.0 servo-thread
    setp mult2.0.in1 28.75
    net cypher_speed mult2.0.in0 <= spindle-vfd.frequency-out
    net speed_out pyvcp.spindle_rpm <= mult2.0.out

    # la led On
    net gs2-run => pyvcp.on-led

    # la led Sens horaire
    net gs2-fwd => pyvcp.fwd-led

    # la led Sens anti-horaire
    net running-rev spindle-vfd.spindle-rev => pyvcp.rev-led

Certaines lignes demandent quelques explications.

-   La ligne de la led Sens horaire utilise le signal créé dans le fichier custom.hal dans lequel la led Sens inverse doit utiliser le bit *spindle-rev*.

-   On ne *peut pas* lier deux fois le bit *spindle-fwd* pour utiliser le signal auquel il est déjà lié.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


