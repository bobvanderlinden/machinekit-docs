G Código de Referencia
======================

<span id="author">(((G Codes)))</span>

Convenciones utilizadas en esta sección

En los prototipos Código G del guión (`-`) representa un valor real.

El valor real puede ser:

-   Un número explícito, `4`

-   Una expresión, `[2 +2]`

-   Un parámetro de valor, *\# 88*

-   Un valor de la función unario, \`acos \[0\] '

En la mayoría de los casos, es decir si el eje está dado (Cualquiera o todos los `XYZABCUVW`), que especifica un punto de destino.

Número de ejes en el sistema de coordenadas activo, a menos que explícitamente describen como en el sistema de coordenadas absolutas. Donde las palabras del eje son opcionales, los ejes se omite tendrá su valor actual.

Todos los elementos en los prototipos de código G no se describe explícitamente como opcional son obligatorios.

Los siguientes valores se dan a menudo las letras como los números explícitos. A menos que se indique lo contrario, los números pueden ser explícitos los valores reales. Para ejemplo, `G-10 L2` también podría ser por escrito `G[2 * 5] L[1 +1]`. Si el valor del parámetro 100 a los 2, `G10 L#100` también significan lo mismo.

Si la opción `-L` está escrito en un prototipo del `-` a menudo se hace referencia como el "número L", y así sucesivamente para cualquier otra letra.

Coordenadas polares
-------------------

Las coordenadas polares se puede utilizar para especificar las coordenadas XY de un movimiento. El n @ es la distancia y ^n es el ángulo. La ventaja de esto es para cosas como los círculos de taladros que se puede hacer muy simplemente llegando a un punto en el centro del círculo, el establecimiento de la compensación y luego pasar a primer agujero a continuación, ejecutar el ciclo de perforación. Las coordenadas polares son siempre desde la posición cero de la corriente XY. Para cambiar las coordenadas polares de cero máquina utiliza un desplazamiento o seleccione un sistema de coordenadas.

En el modo absoluto de la distancia y el ángulo es de la posición cero XY y el ángulo empieza con 0 en el eje X positivo y aumenta en un CCW dirección sobre el eje Z. El G1 de código @ 1 ^ 90 es el mismo que G1 Y1.

En el modo relativo a la distancia y el ángulo es de cero XY posición, pero es acumulativa. Esto puede ser confuso al principio, cómo funciona esto en el modo incremental.

Por ejemplo, si usted tiene el siguiente programa se puede esperar que ser un patrón cuadrado.

    G1 F100 @ 0,5 ^ 90
    G91 @ 0,5 ^ 90
    @ 0.5 ^ 90
    @ 0.5 ^ 90
    @ 0.5 ^ 90
    G90 G0 X0 Y0 M2

Se puede ver en la siguiente figura que la salida no es lo que se podría esperar. Debido a que hemos agregado 0,5 a la distancia cada vez que el distancia desde la posición cero XY aumentó con cada línea.

![](images/polar01.png)

Figure 1. Espiral Polar<span id="fig:Polar-Espiral"></span>

El siguiente código producirá nuestro patrón cuadrado.

    G1 F100 @ 0,5 ^ 90
    G91 ^ 90
    ^ 90
    ^ 90
    ^ 90
    G90 G0 X0 Y0 M2

Como se puede ver con sólo añadir que el ángulo de 90 grados cada vez que el distancia de punto final es el mismo para cada línea.

![](images/polar02.png)

Figure 2. Polar Plaza de<span id="fig:Polar-Plaza"></span>

Es un error si:

-   Un movimiento incremental que se inicia en el origen

-   Una mezcla de Polar y X y Y o palabras se utilizan

Tabla de Referencia Rápida<span id="sec:Tabla-de-Referencia-Rapida"></span>
---------------------------------------------------------------------------

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Código</th>
<th align="left">Descripción</th>
<th align="left">Sección</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>G0</p></td>
<td align="left"><p>movimiento coordinado recta rápida</p></td>
<td align="left"><p><a href="#sec:G0-Rapido">[sec:G0-Rapido]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G1</p></td>
<td align="left"><p>Coordinado movimiento recto Velocidad de alimentación</p></td>
<td align="left"><p><a href="#sec:G1-lineal-de-movimiento">[sec:G1-lineal-de-movimiento]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G2, G3</p></td>
<td align="left"><p>movimiento coordinado helicoidal velocidad de alimentación</p></td>
<td align="left"><p><a href="#sec:G2-G3-Arco">[sec:G2-G3-Arco]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G4</p></td>
<td align="left"><p>Mora</p></td>
<td align="left"><p><a href="#sec:G4-Mora">[sec:G4-Mora]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G5.1</p></td>
<td align="left"><p>cuadráticas B-Spline</p></td>
<td align="left"><p><a href="#sec:G5.1-B-spline-cuadratica">[sec:G5.1-B-spline-cuadratica]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G5.2, G5.3</p></td>
<td align="left"><p>NURBS Bloquear</p></td>
<td align="left"><p><a href="#sec:G5.2-G5.3-NURBs">[sec:G5.2-G5.3-NURBs]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G7</p></td>
<td align="left"><p>Diámetro Mode (torno)</p></td>
<td align="left"><p><a href="#sec:G7-Diametro-Modo">[sec:G7-Diametro-Modo]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G8</p></td>
<td align="left"><p>Radio Mode (torno)</p></td>
<td align="left"><p><a href="#sec:G8-Radio-Mode">[sec:G8-Radio-Mode]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G10 L1</p></td>
<td align="left"><p>Juego de herramientas de entrada de la tabla</p></td>
<td align="left"><p><a href="#sec:G10-L1_">[sec:G10-L1_]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G10 L10</p></td>
<td align="left"><p>Establecer tabla de herramientas, calculado, piezas</p></td>
<td align="left"><p><a href="#sec:G10-L10">[sec:G10-L10]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G10 L11</p></td>
<td align="left"><p>Establecer tabla de herramientas, calculado, Fixture</p></td>
<td align="left"><p><a href="#sec:G10-L11">[sec:G10-L11]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G10 L2</p></td>
<td align="left"><p>Sistema de coordenadas Configuración de Origen</p></td>
<td align="left"><p><a href="#sec:G10-L2_">[sec:G10-L2_]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G10 L20</p></td>
<td align="left"><p>Sistema de coordenadas Configuración de Origen Calculado</p></td>
<td align="left"><p><a href="#sec:G10-L20">[sec:G10-L20]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G17 - G19.1</p></td>
<td align="left"><p>Plano Seleccionar</p></td>
<td align="left"><p><a href="#sec:G17-G18-G19">[sec:G17-G18-G19]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G20, G21</p></td>
<td align="left"><p>Unidades de Medida</p></td>
<td align="left"><p><a href="#sec:G20-G21-Unidades">[sec:G20-G21-Unidades]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G28 - G28.1</p></td>
<td align="left"><p>Ir a las posiciones predefinidas</p></td>
<td align="left"><p><a href="#sec:G28-G28.1">[sec:G28-G28.1]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G30 - G30.1</p></td>
<td align="left"><p>Ir a las posiciones predefinidas</p></td>
<td align="left"><p><a href="#sec:G30-G30.1">[sec:G30-G30.1]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G33</p></td>
<td align="left"><p>Movimiento eje sincronizado</p></td>
<td align="left"><p><a href="#sec:G33-eje-Sync">[sec:G33-eje-Sync]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G33.1</p></td>
<td align="left"><p>Roscado</p></td>
<td align="left"><p><a href="#sec:G33.1-rigido-Tapping">[sec:G33.1-rigido-Tapping]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G38.2 - G38.5</p></td>
<td align="left"><p>Sondeo</p></td>
<td align="left"><p><a href="#sec:G38.x-Straight-Probe">[sec:G38.x-Straight-Probe]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G40</p></td>
<td align="left"><p>Compensación de corte cancelar</p></td>
<td align="left"><p><a href="#sec:G40">[sec:G40]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G41, G42</p></td>
<td align="left"><p>Cortador de Compensación</p></td>
<td align="left"><p><a href="#sec:G41-G42">[sec:G41-G42]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G41.1, G42.1</p></td>
<td align="left"><p>Compensación de corte transitorio</p></td>
<td align="left"><p><a href="#sec:G41.1-G42.1">[sec:G41.1-G42.1]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G43, G43.1</p></td>
<td align="left"><p>El uso de herramientas desplazamiento de longitud de la tabla de Herr</p></td>
<td align="left"><p><a href="#sec:G43-G43.1-Tool">[sec:G43-G43.1-Tool]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G49</p></td>
<td align="left"><p>Duración herramienta Cancelar offset</p></td>
<td align="left"><p><a href="#sub:G49-herramientas">[sub:G49-herramientas]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G53</p></td>
<td align="left"><p>movimiento en la máquina del sistema de coordenadas</p></td>
<td align="left"><p><a href="#sub:G53-Move-in">[sub:G53-Move-in]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G54-G59</p></td>
<td align="left"><p>Seleccionar sistema de coordenadas (1-6)</p></td>
<td align="left"><p><a href="#sec:G54-G59.3">[sec:G54-G59.3]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G59.1-G59.3</p></td>
<td align="left"><p>Seleccionar sistema de coordenadas (7-9)</p></td>
<td align="left"><p><a href="#sec:G54-G59.3">[sec:G54-G59.3]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G61, G61.1</p></td>
<td align="left"><p>modo de control de ruta</p></td>
<td align="left"><p><a href="#sec:G61-G61.1-G64">[sec:G61-G61.1-G64]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G64</p></td>
<td align="left"><p>modo de control de ruta con la tolerancia opcional</p></td>
<td align="left"><p><a href="#sec:G61-G61.1-G64">[sec:G61-G61.1-G64]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G73</p></td>
<td align="left"><p>Ciclo de taladrado con rotura de virutas</p></td>
<td align="left"><p><a href="#sec:G73-Perforacion-del-ciclo">[sec:G73-Perforacion-del-ciclo]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G76</p></td>
<td align="left"><p>Multipass Threading de ciclo (Tornos)</p></td>
<td align="left"><p><a href="#sec:G76-Threading-Canned">[sec:G76-Threading-Canned]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G80</p></td>
<td align="left"><p>Cancelar movimiento Modos</p></td>
<td align="left"><p><a href="#sec:G80-Cancelar-Modal">[sec:G80-Cancelar-Modal]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G81</p></td>
<td align="left"><p>Ciclo de taladrado</p></td>
<td align="left"><p><a href="#sec:G81-Perforacion-del-ciclo">[sec:G81-Perforacion-del-ciclo]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G82</p></td>
<td align="left"><p>Ciclo de taladrado con temporización</p></td>
<td align="left"><p><a href="#sec:G82-Perforacion-Dwell">[sec:G82-Perforacion-Dwell]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G83</p></td>
<td align="left"><p>Ciclo de taladrado con Peck</p></td>
<td align="left"><p><a href="#sec:G83-Perforacion-Peck">[sec:G83-Perforacion-Peck]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G84</p></td>
<td align="left"><p>Mano Derecha * Tapping ciclo (no implementada) *</p></td>
<td align="left"><p><a href="#sec:G84-de-la-mano-derecha-Tapping">[sec:G84-de-la-mano-derecha-Tapping]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G85</p></td>
<td align="left"><p>Ciclo de aburrido, no de permanencia, salida de alimentación</p></td>
<td align="left"><p><a href="#sec:G85-aburrido-Feed-Out">[sec:G85-aburrido-Feed-Out]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G86</p></td>
<td align="left"><p>Ciclo aburrido, Stop, salida rápida</p></td>
<td align="left"><p><a href="#sec:G86-aburrido-Rapid-Out">[sec:G86-aburrido-Rapid-Out]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G87</p></td>
<td align="left"><p>Back-Aburrido Ciclo * (no implementada) *</p></td>
<td align="left"><p><a href="#sec:G87-Back-Aburrido">[sec:G87-Back-Aburrido]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G88</p></td>
<td align="left"><p>Ciclo aburrido, Stop, Manl Out * (no implementada) *</p></td>
<td align="left"><p><a href="#sec:G88-aburrido-Manual-Out">[sec:G88-aburrido-Manual-Out]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G89</p></td>
<td align="left"><p>Ciclo aburrido, permanencia, salida de alimentación</p></td>
<td align="left"><p><a href="#sec:G89-aburrido-Dwell">[sec:G89-aburrido-Dwell]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G90, G91</p></td>
<td align="left"><p>Modo Distancia</p></td>
<td align="left"><p><a href="#sec:G90-G91-Set">[sec:G90-G91-Set]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G90.1, G91.1</p></td>
<td align="left"><p>Modo Distancia Arco</p></td>
<td align="left"><p><a href="#sec:G90.1-G91.1">[sec:G90.1-G91.1]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G92</p></td>
<td align="left"><p>de los sistemas de coordenadas y parámetros de Set</p></td>
<td align="left"><p><a href="#sec:G92-G92.1-G92.2-G92.3">[sec:G92-G92.1-G92.2-G92.3]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G92.1</p></td>
<td align="left"><p>Compensaciones cancelar</p></td>
<td align="left"><p><a href="#sec:G92-G92.1-G92.2-G92.3">[sec:G92-G92.1-G92.2-G92.3]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G92.2</p></td>
<td align="left"><p>Compensaciones cancelar</p></td>
<td align="left"><p><a href="#sec:G92-G92.1-G92.2-G92.3">[sec:G92-G92.1-G92.2-G92.3]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G92.3</p></td>
<td align="left"><p>Aplicar los parámetros de compensación de sistemas de coordenadas</p></td>
<td align="left"><p><a href="#sec:G92-G92.1-G92.2-G92.3">[sec:G92-G92.1-G92.2-G92.3]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G93, G94, G95</p></td>
<td align="left"><p>Modos de alimentación</p></td>
<td align="left"><p><a href="#sec:G93-G94-G95-modo">[sec:G93-G94-G95-modo]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G96</p></td>
<td align="left"><p>velocidad de corte constante</p></td>
<td align="left"><p><a href="#sec:G96-G97-eje">[sec:G96-G97-eje]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>G97</p></td>
<td align="left"><p>RPM Mode</p></td>
<td align="left"><p><a href="#sec:G96-G97-eje">[sec:G96-G97-eje]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>G98, G99</p></td>
<td align="left"><p>Ciclo enlatados Z retracción Simple</p></td>
<td align="left"><p><a href="#sec:G98-G99-Set">[sec:G98-G99-Set]</a></p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Código</th>
<th align="left">Descripción</th>
<th align="left">Sección</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>M0, M1, M2</p></td>
<td align="left"><p>Programa de Control</p></td>
<td align="left"><p><a href="#sec:M0-M1-M2">[sec:M0-M1-M2]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>M3, M4, M5</p></td>
<td align="left"><p>Eje de Control</p></td>
<td align="left"><p><a href="#sec:M3-M4-M5">[sec:M3-M4-M5]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>M6</p></td>
<td align="left"><p>Herramienta de Cambio</p></td>
<td align="left"><p><a href="#sec:M6-Tool-Cambio">[sec:M6-Tool-Cambio]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>M7, M8, M9</p></td>
<td align="left"><p>Refrigerante de Control</p></td>
<td align="left"><p><a href="#sec:M7-M8-M9">[sec:M7-M8-M9]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>M30, M60</p></td>
<td align="left"><p>Shuttle Palet</p></td>
<td align="left"><p><a href="#sec:M0-M1-M2">[sec:M0-M1-M2]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>M48</p></td>
<td align="left"><p>Controles anulación</p></td>
<td align="left"><p><a href="#sub:M48-Ambos-Override">[sub:M48-Ambos-Override]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>M49</p></td>
<td align="left"><p>Controles anulación</p></td>
<td align="left"><p><a href="#sub:M49-Ni-Anular">[sub:M49-Ni-Anular]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>M50</p></td>
<td align="left"><p>Controles anulación</p></td>
<td align="left"><p><a href="#sub:M50-Feed-Anular">[sub:M50-Feed-Anular]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>M51</p></td>
<td align="left"><p>Controles anulación</p></td>
<td align="left"><p><a href="#sub:M51-eje-Anular">[sub:M51-eje-Anular]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>M52</p></td>
<td align="left"><p>Controles anulación</p></td>
<td align="left"><p><a href="#sub:M52-adaptativa-Feed-Control">[sub:M52-adaptativa-Feed-Control]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>M53</p></td>
<td align="left"><p>Controles anulación</p></td>
<td align="left"><p><a href="#sub:M53-Feed-Stop-Control">[sub:M53-Feed-Stop-Control]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>M61</p></td>
<td align="left"><p>Establecer Herramienta Número actual</p></td>
<td align="left"><p><a href="#sec:M61-Set-actual-Tool-Numero">[sec:M61-Set-actual-Tool-Numero]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>M62-65</p></td>
<td align="left"><p>Control de Salida</p></td>
<td align="left"><p><a href="#sec:M62-a-M65">[sec:M62-a-M65]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>M66</p></td>
<td align="left"><p>Entrada de control</p></td>
<td align="left"><p><a href="#sec:M66-Input-Control">[sec:M66-Input-Control]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>M67</p></td>
<td align="left"><p>Control de salida analógica</p></td>
<td align="left"><p><a href="#sec:M67-analogico-de-salida">[sec:M67-analogico-de-salida]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>M68</p></td>
<td align="left"><p>Control de salida analógica</p></td>
<td align="left"><p><a href="#sec:M68-analogico%20de%20salida">[sec:M68-analogico de salida]</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p>M100-M199</p></td>
<td align="left"><p>Definido por el usuario M-Codes</p></td>
<td align="left"><p><a href="#sec:M100-a-M199">[sec:M100-a-M199]</a></p></td>
</tr>
<tr class="even">
<td align="left"><p>S</p></td>
<td align="left"><p>S</p></td>
<td align="left"><p>Códigos</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#cha:O-codigos">[cha:O-codigos]</a></p></td>
<td align="left"><p>F</p></td>
<td align="left"><p>Feed</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sub:F-Set-Feed">[sub:F-Set-Feed]</a></p></td>
<td align="left"><p>S</p></td>
<td align="left"><p>Velocidad del eje</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sub:S-Set-eje">[sub:S-Set-eje]</a></p></td>
<td align="left"><p>T</p></td>
<td align="left"><p>Herramienta de selección</p></td>
</tr>
</tbody>
</table>

G0 movimiento rápido<span id="sec:G0-Rapido"></span>
----------------------------------------------------

    G0 ejes

Para una rápida lineal (línea recta) de movimiento, el programa `G0 'ejes'`, donde todas las palabras del eje son opcionales. La `G0` es opcional si la corriente modo de movimiento es `G0`. Esto produce un movimiento coordinado lineal el punto de destino en la actual velocidad de desplazamiento (o menos si la máquina no va a ir tan rápido). Se espera que la reducción de no se llevará a cabo cuando un comando `G0` se está ejecutando.

Si la compensación de radio de la fresa está activo, el movimiento será diferente de lo anterior, véase la sección [\[sec:Cutter-Radius-Compensation\]](#sec:Cutter-Radius-Compensation).

Si `G53` está programado en la misma línea, el movimiento también será diferente; véase la sección [\[sub:G53-Move-in\]](#sub:G53-Move-in).

Es un error si:

-   Una carta de eje sin un valor real.

G1 Linear Motion<span id="sec:G1-lineal-de-movimiento"></span>
--------------------------------------------------------------

    G1 ejes

Para lineal (línea recta) de movimiento en la velocidad de avance programada (para el corte o no), el programa `G1 'ejes'`, donde todas las palabras del eje son opcionales. El `G1` es opcional si el modo de movimiento actual es `G1`. Esto producir un movimiento coordinado lineal al punto de destino en la velocidad de avance actual (o más lento si la máquina no va a ir por ese rápido).

Si la compensación de radio de la fresa está activo, el movimiento será diferente de lo anterior, véase la sección [\[sec:Cutter-Radius-Compensation\]](#sec:Cutter-Radius-Compensation).

Si `G53` está programado en la misma línea, el movimiento también será diferente; véase la sección [\[sub:G53-Move-in\]](#sub:G53-Move-in).

Es un error si:

-   No hay velocidad de avance se ha establecido.

G2, G3 Arco<span id="sec:G2-G3-Arco"></span>
--------------------------------------------

Un arco circular o helicoidal se especifica utilizando `G2` (a la derecha arco) o `G3` (a la izquierda del arco). La dirección (CW, CCW) es tan visto desde el lado positivo del eje sobre el que el movimiento circular se produce.

El eje del círculo o espiral debe ser paralelo a la X, Y, o Z del sistema de coordenadas máquina. El eje (o, equivalentemente, el plano perpendicular al eje) se selecciona con `G17` (Z-eje, plano XY), `G18` (eje Y, XZ plano), o `G19` (eje X, YZ-avión). Planes `17.1`, `18.1`, y `19.1` no están soportados. Si el arco es circular, se encuentra en un plano paralelo al plano seleccionado.

Para programar una hélice, incluya la palabra eje perpendicular al arco Avión: por ejemplo, si en el `G17` de avión, incluyen una `Z` word. Este hará que la `Z` eje para pasar al valor programado en el circular `XY` movimiento.

Para programar un arco que da más de una vuelta completa, use un `P` word especificando el número de vueltas, total o parcial de arco. Si `P` es sin especificar, el comportamiento es como si `P1` se le dio, es decir, sólo una a su vez, total o parcial dará lugar, dando un arco menor o igual a un círculo completo. Por ejemplo, si un arco se programa con el P2, la movimiento resultante será más que un punto de partida y un máximo de dos completa círculos (dependiendo del punto final programado.) Multivuelta helicoidal arcos Se apoyan y dan movimiento útil para los agujeros de la molienda o hilos.

Si una línea de código hace un arco, e incluye el movimiento del eje giratorio, los ejes giratorios a su vez a un ritmo constante para que la rotativa movimiento comienza y termina cuando el movimiento de XYZ se inicia y termina. Líneas de este tipo casi nunca son programados.

Si la compensación de radio de la fresa está activo, el movimiento será diferente de lo que se describe aquí. Vea la sección [\[sec:Cutter-Radius-Compensation\]](#sec:Cutter-Radius-Compensation).

Dos formatos permitidos para especificar un arco: Centro de Radio y formato.

Es un error si:

-   No hay velocidad de avance se ha establecido.

### Centro formato arcos (formato preferido)

En el formato de centro, las coordenadas del punto final del arco en plano seleccionado se especifican, junto con los desplazamientos del centro del arco desde la ubicación actual. En este formato, que está bien si la punto final del arco es el mismo que el punto actual.

Es un error si:

-   Cuando el arco se proyecta sobre el plano seleccionado, la distancia desde el punto actual en el centro se diferencia de la distancia desde el extremo punto en el centro de más de 0.0002 pulgadas (centímetros si se utilizan) o 0.002 milímetros (mm si se utilizan).

Cuando el plano XY se selecciona el programa:

    Ejes G2 o G3 I- J-

Las palabras del eje son opcionales, excepto que al menos uno de X e Y debe ser usado para programar un arco de menos de 360 ​​grados. I y J son los desplazamientos desde la ubicación actual (en las direcciones X e Y, respectivamente) del centro del círculo. I y J son opcionales, excepto que al menos uno de los dos debe ser utilizado. Si sólo se especifica, el valor de la otra se toma como 0. Si se incluye la palabra Z se hélice.

Es un error si:

-   I y J son omitidos.

Cuando el plano XZ se selecciona el programa:

    G2 o G3 ejes I-K-

Las palabras del eje son opcionales, excepto que al menos uno de X y Z debe ser usado para programar un arco de menos de 360 ​​grados. I y K son los desplazamientos desde la ubicación actual (en las direcciones X y Z, respectivamente) del centro del círculo. I y K son opcionales, excepto que al menos uno de los dos debe ser utilizado. Si sólo se especifica, el valor de la otra se toma como 0. En el modo de diámetro G7 I y K son todavía una cota de radio.

Es un error si:

-   I y K son omitidos.

Cuando el plano yz se selecciona el programa:

    Ejes G2 o G3 J- K-

Las palabras del eje son opcionales, excepto que al menos uno de Y y Z debe ser usado para programar un arco de menos de 360 ​​grados. J y K son los desplazamientos desde la ubicación actual (en la Y y Z, respectivamente) del centro del círculo. J y K son opcionales, excepto que al menos uno de los dos debe ser utilizado. Si sólo se especifica, el valor de la otra se toma como 0.

Es un error si:

-   J y K son omitidos.

### Ejemplos

El cálculo de arcos con la mano puede ser difícil a veces. Una opción es dibujar el arco con un programa de CAD para obtener las coordenadas y las compensaciones. Tenga en cuenta la tolerancia citada más arriba, puede que tenga que cambiar la precisión de su programa de CAD para obtener los resultados deseados. Otro opción es calcular las coordenadas y el uso de fórmulas de compensación. Como se puede ver en las siguientes figuras de un triángulo se puede formar a partir de la posición actual de la posición final y el centro del arco.

En la figura siguiente se puede ver la posición de inicio es X0 Y0, el posición final X1 Y1. La posición del centro del arco está en Y0 X1. Esto le da nosotros un desplazamiento desde la posición inicial de 1 en el eje X y 0 en la Y eje. En este caso sólo una compensación que se necesita.

El código para el ejemplo:

    G2 X1 Y1 I1 F10

![](images/g2.png)

Figure 3. Ejemplo G2<span id="fig:G2-Ejemplo"></span>

En el siguiente ejemplo podemos ver la diferencia entre los desplazamientos de Y si estamos haciendo un G2 o G3 un movimiento. Para el traslado G2 es la posición de inicio X0 Y0, para el traslado G3 es X0 Y1. El centro del arco está en X1 Y0.5 para ambos movimientos. La medida G2 del desplazamiento es de 0,5 J y el G3 mover el J desplazamiento es -0,5.

El código de g para el siguiente ejemplo:

    G2 X0 Y1 I1 J0.5 F25
    G3 X0 Y0 I1 J-0.5 F25

![](images/g2-3.png)

Figure 4. G2-G3 ejemplo<span id="fig:G2/3-Example"></span>

He aquí un ejemplo de un comando de formato de centro de molino de una hélice:

    G17 G2 X10 Y16 I3 J4 Z9

Eso significa que para hacer un reloj (visto desde el eje z positivo) circular o un arco helicoidal cuyo eje es paralelo al eje Z, que termina donde X = 10, Y = 16 y Z = 9, con la desviación en la dirección X por 3 unidades de la ubicación actual de X y el desplazamiento en la dirección Y por 4 unidades de la ubicación actual en Y. Si la ubicación actual X = 7, Y = 7 desde el principio, el centro estará en X = 10, Y = 11. Si el valor inicial de Z es de 9, este es un arco circular, de lo contrario se trata de un arco helicoidal. La radio de este arco será de 5.

En el formato de centro, el radio del arco no se especifica, pero se pueden encontrar fácilmente como la distancia desde el centro del círculo para ya sea el punto actual o el punto final del arco.

### Completa Cañas

    G2 o G3 I- J- K-

Para hacer un círculo completo de 360 desde la ubicación actual único programa de la I, J o K desplazamiento de la ubicación actual del G2/G3. Para programar una 360 hélice de grado en el plano XY basta con incluir la palabra Z.

Es un error si:

-   El desplazamiento K se utiliza en el plano XY

-   La J offset se utiliza en el plano XZ

-   La compensación que se utiliza en el plano YZ

### Arcos Radio formato (formato desanimado)

En el formato de radio, las coordenadas del punto final del arco en plano seleccionado se especifican junto con el radio del arco. Programa de `G2` ejes `R` (o usar `G3` en lugar de `G2`). R es la radio. Las palabras del eje son opcionales, excepto que en por lo menos una de las dos palabras de los ejes en el plano seleccionado debe ser utilizados. El número de R es el radio. Un radio positivo indica que la arco se convierte a través de menos de 180 grados, mientras que un radio negativo indica un viraje de más de 180 grados. Si el arco es helicoidal, el valor del punto final del arco sobre el paralelo del eje de coordenadas para el eje de la hélice también se especifica.

Es un error si:

-   Tanto de las palabras del eje de los ejes del plano seleccionado se omiten

-   El punto final del arco es el mismo que el punto actual.

No es una buena práctica arcos programa de formato de radio que son casi círculos o semicírculos casi completa debido a que un pequeño cambio en la ubicación del punto final se produce un cambio mucho mayor en el ubicación del centro del círculo (y, por tanto, la mitad de la de arco). El efecto de aumento es lo suficientemente grande que un error de redondeo en número puede producir fuera de la tolerancia cortes. Por ejemplo, un 1% desplazamiento del punto final de un arco de 180 grados producido un 7% desplazamiento del punto de 90 grados a lo largo del arco. Casi círculos completos son aún peores. Otros arcos de tamaño (en el rango de pequeña a 165 grados o 195 a 345 grados) están bien.

He aquí un ejemplo de un comando de formato de radio a la fábrica de un arco:

    G17 G2 X10 Y15 R20 Z5

Eso significa que para hacer un reloj (visto desde el eje Z positivo) circular o un arco helicoidal cuyo eje es paralelo al eje Z, que termina donde X = 10, Y = 15, y Z = 5, con un radio de 20. Si el valor inicial de Z es de 5, esto es un arco de círculo paralelo al plano XY; de lo contrario, es un arco helicoidal.

G4 Dwell<span id="sec:G4-Mora"></span>
--------------------------------------

    G4 P[segundos]

Para un tiempo de espera, el programa G4 P-. Esto evitará que los ejes inmóviles para la período de tiempo en cuestión de segundos especificado por el número P.

Es un error si:

-   El número P es negativo.

G5.1 cuadráticas B-spline<span id="sec:G5.1-B-spline-cuadratica"></span>
------------------------------------------------------------------------

    G5.1 Xn Yn I[desplazamiento X] J[desplazamiento Y]

G5.1 crea un B-spline cuadrática en el plano XY con los ejes X e Y solamente.

Es un error si:

-   I y J de compensación no se especifica

-   El otro eje de X o Y se especifica

-   El avión no está activo G17

G5.2 G5.3 NURBS bloque<span id="sec:G5.2-G5.3-NURBs"></span>
------------------------------------------------------------

Advertencia: G5.2, G5.3 es experimental y no han sido evaluados completamente.

G5.2 es para abrir el bloque de datos que definen una NURBS y G5.3 de cierre el bloque de datos. En los límites entre estos dos códigos de la curva puntos de control se definen con sus dos relacionados con "pesos" (P) y sus parámetros (L), que determina el orden de la curva (k) y Posteriormente, su grado (k-1).

Usando esta definición la curva de los nudos de la curva de NURBS no son definido por el usuario son calculados por el algoritmo en el interior, en el misma manera que ocurre en un gran número de aplicaciones gráficas, donde la forma de la curva sólo se puede modificar actuando sobre los puntos de control o pesos.

NURBS Código de ejemplo

        G0 X0 Y0
        F10
        G5.2 X0 Y1 P1 L3
             X2 Y2 P1
             X2 Y0 P1
             X0 Y0 P2
        G5.3
        / Los movimientos rápidos muestran el mismo camino, sin el bloque de NURBS
        G0 X0 Y1
           X2 Y2
           X2 Y0
           X0 Y0
        M2

NURBS Ejemplo de salida

image::images/nurbs01.png \[\]

Más información sobre NURBS se pueden encontrar aquí:

<http://wiki.machinekit.org/cgi-bin/emcinfo.pl?NURBS> \[<http://wiki.machinekit.org/cgi-bin/emcinfo.pl?NURBS>\]

Modo Diámetro del G7<span id="sec:G7-Diametro-Modo"></span>
-----------------------------------------------------------

Programa del G7 para entrar en el modo de diámetro para el eje X en un torno. Cuando en el modo de diámetro en el eje X se mueve en un torno, se media la distancia en el centro del torno. Por ejemplo X1 se trasladaría a la cortadora 0.500 "desde el centro del torno dando así un una" parte de diámetro.

G8 modo Radio<span id="sec:G8-Radio-Mode"></span>
-------------------------------------------------

Programa del G8 para entrar en el modo de radio para el eje X en un torno. Cuando en El modo de radio en el eje X se mueve en un torno, será la distancia de la centro. Así, un corte en el X1 se traduciría en una parte que es de 2" de diámetro. G8 es por defecto al encender el equipo.

G10 L1 Set de herramientas Tabla<span id="sec:G10-L1_"></span>
--------------------------------------------------------------

    G10 L1 P [número de herramientas] R [radio]
        X [desplazamiento] Y [desplazamiento] Z [desplazamiento]
        A [desplazamiento] B [offset] C [desplazamiento]
        U [offset] V [offset] W [desplazamiento]
        I [frontangle] J [backangle] Q [orientación]

Un programa de L1 del G10 para establecer una entrada de tabla de herramientas de un programa o la ventana MDI. Una válida G10 L1 reescribe la tabla de herramientas.

Es un error si:

-   Compensación de corte está en

-   El número no se especifica P

Para más información sobre la orientación de corte, ver el [\[cap:Lathe-Tool-Orientations\]](#cap:Lathe-Tool-Orientations) diagrama.

G10 L2 conjunto de coordenadas del sistema<span id="sec:G10-L2_"></span>
------------------------------------------------------------------------

    G10 L2 P [sistema de coordenadas] R [rotación XY sobre Z]
        X [desplazamiento] Y [desplazamiento] Z [desplazamiento]
        A [desplazamiento] B [offset] C [desplazamiento]
        U [offset] V [offset] W [desplazamiento]

El sistema de coordenadas se describe en la sección [\[cha:Coordinate-System\]](#cha:Coordinate-System).

Para establecer el origen de un sistema de coordenadas, el programa `G10 L2 P-R-ejes`, donde el número P está en el rango de 0 a 9. Para el programa de sistema de coordenadas activo P0. Para especificar un programa de sistema de coordenadas de 1 a 9 correspondientes a \`\` a `G54 G59.3`. Opcionalmente R programa para indicar el giro del eje XY alrededor del eje Z. Todas las palabras eje son opcionales. El origen del sistema de coordenadas especificado por el número P ajustado a los valores dados (en términos del sistema de coordenadas de la máquina no offset). Sólo esas coordenadas para el que se incluye una palabra en la línea de eje se establecerá. Estando en modo de distancia incremental (G91 \`\`) no tiene ningún efecto sobre *G10* L2. El sentido de giro es CCW como se ve desde la vista superior.

Conceptos importantes:

-   G10 L2 Pn no cambia de un sistema de coordenadas actual a la especificada por P, usted tiene que utilizar G54-59.3 para seleccionar un sistema de coordenadas.

-   Cuando la rotación es en efecto correr un eje sólo se moverá de ese eje en un sentido positivo o negativo, y no a lo largo del eje de rotación.

Es un error si:

-   El número P no se evalúa como un número entero en el rango de 0 a 9.

-   Un eje se programa que no está definido en la configuración.

Si un `G92` origen de compensación estaba en vigor antes del `G10 L2`, seguirá en vigor después.

El sistema de coordenadas cuyo origen se establece por un comando `G10` puede ser activo o inactivo en el momento en el `G10` se ejecuta. Si se activa, las nuevas coordenadas en vigor inmediatamente.

Ejemplos:

 G10 L2 P1 X3.5 Y17.2   
Establece el origen del sistema de coordenadas primera (la seleccionada por `G54`) que X = 3.5 y Y = 17.2. Debido a que sólo X e Y se especifican, el punto de origen no puede ser movido en X e Y; las coordenadas de otro no se cambian.

G10 L2 P1 X0 Y0 Z0: Establece las coordenadas XYZ del origen G54 al origen unoffset.

<table>
<caption>Table 1. Establecer sistema de coordenadas<span id="cap:Set-Coordinar-System"></span></caption>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">P Valor</th>
<th align="left">Sistema de coordenadas</th>
<th align="left">G Código</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>Actualmente Activo</p></td>
<td align="left"><p>n / a</p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>54</p></td>
</tr>
<tr class="odd">
<td align="left"><p>2</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>55</p></td>
</tr>
<tr class="even">
<td align="left"><p>3</p></td>
<td align="left"><p>3</p></td>
<td align="left"><p>56</p></td>
</tr>
<tr class="odd">
<td align="left"><p>4</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>57</p></td>
</tr>
<tr class="even">
<td align="left"><p>5</p></td>
<td align="left"><p>5</p></td>
<td align="left"><p>58</p></td>
</tr>
<tr class="odd">
<td align="left"><p>6</p></td>
<td align="left"><p>6</p></td>
<td align="left"><p>59</p></td>
</tr>
<tr class="even">
<td align="left"><p>7</p></td>
<td align="left"><p>7</p></td>
<td align="left"><p>59,1</p></td>
</tr>
<tr class="odd">
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>59,2</p></td>
</tr>
<tr class="even">
<td align="left"><p>9</p></td>
<td align="left"><p>9</p></td>
<td align="left"><p>59.3</p></td>
</tr>
</tbody>
</table>

G10 L10 Set Tabla de herramientas<span id="sec:G10-L10"></span>
---------------------------------------------------------------

    G10 L10 P [número de herramientas] R [radio]
        X [set_curr_sys_to] Y [set_curr_sys_to] Z [set_curr_sys_to]
        A [set_curr_sys_to] B [set_curr_sys_to] C [set_curr_sys_to]
        U [set_curr_sys_to] V [set_curr_sys_to] W [set_curr_sys_to]
        I [frontangle] J [backangle] Q [orientación]

G10 L10 cambia la entrada de la tabla de herramientas de P herramienta para que si el de herramienta se vuelve a cargar con la máquina en su posición actual y con el actual G5x y compensaciones G92 activa, las coordenadas actuales para los ejes dado se convertirán en los valores dados. Los ejes que se no especificado en el comando del G-10 L10 no se cambiará.

Es un error si:

-   Compensación de corte está en

G10 L11 Set Tabla de herramientas<span id="sec:G10-L11"></span>
---------------------------------------------------------------

    G10 L11 P [número de herramientas] R [radio]
        X [set_curr_loc_to] Y [set_curr_loc_to] Z [set_curr_loc_to]
        A [set_curr_loc_to] B [set_curr_loc_to] C [set_curr_loc_to]
        U [set_curr_loc_to] V [set_curr_loc_to] W [set_curr_loc_to]
        I [frontangle] J [backangle] Q [orientación]

G10 L11 es como G-10 L10, excepto que en lugar de establecer la entrada de acuerdo con las compensaciones actuales, se establece para que la corriente coordenadas se convertiría en el valor dado, si la nueva herramienta de compensación se vuelve a cargar y la máquina se coloca en la coordenada G59.3 sistema sin ningún tipo de compensación G92 activa.

Esto permite al usuario configurar el sistema de coordenadas G59.3 de acuerdo con un punto fijo de la máquina, y luego usar ese aparato para medir herramientas sin tener en cuenta otras compensaciones actualmente activa.

Es un error si:

-   Compensación de corte está en

G10 L20 conjunto de coordenadas del sistema<span id="sec:G10-L20"></span>
-------------------------------------------------------------------------

    G10 L20 P [sistema de coordenadas] R [la rotación alrededor de Z]
        X [set_curr_loc_to] Y [set_curr_loc_to] Z [set_curr_loc_to]
        A [set_curr_loc_to] B [set_curr_loc_to] C [set_curr_loc_to]
        U [set_curr_loc_to] V [set_curr_loc_to] W [set_curr_loc_to]

G10 L20 es similar a la del G-10 L2, excepto que en lugar de establecer la offset / entrada a un valor dado, se establece en un valor calculado que hace las coordenadas actuales convertido en el valor dado.

Es un error si:

-   El número P no se evalúa como un número entero en el rango de 0 a 9.

-   Un eje se programa que no está definido en la configuración.

G17, G18, G19, G17.1, G18.1, G19.1 Selección de Plano<span id="sec:G17-G18-G19"></span>
---------------------------------------------------------------------------------------

Estos códigos de establecer el plano actual de la siguiente manera:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">G17</th>
<th align="left">XY (por defecto)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>G18</p></td>
<td align="left"><p>ZX</p></td>
</tr>
<tr class="even">
<td align="left"><p>G19</p></td>
<td align="left"><p>YZ</p></td>
</tr>
<tr class="odd">
<td align="left"><p>G17.1</p></td>
<td align="left"><p>UV</p></td>
</tr>
<tr class="even">
<td align="left"><p>G18.1</p></td>
<td align="left"><p>WU</p></td>
</tr>
<tr class="odd">
<td align="left"><p>G19.1</p></td>
<td align="left"><p>VW</p></td>
</tr>
</tbody>
</table>

Los efectos de contar con un plano seleccionado se discuten en la Sección [\[sec:G2-G3-Arco\]](#sec:G2-G3-Arco) Y en la sección [\[sec:G81-G89\]](#sec:G81-G89)

G20, G21 Unidades de longitud<span id="sec:G20-G21-Unidades"></span>
--------------------------------------------------------------------

Programa G20 para uso pulgadas para unidades de longitud.
 Programa G21 para uso milímetros para las unidades de longitud.

Por lo general es una buena idea del programa ya sea del G20 o G21, cerca de la inicio de un programa antes de que cualquier movimiento se produce, y no utilizar uno en el resto del programa.

G28, G28.1 Ir a las posiciones predefinidas<span id="sec:G28-G28.1"></span>
---------------------------------------------------------------------------

G28 utiliza los valores de los parámetros de 5161-5166, como los valores absolutos hacer un rápido movimiento transversal a partir de la posición actual. El parámetro Los valores se expresan en términos del sistema de coordenadas absolutas y el de la máquina sistema de coordenadas de origen.

`G28 ejes` hará un rápido movimiento transversal a la posición especificada por *ejes*, hará una rápida travesía mover a la posición predefinida en los parámetros 5161-5166.

G28.1 guarda la posición actual absoluta en los parámetros 5161-5166.

Es un error si:

-   La compensación de radio se enciende

G30, G30.1 Ir a las posiciones predefinidas<span id="sec:G30-G30.1"></span>
---------------------------------------------------------------------------

G30 utiliza los valores de los parámetros de 5181-5186 como los valores absolutos para hacer un rápido movimiento transversal a partir de la posición actual. Los valores de los parámetros en términos de la absoluta del sistema de coordenadas y el sistema de la máquina de coordenadas nativos.

G30 *ejes* hará un rápido movimiento transversal a la posición especificada por *ejes*, hará una rápida travesía hacia el movimiento de las posiciones predefinidas en los parámetros 5181-5186.

G30.1 guarda la posición actual absoluta en los parámetros 5181-5186.

G30 parámetros se utilizan para mover la herramienta cuando el M6 está programado si \[TOOL\_CHANGE\_AT\_G30\] = 1 se encuentra en la sección \[EMCIO\] del archivo ini.

Es un error si:

-   La compensación de radio se enciende

G33 husillo sincronizado movimiento<span id="sec:G33-eje-Sync"></span>
----------------------------------------------------------------------

    G33 X- Y- Z- K-

Para el eje sincronizado movimiento en una dirección, el código de `G33 X- Y- Z- K-` donde K da la distancia recorrida en XYZ para cada revolución del eje. Por ejemplo, si empieza por `Z=0`, `G33 Z-1 K.0625` produce un movimiento de 1 pulgada de Z más de 16 revoluciones del husillo. Este comando puede ser parte de un programa para producir un hilo 16TPI. Otro ejemplo en el sistema métrico, `G33 Z-15 K1.5` produce un movimiento de 15 mm, mientras que el eje gira 10 veces por un hilo de 1,5 mm.

Nota: K sigue la línea de la unidad descrita por `X- Y- Z-` y no es paralelo al eje Z.

Sincronizado de huso movimientos esperar para el índice de eje, la línea pasa por lo que múltiples arriba. `G33` extremo se mueve en el punto final programado.

Todas las palabras que el eje son opcionales, excepto que al menos uno debe ser utilizado.

Es un error si:

-   Todas las palabras eje se omiten.

-   El eje no está girando cuando se ejecuta este comando

-   El movimiento lineal solicitado excede los límites de velocidad de la máquina debido a la velocidad del husillo

G33.1 roscado rígido<span id="sec:G33.1-rigido-Tapping"></span>
---------------------------------------------------------------

    G33.1 X- Y- Z- K-

Para el roscado rígido (husillo sincronizado con el movimiento de retorno), código `G33.1 X-Y-Z-K-`, donde `K-` da la distancia recorrida por cada revolución del eje. Un movimiento de roscado rígido se compone de la siguiente secuencia:

-   El traslado a la coordenada especificada, sincronizado con el husillo en dado la razón y comenzando con un pulso índice del husillo.

-   Cuando se alcanza el punto final, un comando para invertir el eje (por ejemplo, de las agujas del reloj a la izquierda).

-   Continúa el movimiento sincronizado **más** allá de la coordenada final especificado hasta el eje de hecho se detiene y se invierte.

-   Continúa el movimiento sincronizado de nuevo a la coordenada original.

-   Al llegar a la coordenada original, un comando para invertir el eje por segunda vez (Por ejemplo, de izquierda a derecha).

-   Continúa el movimiento sincronizado **más** allá de las coordenadas originales hasta el eje de hecho se detiene y se invierte.

-   Un **sincronizados** volver a la coordenada original.

Sincronizado de huso movimientos esperar para el índice de husillo, por lo que múltiples pases en fila. `G33.1` extremo se mueve en las coordenadas originales.

Todas las palabras que el eje son opcionales, excepto que al menos uno debe ser utilizado.

Es un error si:

-   Todas las palabras eje se omiten.

-   El eje no está girando cuando se ejecuta este comando

-   El movimiento lineal solicitado excede los límites de velocidad de la máquina debido a la velocidad del husillo

Ejemplo:

    , Se mueven a la posición inicial
    G0 X1.000 Y1.000 Z0.100
    ; Rígida aprovechar un hilo 20 TPI
    G33.1 Z-0.750 K0.05

G38.x recto de la sonda<span id="sec:G38.x-Straight-Probe"></span>
------------------------------------------------------------------

IMPORTANTE: Usted no será capaz de utilizar correctamente este comando hasta que su máquina ha sido creada para dar una señal de la sonda a través de HAL de EMC2. La señal de la sonda debe introducirse a través de un bit de entrada HAL, y remitido a `motion.probe de entrada (bit, en)`. G38.x utiliza el valor de este pin para determinar cuando la sonda ha hecho (o perdido) de contacto. TRUE para sonda de contacto cerrado (tocar), FALSE para el contacto de la sonda abierta.

Programa `G38.2 'ejes'`, `G38.3 'ejes'`, `G38.4 'ejes'` o `G38.5 'ejes'` para realizar una operación de la sonda recta. Las palabras del eje son opcionales, excepto que al menos uno de ellos debe ser utilizado. Las palabras del eje en conjunto, definen el punto de destino de que la sonda se desplaza hacia, a partir de la ubicación actual. La herramienta en el cabezal tiene que ser una sonda.

Es un error si:

-   El punto actual es el mismo que el punto programado.

-   No se utiliza la palabra eje

-   Compensación de radio de la fresa está activado

-   La velocidad de avance es cero

-   La sonda ya está en el estado de destino

En respuesta a este comando, la máquina se mueve el punto de control (Que debe estar en el centro de la bola de la sonda) en una línea recta en la velocidad de avance actual hacia el punto programado. En el modo de alimentación inversa del tiempo, la velocidad de avance es tal que todo el movimiento desde el punto actual al punto programado se tomara el tiempo especificado. El movimiento se detiene (dentro de los límites de aceleración de la máquina) cuando el punto programado se alcanza, o cuando el cambio solicitado en la entrada de la sonda se lleva a cabo, lo que ocurra primero.

La tabla [\[cap:El-sondeo-de-codigos\]](#cap:El-sondeo-de-codigos) muestra la importancia de diferentes códigos de sondeo.

<table>
<caption>Table 2. Códigos de sondeo<span id="cap:El-sondeo-de-codigos"></span></caption>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Código</th>
<th align="left">Estado de destino</th>
<th align="left">dirección deseada</th>
<th align="left">señal de error</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>G38.2</p></td>
<td align="left"><p>Contacto</p></td>
<td align="left"><p>Hacia la pieza de trabajo</p></td>
<td align="left"><p>Sí</p></td>
</tr>
<tr class="even">
<td align="left"><p>G38.3</p></td>
<td align="left"><p>Contacto</p></td>
<td align="left"><p>Hacia la pieza de trabajo</p></td>
<td align="left"><p>No hay</p></td>
</tr>
<tr class="odd">
<td align="left"><p>G38.4</p></td>
<td align="left"><p>No Contacto</p></td>
<td align="left"><p>Lejos de la pieza de trabajo</p></td>
<td align="left"><p>Sí</p></td>
</tr>
<tr class="even">
<td align="left"><p>G38.5</p></td>
<td align="left"><p>No Contacto</p></td>
<td align="left"><p>Lejos de la pieza de trabajo</p></td>
<td align="left"><p>No hay</p></td>
</tr>
</tbody>
</table>

Tras el éxito de sondeo, los parámetros de 5061 a 5069 se establece en el coordenadas de XYZABCUVW de la ubicación del punto de control en el momento de la sonda ha cambiado de estado. Después de infructuosos sondeo, que se establecen las coordenadas del punto programado. Parámetro 5070 está ajustado a 1 si la sonda tuvo éxito y 0 si la avería. Si la operación de sondeo no, G38.2 G38.4 y será una señal de error mediante la publicación de un mensaje en pantalla si la interfaz gráfica de usuario seleccionado es compatible con eso. (Y por detener la ejecución del programa? \* FIXME \* TODO)

Un comentario de la forma `(filename.txt PROBEOPEN)` se abrirá "Archivo.txt" y almacenar las coordenadas 9-número que consta de XYZABCUVW de cada sonda recta éxito en ella. El archivo debe estar cerrado con `(PROBECLOSE)`.

G40 Compensación Off<span id="sec:G40"></span>
----------------------------------------------

Programa G40 para activar la compensación de radio de la fresa apagado. El siguiente paso debe ser un movimiento recto. Está bien que a su vez una indemnización de cuando ya está apagado.

Es un error si:

-   Un movimiento de arco G2/G3 está programada siguiente después de un G40.

Compensación G41, G42 radio de la herramienta<span id="sec:G41-G42"></span>
---------------------------------------------------------------------------

    G41 o G42 D[herramienta]

Para iniciar la compensación de radio a la izquierda del perfil de la pieza, utilice G41. Comienza cortador G41 Compensación de radio a la izquierda de la línea de programa como se ve desde el lado positivo del eje perpendicular al plano.

Para iniciar la compensación de radio a la derecha del perfil de la pieza, utilice G42. G42 inicia la compensación radio de la fresa a la derecha de la línea de programa como se ve desde el lado positivo del eje perpendicular al plano.

El plomo en movimiento debe ser al menos tan largo como el radio de la herramienta. El plomo en movimiento puede ser una decisión rápida.

Compensación de radio de la fresa se puede realizar si el plano XY o XZ plano está activa.

M100-M199 usuario comandos están permitidos cuando compensación de cortador es el.

El comportamiento del centro de mecanizado cuando la compensación de radio de la fresa está en el se describe en la sección [\[sec:Cutter-Radius-Compensation\]](#sec:Cutter-Radius-Compensation)

Cortador === compensación de radio de la tabla de herramientas

Para activar la compensación de radio de la fresa a la izquierda (es decir, el corte se mantiene a la izquierda de la trayectoria programada, cuando el radio de la herramienta es positivo), programa `D-G41`. Para activar la compensación de radio de la fresa a la derecha (es decir, el corte se mantiene a la derecha de la trayectoria programada, cuando el radio de la herramienta es positivo), programa `D-G42`. La palabra D es opcional, si no hay ninguna palabra D, el radio de la herramienta que está en el huso se utilizará. Si se utiliza, el número D debe ser normalmente el número de ranura de la herramienta en el eje, aunque esto no es necesario. Está bien que el número D a cero, un valor de radio de cero se utiliza.

Es un error si:

-   El número D no es un número entero, es negativo, o es mayor que el número de ranuras del carrusel,

-   El plano YZ es activo,

-   Compensación de radio de la fresa se le ordena a su vez en cuando ya está encendida.

G41.1, G42.1 Compensación dinámica de radio de corte<span id="sec:G41.1-G42.1"></span>
--------------------------------------------------------------------------------------

    G41.1 G42.1 o D [diámetro] <L[orientation]>

Para activar la compensación de radio de la fresa a la izquierda, el programa `G41.1 D- L-`.

Para activar la compensación de corte en el programa de la derecha, `G42.1 D- L-`.

La palabra D especifica el diámetro de la fresa. La palabra L especifica la orientación del corte, y por defecto a 0 si no se especifica.

Es un error si:

-   El plano YZ es activo.

-   El número L no está en el rango de 0 a 9.

-   El número L se utiliza cuando el plano XZ no está activo.

-   Compensación de corte se le ordena a su vez en cuando ya está encendida.

Para más información sobre la orientación de corte ver el [\[cap:Lathe-Tool-Orientations\]](#cap:Lathe-Tool-Orientations) y [\[fig:Tool-Positions-1-2-3-4\]](#fig:Tool-Positions-1-2-3-4) Y [\[fig:Tool-Positions-5-6-7-8\]](#fig:Tool-Positions-5-6-7-8) diagramas.

G43, G43.1, G49 Compensación Longitud de la herramienta<span id="sec:G43-G43.1-Tool"></span>
--------------------------------------------------------------------------------------------

### G43, G43.1: Activar compensación de longitud

G43 y G43.1 movimientos cambio posterior al compensar la Z y / o X coordenadas de la longitud de la herramienta. G43 G43.1 y no causan ningún movimiento. La próxima vez que se mueve un eje de compensación, de punto final que el eje de la es el lugar compensado.

#### G43: herramienta de uso corriente carga

Para utilizar la herramienta está cargado de los últimos Tn M6 un programa G43

#### Hn G43: Compensación de la tabla de herramientas

Para utilizar una longitud de herramienta de la tabla de herramientas, el programa `G43` Hn, donde el número n es el índice que desee en la tabla de herramientas. El H número será normalmente, pero no tiene que ser, al igual que la ranura número de la herramienta que está en el huso. Está bien que el número H a ser cero, un valor de desplazamiento de cero se utiliza.

Es un error si:

-   El número H no es un número entero, es negativo, o es mayor que el número de ranuras del carrusel.

#### G43.1: la compensación de herramienta dinámica

Para utilizar una longitud de herramienta del programa, use `G43.1 Xn Yn ... Wn` para establecer cualquier TLO eje en tiempo de ejecución.

Es un error si:

-   El movimiento es ordenado en la misma línea que `G43.1`

### G49: Cancelar herramienta de corrección de longitud<span id="sub:G49-herramientas"></span>

No utilizar la corrección de longitud de herramienta, el programa G49.

Está bien el programa con el mismo desplazamiento ya está en uso. También es OK para programar sin usar la longitud de herramienta si no se está utilizados.

Movimiento G53 en coordenadas absolutas<span id="sub:G53-Move-in"></span>
-------------------------------------------------------------------------

Para moverse en coordenadas absolutas desde el origen de la máquina, el programa `G53` en la misma línea como un movimiento lineal. `G53` no es modal y se debe programados en cada línea. `G0` o `G1` no tiene que ser programado en la misma línea si uno está activo actualmente. Por ejemplo `G53 G0 X0 Y0 Z0` se moverá los ejes para el hogar incluso si la actual sistema de coordenadas seleccionado se compensa, en efecto.

Es un error si:

-   G53 se utiliza sin G0 o G1 ser activo,

-   O G53 se utiliza mientras que la remuneración es el radio de la fresa.

G54-G59.3 Seleccione Sistema de coordenadas<span id="sec:G54-G59.3"></span>
---------------------------------------------------------------------------

Para seleccionar un sistema de coordenadas, programa G54, y lo mismo para otros sistemas de coordenadas. Los pares del sistema-número de código-G son: (1 - `G54`), (2 - `G55`), (3 - `G56`), (4 - `G57`), (5 - `G58`), (6 - `G59`), (7 - `G59.1`), (8 - `G59.2`) y (9 - `G59.3`). Los sistemas de coordenadas almacenar los valores para cada sistema en el variables que se muestran en la siguiente tabla.

<table>
<caption>Table 3. Sistemas de coordenadas<span id="cap:Coordinar-Systems"></span></caption>
<colgroup>
<col width="90%" />
<col width="90%" />
<col width="90%" />
<col width="90%" />
<col width="90%" />
<col width="90%" />
<col width="90%" />
<col width="90%" />
<col width="90%" />
<col width="90%" />
<col width="90%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Seleccionar</th>
<th align="left">CS</th>
<th align="left">X</th>
<th align="left">Y</th>
<th align="left">Z</th>
<th align="left">A</th>
<th align="left">B</th>
<th align="left">C</th>
<th align="left">U</th>
<th align="left">V</th>
<th align="left">W</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>G54</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>5221</p></td>
<td align="left"><p>5222</p></td>
<td align="left"><p>5223</p></td>
<td align="left"><p>5224</p></td>
<td align="left"><p>5225</p></td>
<td align="left"><p>5226</p></td>
<td align="left"><p>5227</p></td>
<td align="left"><p>5228</p></td>
<td align="left"><p>5229</p></td>
</tr>
<tr class="even">
<td align="left"><p>G55</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>5241</p></td>
<td align="left"><p>5242</p></td>
<td align="left"><p>5243</p></td>
<td align="left"><p>5244</p></td>
<td align="left"><p>5245</p></td>
<td align="left"><p>5246</p></td>
<td align="left"><p>5247</p></td>
<td align="left"><p>5248</p></td>
<td align="left"><p>5249</p></td>
</tr>
<tr class="odd">
<td align="left"><p>G56</p></td>
<td align="left"><p>3</p></td>
<td align="left"><p>5261</p></td>
<td align="left"><p>5262</p></td>
<td align="left"><p>5263</p></td>
<td align="left"><p>5264</p></td>
<td align="left"><p>5265</p></td>
<td align="left"><p>5266</p></td>
<td align="left"><p>5267</p></td>
<td align="left"><p>5268</p></td>
<td align="left"><p>5269</p></td>
</tr>
<tr class="even">
<td align="left"><p>G57</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>5281</p></td>
<td align="left"><p>5282</p></td>
<td align="left"><p>5283</p></td>
<td align="left"><p>5284</p></td>
<td align="left"><p>5285</p></td>
<td align="left"><p>5286</p></td>
<td align="left"><p>5287</p></td>
<td align="left"><p>5288</p></td>
<td align="left"><p>5289</p></td>
</tr>
<tr class="odd">
<td align="left"><p>G58</p></td>
<td align="left"><p>5</p></td>
<td align="left"><p>5301</p></td>
<td align="left"><p>5302</p></td>
<td align="left"><p>5303</p></td>
<td align="left"><p>5304</p></td>
<td align="left"><p>5305</p></td>
<td align="left"><p>5306</p></td>
<td align="left"><p>5307</p></td>
<td align="left"><p>5308</p></td>
<td align="left"><p>5309</p></td>
</tr>
<tr class="even">
<td align="left"><p>G59</p></td>
<td align="left"><p>6</p></td>
<td align="left"><p>5321</p></td>
<td align="left"><p>5322</p></td>
<td align="left"><p>5323</p></td>
<td align="left"><p>5324</p></td>
<td align="left"><p>5325</p></td>
<td align="left"><p>5326</p></td>
<td align="left"><p>5327</p></td>
<td align="left"><p>5328</p></td>
<td align="left"><p>5329</p></td>
</tr>
<tr class="odd">
<td align="left"><p>G59.1</p></td>
<td align="left"><p>7</p></td>
<td align="left"><p>5341</p></td>
<td align="left"><p>5342</p></td>
<td align="left"><p>5343</p></td>
<td align="left"><p>5344</p></td>
<td align="left"><p>5345</p></td>
<td align="left"><p>5346</p></td>
<td align="left"><p>5347</p></td>
<td align="left"><p>5348</p></td>
<td align="left"><p>5349</p></td>
</tr>
<tr class="even">
<td align="left"><p>G59.2</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>5361</p></td>
<td align="left"><p>5362</p></td>
<td align="left"><p>5363</p></td>
<td align="left"><p>5364</p></td>
<td align="left"><p>5365</p></td>
<td align="left"><p>5366</p></td>
<td align="left"><p>5367</p></td>
<td align="left"><p>5368</p></td>
<td align="left"><p>5369</p></td>
</tr>
<tr class="odd">
<td align="left"><p>G59.3</p></td>
<td align="left"><p>9</p></td>
<td align="left"><p>5381</p></td>
<td align="left"><p>5382</p></td>
<td align="left"><p>5383</p></td>
<td align="left"><p>5384</p></td>
<td align="left"><p>5385</p></td>
<td align="left"><p>5386</p></td>
<td align="left"><p>5387</p></td>
<td align="left"><p>5388</p></td>
<td align="left"><p>5389</p></td>
</tr>
</tbody>
</table>

Es un error si:

-   Uno de los G-códigos se utiliza mientras que la remuneración es el radio de la fresa.

Vea la sección [\[cha:Coordinate-System\]](#cha:Coordinate-System) para una descripción de coordinar sistemas.

G61, G61.1, G64 Ruta del Menú de control<span id="sec:G61-G61.1-G64"></span>
----------------------------------------------------------------------------

    G61 Modo ruta exacta
    G61.1 modo de parada precisa
    G64 mejor velocidad posible
    G64 P- (tolerancia al movimiento de mezcla) Q- (ingenua tolerancia de levas)

G61 visita el punto programado exactamente, a pesar de que los medios temporalmente de llegar a una parada completa.

G64 sin P de medios para mantener la mejor velocidad posible, sin importar cómo lejos del punto programado se termina.

G64 P- Q- es una manera de afinar el sistema para mejor compromiso entre velocidad y precisión. El P-tolerancia significa que el camino real no será más que el P-lejos de la punto final programado. La velocidad se reducirá, si es necesario para mantener la trayectoria. Además, cuando activar G64 P- Q- se convierte en el "detector de ingenuo cam", cuando hay una serie de alimentación lineales XYZ se mueve a la velocidad de avance mismo de que son menos de Q- lejos de ser alineados, que se desplomó en una sola movimiento lineal. El G2/G3 se mueve en el G17 (XY) avión cuando el máximo desviación de un arco de la línea recta es menor que el G64 P- la tolerancia del arco se divide en dos líneas (desde el inicio del arco punto medio, y desde el punto medio a fin). esas líneas son sujetos a el algoritmo ingenuo de levas para las líneas. Por lo tanto, la línea del arco, un arco de arco, y la línea del arco de los casos, así como de línea a línea se benefician de la leva "ingenuo detector ". Esto mejora el rendimiento mediante la simplificación de los contornos trayectoria. Está bien para el programa de modo que ya está activa. Véase también Sección [\[sub:Path-Control-Mode\]](#sub:Path-Control-Mode) para una discusión de estos modos. Si Q no se especifica a continuación, tendrá el mismo comportamiento que antes y utilizar el valor de P-.

G73 Ciclo de taladrado con rotura de la viruta<span id="sec:G73-Perforacion-del-ciclo"></span>
----------------------------------------------------------------------------------------------

    G73 X- Y- Z- A- B- C- R- L- Q-

El `G73` ciclo está destinado a la perforación profunda o de fresado con rotura de la viruta. Se retrae en este ciclo de cortar cualquier largueros de largo (que son comunes cuando se perfora en aluminio). Este ciclo tiene una serie Q, que representa un "delta" incremento a lo largo del eje Z.

1.  Movimiento preliminar, como se describió anteriormente.

2.  Mover el eje Z sólo en la velocidad de alimentación de corriente a la baja en delta o la posición Z, lo que es menos profundo.

3.  Rápida un poco.

4.  Repita los pasos 2 y 3 hasta la posición Z se alcanza en el paso 2.

5.  Retraer el eje Z en la velocidad de desplazamiento para borrar Z.

Es un error si:

-   El número Q es negativo o cero.

G76 Roscado ciclo<span id="sec:G76-Threading-Canned"></span>
------------------------------------------------------------

    G76 P- Z- I- J- R- K- Q- H- E- L-

Es un error si:

-   El plano activo no es el plano ZX

-   Las palabras de otro eje, como por ejemplo X o Y, se especifican

-   El `R`-reducción, el valor es inferior a 1,0.

-   Todas las palabras necesarias no se especifican

-   `P-`,`J-`,`K-` o `H-` es negativo

-   `E-` es mayor que la mitad de la longitud de la línea de impulsión

G76 Roscado<span id="fig:G76-Threading"></span>

image::images/g76-threads.png \[\]

 Línea de unidad   
Una línea a través de la X en paralelo posición inicial a la Z.

 P-   
El "paso de rosca" en la distancia por vuelta.

 Z   
La posición final de las discusiones. Al final del ciclo de la herramienta estar en esta posición Z.

 I-   
El "hilo pico" desplazamiento de la "línea de mando". Negativo `I` valores son temas externos, positivos y `I` valores son las discusiones internas. En general, el material se ha convertido de este tamaño antes de que el `G76` ciclo.

 J-   
Un valor positivo que especifica la "profundidad de corte inicial". La primera cortar hilos será `J` más allá de la "cumbre hilo" posición.

 K-   
Un valor positivo que especifica la "profundidad de la rosca completa". La final cortar hilos será `K` más allá de la "cumbre hilo" posición.

Los ajustes opcionales

 R-   
La "reducción progresiva de la profundidad". `R1.0` selecciona una profundidad constante en los sucesivos roscado pasa. `R2.0` selecciona la zona de constante. Valores entre 1.0 y 2.0 seleccionar la disminución profundidad, pero aumentar la superficie. Valores por encima de 2,0 seleccionar la disminución zona. Tenga en cuenta que los valores de reducción, sin necesidad de alta causará una gran número de pases que se utilizará. Regresividad (= un descenso por etapas o pasos.)

 Q-   
El "ángulo compuesto de diapositivas" es el ángulo (en grados) que describe a lo que pasa medida sucesivos deben ser compensados ​​a lo largo de la línea de transmisión. Esto se utiliza para que un lado de la herramienta para extraer más material del que la otra. Un resultado positivo `Q` valor hace que el borde delantero de la herramienta para corte en mayor medida. Los valores típicos son 29, 29,5 o 30.

 H-   
El número de "pases de la primavera". Pasa a la primavera se pasa a otros profundidad de la rosca completa. Si no pasa más se desea, el programa `H0`.

Entrada cónica y se mueve salida se puede programar el uso de `E-` y `L-`.

 E-mail   
Especifica la distancia a lo largo de la línea de unidad utilizada para la puesta a punto. La ángulo de la vela será así el último paso se estrecha a la cresta hilo más de la distancia especificada con E. `E0.2` dará una vela para el primer / último 0,2 unidades de longitud a lo largo del hilo. Para un programa de E cono de 45 grados lo mismo que K

 L-   
Especifica que los extremos del hilo de conseguir la puesta a punto. Programa `L0` para no cono (por defecto), `L1` para la puesta a punto de entrada, `L2` de cono de salida, o `L3` tanto para la entrada y salida se estrecha. Se estrecha la entrada se detendrá en la unidad línea para sincronizar con el pulso índice se incorporarán a los principios de la forma cónica. No cono de entrada y la herramienta será rápido a la profundidad de corte a continuación, sincronizar y comenzar el corte.

La herramienta se mueve a la X inicial y las posiciones de Z antes de emitir el G76. La posición X es la "línea de mando" y es la posición de la Z inicio de las discusiones.

La herramienta de sincronización de una breve pausa antes de cada rosca pasar, por lo que una ranura de desahogo se requerirá a la entrada a menos que el comienzo de la rosca es más allá del final de la materia, o la inscripción cono se utiliza.

A menos que use un cono de salida, el movimiento de salida (transversal a X original) es no está sincronizado con la velocidad de giro. Con un eje lento, la salida medida puede tener sólo una pequeña fracción de una revolución. Si el cabezal la velocidad es mayor después de varios pases de salida se completa, con posterioridad mueve requerirá una mayor parte de una revolución, lo que resulta en una corte muy fuerte durante el traslado de salida. Esto puede evitarse mediante una alivio de la ranura en la salida, o por no cambiar la velocidad de giro, mientras que roscado.

La posición final de la herramienta estará en la final de la "línea de mando". Una caja fuerte movimiento Z será necesario con una rosca interna para quitar la herramienta del hoyo.

El programa de ejemplo `g76.ngc` muestra el uso del ciclo de G76 en lata, y se puede visualizar y ejecutarse en cualquier máquina con el 'SIM / lathe.ini \`configuración.

El siguiente ejemplo muestra el resultado de la ejecución de este G-Code:

    G0 Z-0.5 X 0.2
    P0.05 G76 Z-1 I-0.075 J0.008 K0.045 Q29.5 L2 E0.045

La herramienta está en la posición final después de que el ciclo G76 se ha completado. Usted puede ver la trayectoria de entrada a la derecha de la Q29.5 y salir de la camino a la izquierda de la E0.045 L2. Las líneas blancas son el corte se mueve.

Ejemplo G76 Roscado<span id="fig:G76-Threading-Ejemplo"></span>

image::images/g76-01.png \[\]

G80 Cancelar modal de movimiento<span id="sec:G80-Cancelar-Modal"></span>
-------------------------------------------------------------------------

Programa `G80` para asegurar que no se producirá el movimiento del eje. Es un error si:

-   Las palabras del Eje se programan cuando G80 está activo, a menos que un referente el grupo 0 Gcode está programado que usa palabras eje.

Los ciclos preprogramados G81-G89<span id="sec:G81-G89"></span>
---------------------------------------------------------------

Los ciclos fijos `G81` a través `G89` se describen en esta sección. Dos ejemplos se dan con la descripción de `G81` de abajo.

Todos los ciclos fijos se llevan a cabo con respecto a la actualmente seleccionada avión. Cualquiera de los seis aviones pueden ser seleccionados. En esta sección, la mayoría de las descripciones de asumir el plano XY se ha seleccionado. La comportamiento es análogo, si se selecciona otro plano, y corregir la palabras deben ser empleadas. Por ejemplo, en el `G17.1` avión, la acción de el ciclo fijo es a lo largo de W, y los lugares o incrementos se dan con U y V. En este caso, sustituir U, V, W para X, Y, Z en las instrucciones de abajo.

Palabras de ejes giratorios no están permitidos en los ciclos enlatados. Cuando el plano activo es uno de los familiares XYZ, las palabras no son el eje UVW permitido. Del mismo modo, cuando el plano activo es uno de los familiares UVW, la XYZ palabras eje no están permitidos.

### Palabras comunes

Todos los ciclos enlatados usan X, Y, Z o U, V, W grupos en función de la plano seleccionado y las palabras de R. La posición R (por lo general significa retraer) es a lo largo del eje perpendicular al plano seleccionado en ese momento (el eje Z de plano XY, etc) Algunos ciclos enlatados usan argumentos adicionales.

### Palabras Adherido

Para los ciclos enlatados, vamos a llamar a un número "pegajosa" si, cuando el mismo ciclo se utiliza en varias líneas de código en una fila, el número debe ser usó la primera vez, pero es opcional en el resto de las líneas. Pegajoso números de mantener su valor en el resto de las líneas si no se explícitamente programado para ser diferente. El número de R es siempre pegajoso.

En el modo incremental distancia X, Y, y los números R son tratados como incrementos de la posición actual y Z como un incremento de la Z-eje de la posición antes de la mudanza participación de Z se lleva a cabo. En términos absolutos modalidad a distancia, la X, Y, R, Z y los números son posiciones absolutas en el sistema de coordenadas actual.

### Repetir el ciclo

El número L es opcional y representa el número de repeticiones. latexmath: \[L = 0\] no está permitido. Si la función de repetición se utiliza, es normalmente se utiliza en modo de distancia incremental, de modo que la misma secuencia de los movimientos se repite en varios lugares equidistantes a lo largo de un línea recta. Cuando L-es mayor que 1 en el modo incremental con el XY-plano seleccionado, las posiciones X e Y se determina sumando el dados X e Y los números ni a la actual de X y Y las posiciones (en el primera vuelta) o en las posiciones X e Y en el final de la anterior y al aire (en las repeticiones). Por lo tanto, si el programa `L10`, que se obtener 10 ciclos. El primer ciclo se distancia X, Y de la ubicación original. Las posiciones de R y Z no cambian durante el repite. El número L no es pegajoso. En el modo de distancia absoluta, latexmath: \[L&gt; 1\] significa "hacer el mismo ciclo en el mismo lugar varios tiempos ", omitiendo la palabra L es equivalente a especificar latexmath: \[L = 1\].

### Retracción modo

La altura del movimiento de retracción al final de cada repetición (llamada "Claro Z" en las descripciones de abajo) es determinado por el valor de la retracción modo: o bien a la posición original Z (si es que está por encima de la posición de R y el modo de retractarse es `G98`, OLD\_Z), o bien a la posición R. Vea la sección [\[sec:G98-G99-Set\]](#sec:G98-G99-Set)

### Los errores de un ciclo fijo

Es un error si:

-   X, Y, Z y las palabras están desaparecidos durante un ciclo fijo,

-   Eje palabras de los diferentes grupos (XYZ) (UVW) se usan juntos,

-   Un número de P que se requiere y un número negativo P se utiliza,

-   Un número L se utiliza que no se evalúa a un entero positivo,

-   El movimiento del eje de giro se utiliza durante un ciclo fijo,

-   Tasa inversa de alimentación vez que se activa durante un ciclo fijo,

-   O compensación radio de la fresa está activa durante un ciclo fijo.

Si el plano XY se activa, el número Z es pegajoso, y es un error si:

-   El número Z es falta y el ciclo fijo misma no estuviera ya activa,

-   O el número de R es menor que el número Z.

Si los aviones están activas las otras, las condiciones de error son análogos a los XY condiciones anteriores.

### Preliminar y en el medio de movimiento

En el comienzo de la ejecución de cualquiera de los ciclos fijos, si la actual posición de Z está por debajo de la posición R, el Z-eje se desplaza a la posición R. Esto sucede sólo una vez, independientemente del valor de L.

Además, al comienzo del primer ciclo y repite cada uno, el se mueve siguiendo una o dos se hacen

1.  una recta paralela al atravesar el plano XY a la posición dada XY,

2.  una recta transversal del eje Z sólo a la posición R, si no se Ya en la posición R.

Si otro avión está activa, los movimientos preliminares y en el medio, se análoga.

G81 Ciclo de taladrado<span id="sec:G81-Perforacion-del-ciclo"></span>
----------------------------------------------------------------------

    G81 (X-Y-Z) o (U-V-W-) R-L-

El `G81` ciclo está destinado a la perforación.

1.  Movimiento preliminar, como se describió anteriormente.

2.  Mover el eje Z sólo en la velocidad de avance actual en la posición Z.

3.  Retraer el eje Z en la velocidad de desplazamiento para borrar Z.

    -   Ejemplo 1.\* Supongamos que la posición actual es (1, 2, 3) y el Plano XY se ha seleccionados, y la siguiente línea de código NC se interpreta.

            G90 G81 G98 X4 Y5 Z1.5 R2.8

Esto requiere el modo de distancia absoluta (`G90`) y el modo de retirar OLD\_Z (`G98`) y pide la `G81` ciclo de perforación que se realiza una vez. El número X y la posición X son 4. Y el número y la posición Y son cinco. El número Z y la posición Z se 1.5. El número de R y Z son claras 2,8. Antiguo Z es de 3. Los siguientes movimientos llevará a cabo.

1.  un paralelo a atravesar el plano XY a (4,5,3)

2.  un paralelo transversal al eje Z de (4,5,2.8)

3.  un paralelo de alimentación a la Z-eje (4,5,1.5)

4.  un paralelo transversal al eje Z de (4,5,3)

    -   Ejemplo 2.\* Supongamos que la posición actual es (1, 2, 3) y el Plano XY se ha seleccionados, y la siguiente línea de código NC se interpreta.

            G91 G81 G98 X4 Y5 Z-0.6 R1.8 L3

Esto requiere modo de distancia incremental (`G91`) y OLD\_Z retraer modo (`G98`) y pide la `G81` ciclo de perforación que se repita tres veces. El número de X es 4, la Y número 5, el número Z es -0,6 y el número de R es de 1,8. La primera X es la posición 5 (= 1 +4), la posición Y inicial es 7 (= 2 +5), la clara Z posición es de 4,8 (= 1,8 3), y la posición Z es de 4,2 (4,8-0,6 =). Antiguo Z 3.

El primer movimiento es una travesía a lo largo del eje Z de (1,2,4.8), desde que el viejo Z &lt;claro Z.

La primera repetición consta de tres movimientos.

1.  un paralelo a atravesar el plano XY a (5,7,4.8)

2.  un paralelo de alimentación a la del eje Z a (5,7, 4,2)

3.  un paralelo transversal al eje Z de (5,7,4.8)

La segunda repetición se compone de 3 movimientos. La posición X se pone a 9 (= 5 +4) y la posición Y a 12 (= 7 +5).

1.  un paralelo a atravesar el plano XY a (9,12,4.8)

2.  un paralelo de alimentación a la Z-eje (9,12, 4,2)

3.  un paralelo transversal al eje Z de (9,12,4.8)

La tercera repetición consta de tres movimientos. La posición X se pone a 13 (= 9 +4) y la posición Y a 17 (= 12 +5).

1.  un paralelo a atravesar el plano XY a (13,17,4.8)

2.  un paralelo de alimentación a la Z-eje (13,17, 4,2)

3.  un paralelo transversal al eje Z de (13,17,4.8)

G82 Ciclo de taladrado con temporización<span id="sec:G82-Perforacion-Dwell"></span>
------------------------------------------------------------------------------------

    G82 (X-Y-Z) o (U-V-W-) R-L-P-

El *G82* ciclo está destinado a la perforación con una temporización en el fondo de el agujero.

1.  Movimiento preliminar, como se describió anteriormente.

2.  Mover el eje Z sólo en la velocidad de avance actual en la posición Z.

3.  Morar por el número P de segundos.

4.  Retraer el eje Z en la velocidad de desplazamiento para borrar Z.

G83 taladrado<span id="sec:G83-Perforacion-Peck"></span>
--------------------------------------------------------

    G83 (X- Y- Z-) o (U- V- W-) R- L- Q-

El *G83* ciclo (a menudo llamado paso de taladrado) está destinado a profundidad de perforación o fresado con rotura de la viruta. Se retrae en este ciclo de limpiar el agujero de los chips y cortar cualquier largueros de largo (que son comunes durante la perforación de aluminio). Este ciclo tiene una serie Q, que representa un "delta" incremento a lo largo del eje Z. La profundidad final antes de retractarse de siempre se a la "retracción" avión incluso si G98 es en efecto. La final se retracte honrar a los G98/99 en efecto.

1.  Movimiento preliminar, como se describió anteriormente.

2.  Mover el eje Z sólo en la velocidad de alimentación de corriente a la baja en delta o la posición Z, lo que es menos profundo.

3.  Rápido de vuelta al plano de retractarse especificado por la palabra R.

4.  Vuelta rápida hasta el fondo del agujero actual, retrocedió un poco.

5.  Repita los pasos 2, 3 y 4 hasta la posición Z se alcanza en el paso 2.

6.  Retraer el eje Z en la velocidad de desplazamiento para borrar Z.

Es un error si:

-   El número Q es negativo o cero.

G84 de la Mano Derecha tocar<span id="sec:G84-de-la-mano-derecha-Tapping"></span>
---------------------------------------------------------------------------------

Este código no está implementada en EMC2. Se acepta, pero el comportamiento no está definido. Vea las secciones [\[sec:G33-eje-Sync\]](#sec:G33-eje-Sync) Y [\[sec:G33.1-rigido-Tapping\]](#sec:G33.1-rigido-Tapping)

G85 aburrido, no de permanencia, salida de alimentación<span id="sec:G85-aburrido-Feed-Out"></span>
---------------------------------------------------------------------------------------------------

    G85 (X-Y-Z) o (U-V-W-) R-L-

El *G85* ciclo está diseñado para perforar o fresar, pero podría ser utilizado para la perforación o fresado.

1.  Movimiento preliminar, como se describió anteriormente.

2.  Mover el eje Z sólo en la velocidad de avance actual en la posición Z.

3.  Retraer el eje Z en la velocidad de alimentación de corriente para limpiar Z.

G86 aburrido, parada del cabezal, salida rápida<span id="sec:G86-aburrido-Rapid-Out"></span>
--------------------------------------------------------------------------------------------

    G86 (X- Y- Z) o (U- V- W-) R- L- P-

El `G86` ciclo está destinado a aburrido. Este ciclo se utiliza un número de P para el número de segundos que habitan.

1.  Movimiento preliminar, como se describió anteriormente.

2.  Mover el eje Z sólo en la velocidad de avance actual en la posición Z.

3.  Morar por el número P de segundos.

4.  Detener el giro del cabezal.

5.  Retraer el eje Z en la velocidad de desplazamiento para borrar Z.

6.  Reinicie el cabezal en el sentido que iba.

El eje debe estar girando antes de que este ciclo se utiliza. Se trata de un error si:

-   El eje no está girando antes de que este ciclo se ejecuta.

G87 Volver aburrido<span id="sec:G87-Back-Aburrido"></span>
-----------------------------------------------------------

Este código no está implementada en EMC2. Se acepta, pero el comportamiento no está definido.

G88 aburrido, parada del cabezal, Manual de salida<span id="sec:G88-aburrido-Manual-Out"></span>
------------------------------------------------------------------------------------------------

Este código no está implementada en EMC2. Se acepta, pero el comportamiento no está definido.

G89 aburrido, permanencia, salida de alimentación<span id="sec:G89-aburrido-Dwell"></span>
------------------------------------------------------------------------------------------

    G89 (X-Y-Z) o (U-V-W-) R-L-P-

El *G89* ciclo está destinado a aburrido. Este ciclo se utiliza un número P, donde P especifica el número de segundos que habitan.

1.  Movimiento preliminar, como se describió anteriormente.

2.  Mover el eje Z sólo en la velocidad de avance actual en la posición Z.

3.  Morar por el número P de segundos.

4.  Retraer el eje Z en la velocidad de alimentación de corriente para limpiar Z.

G90, G91 Set modalidad a distancia<span id="sec:G90-G91-Set"></span>
--------------------------------------------------------------------

    G90 es el modo Absolute Distance +
    G91 es el modo de distancia incremental

Interpretación de G Código puede estar en uno de los dos modos de distancia: absoluta o incremental.

Para entrar en modo absoluto la distancia, el programa de `G90`. En términos absolutos modalidad a distancia, los números de los ejes (X, Y, Z, A, B, C, U, V, W) por lo general representan las posiciones en cuanto a la coordinación activa del sistema. Cualquier excepción a esta regla se describen explícitamente en sección [\[sec:G81-G89\]](#sec:G81-G89).

Para entrar en modo incremental la distancia, el programa de `G91`. En incremental números de modalidad a distancia, por lo general representan el eje incrementos de las coordenadas actual.

G90.1, G91.1 Arco modalidad a distancia<span id="sec:G90.1-G91.1"></span>
-------------------------------------------------------------------------

G90.1 modalidad a distancia absoluta de que, las compensaciones de I, J & K.

-   I y J tanto debe ser especificado o es un error

G91.1 modo de distancia incremental para I, J & K compensaciones.

-   Returns I, J & K a su comportamiento normal.

G92, G92.1, G92.2, G92.3 compensaciones sistema de coordenadas<span id="sec:G92-G92.1-G92.2-G92.3"></span>
----------------------------------------------------------------------------------------------------------

    G92 X-Y-Z-A-B-C-U-V-W-

Vea la sección [\[cha:Coordinate-System\]](#cha:Coordinate-System) para una descripción de coordinar sistemas.

Vea la sección [\[sec:G92-Offsets\]](#sec:G92-Offsets) para obtener más información sobre las compensaciones.

Para hacer el punto actual de las coordenadas que desee (sin movimiento), el programa `G92 X-Y-Z-A-B-C-U-V-W-`, donde el eje palabras contienen los números de eje que desea. Todos los ejes las palabras son opcionales, excepto que al menos uno debe ser utilizado. Si un eje palabra no se utiliza para un eje dado, las coordenadas en que el eje de la punto actual no cambia. Es un error si:

-   Eje de todas las palabras se han omitido.

Cuando `G92` se ejecuta, el origen de todos los sistemas de coordenadas se mueven. Se mueven de tal manera que el valor del punto de control actual, en la actualidad activa el sistema de coordenadas, se convierte en el valor especificado. Todas las coordenadas orígenes del sistema se compensan esta misma distancia.

Por ejemplo, supongamos que el punto actual en X = 4 y es allí Actualmente no hay offset `G92` activa. A continuación, `G92` x7 está programado. Este mueve todos los orígenes -3 en X, que hace que el punto actual para convertirse en X = 7. Esta -3 se guarda en el parámetro 5211.

Estando en modo de distancia incremental no tiene ningún efecto sobre la acción de `G92`.

`G92` compensaciones pueden estar ya en vigor cuando el G92 se llama. Si este es el caso, el desplazamiento es reemplazada por una nueva desplazamiento que hace que el punto actual a ser el valor especificado.

Para restablecer las compensaciones de eje a cero, el programa `G92.1` o `G92.2`. `G92.1` establece los parámetros de 5211 a 5219 a cero, mientras que `G92.2` deja su valores actuales solo.

Para definir el eje de desplazamiento a los valores guardados en los parámetros de 5211 a 5219, programa `G92.3`.

Puede establecer las compensaciones de eje en un solo programa y el uso de las compensaciones en el mismo otro programa. Programa `G92` en el primer programa. Esto fijará los parámetros de 5211 a 5219. No usar `G92.1` en el resto del primer programa. El parámetro Los valores se salvo cuando se sale del primer programa y restaura cuando el segundo se pone en marcha. Use `G92.3` cerca del comienzo del segundo programa. Que se restaurará la compensaciones salvo en el primer programa.

EMC2 almacena el G92 compensaciones y los reutiliza en la próxima ejecución de un del programa. Para evitar esto, se puede programar un G92.1 (para eliminar), o un programa de G92.2 (para eliminarlos - que se conserva aún).

G93, G94, G95: Feed Rate Set Mode<span id="sec:G93-G94-G95-modo"></span>
------------------------------------------------------------------------

    G93 es el modo de Tiempo Inverso
    G94 es el modo de unidades por minuto
    G95 es el modo de unidades por la Revolución.

Tres modos de velocidad de alimentación se reconocen: unidades por minuto, tiempo inverso, y las unidades por revolución. Programa G94 para iniciar la unidades por minuto modo. Programa G93 para iniciar el modo de tiempo inverso. Programa G95 para empezar las unidades por el modo de la revolución.

En unidades por minuto modo de avance, una palabra F se interpreta como el punto de control debe moverse en un cierto número de pulgadas por minutos, milímetros por minuto, o grados por minuto, dependiendo de lo que las unidades de longitud están siendo utilizados y que el eje o ejes se están moviendo.

En unidades por el modo de la revolución, una palabra de F se interpreta como la punto de control debe pasar un cierto número de pulgadas por revolución del eje, en función de las unidades de longitud que se están utilizando y que eje o ejes están en movimiento. G95 no es apropiado para roscar, para roscado uso G33 o G76.

En el modo de índice inverso tiempo de alimentación, una palabra F significa que el movimiento debe ser completado en \[uno dividido por el número F\] minutos. Por ejemplo, si el Número F es de 2.0, la medida debe ser completada en medio minuto.

Cuando el alimento de tiempo inverso modo de índice está activo, la palabra F debe aparecer en cada línea que tiene un G1, G2, G3 o el movimiento, y una palabra de F en una línea que no tiene G1, G2, G3 o se ignora. Estar en la alimentación de tiempo inverso el modo de tasa no afecta G0 (marcha rápida) movimientos.

Es un error si:

-   Alimentar a tiempo inverso modo de tasa activa y una línea con G1, G2 o G3 (Explícita o implícitamente) no tiene una palabra F.

-   Un nuevo avance no se especifica después de cambiar a G94 o G95

G96, G97 del eje del modo de control<span id="sec:G96-G97-eje"></span>
----------------------------------------------------------------------

    G96 D [la velocidad del husillo max] S [unidades por minuto] es el modo de velocidad de corte constante +
    G97 es el modo RPM

Dos modos de la varilla de mando son reconocidos: revoluciones por minuto, y CSS (velocidad de corte constante). Programa D-G96-S para seleccionar constante la superficie de la velocidad de los pies por minuto S (G-20 si está en vigor) o metros por minuto (G21 si está en vigor). La velocidad de giro máximo es fijado por el D-número de revoluciones por minuto.

Cuando se utiliza G96, asegurar que en X0 el sistema de coordenadas actual (incluyendo compensaciones y longitud de la herramienta) es el centro de rotación o de EMC no dará la velocidad de giro deseado. G96 no se ve afectada por la radio o el modo de diámetro.

Programa G97 para seleccionar el modo RPM.

Es un error si:

-   S no se especifica con el G96

-   Un movimiento de alimentación se especifica en el modo G96, mientras que el eje no está girando

G98, G99 Ajustar los ciclos enlatados nivel de retorno<span id="sec:G98-G99-Set"></span>
----------------------------------------------------------------------------------------

    G98 retracción en la posición de ese eje se encontraba justo antes de esta serie
        de uno o más ciclos fijos contiguos se inició. +
    G99 retracción de la posición especificada por la palabra R del ciclo fijo.

Cuando se retrae el cabezal durante los ciclos fijos, no dos opciones para indicar la forma en que se retrae: (1) perpendicular Retiro a la posición actual a la posición indicada por la palabra R, o (2) Retiro perpendicular a la posición actual a la posición de que era la de este eje justo antes del inicio de la el ciclo fijo (a menos que la posición es inferior a la indicada por la palabra R, en cuyo caso éste se se utiliza).

Para utilizar (1), el programa *G99*. Para utilizar (2), el programa *G98*. Recordemos que la palabra R tiene el modo de significados diferentes desplazamiento absoluto y modo incremental de los viajes.

La "primera" (G98) avión se restablece cualquier ciclo de tiempo del modo de cámara está abandonados, ya sea explícitamente (G80) o implícitamente (cualquier código de movimiento que no es un ciclo). Cambiar entre los modos de ciclo (por ejemplo G81 a G83) no restablece el "primer" avión. Es posible cambiar G98 y G99 entre en una serie de ciclos.

M Códigos
=========

M0, M1, M2, M30, M60 Programa de Detención y final<span id="sec:M0-M1-M2"></span>
---------------------------------------------------------------------------------

Para poner en pausa un programa en ejecución temporalmente (Independientemente de la configuración del interruptor de parada opcional), programa de `M0`. EMC2 permanece en el modo automático para MDI y otras acciones manuales no están habilitadas.

Para poner en pausa un programa en ejecución temporalmente (Pero sólo si el interruptor de parada opcional en), programa de `M1`. EMC2 permanece en el modo automático para MDI y otras acciones manuales no están habilitadas.

Está bien el programa `M0` y `M1` en el modo MDI, pero el efecto probablemente no se note, debido a un comportamiento normal en el modo MDI se para detenerse después de cada línea de entrada de todos modos.

Para el intercambio de transbordadores de palets y luego se detiene un programa en ejecución temporalmente (independientemente de la configuración del interruptor de parada opcional), programa `M60`.

Si un programa se detiene por un `M0`,`M1`, o `M60`, al pulsar el botón de inicio de ciclo se reiniciará el programa en la siguiente línea.

Para finalizar un programa, el programa `M2`. Para el intercambio de transbordadores de palets y después terminar un programa, el programa `M30`. Ambos comandos tienen los siguientes efectos:

1.  Cambio de modo automático al modo MDI.

2.  Las compensaciones de origen se establecen en el valor por defecto (como `G54`).

3.  Plano seleccionado está en el plano XY (como `G17`).

4.  Modalidad a distancia está en modo absoluto (como `G90`).

5.  El modo de alimentación tasa se establece en unidades por minuto (como `G94`).

6.  Anula avance y la velocidad se establece en ON (como `M48`).

7.  Compensación de cortador está apagado (como `G40`).

8.  El cabezal está parado (como `M5`).

9.  El modo de movimiento actual se establece en los piensos (como `G1`).

10. Refrigerante se apaga (como `M9`).

No hay más líneas de código en un archivo de RS274/NGC se ejecutará después del comando M2 o M30 se ejecuta. Al pulsar inicio del ciclo se iniciará el programa en el principio del archivo.

M3, M4, M5 de control del eje<span id="sec:M3-M4-M5"></span>
------------------------------------------------------------

Para iniciar el huso horario en la "S" de velocidad, el programa `M3`.
 Para iniciar el cabezal a izquierdas en la "S" de velocidad, el programa `M4`.
 Para detener el eje de giro, el programa `M5`.

No hay problema en usar `M3` o `M4` si la velocidad de giro se ajusta a cero. Si esto se hace (O si el interruptor de cancelación de velocidad se habilita y se puso a cero), el eje no empezará a girar. Si, más tarde, la velocidad de giro se encuentra por encima de cero (O el interruptor de cancelación está activado), el eje comenzará a girar. No hay problema en usar `M3` o `M4` cuando el cabezal ya está giro o el uso de `M5` cuando el cabezal se ha detenido.

M6 Cambio de herramienta<span id="sec:M6-Tool-Cambio"></span>
-------------------------------------------------------------

### Cambio de herramienta manual

Si el componente hal\_manualtoolchange HAL está cargado, M6 parará el cabezal y pedir al usuario cambiar la herramienta. Para más información sobre hal\_manualtoolchange ver Sección ([\[sec:Manual-Tool-Change\]](#sec:Manual-Tool-Change))

### Cambiador de herramientas

Para cambiar una herramienta en el eje de la herramienta que está en el eje a la herramienta más recientemente seleccionado (usando una palabra T - véase la Sección [\[sub:T-Seleccionar-Herramienta\]](#sub:T-Seleccionar-Herramienta)), El programa `M6`. Cuando el cambio de herramienta es completa:

-   El eje se detendrá.

-   La herramienta que se ha seleccionado (por una palabra T en la misma línea o en cualquier línea después del cambio de la herramienta anterior) se encuentra en el cabezal. El número de T es un entero que indica la cambiador de la ranura de la herramienta (no su id).

-   Si la herramienta seleccionada no se encuentra en el cabezal antes de que el cambio de herramienta, la herramienta que se encuentra en el cabezal (Si la hubo) estará en la ranura del cambiador. -. Si se ha configurado en el archivo ini algunas posiciones de los ejes pueden moverse cuando un M6 se emite. Vea la sección EMCIO del Manual del Integrador para obtener más información sobre las opciones de cambio de herramienta.

-   No hay otros cambios se harán. Por ejemplo, el refrigerante continuará flujo durante el cambio de herramienta a menos que se haya apagado por un \`M9.

-   La longitud de herramienta no se cambia, utilizar G43 para cambiar la longitud de herramienta.

El cambio de herramienta puede incluir el movimiento del eje. Que está bien (pero no muy útiles) para programar un cambio de la herramienta ya está en el eje. Está bien si no existe una herramienta en la ranura seleccionada; en ese caso, el eje estará vacío después de que el cambio de herramienta. Si es cero slot seleccionado en último lugar, definitivamente habrá ninguna herramienta en el cabezal tras un cambio de herramienta.

M7, M8, M9 refrigerante de control<span id="sec:M7-M8-M9"></span>
-----------------------------------------------------------------

Para activar el refrigerante en la niebla, el programa \`\` M7.
 Para activar el refrigerante de inundación en, el programa *M8*.
 A su vez, todo el refrigerante fuera, el programa \`\` M9.

Siempre es conveniente usar cualquiera de estos comandos, independientemente de lo que el refrigerante está encendido o apagado.

M48-M53 Anulars<span id="sec:Anulars"></span>
---------------------------------------------

### M48, M49 anulación de control<span id="sub:M48-Ambos-Override"></span><span id="sub:M49-Ni-Anular"></span>

Para permitir que la velocidad de giro y los controles de velocidad de avance de invalidación, el programa \`M48. Para desactivar los controles, el programa *M49*. Vea la sección [\[sec:Feed-Interaction\]](#sec:Feed-Interaction) para más detalles. Está bien para activar o desactivar los controles cuando que ya están activados o desactivados. Estos controles también se puede activar de forma individual con M50 y M51 como se describe en el secciones [\[sub:M50-Feed-Anular\]](#sub:M50-Feed-Anular) y [\[sub:M51-eje-Anular\]](#sub:M51-eje-Anular).

### M50 Alimentación anulación de control<span id="sub:M50-Feed-Anular"></span>

Para permitir que la velocidad de avance control de anulación, programa `M50` o `M50 P1`. Para desactivar el control, el programa `M50 P0`. Mientras que las personas con discapacidad anular alimentación no tendrá ninguna influencia, y el movimiento se ejecutará al avance programado. (A menos que haya una adaptación velocidad de alimentación anular activo).

### Husillo M51 de la velocidad de control<span id="sub:M51-eje-Anular"></span>

Para habilitar el control de la velocidad de giro de invalidación, programa \`\` o \`M51 M51 *P1. Para desactivar el programa de control 'M51* P0. Mientras que la velocidad de giro con discapacidad anular no tendrá ninguna influencia, y la velocidad del husillo tendrá el valor exacto del programa especificado (Con el S-palabra como se describe en [\[sub:S-Set-eje\]](#sub:S-Set-eje)).

### M52 control adaptativo de la alimentación<span id="sub:M52-adaptativa-Feed-Control"></span>

Para utilizar una fuente de adaptación, el programa \`\` o `` M52 M52 P1 '. Para dejar de utilizar alimento de adaptación, el programa 'M52' P0. Cuando el alimento de adaptación es activado, un valor de entrada externa se utiliza junto con la interfaz de usuario de alimentación anular el valor y la alimentación de mando tasa para establecer la velocidad de avance real. En EMC2, el HAL pin `motion.adaptive de alimentación 'se utiliza para este propósito. Los valores en `motion.adaptive de alimentación `` debe oscilar de 0 (mantener alimentar) a 1 (máxima velocidad).

### M53 Feed Control Stop<span id="sub:M53-Feed-Stop-Control"></span>

Para habilitar la parada de alimentar a cambiar, el programa \`\` o \`M53 M53 P1 *. Para desactivar el el programa \`switch M53 P0 '. Permitiendo el interruptor de parada de alimentación le permitirá moción para ser interrumpido por medio del control de parada de alimentación. En EMC2, el pasador de HAL \`motion.feed-hold 'se utiliza para este propósito. Los valores de 1, hará que el movimiento se detenga (si 'M53* se activa).

M61 Fijación del Número de la herramienta actual<span id="sec:M61-Set-actual-Tool-Numero"></span>
-------------------------------------------------------------------------------------------------

Para cambiar el número actual de la herramienta, mientras que en inhaladores de dosis medidas o el programa de modo manual Qxx un M61 en la ventana MDI. Un uso es cuando se enciende con una EMC herramienta que está en el eje se puede establecer que el número de herramienta sin hacer un cambio de herramienta.

    Es un error si:

-   Q-no es 0 o mayor

M62 para controlar la salida M65<span id="sec:M62-a-M65"></span>
----------------------------------------------------------------

Para controlar un poco la salida digital, el programa `M- P-`, donde el M-palabra oscila entre 62 a 65, y los rangos de P-palabra de 0 a un valor predeterminado de 3. Si es necesario el número de E / S se puede aumentó con el parámetro num\_dio al cargar el movimiento controlador. Ver el Manual de configuración para los integradores de EMC y de la Sección HAL para obtener más información.

-   El P-palabra especifica el número de salidas digitales.

M62:: A su vez en la salida digital sincronizado con el movimiento

M63:: Apagar la salida digital sincronizado con el movimiento

M64:: A su vez en la salida digital de inmediato

M65:: Desactivar la salida digital de forma inmediata

La M62 y M63 comandos se pondrán en cola. Acciones posteriores se refiere con el número misma salida se sobreponen a los valores anteriores. Más que un cambio en la salida se puede especificar mediante la emisión de más de un M62/M63 comandos.

El cambio real de los resultados especificados va a pasar en el principio del comando siguiente movimiento. Si no hay ningún movimiento posterior comando, los cambios en la cola de salida no va a suceder. Lo mejor es siempre programa de un movimiento de código G (G0, G1, etc) justo después de la M62/63.

M64 y M65 ocurrir de inmediato, ya que son recibidos por el movimiento controlador. Ellos no están sincronizados con el movimiento, y que romper la mezcla.

M66 de entrada de control<span id="sec:M66-Input-Control"></span>
-----------------------------------------------------------------

Para leer el valor de un pin de entrada analógica o digital, programa de ‘M66 P-E-L-Q-’, donde la palabra-P y los intervalos de la palabra E- 0 a 3. Si es necesario, el número de E / S se puede aumentar con el parámetro num\_dio o cuando num\_aio cargar el controlador de movimiento. Ver el integrador de Manual, sección de configuración, EMC y el inciso HAL, para más de la información. Sólo uno de los P o palabras E deben estar presentes. Es un error si ambos están desaparecidos.

M66:: Espera en una entrada

-   El P-palabra especifica el número de entradas digitales.

-   El E-palabra especifica el número de entradas analógicas.

-   La L-palabra especifica el tipo de espera:

     0   
    WAIT\_MODE\_IMMEDIATE - sin esperas, vuelve inmediatamente. El valor actual de la entrada se almacena en el parámetro \# 5399

     1   
    WAIT\_MODE\_RISE - espera para la entrada seleccionada para llevar a cabo un evento de origen.

     2   
    WAIT\_MODE\_FALL - espera para la entrada seleccionada para llevar a cabo un evento de otoño.

     3   
    WAIT\_MODE\_HIGH - espera para la entrada seleccionada a ir al estado ALTO.

     4   
    WAIT\_MODE\_LOW - espera para la entrada seleccionada para ir al estado BAJO.

-   El Q-palabra especifica el tiempo de espera para la espera. Si el tiempo de espera es superado, la espera se interrumpe, y la variable \# 5399 será la celebración de ser el valor -1. El valor de Q se ignora si la L-palabra es igual a cero (inmediata). AQ valor de cero es un error si la L-palabra no es cero.

-   Modo 0 es el único permitido para una entrada analógica.

M66 esperar en una entrada detiene la ejecución ulterior del programa, hasta que el evento seleccionado (o el tiempo de espera programado) se produce.

Se trata de un error de programa M66 tanto con un P-palabra y una palabra de E-(por lo tanto seleccionar tanto una analógica y una entrada digital). EMC2 en estas entradas son no se controla en tiempo real y por lo tanto no debe utilizarse para momento las aplicaciones críticas.

M67 salida analógica<span id="sec:M67-analogico-de-salida"></span>
------------------------------------------------------------------

Para el control de una salida analógica sincronizado con el movimiento, el programa de *M67-Q-E*, donde los rangos de palabra E desde 0 hasta el máximo predeterminado de 3 y Q es el valor de conjunto. El número de I / O se puede aumentar utilizando el parámetro num\_aio al cargar el controlador de movimiento. Ver el "EMC2 y HAL" capítulo en la sección de configuración del integrador Manual para obtener más información sobre el controlador de movimiento. Las funciones de la M67 igual que M62-63. Vea la sección M62-65 para obtener información sobre colas comandos de salida sincronizada con el movimiento.

M68 salida analógica<span id="sec:M68-analogico de salida"></span>
------------------------------------------------------------------

Para el control de una salida analógica de inmediato, el programa 'E-M68-Q, donde\` la palabra E va de 0 al máximo por defecto de 3 y Q el valor de conjunto. El número de I / O se puede aumentar mediante el uso de la num\_aio parámetro al cargar el controlador de movimiento. Ver el EMC2 ", y HAL "capítulo en la sección de configuración del Manual del Integrador para más información sobre el controlador de movimiento. M68 funciona igual que M64-65. Vea la sección M62-65 para obtener información sobre la salida inmediata comandos.

M100 a M199 comandos definidos por el usuario<span id="sec:M100-a-M199"></span>
-------------------------------------------------------------------------------

Para invocar un comando definido por el usuario, el programa `M1nn P-Q-`, donde `P` y `-Q-` son opcionales y debe ser un número. El programa externo "M1nn" debe estar en el directorio con el nombre de \[DISPLAY\] PROGRAM\_PREFIX en el ini archivo y se ejecuta con la P y Q valores como sus dos argumentos. La ejecución del archivo RS274NGC se detiene hasta que se sale del programa invocado. Cualquier archivo ejecutable válido puede ser utilizado.

El error "código M Desconocido utilizados" se refiere a uno de los siguientes

-   El comando especificado definido por el usuario no existe

-   El archivo no es un archivo ejecutable

Por ejemplo, para abrir y cerrar una pinza más cerca que está controlado por un pin parport utilizando un archivo script bash con M101 y M102. Crear dos archivos llamados M101 y M102. Establecer como archivos ejecutables (por lo general botón derecho / propiedades / permisos) antes de ejecutar EMC2. Asegúrese de que el parport pin no está conectado a nada en un archivo HAL.

M101 (nombre de archivo)

    #!/Bin/sh
    # Archivo para activar el parport pin 14 para abrir la pinza más cerca
    halcmd setp parport.0.pin-14-out True
    exit 0

M102 (nombre de archivo)

    #!/Bin/sh
    # Archivo para apagar parport pin 14 para abrir la pinza más cerca
    halcmd setp parport.0.pin-14-out False
    exit 0

Para pasar una variable a un archivo M1nn utilizar el P y Q de esta opción:

    M100 P123.456 Q321.654

En el archivo de M100 que podría tener este aspecto:

    #!/Bin/sh
    voltage = $1
    feedrate = $2
    halcmd setp thc.voltage $voltage
    halcmd setp thc.feedrate $feedrate
    exit 0

O códigos<span id="cha:O-codigos"></span>
=========================================

O-códigos de proporcionar control de flujo en los programas NC. Cada bloque tiene una número de asociados, que es el número que se usa después de la O. Se debe tener cuidado adecuadamente para que coincida con el número S-. O utilizar los códigos de la letra "O" no es el número cero como primer carácter en el número como Ø100.

El comportamiento no está definido si:

-   Otras palabras se usan en una línea con una junta palabra

-   Los comentarios se utilizan en una línea con una junta palabra

Subrutinas: sub, endsub, return, call
-------------------------------------

Subrutinas se extienden desde un `O-sub` a una 'O-endsub \`. Las líneas dentro de la subrutina (el "cuerpo") no se ejecutan en orden, sino que se ejecutan cada vez que se llama la subrutina con \`O-llamada.

    o100 sub (subrutina para mover a la casa de máquinas)
    G0 X0 Y0 Z0
    o100 endsub
    (Número de líneas de intervención)
    o100 call

Dentro de una subrutina, `O-return` puede ser ejecutado. Esto inmediatamente vuelve al código de llamada, sólo como si `O-endsub` se encontró.

`O-call` toma hasta 30 argumentos opcionales, que se pasan a la subrutina como *\# 1*, *\# 2*, …, N. \# Parámetros de \# N 1 al \# 30 tienen la misma valor como en el llamando a su contexto. Al volver de la subrutina, los valores de parámetros del \# 1 al \# 30 (independientemente del número de argumentos) se ser restaurado a los valores que tenían antes de la llamada. Parámetros \# 1 - \# 30 son locales a la subrutina.

Debido a que "`1 2 3`" se analiza como el número 123, los parámetros deben ser encerrado en entre corchetes. El siguiente se llama a una subrutina con tres argumentos:

    O200 llame al [1] [2] [3]

Cuerpos subrutina no se pueden anidar. Sólo se puede llamar después de se definen. Se les puede llamar de otras funciones, y la llamada puede ellos mismos de forma recursiva si tiene sentido hacerlo. El máximo el nivel de imbricación de subrutinas es de 10.

Subrutinas no tienen "valor de retorno", pero se puede cambiar el valor de los parámetros por encima de \# 30 y esos cambios serán visibles para el código de llamada. Subrutinas también puede cambiar el valor de nombre global parámetros.

Looping: do, while, endwhile, break, continue
---------------------------------------------

El "while" tiene dos estructuras: while/endwhile, y do/while. En cada caso, se sale del bucle cuando el "mientras" condición se evalúa como falsa.

    (Dibujar una forma de diente de sierra)
    F100
    # 1 = 0
    O101 while [# 1 lt 10]
    G1 X0
    G1 Y [# 1 / 10] X1
    # 1 = [# 1 +1]
    O101 endwhile

Dentro de un bucle while, `O-break` de inmediato sale del bucle, y `O-continue` seguir inmediatamente salta a la próxima evaluación del tiempo\` condición. Si es cierto, el bucle comienza de nuevo en la parte superior. Si es falso, que sale del bucle.

Condicionales: if, else, endif
------------------------------

El "if" condicional se ejecuta un grupo de instrucciones si una condición es verdadera y otra si es falsa.

    (Fijar cambio de alimentación en función de una variable)
    O102 if [# 2 GT 5]
    F100
    O102 else
    F200
    O102 endif

Repita/Repeat
-------------

La "repetición" se ejecutará las instrucciones dentro de la repetición / endrepeat el número específico de veces. El ejemplo muestra cómo es posible que un molino serie diagonal de formas a partir de la presente posición.

    (Mill 5 formas en diagonal)
    G91 (modo incremental)
    O103 repeat [5]
    ... (Insertar el código de molienda aquí)
    G0 X1 Y1 (movimiento diagonal a la siguiente posición)
    O103 endrepeat
    G90 (modo absoluto)

Indirección
-----------

El O-número puede ser dada por un parámetro o el cálculo.

    O [# 101 2] llamada

Valores de Informática en O-palabras
------------------------------------

En O-es decir, los parámetros (sección [\[sub:Numbered-Parameters\]](#sub:Numbered-Parameters)), Expresiones (sección [\[sub:Expressions\]](#sub:Expressions)), Los operadores binarios (sección [Binary Operators](#sub:Binary-Operators)), y funciones (tabla [operators](#cap:Functions)) son particularmente útiles.

Archivos de Llamadas
--------------------

Para llamar a un archivo separado con un nombre de subrutina el archivo de la misma su llamada e incluyen un sub y endsub en el archivo. El archivo debe estar en el directorio apuntado por PROGRAM\_PREFIX. El nombre del archivo puede incluir letras minúsculas, números, guión y guión bajo solamente.

        o<myfile> call (un archivo llamado)
    o
        o123 call (número de archivo)

En el archivo llamado se debe incluir el sub oxxx y endsub y la archivo debe ser un archivo válido.

    myfile.ngc
    o sub <myfile>
    ...
    o <myfile> endsub
    M2

Otros Códigos
=============

F: Fijar la alimentación<span id="sub:F-Set-Feed"></span>
---------------------------------------------------------

Para configurar la velocidad de avance, el programa `F<n>` donde "n" es un número. La aplicación de la tasa de alimentación es como se describe en la Sección [\[sec:Feed-Rate\]](#sec:Feed-Rate), A menos que alimentar a tiempo inverso modo de índice es, en efecto, en cuyo caso la tasa de alimentación es como se describe en Sección [\[sec:G93-G94-G95-modo\]](#sec:G93-G94-G95-modo).

S: Velocidad conjunto de ejes<span id="sub:S-Set-eje"></span>
-------------------------------------------------------------

Para establecer la velocidad en revoluciones por minuto (RPM) del cabezal, el programa `S`. El eje gira a esa velocidad cuando se ha programado para comenzar a dar vuelta. Está bien para programar una palabra S si el cabezal está girando o no. Si el interruptor de cancelación de velocidad y activado, y no a 100% la velocidad va a ser diferente de lo que está programado. Está bien para programar S0, el eje no se enciende si se hace eso.

Es un error si:

-   El número S es negativo.

Como se describe en la sección [\[sec:G84-de-la-mano-derecha-Tapping\]](#sec:G84-de-la-mano-derecha-Tapping), si un *G84* (Tapping) ciclo fijo se activa y anular el avance y la velocidad interruptores están habilitadas, el conjunto de uno en la posición más baja se efecto. La velocidad y la velocidad de avance aún se sincronizarán. En este caso, la velocidad puede variar de lo que está programado, aunque la velocidad interruptor de corrección se establece en 100%.

T: Herramienta de selección<span id="sub:T-Seleccionar-Herramienta"></span>
---------------------------------------------------------------------------

Para seleccionar una herramienta, el programa `T` &lt;n&gt;, donde el número &lt;n&gt; es el carrusel de ranura de la herramienta. La herramienta está no cambió hasta que un M6 \`está programado (ver Sección [\[sec:M6-Tool-Cambio\]](#sec:M6-Tool-Cambio)). La palabra T puede aparecen en la misma línea que el *M6* o en una línea anterior. Está bien, pero normalmente no útil, si las palabras de T aparecen en dos o más líneas sin cambiar de herramienta. El carrusel se puede mover mucho, pero la palabra T más reciente entrará en vigor en los próximos herramienta de cambio. Está bien el programa *T0*; ninguna herramienta será seleccionado. Este es útil si desea que el husillo se vacía después de un cambio de herramienta.

Es un error si:

-   Un número negativo se utiliza T,

-   O un número de T mayor que el número de ranuras en el carrusel se utiliza.

En algunas máquinas, el carrusel se moverá cuando una palabra T está programado, al mismo tiempo de mecanizado que está ocurriendo. En este tipo de máquinas, la programación las líneas T palabra varios antes de un cambio de herramienta permitirá ahorrar tiempo. Un común práctica de programación para este tipo de máquinas es de poner la palabra T para la siguiente herramienta que se utilizará en la línea después de un cambio de herramienta. Esto maximiza el tiempo disponible para el carrusel de moverse.

Se mueve rápido después de un T&lt;n&gt; no se mostrará en la vista previa hasta después de AXIS un movimiento de alimentación. Esto es para las máquinas que viajar largas distancias para el cambio la herramienta como un torno. Esto puede ser muy confuso al principio. A su vez esta función para el programa de cambio de la herramienta actual de un G1 sin ningún tipo de movimiento después de la T&lt;n&gt;.

Comentarios<span id="sec:Comentarios"></span>
---------------------------------------------

Caracteres imprimibles y espacios en blanco entre paréntesis es un comentario. Un paréntesis a la izquierda siempre se inicia un comentario. El comentario termina en el paréntesis encontró primero a partir de entonces. Una vez que un paréntesis de apertura se colocada en una línea, un paréntesis de juego deberá comparecer ante la final de la línea. Los comentarios no pueden anidarse, es un error si a la izquierda se encuentra entre paréntesis después del inicio de un comentario y antes de fin de el comentario. He aquí un ejemplo de una línea que contiene un comentario:

`G80 M5 (stop motion)`

Comentarios son sólo informativos, no son la causa de una máquina de hacer nada.

Mensajes<span id="sec:Mensajes"></span>
---------------------------------------

Un comentario contiene un mensaje si \`glutamato monosódico 'aparece después de la izquierda paréntesis y antes de los caracteres de impresión. Las variantes de \`Glutamato monosódico que incluyen espacios en blanco y caracteres en minúsculas están permitidos. El resto de los personajes antes del paréntesis de la derecha se consideran ser un mensaje. Los mensajes deben ser mostrados en la pantalla de mensajes dispositivo. Mensajes que no contienen comentarios no tienen que ser exhibidos allí.

Sonda de Registro<span id="sub: Sonda de Registro de"></span>
-------------------------------------------------------------

Un comentario también se puede utilizar para especificar un archivo de los resultados de G38.x el sondeo. Vea la sección [\[sec:G38.x-Straight-Probe\]](#sec:G38.x-Straight-Probe).

A menudo, en general el registro es más útil que el registro de la sonda. Uso generales de registro, el formato de los datos de salida se puede controlar.

### Registro General de<span id="sub: General de registro de"></span>

#### (LOGOPEN, nombre de archivo)

Abre el archivo de registro con el nombre. Si el archivo ya existe, se trunca.

#### (LOGAPPEND, nombre de archivo)

Abre el archivo de registro con el nombre. Si el archivo ya existe, los datos se añade.

#### (LOGCLOSE)

Si el archivo de registro está abierto, se cierra.

#### (LOG,…)

El mensaje "…" se expande como se describe a continuación y por escrito a la archivo de registro, si está abierta.

### Mensajes de depuración<span id="sub:mensajes-de-depuracion"></span>

Comentarios que se parecen: `(debug, el resto del comentario)` son los mismos que comentarios como `(msg, el resto del comentario)` con la adición de especial el manejo de los parámetros.

Comentarios que se parecen: `(print, el resto del comentario)` se emiten a stderr con un tratamiento especial para los parámetros.

### Parámetros de comentarios especiales

En los comentarios DEBUG, Print and log, los valores de los parámetros de la mensaje se expanden.

Por ejemplo: para imprimir una variable llamada mundial a stderr (el valor predeterminado ventana de la consola) añadir una línea a su Gcode como …

    (print,fresa dia = #<_endmill_dia>)

Dentro de estos tipos de comentarios, secuencias como `#123` se sustituyen por el valor del parámetro 123. Secuencias como `#<named parameter>` se sustituyen por el valor del parámetro con nombre. Recuerde que el nombre parámetros se han eliminado los espacios en blanco de ellos. Parámetro así, `#<nombre de parameter>` es lo mismo que `#<nombredeparameter>`.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


