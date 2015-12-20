Variateur de fréquence GS2
==========================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

<span id="cha:Variateur-GS2"></span>

Composant de HAL pour la série de variateurs de fréquence GS2 fournie par la société Automation Direct. <span class="footnote">
\[ En Europe on trouve l'équivalent sous la marque Omron.\]
</span>

Chargement du composant
-----------------------

-   Ce composant est chargé en utilisant la commande suivante:

    loadusr -Wn spindle-vfd gs2_vfd -n spindle-vfd

La commande de HAL *loadusr* est détaillée au chapitre: [loadusr](#sec:loadusr).

Options spécifiques au chargement
---------------------------------

Les options spécifiques au chargement du composant gs2\_vfd:

-   *-b* ou *--bits &lt;n&gt;* (défaut 8) Fixe le nombre de bits de donnée à *&lt;n&gt;*, dans lequel *&lt;n&gt;* doit être compris entre 5 et 8 inclus.

-   *-d* ou *--device &lt;path&gt;* (défaut /dev/ttyS0) Fixe le nom de la liaison série à utiliser.

-   *-g* ou *--debug* Active les messages de débogage. Le drapeau du mode verbeux pourra être activé. Le débogage affichera tous les messages modbus en hexadécimal sur terminal.

-   *-n* ou *--name &lt;string&gt;* (défaut gs2\_vfd) Fixe le nom du composant de HAL à *&lt;string&gt;*, les noms de toutes ses pins et paramètres commenceront également par *&lt;string&gt;*.

-   *-p* ou *--parity {even, odd, none}* (défaut odd) Fixe la parité de la liaison série à parité paire, parité impaire ou sans parité.

-   *-r* ou *--rate &lt;n&gt;* (défaut 38400) Fixe le débit de la liaisons à *&lt;n&gt;*. C’est une erreur si le débit n’est pas une des valeurs suivantes: 110, 300, 600, 1200, 2400, 4800, 9600, 19200, 38400, 57600, 115200.

-   *-s* ou *--stopbits {1,2}* (défaut 1) Fixe le nombre de bits de stop de la liaison série à 1 ou 2.

-   *-t* ou *--target &lt;n&gt;* (défaut 1) Fixe le nombre de cibles MODBUS (esclaves). Doit correspondre au nombre de périphériques réglé dans le GS2.

-   *-v* ou *--verbose* Active les messages de débogage. Noter qu’en cas d’erreurs série, cela ne fera pas beaucoup de différence ce qui peut être gênant.

Consignes de dialogue avec le variateur
---------------------------------------

Les valeurs *&lt;name&gt;* sont les noms donnés par l’option *-n* durant la phase de chargement du composant.

-   *&lt;name&gt;.DC-bus-volts* (float, out) La tension du bus DC sur le variateur.

-   *&lt;name&gt;.at-speed* (bit, out) Quand la consigne vitesse est atteinte.

-   *&lt;name&gt;.err-reset* (bit, in) Envoi d’un *reset errors* au variateur.

-   *&lt;name&gt;.firmware-revision* (s32, out) envoyé par le variateur.

-   *&lt;name&gt;.frequency-command* (float, out) envoyé par le variateur.

-   *&lt;name&gt;.frequency-out* (float, out) envoyé par le variateur.

-   *&lt;name&gt;.is-stopped* (bit, out) when the VFD reports 0 Hz output.

-   *&lt;name&gt;.load-percentage* (float, out) envoyé par le variateur.

-   *&lt;name&gt;.motor-RPM* (float, out) envoyé par le variateur.

-   *&lt;name&gt;.output-current* (float, out) envoyé par le variateur.

-   *&lt;name&gt;.output-voltage* (float, out) envoyé par le variateur.

-   *&lt;name&gt;.power-factor* (float, out) envoyé par le variateur.

-   *&lt;name&gt;.scale-frequency* (float, out) envoyé par le variateur.

-   *&lt;name&gt;.speed-command* (float, in) Consigne vitesse envoyée. au variateur en tr.mn<sup>-1</sup>. C’est une erreur d’envoyer une consigne de vitesse supérieure à la valeur maximum réglée dans le variateur.

-   *&lt;name&gt;.spindle-fwd* (bit, in) Sens de rotation envoyé au variateur, 1 pour le sens horaire et 0 pour le sens anti-horaire.

-   *&lt;name&gt;.spindle-rev* (bit, in) 1 pour marche en sens anti-horaire et 0 pour ARRÊT.

-   *&lt;name&gt;.spindle-on* (bit, in) 1 pour MARCHE et 0 pour ARRÊT du variateur.

-   *&lt;name&gt;.status-1* (s32, out) Drive Status du VFD (voir le manuel du GS2).

-   *&lt;name&gt;.status-2* (s32, out) Drive Status du VFD (voir le manuel du GS2). Note: la valeur est la somme de tous les bits à 1. Ainsi, 163 signifie que le pilote est dans le mode de marche qui est la somme de:

    -   3 (marche)

    -   + 32 (fréquence fixée par liaison série)

    -   +128 (opération fixée par liaison série).

Paramètres de réglage du variateur
----------------------------------

Les valeurs *&lt;name&gt;* sont les noms donnés par l’option *-n* durant la phase de chargement du composant.

-   *&lt;name&gt;.error-count* (s32, RW)

-   *&lt;name&gt;.loop-time* (float, RW) Nombre d’interrogation d modbus (défaut 0.1).

-   *&lt;name&gt;.nameplate-HZ* (float, RW) Vitesse plaquée du moteur en Hz (défaut 50).

-   *&lt;name&gt;.nameplate-RPM* (float, RW) Vitesse plaquée du moteur en tr.mn<sup>-1</sup> (défaut 1500).

-   *&lt;name&gt;.retval* (s32, RW) la valeur de retour d’une erreur dans HAL.

-   *&lt;name&gt;.tolerance* (s32, RW) Tolérance en vitesse (défaut 0.01).

Un exemple d’utilisation d’un variateur de fréquence pour piloter une broche est donné dans le manuel de l’intégrateur au chapitre Exemples: utiliser un GS2.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


