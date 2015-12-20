Asistente de configuracion Stepconf
===================================

<span id="cha:stepconf-wizard"></span>

Machinekit es capaz de controlar un vasto rango de diferentes tipos de maquinaria, utilizando diferentes interfaces de Hardware.

Stepconf es un programa que genera archivos de configuracion para Machinekit para un tipo especifico de maquina CNC: Aquellas que son controladas atravez de un *Puerto paralelo estandar*, y que son controladas utilizando las senales *Paso y Direccion*

Stepconf se instala automaticamente cuando instala Machinekit y se encuentra en el menu CNC.

Stepconf genera un archivo en el directorio emc2/config en el cual guarda las selecciones de cada configuracion que usted genere. Cuando se desea cambia algo, se necesita seleccionar el archivo que tenga el mismo numaero que la configuracion que desea modificar. La extencion del archivo es .stepconf.

El asistente Stepconf necesita al menos una resolucion de pantalla de 800 x 600 para que los botones de la parte baja de la pantalla sean visibles.

Instrucciones paso a paso
=========================

Pagina de entrada<span id="sec:Entry-Page"></span>
--------------------------------------------------

![](images/stepconf-config.png)

Figure 1. Pagina de entrada<span id="cap:Entry-Page"></span>

 Create New   
Crea una configuracion nueva.

 Modify   
Modifica una configuracion existente. Despues de seleccionar esta opcion una pantalla de seleccion de archivo aparecera y usted devera el archivo con extencion .stepconf que dese modificar. Si usted relizo alguna modificacion previa a los archivos principales .hal o .ini estas modificaciones se perderan. Modificaciones a los archivos custom.hal y custom\_postgui.hal no seran canbiadas por el

 asistente Stepconf.Create Desktop Shortcut   
This will place a link on your desktop to the files.

 Create Desktop Shortcut   
Se generara un acceso rapido en su a los archivos.

 Create Desktop Launcher   
Se generara un acceso rapido pra iniciar la aplicacion.

Informacion Basica<span id="sec:Basic-Information"></span>
----------------------------------------------------------

![](images/stepconf-basic.png)

Figure 2. Informacion Basica<span id="cap:Basic-Information-Page"></span>

 Machine Name   
Seleccione un nombre para su maquina Utilise solo letras mayusculas, minusculas, digitos, *-* y *\_*.

 Axis Configuration   
Seleccione XYZ (Fresadora), XYZA (Fresadora de 4 ejes) o XZ (Torno).

 Machine Units   
Seleccione entre pulgadas y milimetros. Todas las preguntas posteriores (Tales como el largo de los ejes, el paso de los tornillos, etc) deveran ser contestadas utilizando las unidades seleccionadas

 Driver Type   
Si usted tiene uno de los controladores de motor a pasos listados en el menu desplegable, seleccionelo directamente. En cualquier otor caso, busque los 4 valores de tiempo necesarios utilize los manuales de su controlador y rellene los campos. Si sus manuales le dan los datos en microsegundos multipliquelos por 1000. por ejemplo, si el manual marca 4.5us escriva 4500ns.

Una lista de controladores populares, asi como sus tiempos puede ser consultada en la pagina wiki de Machinekit.org en la siguiente direccion [Stepper Drive Timing](http://wiki.machinekit.org/cgi-bin/emcinfo.pl?Stepper_Drive_Timing).

Acondicionamiento estra de senal o aislamiento electrico como el uso de optoacopladores y filtros RC en targetas de aislamiento pueden imponer diferentes valores de tiempo a los normales de su controlador. Puede ser el caso que se requiera agregar tiempo extra a los valores de tiepo para compensar los filtros o aislamientos. La seccionde seleccion de configuracion tiene las maquinas de marca Sherline ya configuradas para su uso en caso de que posea una de estas.

 Step Time   
Cuanto tiempo el pulso de paso esta "Encendido" en nanosegundos.

 Step Space   
Tiempo minimo entre dos pulsos de paso en nanosegundos.

 Direction Hold   
Cuanto tiempo el pin de direccion deve ser mantenido despues de un cambio de direecion en nanosegundos.

 Direction Setup   
Cuanto tiempo debe aver antes de un cambio de direccion despues del ultimo pulso de paso.

 First Parport   
Usualmente la direcion en hexadecimal del primer puerto paralelo es 0x378.

 Second Parport   
En caso de ser necesario especificar un puerto paralelo extra introduca la direccion y el tipo. Para informacion de como encontrar la direccion de puertos paralelos PCI vea la seccion Port Address en el manual de integrador. (Trate primero con 0x278 o 0x3BC)

 Base Period Maximum Jitter   
Introduca el resultado de la prueba de latencia. Para correr la prueba de latencia precione el boton "Test Base Period Jitter". Vea la seccion de la prueba de latencia para mas detalles.

 Max Step Rate   
Stepconf automaticamente calculara la taza maxima de pulsos de pasos basandose en las caracteristicas del controlador de motor a pasos y el resultado de la prueba de latencia.

 Min Base Period   
Stepconf automaticamente calculara el periodo base minimo basandose en las caracteristicas del controlador de motor a pasos y el resultado de la prueba de latencia.

 Onscreen Prompt For Tool Change   
Si esta casilla es seleccionada, Machinekit pausara la ejecucion de un programa y le preguntara por el cambio de herramienta cuando el comando **M6** sea encontrado en el codigo G. Deje esta casilla sin checar amenos que usted planie agregar soporte para una torreta automatica de cambio de herramientas en un archivo HAL personalizado.

Prueba de latencia<span id="sub:latency-test"></span>
-----------------------------------------------------

Mientras se ejecute la prueba, usted devera de *abusar* de la computadora. Mueva ventanas alrededor de la pantalla. Navegue en internet. Copie algunos archivos de gran tamano en diferentes partes del disco duro. Reproduca musica. Corra algun programa OpenGl como glxgears. La idea es poner a la computadora en apuros mientras se ejecuta la prueba para poder tener una idea de cuales seran los peores casos de demanda a la computadora y sus tiempo de respuesta. Ejecute la prueba almenos unos cuantos minutos. Entre mas tiempo la ejecute mas probable es que detecte casos especiales que solo suceden en intervalos poco frecuentes. Esta prueba es solo para la computadora, no se requiere que conecte los controladores de motores o la maquina herramienta.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Warning
</div></td>
<td align="left">No ejecute Machinekit mientras realiza la prueba de latencia.</td>
</tr>
</tbody>
</table>

![](images/latency.png)

Figure 3. Prueba de Latencia<span id="cap:Latency-Test"></span>

Latencia es cuanto le tomara a la PC detenerse en lo que esta haciendo y responder a una solicitud externa. En este caso, la solicitud el el *latido periodico* que sirve como referencia de tiempo para la genracion de los pulsos de paso. Entre menor sea la latencia, mas rapido se generaran los latidos, y mas rapidos y suabes seran los pulsos de paso.

La latencia es mucho mas importante que la velocidad del CPU. La velocidad del CPU no es el unico factor determinate en la latencia. Tahgetas madre, targetas de video, puertos USB, Problemas con SMI, y otra cantidad de coasas pueden afectar la latencia.

Troubleshooting SMI Issues (Machinekit.org Wiki)

Encuentre soluciones a algunos problemas de SMI comunes en Debian

<http://wiki.machinekit.org/cgi-bin/emcinfo.pl?FixingSMIIssues>

Los numeros importantes son el "max jitter". en el ejemplo de abajo 9075 nanosegundos, o 9.075 microsegundos, es el maximo retraso. Guarde este numero, y escrivalo en la caja Base Period Maximum Jitter.

Si el maximo retrazo es menor o se encuentra entre 15-20 microsegundos (15000-20000 nanosegundos), la computadora deveria de dar muy buenos resultados con la generacion de pulsos de pasos. Si la latencia maxima esta entre 30-50 microsegundos, se pueden seguir obteniendo buenos resultados, pero la tasa maxima de generacion de pulsos puede ser un poco desepcionante, especialmente si se usan micropasos o un tornillo con un paso muy fino. si los numeros son 100us o mas (100 000 nanosegundos), la PC no es una buena candidata para la generacion de pulsos de paso por software. Numeros arriva de 1 milisegundo (1 000 000 nanosegundos) significan que la PC no es una buena candidata para ejecutar Machinekit, sin importar si se usa generacion de pulsos de paso por software o no.

Ajustes del puerto Paralelo<span id="sec:Parallel-Port-Setup"></span>
---------------------------------------------------------------------

![](images/stepconf-pinout.png)

Figure 4. Pagina de ajuste del Puerto Paralelo<span id="cap:Parallel-Port-Setup"></span>

Para cada pin se devera seleccionar la señal de control que concuerde con la configuracion del puerto.

Active la casilla "invert" si la señal de control requiere ser invertida (0V para activo/Verdadero, 5V para inactivo/Falso)

 Esquemas de pines predefinidos   
Se configuraran automaticamente los pines del 2 al 9 deacuerdo al estandar de las maquinas Sherline (Direccion en los pines 2, 4, 6, 8) o Xylotex (Direccion en los pines 3, 5, 7, 9).

 Entradas y Salidas   
Si el pin no sera utilizado como entrada o salida seleccionarlo como "Unused".

 Señal de Paro Externo (E stop)   
Esta señal pude ser tipicamente seleccionado en la casilla desplegable. Una cadena de señal de paro tipica utiliza solo contactos normalmnete cerrados en serie.

 Posicion de inicio y limites de seguridad (Homing & Limit Switches)   
Estos pines pueden ser seleccionados para la mayoria de las configuraciones utilizando la casilla desplegable.

 Bomba de Carga (Charge Pump)   
Si el controlador de motores requiere de una se;al de bomba de carga simplemente seleccione esta opcion de lalista desplegable y conecte la señal al pin seleccionado. La salida de la bomba de carga sera conectada a la tarea base por el programa Stepconf. La salida de bomba de carga sera aproximadamente 1/2 de la maxima tasa de generacion de pulsos de paso mostrados en la pagina de configuracion basica.

Configuracion de los Ejes<span id="sec:Axis-Configuration"></span>
------------------------------------------------------------------

![](images/stepconf-axis.png)

Figure 5. Pagina de configuracion de eje<span id="cap:Axis-Configuration-Page"></span>

 Pasos del motor por revolucion (Motor Steps Per Revolution)   
El numero de pasos completos por revolucion del motor. Si solo se tiene el dato de los grados por paso del motor (ejemplo 1.8 grados), se deve dividir 360 por el numero de grados por paso para encontrar el numero de pasos por revolucion.

 Micro pasos (Driver Microstepping)   
El numero de micropasos producidos por el controlador por cada paso fisico completo del motor. entre "2" para medio paso. (ejemplo, si el controlador produce 1/10 de giro de un paso completo en la flecha del motor por cada pulso de paso que recive, escriva 10 en la casilla.

 Relacion de Poleas (Pulley Ratio)   
Si su maquina tiene poleas o engranes entre el motor y el tornillo, escriva la relacion aqui. Si no, escriva "1:1".

 Paso del tornillo (Leadscrew Pitch)   
Entre el paso del tornillo aqui. Si se selecciono unidades en "Inch", entre el numero de cuerdas por pulgada (ejemplo, entre 8 para 8 TPI). Si se tiene un tornillo con multiples cuerdas se requiere saber cuantas vueltas por pulgada se requieren para mover la "nues". Si se selecciono *mm* como unidades, entre el numero de milimetros que la "nues" se movera por revolucion (ejemplo, entre 2 para 2 mm/rev). Si la maquina se mueve en la direccion opuesta a la esperada, entre un valor negativo, o invierta la direccion del pin para el eje.

 Velocidad Maxima (Maximum Velocity)   
Entre la velocidad maxima del eje en unidades por segundo.

 Aceleracion Maxima (Maximum Acceleration)   
El valor correcto de esta casilla solo puede ser determinado por experimentacion. Vea [\[sec:finding-maximum-velocity\]](#sec:finding-maximum-velocity) para ajustar la velocidad [\[sec:finding-maximum-acceleration\]](#sec:finding-maximum-acceleration) para ajustar la aceleracion.

 Posicion de Inicio (Home Location)   
La posicion a la que la maquina se movera despues de completar el procedimiento de inicio del eje. Para maquinas sin interruptores de posicion de inicio, esta es la posicion a la cual el operador devera mover la maquina antes de precionar el boton de inicializanon del eje (Home). Si se combinan los interruptores de inicio y de limite se devera mover la maquina fuera del interruptor para inicializar el eje o se recivira un error de limite en el eje.

 Area de la bancada (Table Travel)   
El rango de viaje que el codigo g no podra sobrepasar. La posicion de inicializacion del eje deve estar dentro del area de bancada. En particular, tener la posicion de inicializacion (Home) de un eje exactamente en un limite del area de bancada producira una configuracion invalida.

 Localizacionde los interruptores de inializacion(Home Switch Location)   
La posicion en la cual el interruptor de inializacion se activa o desactiva durante un proceso de inicializacion. Este apartado y los dos siguientes solo apareceran cuando se seleccione la existencia de interruptores de limite en la configuracion del los pines del puerto paralelo. Si se combinan los interruptores de limite y de inicializacion la posicion del interruptor de inicializacion no puede ser la misma que la posicoin de inicializacion o se producira un error de limite en el eje.

 Velocidad de inicializacion (Home Search Velocity)   
La velocidad usada en la busqueda de los interruptores. Si el interruptor se encuentra cercano al limite de viaje del eje, esta velocidad deve ser seleccionada de tal forma que el eje tenga suficiente tiempo para desacelerar hasta detenerse antes de llegar al limite fisico de la bancada. Si el interuptor se encuentra cercano por un rango de viaje corto (En lugar de estar cercano desde el punto de inicio al final del viaje), la velocidad devera ser seleccionada de tal forma que el eje pueda desacelerar hasta detenerse antes de que el interruptor se habra otra vez, el procedimiento de inicializacion devera ser comenzarse siempre del mismo lado del interruptor. Si la maquina se mueve en la direccion contraria al inicio de la inicializacion, cambie el signo a negativo del parametro **Home Search Velocity**.

 Direccion de busqueda de posicion de inicio (Home Latch Direction)   
Seleccione "Igual (Same)" para que el interruptor sea liberado y posteriormente la maquina se acerque a el a muy baja velocidad. La segunda vez que el interruptor se cierre, se definira la posiocn de inializacion. Seleccione "Opuesto (Opposite)" para realizar la inializacion moviendose despacio fuera del interruptor, cuando el interruptor se habra la posiocion de inializacion sera marcada.

 Tiempo para acelerar a maxima velocidad (Time to accelerate to max speed)   
Tiempo calculado.

 Distancia para acelerar a maxima velocidad (Distance to accelerate to max speed)   
Distancia calculada.

 Taza de generacion de pulsos a maxima velocidad (Pulse rate at max speed)   
Este dato se calcula en base a los valores anteriores. El valor maximo de la **Taza de generacion de pulsos a maxima velocidad** determina el **Periodo base**. Valores por encima de 20000Hz pueden producir tiempos de respuesta muy bajos o incluso bloqueos (La taza de generacion maxima de pulsos varia entre computadoras)

 Escala del Eje (Axis SCALE)   
El numero que sera usado en el archivo ini en la seccion \[SCALE\]. Representa cuantos pasos se deven dar por unidad de usuario.

 Probar este Eje (Test this axis)   
Esta opcion abre una ventana para permitir probar cada eje. Esta opcion puede ser utilizada despues de llenar toda la informacion referente al eje.

### Probar este Eje

![](images/stepconf-test.png)

Figure 6. Probar este Eje<span id="cap:Test-This-Axis"></span>

Con Stepconf es sencillo probar diferentes valores de aceleracion y velocidad.

#### Busqueda de Velocidad Maxima

Comiense con una aceleracion baja (por ejemplo, **`2 pulgadas/s2`** or **`50 mm/s2`**) la velocidad que se desea obtener. Utilizando los botones disponibles, mueva el eje cerca al centro de su carrera. Tenga cuidado porque con un valor de aceleracion bajo, puede tomarle al eje una sorpendente distancia para desacelerar hasta detenerse.

Despues de medir la cantidad de espacio de movimiento disponible para el eje, introduca una distancia segura en el area de prueba, mantenga en mente que despues de un atoramiento, el motor puede acontinuacion comenzar a moverse en una direccion inesperada. Entonses haga click en la opcion Correr (Run).

La maquina comenzara a moverse hacia adelante y atras a lo largo del eje. En esta prueba, es importante que la combinacion de aceleracion y area de prueba permita a la maquina alcanzar la velocidad seleccionada y que la bancada viaje por almenos una distancia corta a esta velocidad — entre mas distancia mejor sera la prueba. La formula **`d = 0.5 * v * v/a`** proporciona la minima distancia requerida para alcanzar la velocidad especificada con la aceleracion seleccionada. Si es conveniente y seguro de hacer, precione la bancada contra la direcion del movimiento para simular las fuerzas de corte. si la maquina se detiene, redusca la velocidad y comiense la prueba de nuevo.

Si la maquina no se detiene de manera evidente, precione el boton *Run* de nuevo, para detener la prueba. La maquina regresara a la posicion donde comenso la prueba. Si la posicion es incorrecta, la maquina perdio pasos o se detubo durante la prueba. Redusca la velocidad y comienze la prueba de nuevo.

Si la maquina no se mueve, se detiene, o pierde pasos, sin importar cuan baja sea la velocidad seleccionada, verifique lo siguiente:

-   Valores correctos de la forma de pulsos de pasos

-   Seleccion correcta de los pines de salida del puerto, incluyendo si es necesario la opcion de *Invertido*

-   Cableado blindado para reducir interferencia

-   Problemas fisicos con el motor, acoplamientos, tornillos embalados o de bolas, etc.

Una ves que se encuentre una velocidad a la cual el eje no se detenga o pierda pasos durante la prueba, redusca la velocidad un 10% y utilize esta nueva velocidad como velocidad Maxima.

#### Encontrando la maxima aceleracion

Con la velocidad maxima que se encontro en el paso anterior, introduca un valor de aceleracion a probar. Utilizando el mismo procedimiento antes descrito, redusca la aceleracion si en necesario. En esta prueba, es importante que la combinacion de Aceleracion y area de prueba permita a la maquina alcanzar la velocidad seleccionada. Una ves que se encuentre un valor de aceleracion en el cual la maquina no pierda pasos o se detenga durante la prueba, redusca el valor encontrado un 10% y utilice este nuevo valor como el valor de Aceleracion Maxima.

Configuracion del Husillo<span id="sec:Spindle-Configuration"></span>
---------------------------------------------------------------------

![](images/stepconf-spindle.png)

Figure 7. Pagina configuracion del Husillo<span id="cap:Spindle-Configuration-Page"></span>

Esta pagina solo aparece cuando la opcion *Spindle PWM* es seleccionada en la pagina de seleccion de las salidad 'Parallel Port Pinout

### Control de la velocidad del Husillo<span id="spindle-speed-control"></span>

Si la opcion *Spindle PWM* fue seleccionada en el mapeo de salidas, la siguiente informacion deve ser proporcionada:

 Taza de PWM (PWM Rate)   
La frecuencia portadora de la señal PWM que controlara la velocidad del Husillo. Introdusca un *0* para modo PDM, El cual es util para generar un voltage de control. Revisese la documentacion del controlador del husillo para el valor adecuado.

 Velocidad 1 y 2, PWM 1 y 2   
Los archivos de configuracion generados usan una relacion lineal simple para determinar el valor de PWM para un valor de RPM dado. Si los valores son desconocidos, puedn ser determinados. Para mas informacion vease la seccion: ([Determining Spindle Calibration](#determining-spindle-calibration))

===Sincronizacion de movimientos del Husillo (Cuerdas automaticas y uso de machuelo)<span id="sub:Spindle-synchronized-motion-lathe"></span>

Cuando son proporcionadas las señales correctas desde un encoder conectado a Machinekit utilizando los componentes del HAL (Capa de abstraccionde Hardware), Machinekit soportara el roscado en torno. Las señales requeridas son:

 Indice del Husillo   
Es un pulso que ocurre una vez por revolucion del Husillo.

 Fase A del Husillo   
Es un pulso que ocurre en varias ocaciones igualmente espaciadas conforme el husillo gira.

 Fase B del Husillo (Opcional)   
Este es un pulso secundario que ocurre en desface con respecto al pulso de Fase A. La ventaja de usar ambos pulsos A y B son la capacidad de determinar la direccion del giro, ahumento de la inmunidad al ruido y el aumento de la resolucion.

Si las opciones de *Fase A* e *Indice del Husillo* fueron seleccionadas en la configuracion del puerto, la siguiente informacion devera ser introducida:

 Ciclos por revolucion   
El numero de ciclos de la señal fase A que se producen durante una vuelta completa del husillo. Esta opcion solo se encontrara disponible Si alguna de las entradas de configuracion del puerto fue seleccionada como *Spindle Phase A*

 Velocidad Maxima al momento de hacer una cuerda   
Seleccionar la maxima velocidad permitida al momento de hacer una cuerda. Para un husillo de altas RPM o un encoder con alta resolucion, un valor bajo de *BASE\_PERIOD* es requerido.

### Determinacion de la Calibracion del Husillo<span id="determining-spindle-calibration"></span>

Introdusca los siguientes valores en la pagina de configuracion del husillo:

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Speed 1:</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>PWM 1:</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>Speed 2:</p></td>
<td align="left"><p>1000</p></td>
<td align="left"><p>PWM 2:</p></td>
<td align="left"><p>1</p></td>
</tr>
</tbody>
</table>

Termine los pasos restantes del proceso de configuracion, posteriormente inicie Machinekit con la configuracion recien creada. Encienda la maquina y seleccione la pestaña MDI. Encienda el husillo entrando el comando: *M3 S100*. Cambie la velocidad del husillo entrando un valor diferente de parametro S: *S800*. Valores validos en este momento van desde 1 hasta 1000.

Para dos valores de parametro S diferentes, mida las RPM que el husillo este proporcionando. Guarde los valores S utilizados en la prueba y los valores reales de RPM proporcionados por el husillo. Ejecute el programa Stepconf de nuevo. Para el parametro *Speed* introdusca la velocidad en RPM medida, y para *PWM* introdusca el valor del parametro S dividido entre 1000. Recuerde que deve tener dos valores del parametro S y sus correspondientes RPM proporcionadas por el husillo para generar un ajuste de velocidad lineal.

Devido a que la mayoria de los controladores de husillo precentan nolinealidades en su respuesta es mejor hacer lo siguiente:

-   Asegurese que las dos calibraciones utilizadas no se encuentren cerca en los valores de RPM proporcionados.

-   Asegurese que las dos calibraciones se encuentren en los rangos de RPM que usted normalmente utilizara al maquinar.

Por lo tanto si el husillo deve de ir de las 0 RPM a las 8000 RPM, pero usted generalmente utiliza velocidades de las 400 RPM (10%) a las 4000 RPM (100%), vusque los valores de PWM que proporcionen 1600 RPM (40%) y 2800 RPM (70%).

Opciones de configuracion avanzada<span id="sec:Advanced-Configuration-Options"></span>
---------------------------------------------------------------------------------------

![](images/stepconf-advanced.png)

Figure 8. Configuracion avanzada<span id="cap:Advanced-Configuration"></span>

 Incluir Halui   
Esta opcion incluira la interface de usuario Halui. (Control remoto de los parametros de pantalla de la GUI) Vea el manual del integrador para mas detalles.

 Incluir pyVCP   
Esta opcion agrega el pnel base de pyVCP y un archivo simple para comenzar a trabajar en el. Vea el manual del integrador para mas detalles.

 Incluir ClassicLadder PLC   
Esta opcion agregara el ClassicLadder PLC (Programmable Logic Controller). Vea el manual del integrador para mas detalles.

Terminando de configurar la Maquina<span id="sub:Machine-Configuration-Complete"></span>
----------------------------------------------------------------------------------------

Seleccione *Apply* para escrivir los archivos de configuracion. Mas tarde puede correrse el programa de configuracion Stepconf de nuevo y recuperar los valores que se introdugeron anteriormente.

Carrera de Eje, Localizacion de los interruptores de inicio y la pocicion inicial<span id="sec:Axis-Travel-Home"></span>
------------------------------------------------------------------------------------------------------------------------

Para cada eje, existe un rango limitado de carrera. El limite fisico de la carrera se conoce como *hard stop*.

ANtes de alcanzar el limite fisico *hard stop* existe un interruptor de limite *limit switch*. Si se encuentra el interruptor de limite durante la operacion normal, EMC apagara el controlador del eje. La distancia entre el limite fisico y el interruptor de limite Deve ser suficiente para permitir al eje sin energia detenerse.

Antes del interruptor de limite existe un limite suabe *soft limit*. Este es un limite determinado por programa despues de la rutina de inicializacion. Si un comando MDI o G exede el limite suabe, el comando no se ejecutara. Si un movimiento manual del eje exede el limite suabe, el movimiento es terminado en el limite suabe.

El interruptor de inicializacion *home switch* puede ser colocado en cualquier lugar de la carrera del eje entre los dos limites fisicos del eje. Mientras algun dispositivo externo no desactive los controladores de motor cuando el interruptor de limite es activado, uno de los interruptores de limite puede ser utilizado para la posicion de inicializacion.

La posicion cero *zero position* es la posicion en el eje que tienen el valor de 0 en el sistema coordenado de la maquina. Usualmente la posicion cero se encontrara dentro de los limites suabes. En los tornos, la opcion de velocidad de superficie constante requiere que la posicion *X=0* corresponda al centro de rotacion del husillo cuando no exista alguna compensacion en la herramienta.

La posicion de inizializacion es la posicion a la que el eje sera desplazado al final de la secuencia de inizializacion. Este valor deve de encontrarse dentro de los limites suabes. En particular la posicion de inizializacion nunca deve ser igual a un limite suabe.

### Operacion sin interruptores de limite<span id="sub:Operating-without-Limit"></span>

Una maquina puede ser operada sin interruptores de limite. En ese caso, solo los limites suabes detendran al eje de alcanzar el limite fisico. Los limites suabes solo operan despues de que se a ejecutado la rutina de inizializacion.

### Operacion sin limites de inizializacion<span id="sub:Operating-without-Home"></span>

Una maquina puede ser operada sin interruptores de inicializacion. Si la maquina tiene interruptores de limite pero no de inicializacion, es mejor utilizar un interruptor de limite como interruptor de inizializacion (ejemplo, seleccione la opcion *Minimum Limit + Home X* cuando configure el puerto en Stepconf). Si la maquina no tiene interruptores de ningun tipo, o si los interruptores no pueden ser utilizados como interruptores de inizializacion por cualquier otra razon, entonces la maquina devera ser inicializada a mano, o utilizando marcas en la bancada. la inicializacion a mano no es tan confiable como la inicializacion con interruptores, pero permite seguir utilizando los limites suabes.

### Opciones de cableado de los interruptores de inicializacion y limite<span id="sub:Home-and-Limit"></span>

El cableado ideal deve de incluir un interruptor por señal. Sin embargo, el puerto paralelo del computador solo ofrece un total de 5 entradas, mientras que se necesitan 9 en una maquina de 3 ejes. Por lo tanto, Varios interruptores pueden ser cableados en conjunto en diversas formas para permitir utilizar menos entradas.

La siguiente figura muestra la idea general de cablear varios interruptores a una unica entrada. En el caso ilustrado, cuando un interruptor es precionado, El valor mandado atraves de la entrada va de un valor logico ALTO a BAJO. Sin embargo Machinekit espera un valor logico ALTO cuando un interruptor es precionado, Por lo tanto se devera seleccionar la opcion de invercion *Invert* cuando se configure la entrada del puerto paralelo en el Stepconf.

La resistencia de polarizacion mostrada en el diagrama fija la señal de entrada a ALTO Amenos que una conexion a tierra sea realizada, en tal caso la señal ira a BAJO. Sin la resistencia la entrada quedaria flotando y la entrada podria variar entre ALTO y BAJO cuando el circuito este abierto. Un balor tipoco de resistencia de polarizacion es de 47K.

Interruptores normalmente cerrados<span id="cap:Normally-Closed-Switches"></span>

Cableado de Interruptores Normalmente Cerrados en serie (diagrama simplificado)

![](images/switch-nc-series.png)

Interruptores normalmente abiertos<span id="cap:Normally-Open-Switches"></span>

Cableado de interruptores normalmente abiertos en paralelo (diagrama simplificado)

![](images/switch-no-parallel.png)

Las siguientes configuraciones de interruptores estan permitidas en Stepconf:

-   Interruptores de inizializacion combinados para todos los ejes

-   Interruptores de limite combinados para todos los ejes

-   Combinar ambos interruptores de limite para un eje

-   Combinar ambos interruptores de limite y el interruptor de inizializacion para un eje

-   Combinar un interruptor de limite y el interruptor de inicializacion de un eje

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


