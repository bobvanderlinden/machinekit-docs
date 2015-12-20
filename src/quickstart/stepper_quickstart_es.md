Configuración de motores a pasos
================================

<span id="cha:stepper-quickstart"></span>

Esta sección asume que una instalación estándar a partir de un LiveCd ha sido realizada. Posterior a la instalación es recomendable conectar la computadora al internet y esperar por la aparición del manejador de actualizaciones y obtener las ultimas actualizaciones para el EMC y Debian antes de continuar. Para instalaciones mas complejas vea el manual del integrador.

Prueba de Latencia
------------------

La prueba de latencia determina cuanto tiempo le toma al procesador de su computadora responder a una solicitud de procesamiento. Algunos Hardware pueden interrumpir el procesamiento lo que puede traducirse a la perdida de algunos pasos cuando se opera una maquina CNC. Esto es la primer cosa que se requiere hacer posterior a la instalación. Siga las instrucciones de la sección [here](#cha:latency-test) Para correr La prueba de latencia.

Sherline
--------

Si usted posee una maquina marca Sherline varias configuraciones predefinidas están disponibles. Estas configuraciones se encuentran en el menú principal CNC/EMC donde puede seleccionar la configuración que sea compatible con el tipo de maquina que posea y hacer clic, para copiarla y salvarla.

Xylotex
-------

Si usted tiene una maquina marca Xylotex, usted puede escapar las siguientes secciones para pasar directamente a la sección de Asistente de Configuración de Motores a Pasos ubicada en [Wizard](#cha:stepconf-wizard). EMC provee una configuración rápida para maquinas Xylotex.

Información sobre la Maquina
----------------------------

Obtenga la información sobre cada eje de su maquina.

Los tiempos de los controladores están en nanosegundos. Si usted no esta seguro con respecto a los tiempos de su controlador de motor a pasos algunos tiempos específicos para controladores populares están incluidos en el asistente de configuración de motores a pasos. Nota: Algunos controladores marca Gecko de nueva generación tienen tiempos que difieren con los originales. Una lista con tiempos de diversos controladores es mantenida en el sitio wiki del Machinekit que es administrado por los mismos usuarios, en la siguiente dirección: [list](http://wiki.machinekit.org/)

<table>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Eje</th>
<th align="left">Tipo de controlador</th>
<th align="left">Tiempo de paso en ns</th>
<th align="left">Tiempo entre pasos en ns</th>
<th align="left">Dir. Mantener en ns</th>
<th align="left">Dir. Cambiar en ns</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>X</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>Y</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>Z</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

Información de los pines de salida
----------------------------------

Obtenga la información sobre las conecciones de su maquina hacia el puerto Paralelo de su computadora.

<table>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pin de Salida</th>
<th align="left">Función Típica</th>
<th align="left">Si es Diferente</th>
<th align="left">Pin de Entrada</th>
<th align="left">Función típica</th>
<th align="left">Si es Diferente</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>Salida de Paro E-stop</p></td>
<td align="left"><p></p></td>
<td align="left"><p>10</p></td>
<td align="left"><p>X Limite/Casa</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p>Paso eje X</p></td>
<td align="left"><p></p></td>
<td align="left"><p>11</p></td>
<td align="left"><p>Y Limite/Casa</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>Dirección Eje X</p></td>
<td align="left"><p></p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>Z Limite/Casa</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>Paso eje Y</p></td>
<td align="left"><p></p></td>
<td align="left"><p>13</p></td>
<td align="left"><p>A Limite/Casa</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>5</p></td>
<td align="left"><p>Dirección Eje Y</p></td>
<td align="left"><p></p></td>
<td align="left"><p>15</p></td>
<td align="left"><p>Zonda de Prueba</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>6</p></td>
<td align="left"><p>Paso eje Z</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>7</p></td>
<td align="left"><p>Dirección Eje Z</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>8</p></td>
<td align="left"><p>Paso eje A</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>9</p></td>
<td align="left"><p>Dirección Eje A</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>14</p></td>
<td align="left"><p>Husillo CW</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>16</p></td>
<td align="left"><p>Husillo PWM</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>17</p></td>
<td align="left"><p>Amplificador Habilitado</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

Nota: Cualquier pin no usado debe ser definido como **Unused** en el menú desplegable de configuaracion. Esto puede ser cambiado posteriormente ejecutando de nuevo el programa Stepconf

Información Mecánica
--------------------

Obtenga la información de sus motores a pasos y en caso de existir de las reducciones mecánicas que este usando. El resultado sera el desplazamiento lineal en cada eje por paso en el motor, esta información sera utilizada en el parámetro SCALE del archivo de configuración .ini.

<table>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Eje</th>
<th align="left">Pasos/Rev.</th>
<th align="left">Micro Pasos</th>
<th align="left">Dientes del motor</th>
<th align="left">Dientes en el tornillo guía</th>
<th align="left">Paso del tornillo guía</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>X</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>Y</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>Z</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

-   *Pasos por revolución* - indica cuantos pasos del motor le toma a la flecha del motor completar una revolución completa, un valor típico es 200 Pasos/Rev.

-   *Micro Pasos* - este parámetro indica cuantos pasos generador por el software necesita el controlador del motor para producir un paso completo en el motor Algunos controladores dividen los pasos del motor para aumentar la resolución si no se utilizaran Micro Pasos este parámetro debe ser puesto en 1, en caso de utilizar micro pasos el valor dependera del Hardware especifico usado.

-   *Dientes del Motor y \*Dientes del tornillo guía* - estos parámetros se utilizan si se esta utilizando algún tipo de reducción mecánica (engranes, cadenas, bandas de tiempo, etc.) entre el motor y el tornillo guía. Si no se utiliza reducción el parámetro debe ser puesto al valor 1.

-   *Paso del tornillo guía* - es cuanto movimiento lineal ocurre (en las unidades de usuario) cuando el tornillo guía da una vuelta completa. Si se utilizan pulgadas entonces es pulgadas por revolución. Si se utilizan milímetros entonces es milímetros por revolución

El resultado de la combinación de parámetros que se busca es cuantos pasos producidos por el software CNC le tomara al eje moverse linealmente una unidad de usuario (pulgadas o mm).

Example 1. Unidades Pulgadas

    Motor a Pasos             = 200 Pasos por revolución
    Controlador de Motor      =  10 micro pasos por paso
    Dientes del motor         =  20
    Dientes del tornillo guía =  40
    Paso del tornillo guía    =   0.2000 pulgadas por revolución

A partir de la información anterior, el tornillo guía se mueve 0.200 pulgadas por vuelta. - El motor da 2 vueltas por una vuelta del tornillo guía. - El controlador necesita 10 micro pasos de entrada para hacer al motor dar un paso. - El controlador necesita 2000 pasos para hacer que el motor de una revolución. Por lo tanto la escala necesitada es:

![](images/step-calc-inch-math.png)

Example 2. Unidades mm

    Motor a Pasos             = 200 Pasos por revolución
    Controlador de Motor      =  8 micro pasos por paso
    Dientes del motor         =  30
    Dientes del tornillo guía =  90
    Paso del tornillo guía    =   5.00 mm por revolución

A partir de la información anterior, el tornillo guía se mueve 5.00 mm por vuelta. - El motor da 3 vueltas por una vuelta del tornillo guía. - El controlador necesita 8 micro pasos de entrada para hacer al motor dar un paso. - El controlador necesita 1600 pasos para hacer que el motor de una revolución. Por lo tanto la escala necesitada es:

![](images/step-calc-mm-math.png)

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


