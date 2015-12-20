Les composants de HAL
=====================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Composants-de-HAL"></span>

Composants de commandes et composants de l’espace utilisateur
-------------------------------------------------------------

Certaines de ces descriptions sont plus approfondies dans leurs pages man. Certaines y auront une description exhaustive, d’autres, juste une description limitée. Chaque composant a sa man page. La liste ci-dessous, montre les composants existants, avec le nom et le N° de section de leur page man. Par exemple dans une console, tapez *man axis* pour accéder aux informations de la man page d’Axis. Ou peut être *man 1 axis*, si le système exige le N° de section des man pages.

-   *axis* - L’interface graphique AXIS pour Machinekit (The Enhanced Machine Controller).

-   *axis-remote* - Interface de télécommande d’AXIS.

-   *comp* - Crée, compile et installe des composants de HAL.

-   *machinekit* - LINUXCNC (The Enhanced Machine Controller).

-   *gladevcp* - Panneau de contrôle virtuel pour Machinekit, repose sur Glade, Gtk et les widgets HAL.

-   *gs2* - composant de l’espace utilisateur de HAL, pour le variateur de fréquence GS2 de la société *Automation Direct*.

-   *halcmd* - Manipulation de HAL, depuis la ligne de commandes.

-   *hal\_input* - Contrôler des pins d’entrée de HAL avec n’importe quelle matériel supporté par Linux, y compris les matériels USB HID.

-   *halmeter* - Observer les pins de HAL, ses signaux et ses paramètres.

-   *halrun* - Manipulation de HAL, depuis la ligne de commandes.

-   *halsampler* - Échantillonner des données temps réel depuis HAL.

-   *halstreamer* - Créer un flux de données temps réel dans HAL depuis un fichier.

-   *halui* - Observer des pins de HAL et commander Machinekit au travers d’NML.

-   *io* - Accepte les commandes NML I/O, interagi avec HAL dans l’espace utilisateur.

-   *iocontrol* - Accepte les commandes NML I/O, interagi avec HAL dans l’espace utilisateur.

-   *pyvcp* - Panneau de Contrôle Virtuel pour Machinekit (Python Virtual Control Panel).

-   *shuttlexpress* - Contrôle des pins de HAL avec la manette ShuttleXpress, de la société *Contour Design*.

Composants temps réel et modules du noyau
-----------------------------------------

Certaines de ces descriptions sont plus approfondies dans leur man page. Certaines auront juste une description limitée. Chaque composant a sa man page. A partir de cette liste vous connaîtrez quels composants existent avec le nom et le N° de leur man page permettant d’avoir plus de détails.

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
<td align="left">Si le composant requière un thread avec flottant, c’est normalement le plus lent, soit <em>servo-thread</em>.</td>
</tr>
</tbody>
</table>

### Composants du coeur de Machinekit<span id="sec:Realtime-Components-coeur"></span>

-   *motion* - Accepte les commandes de mouvement NML, interagi en temps réel avec HAL.

-   *axis* - Commandes de mouvement NML acceptées, intéragi en temps réel avec HAL

-   *classicladder* - Automate temps réel programmable en logique Ladder.

-   *gladevcp* - Affiche un panneaux de contrôle virtuel construit avec GladeVCP.

-   *threads* - Crée des threads de HAL temps réel.

### Composants binaires et logiques<span id="sec:Realtime-Components-logic"></span>

-   *and2* - Porte AND (ET) à deux entrées.

-   *not* - Inverseur.

-   *or2* - Porte OR (OU) à deux entrées.

-   *xor2* - Porte XOR (OU exclusif) à deux entrées.

-   *debounce* - Filtre une entrée digitale bruitée (typiquement antirebond).

-   *edge* \_ Détecteur de front.

-   *flipflop* - Bascule D.

-   *oneshot* - Générateur d’impulsion monostable. Crée sur sa sortie une impulsion de longueur variable quand son entrée change d'état.

-   *logic* - Composant expérimental de logique générale.

-   *lut5* - Fonction logique arbitraire à cinq entrées, basée sur une table de correspondance.

-   *match8* - Détecteur de coïncidence binaire sur 8 bits.

-   *select8* - Détecteur de coïncidence binaire sur 8 bits.

### Composants arithmétiques et flottants<span id="sec:Realtime-Components-flottant"></span>

-   *abs* - <span id="sub:abs"></span> Calcule la valeur absolue et le signe d’un signal d’entrée.

-   *blend* - Provoque une interpolation linéaire entre deux valeurs

-   *comp* - Comparateur à deux entrées avec hystérésis.

-   *constant* - Utilise un paramètre pour positionner une pin.

-   *sum2* - Somme de deux entrées (chacune avec son gain) et d’un offset.

-   *counter* - Comptage d’impulsions d’entrée (obsolète).

Utiliser le composant *encoder* avec *… counter-mode = TRUE*. Voir la section [codeur](#sec:Codeur).

-   *updown* - Compteur/décompteur avec limites optionnelles et bouclage en cas de dépassement.

-   *ddt* - Calcule la dérivée de la fonction d’entrée.

-   *deadzone* - Retourne le centre si il est dans le seuil.

-   *hypot* - Calculateur d’hypoténuse à trois entrées (distance Euclidienne).

-   *mult2* - Le produit de deux entrées.

-   *mux16* - Sélection d’une valeur d’entrée sur seize.

-   *mux2* - Sélection d’une valeur d’entrée sur deux.

-   *mux4* - Sélection d’une valeur d’entrée sur quatre.

-   *mux8* - Sélection d’une valeur d’entrée sur huit.

-   *near* - Détermine si deux valeurs sont à peu près égales.

-   *offset* - Ajoute un décalage à une entrée et la soustrait à la valeur de retour.

-   *integ* - Intégrateur.

-   *invert* - Calcule l’inverse du signal d’entrée.

-   *wcomp* - Comparateur à fenêtre.

-   *weighted\_sum* - Converti un groupe de bits en un entier.

-   *biquad* - Filtre biquad IIR

-   *lowpass* - Filtre passe-bas.

-   *limit1* - Limite le signal de sortie pour qu’il soit entre min et max. <span class="footnote">
    \[Lorsque l’entrée est une position, cela signifie que la *position* est limitée.\]
    </span>

-   *limit2* - Limite le signal de sortie pour qu’il soit entre min et max. Limite sa vitesse de montée à moins de MaxV par seconde. <span class="footnote">
    \[Lorsque l’entrée est une position, cela signifie que la *position* et la *vitesse* sont limitées.\]
    </span>

-   *limit3* - Limite le signal de sortie pour qu’il soit entre min et max. Limite sa vitesse de montée à moins de MaxV par seconde. Limite sa dérivée seconde à moins de MaxA par seconde carré. <span class="footnote">
    \[Lorsque l’entrée est une position, cela signifie que la *position*, la *vitesse* et l'*accélération* sont limitées.\]
    </span>

-   *maj3* - Calcule l’entrée majoritaire parmi 3.

-   *scale* - Applique une échelle et un décalage à son entrée.

### Conversions de type<span id="sec:Realtime-Components-conversiontype"></span>

-   *conv\_bit\_s32* - Converti une valeur de bit vers s32 (entier 32 bits signé).

-   *conv\_bit\_u32* - Converti une valeur de bit vers u32 (entier 32 bit non signé).

-   *conv\_float\_s32* - Converti la valeur d’un flottant vers s32.

-   *conv\_float\_u32* - Converti la valeur d’un flottant vers u32.

-   *conv\_s32\_bit* - Converti une valeur de s32 en bit.

-   *conv\_s32\_float* - Converti une valeur de s32 en flottant.

-   *conv\_s32\_u32* - Converti une valeur de s32 en u32.

-   *conv\_u32\_bit* - Converti une valeur de u32 en bit.

-   *conv\_u32\_float* - Converti une valeur de u32 en flottant.

-   *conv\_u32\_s32* - Converti une valeur de u32 en s32.

### Pilotes de matériel<span id="sec:Realtime-Components-pilotes"></span>

-   *hm2\_7i43* - Pilote HAL pour les cartes *Mesa Electronics* 7i43 EPP, toutes les cartes avec HostMot2.

-   *hm2\_pci* - Pilote HAL pour les cartes *Mesa Electronics* 5i20, 5i22, 5i23, 4i65 et 4i68, toutes les cartes avec micro logiciel HostMot2.

-   *hostmot2* - Pilote HAL pour micro logiciel *Mesa Electronics* HostMot2.

-   *mesa\_7i65* - Support pour la carte huit axes Mesa 7i65 pour servomoteurs.

-   *pluto\_servo* - Pilote matériel et micro programme pour la carte *Pluto-P parallel-port FPGA*, utilisation avec servomoteurs.

-   *pluto\_step* - Pilote matériel et micro programme pour la carte *Pluto-P parallel-port FPGA*, utilisation avec moteurs pas à pas.

-   *thc* - Contrôle de la hauteur de torche, en utilisant une carte Mesa THC.

-   *serport* - Pilote matériel pour les entrées/sorties numériques de port série avec circuits 8250 et 16550.

### Composants cinématiques<span id="sec:Realtime-Components-cinematiques"></span>

-   *kins* - Définition des cinématiques pour machinekit.

-   *gantrykins* - Module de cinématique pour un seul axe à articulations multiples.

-   *genhexkins* - Donne six degrés de liberté en position et en orientation (XYZABC). L’emplacement des moteurs est défini au moment de la compilation.

-   *genserkins* - Cinématique capable de modéliser une bras manipulateur avec un maximum de 6 articulations angulaires.

-   *maxkins* - Cinématique d’une fraiseuse 5 axes nommée *max*, avec tête inclinable (axe B) ​et un axe rotatif horizontal monté sur la table (axe C). Fourni les mouvements UVW dans le système de coordonnées système basculé. Le fichier source, maxkins.c, peut être un point de départ utile pour d’autres systèmes 5 axes.

-   *tripodkins* - Les articulations représentent la distance du point contrôlé à partir de trois emplacements prédéfinis (les moteurs), ce qui donne trois degrés de liberté en position (XYZ).

-   *trivkins* - Il y a une correspondance 1:1 entre les articulations et les axes. La plupart des fraiseuses standard et des tours utilisent ce module de cinématique triviale.

-   *pumakins* - Cinématique pour robot style PUMA.

-   *rotatekins* - Les axes X et Y sont pivotés de 45 degrés par rapport aux articulations 0 et 1.

-   *scarakins* - Cinématique des robots de type SCARA.

### Composants de contrôle moteur<span id="sec:Realtime-Components-moteur"></span>

-   *at\_pid* - Contrôleur Proportionnelle/Intégrale/dérivée avec réglage automatique.

-   *pid* - Contrôleur Proportionnelle/Intégrale/dérivée.

-   *pwmgen* - Générateur logiciel de PWM/PDM, voir la section [PWMgen](#sec:PWMgen)

-   *encoder* - Comptage logiciel de signaux de codeur en quadrature, voir la section [codeur](#sec:Codeur)

-   *stepgen* - Générateur d’impulsions de pas logiciel, voir la section [stepgen](#sec:Stepgen)

### BLDC and 3-phase motor control<span id="sec:Realtime-Components-bldc"></span>

-   *bldc\_hall3* - Commutateur bipolaire trapézoïdal à 3 directions pour moteur sans balais (BLDC) avec capteurs de Hall.

-   *clarke2* - Transformation de Clarke, version à deux entrées.

-   *clarke3* - Transformation de Clarke, à 3 entrées vers cartésien.

-   *clarkeinv* - Transformation de Clarke inverse.

### Autres composants<span id="sec:Realtime-Components-autres"></span>

-   *charge\_pump* - Crée un signal carré destiné à l’entrée *pompe de charge* de certaines cartes de contrôle. Le composant *charg\_pump* doit être ajouté à *base* *thread*. Quand il est activé, sa sortie est haute pour une période puis basse pour une autre période. Pour calculer la fréquence de sortie faire 1/(durée de la période en secondes \* 2) = fréquence en Hz. Par exemple, si vous avez une période de base de 100000ns soit 0.0001 seconde, la formule devient: 1/(0.0001 \* 2) = 5000 Hz ou 5kHz.

-   *encoder\_ratio* - Un engrenage électronique pour synchroniser deux axes.

-   *estop\_latch* - Verrou d’Arrêt d’Urgence.

-   *feedcomp* - Multiplie l’entrée par le ratio vitesse courante / vitesse d’avance travail.

-   *gearchange* - Sélectionne une grandeur de vitesse parmi deux.

-   *ilowpass* - Filtre passe-bas avec entrées et sorties au format entier.

    Sur une machine ayant une grande accélération, un petit jog peut s’apparenter à une avance par pas. En intercalant un filtre *ilowpass* entre la sortie de comptage du codeur de la manivelle et l’entrée *jog-counts* de l’axe, le mouvement se trouve lissé.

    Choisir prudemment l'échelle, de sorte que durant une simple session, elle ne dépasse pas environ 2e9/scale impulsions visibles sur le MPG. Choisir le gain selon le niveau de douceur désiré. Diviser les valeurs de axis.N.jog-scale par l'échelle.

-   *joyhandle* - Définit les mouvements d’un joypad non linéaire, zones mortes et échelles.

-   *knob2float* - Convertisseur de comptage (probablement d’un codeur) vers une valeur en virgule flottante.

-   *minmax* - Suiveur de valeurs minimum et maximum de l’entrée vers les sorties.

-   *sample\_hold* - Échantillonneur bloqueur.

-   *sampler* - Échantillonneur de données de HAL en temps réel.

-   *siggen* - Générateur de signal, voir la section [siggen](#sec:Siggen)

-   *sim\_encoder* - Codeur en quadrature simulé, voir la section [codeur simulé](#sec:Codeur-simul)

-   *sphereprobe* - Sonde hémisphérique.

-   *steptest* - Utilisé par Stepconf pour permettre de tester les valeurs d’accélération et de vitesse d’un axe.

-   *streamer* - Flux temps réel depuis un fichier vers HAL.

-   *supply* - Set output pins with values from parameters (obsolète).

-   *threadtest* - Composant de HAL pour tester le comportement des threads.

-   *time* - Compteur de temps écoulé HH:MM:SS avec entrée *actif*.

-   *timedelay* - L'équivalent d’un relais temporisé.

-   *timedelta* - Composant pour mesurer le comportement temporel des threads.

-   *toggle2nist* - Bouton à bascule pour logique NIST.

-   *toggle* - Bouton à bascule NO/NF à partir d’un bouton poussoir momentané.

-   *tristate\_bit* - Place un signal sur une pin d’I/O seulement quand elle est validée, similaire à un tampon trois états en électronique.

-   *tristate\_float* - Place un signal sur une pin d’I/O seulement quand elle est validée, similaire à un tampon trois états en électronique.

-   *watchdog* - Moniteur de fréquence (chien de garde) sur 1 à 32 entrées.

Appels à l’API de HAL (liste de la section 3 des man pages)
-----------------------------------------------------------

    hal_add_funct_to_thread.3hal
    hal_bit_t.3hal
    hal_create_thread.3hal
    hal_del_funct_from_thread.3hal
    hal_exit.3hal
    hal_export_funct.3hal
    hal_float_t.3hal
    hal_get_lock.3hal
    hal_init.3hal
    hal_link.3hal
    hal_malloc.3hal
    hal_param_bit_new.3hal
    hal_param_bit_newf.3hal
    hal_param_float_new.3hal
    hal_param_float_newf.3hal
    hal_param_new.3hal
    hal_param_s32_new.3hal
    hal_param_s32_newf.3hal
    hal_param_u32_new.3hal
    hal_param_u32_newf.3hal
    hal_parport.3hal
    hal_pin_bit_new.3hal
    hal_pin_bit_newf.3hal
    hal_pin_float_new.3hal
    hal_pin_float_newf.3hal
    hal_pin_new.3hal
    hal_pin_s32_new.3hal
    hal_pin_s32_newf.3hal
    hal_pin_u32_new.3hal
    hal_pin_u32_newf.3hal
    hal_ready.3hal
    hal_s32_t.3hal
    hal_set_constructor.3hal
    hal_set_lock.3hal
    hal_signal_delete.3hal
    hal_signal_new.3hal
    hal_start_threads.3hal
    hal_type_t.3hal
    hal_u32_t.3hal
    hal_unlink.3hal
    intro.3hal
    undocumented.3hal

Appels à RTAPI
--------------

    EXPORT_FUNCTION.3rtapi
    MODULE_AUTHOR.3rtapi
    MODULE_DESCRIPTION.3rtapi
    MODULE_LICENSE.3rtapi
    RTAPI_MP_ARRAY_INT.3rtapi
    RTAPI_MP_ARRAY_LONG.3rtapi
    RTAPI_MP_ARRAY_STRING.3rtapi
    RTAPI_MP_INT.3rtapi
    RTAPI_MP_LONG.3rtapi
    RTAPI_MP_STRING.3rtapi
    intro.3rtapi
    rtapi_app_exit.3rtapi
    rtapi_app_main.3rtapi
    rtapi_clock_set_period.3rtapi
    rtapi_delay.3rtapi
    rtapi_delay_max.3rtapi
    rtapi_exit.3rtapi
    rtapi_get_clocks.3rtapi
    rtapi_get_msg_level.3rtapi
    rtapi_get_time.3rtapi
    rtapi_inb.3rtapi
    rtapi_init.3rtapi
    rtapi_module_param.3rtapi
    RTAPI_MP_ARRAY_INT.3rtapi
    RTAPI_MP_ARRAY_LONG.3rtapi
    RTAPI_MP_ARRAY_STRING.3rtapi
    RTAPI_MP_INT.3rtapi
    RTAPI_MP_LONG.3rtapi
    RTAPI_MP_STRING.3rtapi
    rtapi_mutex.3rtapi
    rtapi_outb.3rtapi
    rtapi_print.3rtap
    rtapi_prio.3rtapi
    rtapi_prio_highest.3rtapi
    rtapi_prio_lowest.3rtapi
    rtapi_prio_next_higher.3rtapi
    rtapi_prio_next_lower.3rtapi
    rtapi_region.3rtapi
    rtapi_release_region.3rtapi
    rtapi_request_region.3rtapi
    rtapi_set_msg_level.3rtapi
    rtapi_shmem.3rtapi
    rtapi_shmem_delete.3rtapi
    rtapi_shmem_getptr.3rtapi
    rtapi_shmem_new.3rtapi
    rtapi_snprintf.3rtapi
    rtapi_task_delete.3rtpi
    rtapi_task_new.3rtapi
    rtapi_task_pause.3rtapi
    rtapi_task_resume.3rtapi
    rtapi_task_start.3rtapi
    rtapi_task_wait.3rtapi

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


