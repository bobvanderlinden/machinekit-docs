Primeros pasos V, 2015-12-20
============================

The Machinekit Team

![common/images/emc2-intro.\*]()

This handbook is a work in progress. If you are able to help with writing, editing, or graphic preparation please contact any member of the writing team or join and send an email to <emc-users@lists.sourceforge.net>.

Copyright © 2000-2012 Machinekit.org

Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.1 or any later version published by the Free Software Foundation; with no Invariant Sections, no Front-Cover Texts, and one Back-Cover Text: This Machinekit Handbook is the product of several authors writing for linuxCNC.org. As you find it to be of value in your work, we invite you to contribute to its revision and growth. A copy of the license is included in the section entitled GNU Free Documentation License. If you do not find the license you may order a copy from Free Software Foundation, Inc. 59 Temple Place, Suite 330 Boston, MA 02111-1307

LINUX® is the registered trademark of Linus Torvalds in the U.S. and other countries. The registered trademark Linux® is used pursuant to a sublicense from LMI, the exclusive licensee of Linus Torvalds, owner of the mark on a world-wide basis.

Requerimientos del sistema
--------------------------

<span id="cha:system-requirements"></span>

### Requerimientos Mínimos

Los requerimientos mínimos de hardware para ejecutar EMC2 y Debian pueden variar dependiendo del uso que se le dará al sistema. Sistemas basados en motores a pasos requieren procesadores mas rápidos para generar trenes de pulsos en comparación con los servomecanismos retro alimentados. Usando el Live-CD usted puedes probar el software antes de modificar la computadora. Mantenga en Mente que los resultados del estudio de latencia son mas importantes que la velocidad del procesador para la generación de pasos por software. Mas información sobre el estudio de latencia se encuentra en la sección ([Latency Test](#sub:latency-test)).

Información adicional se puede encontrar en el sitio de EMC wiki:

[Wiki.Machinekit.org, Hardware\_Requirements](http://wiki.machinekit.org/cgi-bin/emcinfo.pl?Hardware_Requirements)

EMC2 y Debian deberían de ejecutarse razonablemente bien en una computadora con las siguientes especificaciones mínimas de hardware. Estas especificaciones no son las mínimas absolutas pero proporcionan un desempeño razonable para la mayoría de los sistemas basados en motores a pasos.

\*700 Mhz x86 procesador (se recomienda un procesador de 1.2GHz x86) \*384 MB de RAM (se recomienda entre 512MB hasta 1GB) \*8 GB de espacio en disco duro \*Tarjeta gráfica capaz de por lo menos 1024x768 de resolución, que no este ejecutando los controladores propietarios Nvidia o ATI fglrx, y de preferencia que no se trate de una tarjeta de vídeo integrada que comparta memoria con el CPU. \*Una conexión de red o Internet (No es estrictamente necesaria, pero resulta muy útil para realizar actualizaciones y contactar a la comunidad de usuarios del EMC)

Los requerimientos mínimos del sistema cambian conforme Debian evoluciona, por lo tanto revise el sitio web help.ubuntu.com\[Debian\] para mas detalles sobre el LiveCD que esta usando. Hardware antiguo podría beneficiarse si se selecciona una version mas antigua del LiveCD cuando se encuentre disponible.

### Hardware Problemático

#### Computadores Portátiles

Los computadores portátiles en general no son buenos para la generación por software de pasos en tiempo real. De nuevo el Estudio de Latencia ejecutado por un periodo de tiempo prolongado proveerá la información necesaria para determinar si resulta apropiado su uso.

#### Tarjetas de Vídeo

Si su instalación comienza con una resolución de 800 x 600 en la mayoría de los casos eso significa que Debian no reconoció apropiadamente su monitor o tarjeta de vídeo. Las tarjetas de vídeo integradas, la mayoría de los casos, producen malos resultados en el desempeño en tiempo real.

Sobre Machinekit
----------------

### El Software

\*Machinekit (El “Enhanced Machine Control”) es un software para computador que permite el control de maquinas herramienta tales como fresadoras, tornos, robots tipo puma o scara y cualquier otro tipo de maquina de hasta 9 ejes. \*Machinekit es software libre con un código fuente abierto. Las versiones actuales de EMC están enteramente licenciadas bajo las licencia GPL y LGPL (General Public License y Lesser GNU General Public License) \*Machinekit proporciona: **una interfase gráfica (se puede elegir entre varias interfaces diferentes)** un interprete para código G (el lenguaje de programación de maquina RS-274) **un planeador de movimientos en tiempo real con análisis de instrucción siguiente** operación de electrónica de maquina de bajo nivel como sensores y controladores para motores **una capa de aislamiento sencilla de usar que permite crear rápidamente configuraciones únicas para cada maquina** un PLC basado en software programable con lógica de escalera \*\*una instalación sencilla con un Live-CD \* No provee capacidades de dibujo (CAD - Dibujo asistido por computadora) o generación de código G a partir de dibujos (CAM – Manufactura asistida por computadora). \* Puede mover 9 ejes simultáneos y soportar una variedad de interfaces. \* El control puede operar servomecanismos verdaderos (analógicos o por PWM) con retroalimentación del el lazo cerrado por el software Machinekit en la computadora, o puede operar en lazo abierto con motores a pasos o “paso-servos” \*Algunas características del controlador de movimientos: compensación de radio y largo, desviación de la trayectoria limitada a una tolerancia especificada, roscado en torno, movimientos de ejes sincronizados, velocidad de alimentación adaptiva, velocidad de alimentación controlada por el operador, control de velocidad constante. \*Soporte para sistemas no cartesianos a través de un modulo de cinemática. Algunas de las arquitecturas disponibles son hexapodos (plataformas Stewart y conceptos similares) y sistemas con juntas rotatorias para proporcionar movimiento como en los robots PUMA o SCARA. \*Machinekit se ejecuta en Linux usando exenciones de tiempo real.

### El Sistema Operativo

Debian fue seleccionado porque encaja perfectamente en la visión de fuente abierta del Machinekit:

-   Debian sera siempre libre de cargo, y no se tiene que pagar extra por la “versión empresarial”, nosotros hacemos disponible nuestro mejor trabajo para cualquiera en los mismos términos de gratuidad.

-   Machinekit esta acoplado con las versiones con soporte extendido (LTS) de Debian, lo que provee soporte y arreglos de seguridad por parte del equipo de Debian por 3 – 5 años.

-   Debian utiliza lo mejor en traducciones y fácil acceso que la comunidad de software libre tiene para ofrecer, para hacer Debian practico de usar para la mayor cantidad de gente posible.

-   La comunidad de Debian esta completamente alineada a los principios de desarrollo del software libre; Nosotros fomentamos el uso por parte de la gente de software de fuente abierta, su mejoramiento y su distribución.

### Obtener Ayuda

#### IRC

IRC (Internet Relay Chat) es un protocolo de comunicaciones en tiempo real basado en texto. Permite una conexión en vivo con otros usuarios del Machinekit. El canal del Machinekit en IRC es \#machinekit en freenode.

La manera mas simple de utilizar IRC es utilizar el cliente integrado en la siguiente pagina de internet, [pagina](http://www.machinekit.org/index.php/english/community).

 Reglas de comportamiento en IRC   
-   Pregunte cuestiones especificas… Evite realizar preguntas como: “¿Puede alguien ayudarme?”.

-   Si usted es realmente nuevo en la materia, piense un poco en lo que va a preguntar antes de comenzar a escribir. Asegúrese de proporcionar suficiente información así alguien podría ayudarle a resolver su pregunta.

-   Sea paciente mientras espera por una respuesta, en algunas ocasiones toma tiempo formular la respuesta o todos los otros usuarios pueden estar ocupados trabajando en algo mas.

-   Inicie sesión en IRC con un único nombre así la gente sabrá quien es usted. Si utiliza el cliente Java, utilice el mismo apodo cada vez que entre. Esto ayuda a la gente a recordar quien es usted y esto reduce el gasto de tiempo de las dos partes.

-   La mayoría de los usuarios del canal machinekit en IRC son anglo parlantes, tendrá mas probabilidad de éxito si formula sus preguntas en el idioma ingles.

 Compartiendo Archivos   
La forma mas común de compartir archivos en IRC es subir el archivo a uno de los siguientes servidores y pegar el enlace (puede utilizar servidores similares).

-   *Para texto* - <http://pastebin.com/> , <http://pastie.org/>, <https://gist.github.com/>

-   *Para Imagenes* - <http://imagebin.org/> , <http://imgur.com/> , <http://bayimg.com/>

-   *Para archivos* - <https://filedropper.com/> , <http://filefactory.com/> , <http://1fichier.com/>

#### Lista de Correo

Una Lista de Correo es una forma para preguntar a todos los miembros de la lista y obtener una respuesta a su conveniencia. Usted obtiene una mejor exposición de su pregunta utilizando la Lista de Correo que utilizando IRC, pero las respuestas pueden tardar mas tiempo. En pocas palabras usted manda un correo electrónico con su pregunta a la lista de correo y recibe respuestas individuales o un compilado diario de respuestas individuales, dependiendo de como configure su cuenta.

Información sobre la lista de correo de Machinekit se encuentra en: <https://lists.sourceforge.net/lists/listinfo/emc-users> \[emc-users lista de correo\]

#### Machinekit Wiki

Un sitio Wiki es un sitio de internet mantenido por los usuarios en donde todos pueden modificar o agregar información.

El sitio wiki mantenido por los usuarios del Machinekit contiene mucha información y consejos y se encuentra en:

[wiki.machinekit.org](http://wiki.machinekit.org/cgi-bin/emcinfo.pl)

### Obteniendo Machinekit

#### Descarga Normal

Descargue el Live-CD de:

[La pagina principal del Machinekit www.machinekit.org](http://www.machinekit.org/)

y siga el vinculo para descarga.

#### Descarga Multi-sesion

Si el archivo es demasiado grande para ser descargado en una sesión debido a una conexión a internet lenta o defectuosa, utilice `wget` (o bittorrent) para permitir restaurar la descarga despues de una interrupción.

 Linux Wget   
Abra una ventana de terminal. En Debian valla a Applications/Accessories/Terminal. Utilice *cd* para cambiar al directorio donde desea guardar el ISO. Si lo necesita utilice *mkdir* para crear un nuevo directorio.

Note que los nombres de los archivos pueden cambiar, usted debería de ir a <http://www.machinekit.org/> y seguir el vinculo de descarga para obtener el nombre actual del archivo. En la mayoría de los buscadores usted puede hacer clic derecho en el vinculo y seleccionar la opción de copiar la locación del vinculo, posteriormente pegue esa locación en la ventana de la terminal utilizando un clic del botón derecho y seleccionando la opción pegar.

Debian 10.04 Lucid Lynx y Machinekit (versión actual)

Para obtener la versión Debian 10.04 Lucid Lynx, copie una de las siguientes direcciones en la ventana de terminal y presione la tecla enter:

Para el espejo en USA: wget <http://www.machinekit.org/iso/ubuntu-10.04-machinekit3-i386.iso>

Para el espejo en Europa: wget <http://dsplabs.upt.ro/~juve/emc/get.php?file=ubuntu-10.04-machinekit3-i386.iso>

La md5sum del archivo anterior es: *5283b33b7e23e79da1ee561ad476b05f*

+ Para continuar una descarga parcial que fue interrumpida agregue la opción -c al comando wget:

+ wget -c <http://www.machinekit.org/iso/ubuntu-10.04-machinekit1-i386.iso>

+ Para detener una descarga en progreso utilice Ctrl-C o cierre la pantalla de la terminal.

+ .Debian 8.04 Hardy Heron y Machinekit (antiguo)

Si usted requiere una versión antigua de Debian, usted puede descargar Debian 8.04. La imagen CD siguiente tiene el antiguo emc 2.3.x en ella, pero puede ser actualizada a la versión 2.4.x siguiendo las instrucciones en el wiki de Machinekit.org que se encuentran aquí: <http://wiki.machinekit.org/cgi-bin/emcinfo.pl?UpdatingTo2.4>

Para el espejo en USA: wget <http://www.machinekit.org/iso/ubuntu-8.04-desktop-emc2-aj13-i386.iso>

Para el espejo en Europa: wget <http://dsplabs.upt.ro/~juve/emc/get.php?file=ubuntu-8.04-desktop-emc2-aj13-i386.iso>

La md5sum del archivo anterior es: *1bab052ec879f941628927c988863f14*

+ Cuando la descarga sea completada usted encontrara el archivo ISO en el directorio que selecciono. A continuación quemaremos el CD.

 Wget Windows   
El programa wget se encuentra también disponible para Windows descargado de:

<http://gnuwin32.sourceforge.net/packages/wget.htm>

Siga las instrucciones de la pagina de internet para descargar e instalar la versión de Windows del programa wget.

Para correr wget abra una ventana de linea de comandos.

En la mayoría de las instalaciones de Windows esto se hace en Programs/Accessories/Command Prompt

Primero usted tiene que cambiarse al directorio donde wget esta instalado.

Típicamente es en C:\\Program Files\\GnuWin32\\bin por lo tanto en la ventana de la linea de comandos escriba:

--- *cd C:\\Program Files\\GnuWin32\\bin* ---

y el prompt debería de cambiar a: *C:\\Program Files\\GnuWin32\\bin&gt;*

Escriba el comando wget en la ventana de la linea de comandos como se describió en las secciones anteriores dependiendo de la versión de Machinekit que requiera y presione enter.

#### Quemando el CD de Machinekit

Machinekit es distribuido como una imagen de CD con un formato llamado ISO. Para instalar Machinekit, usted necesitara primero quemar el archivo ISO en un CD. Usted necesita un quemador CD/DVD que funcione y un CD en blanco de 80 minutos (700Mb) para hacer esto. Si la escritura del CD falla trate de nuevo con una velocidad de escritura mas baja.

 Verificando la integridad del CD con md5sum en Linux   
Antes de quemar el CD, es altamente recomendable que verifique el md5sum (hash) del archivo .iso.

Abra una ventana de terminal. En Debian valla a Applications/Accessories/Terminal.

Cambie el directorio a donde el archivo ISO fue descargado.

--- cd download\_directory ---

Ejecute el comando de verificación de md5sum con el nombre del archivo guardado.

--- md5sum -b ubuntu-10.04-machinekit1-i386.iso ---

El comando md5sum deberá de imprimir una linea sencilla después de calcular el hash.

En computadoras lentas esto puede tardar un minuto o dos.

+ --- 5283b33b7e23e79da1ee561ad476b05f \*ubuntu-10.04-machinekit1-i386.iso ---

+ Ahora compare este valor con el que realmente debería de ser.

+ Si descarga el md5sum asi como el iso, usted puede preguntar al programa md5sum el hacer la revisión por usted. En el mismo directorio:

+ --- md5sum -c ubuntu-10.04-machinekit1-i386.iso.md5 ---

+ Si todo va bien despues de una pausa la terminal deveria de imprimir:

+ --- ubuntu-10.04-machinekit1-i386.iso: OK ---

+

 Quemando el archivo ISO en Linux   
1.  Inserte un CD en blanco en su quemador. Una ventana de *CD/DVD creador* o *Seleccione tipo de disco* aparecera seleccione no hacer nada y cierre la ventana.

2.  Busque la imagen Iso en el buscador de archivos.

3.  Haga click derecro sobre la imagen ISO y seleccione la opcion escrivir a disco.

4.  Seleccione la velocidad de escritura. Si se esta quemando un disco Live CD de Debian seleccione la velocidad mas baja posible.

5.  Inicie el proceso de quemado.

6.  Si una ventana con el titulo *seleccione el nombre para la imagen de disco* aparece, solo seleccione la opcion OK.

 Verificar md5sum con Windows   
Antes de quemar el CD, es altamente recomendable que verifique el md5sum (hash) del archivo .iso que se descargo.

Windows no incluye un programa de verificacion de mdsum. Se tendra que descargar e instalar uno para provar la md5sum. mas informacion puede ser encontrada en:

<https://help.ubuntu.com/community/HowToMD5SUM>

 Quemando el archivo ISO en Windows   
1.  Descargue e instale Infra Recorder, el cual es un programa para quemar imagenes de disco gratuito y libre: <http://infrarecorder.org/>

2.  Inserte un CD en blanco en la unidad de disco y seleccione la opcion de hacer nada o cancelar si alguna pantalla emergente aparece.

3.  Abra Infra Recorder, seleccione la opcion *Acciones* del menu, posteriormente seleccione *Quemar Imagen*.

#### Probando Machinekit

Con el Live CD en la unidad CD/DVD apague la computadora y enciéndala de nuevo. Esto hará que la computadora arranque desde el Live CD. Una vez que la computadora haya arrancado usted puede probar Machinekit sin instalarlo. Usted no puede crear configuraciones personalizadas o modificar la mayoría de los parámetros del sistema tales como la resolución de pantalla amenos que instale Machinekit.

Para probar Machinekit desde el menú de Applications/CNC seleccione Machinekit. Entonces seleccione una configuración sim (simulador) para hacer pruebas.

Para revisar si su computadora es candidata apta para la generación de pasos por software corra una prueba de latencia como se describe en la sección ([Latency Test](#sub:latency-test))

#### Instalar Machinekit

Si le gustan los resultados que obtuvo probando Machinekit, solo haga clic en el icono de instalación del escritorio, conteste unas cuantas preguntas (su nombre, zona horaria, contraseña) y la instalación se completara en unos pocos minutos. Asegúrese de conservar el nombre y la contraseña que introdujo. Una vez que el proceso de instalación concluya y usted se encuentre en linea el administrador de actualizaciones le permitirá actualizar a la ultima versión estable de Machinekit.

#### Actualizaciones a Machinekit

Con la instalación normal el agente de actualizaciones le notificara de las actualizaciones disponibles para Machinekit cuando se conecte a internet, y usted podrá actualizar sin necesidad de conocer mas sobre LINUX.

Si usted desea actualizar a 10.04 de 8.04 se recomienda una instalación limpia de EMC. Es correcto actualizar todo cuando se le pregunte por hacerlo exepto el sistema operativo.

Advertencia: No actualice Debian a una versión que no sea LTS (Por ejemplo de 8.04 a 8.10) lo anterior arruinara su instalación de EMC y no podrá utilizarlo.

#### Problemas con la instalación

En casos raros deberá de resetear el BIOS a su configuración de fabrica si durante el proceso de instalación desde el Live CD el disco duro no es detectado correctamente.

Actualizando Machinekit
-----------------------

### Actualizando de 2.4.x a 2.5.x

As of version 2.5.0, the name of the project has changed from EMC2 to Machinekit. All programs with "emc" in the name have been changed to "machinekit" instead. All documentation has been updated.

Additionally, the name of the debian package containing the software has changed. Unfortunately this breaks automatic upgrades. To upgrade from emc2 2.4.X to machinekit 2.5.X, do the following:

#### On Debian Lucid 10.04

First you need to tell your computer where to find the new Machinekit software:

-   Click on the System menu in the top panel and select Administration→Software Sources.

-   Select the Other Software tab.

-   Select the entry that says

        http://machinekit.org/lucid lucid base emc2.4

        or

        http://machinekit.org/lucid lucid base emc2.4-sim

        and click the Edit button.

-   In the Components field, change `emc2.4` to `machinekit2.5`, or change `emc2.4-sim` to `machinekit2.5-sim`.

-   Click the OK button.

-   Back in the Software Sources window, Other Software tab, click the Close button.

-   It will pop up a window informing you that the information about available software is out-of-date. Click the Reload button.

Now your computer knows about the new software, next we need to tell it to install it:

-   Click on the System menu in the top panel and select Administration→Synaptic Package Manager

-   In the Quick Search bar at the top, type `machinekit`.

-   Click the check box to mark the new machinekit package for installation.

-   Click the Apply button, and let your computer install the new package. The old emc 2.4 package will be automatically removed to make room for the new Machinekit 2.5 package.

#### On Debian Hardy 8.04

First you need to tell your computer where to find the new Machinekit software:

-   Click on the System menu in the top panel and select Administration→Synaptic Package Manager

-   Go to Settings→Repositories.

-   Select the "Third-Party Software" tab.

-   Select the entry that says

        http://machinekit.org/hardy hard emc2.4

        or

        http://machinekit.org/hardy hardy emc2.4-sim

        and click the Edit button.

-   In the Components field, change `emc2.4` to `machinekit2.5` or `emc2.4-sim` to `machinekit2.5-sim`.

-   Click the OK button.

-   Back in the Software Sources window, click the Close button.

-   Back in the Synaptic Package Manager window, click the Reload button.

Now your computer knows about the new software, next we need to tell it to install it:

-   In the Synaptic Package Manager, click the Search button.

-   In the Find dialog window that pops up, type `machinekit` and click the Search button.

-   Click the check-box to mark the machinekit package for installation.

-   Click the Apply button, and let your computer install the new package. The old emc 2.4 package will be automatically removed to make room for the new Machinekit 2.5 package.

### Config changes

The user configs moved from $HOME/emc2 to $HOME/machinekit, so you will need to rename your directory, or move your files to the new place.

The hostmot2 watchdog in Machinekit 2.5 does not start running until the HAL threads start running. This means it now tolerates a timeout on the order of the servo thread period, instead of requiring a timeout that’s on the order of the time between loading the driver and starting the HAL threads. This typically means a few milliseconds (a few times the servo thread period) instead of many hundreds of milliseconds. The default has been lowered from 1 second to 5 milliseconds. You generally don’t need to set the hm2 watchdog timeout any more, unless you’ve changed your servo thread period.

The old driver for the Mesa 5i20, hal\_m5i20, has been removed after being deprecated in favor of hostmot2 since early 2009 (version 2.3.) If you are still using this driver, you will need to build a new configuration using the hostmot2 driver. Pncconf may help you do this, and we have some sample configs (hm2-servo and hm2-stepper) that act as examples.

### Actualizando de 2.3.x a 2.4.x

Las siguientes instrucciones solo aplican a Debian 8.04 "Hardy Heron". Machinekit 2.4 no se encuentra disponible en versiones mas antiguas de Debian.

Debido a la existencia de incompatibilidades entre 2.3.5 y 2.4.x, su instalación actual no se actualizara automáticamente por el agente de actualización a 2.4.x. Si usted desea correr la versión 2.4.x, Cambie al repositorio Machinekit-2.4 siguiendo estas instrucciones:

Ejecute System/Administration/Synaptic Package Manager

Vaya a Settings/Repositories

En la lista de “Third-Party software” debe haber al menos dos lineas para machinekit.org

Para cada una de ellas:

-   Seleccione la linea y haga clic en Editar

-   En la linea de componentes, cambie emc2.3 a emc2.4

-   Haga clic en OK

-   Cierre la ventana "Software Preferences"

-   Haga clic en "Reload"

-   Haga clic en "Mark All Upgrades"

Usuarios de tarjetas Mesa y hostmot2:

Si usa una tarjeta Mesa, busque el hostmot2-firmware apropiado para su tarjeta y márquelo para instalación. Consejo: haga una búsqueda por "hostmot2-firmware" en el manejador de paquetes Synaptic.

-   Haga clic en *Apply*

### Cambios entre 2.3.x y 2.4.x

Una vez completada la actualización, actualice cualquier archivo de configuración de maquinas que tenga previamente configuradas siguiendo estas instrucciones:

#### emc.nml cambios (2.3.x a 2.4.x)

Para configuraciones que no posen un archivo emc.nml personalizado, remueva o comente la linea NML\_FILE = emc.nml en el archivo .ini. Esto causara que se use la versión mas actual de emc.nml.

Para configuraciones que tienen un archivo emc.nml personalizado, un cambio similar es requerido.

El no realizar el cambio puede ocasionar la aparición de un error tal como:

    libnml/buffer/physmem.cc 143: PHYSMEM_HANDLE:
    Can't write 10748 bytes at offset 60 from buffer of size 10208.

#### Cambios en la tabla de Herramientas (2.3.x a 2.4.x)

El formato de la tabla de herramientas a cambiado y es incompatible. La documentación muestra el nuevo formato. La antigua tabla de herramientas sera automáticamente actualizada al nuevo formato.

#### Imágenes del firmware hostmot2 (2.3.x a 2.4.x)

Las Imágenes del firmware hostmot2 son ahora paquetes separados. Usted puede:

-   Continuar usando la versión ya instalada *emc2-firmware-mesa-\** paquete 2.3.x

-   Instalar los paquetes nuevos desde el manejador de paquetes Synaptic. Los nuevos paquetes se llaman *hostmot2-firmware-\**

-   Descargar las imágenes del firmware como archivos tar desde <http://emergent.unpy.net/01267622561> e instalarlos manualmente.

Configuración de motores a pasos
--------------------------------

<span id="cha:stepper-quickstart"></span>

Esta sección asume que una instalación estándar a partir de un LiveCd ha sido realizada. Posterior a la instalación es recomendable conectar la computadora al internet y esperar por la aparición del manejador de actualizaciones y obtener las ultimas actualizaciones para el EMC y Debian antes de continuar. Para instalaciones mas complejas vea el manual del integrador.

### Prueba de Latencia

La prueba de latencia determina cuanto tiempo le toma al procesador de su computadora responder a una solicitud de procesamiento. Algunos Hardware pueden interrumpir el procesamiento lo que puede traducirse a la perdida de algunos pasos cuando se opera una maquina CNC. Esto es la primer cosa que se requiere hacer posterior a la instalación. Siga las instrucciones de la sección [here](#cha:latency-test) Para correr La prueba de latencia.

### Sherline

Si usted posee una maquina marca Sherline varias configuraciones predefinidas están disponibles. Estas configuraciones se encuentran en el menú principal CNC/EMC donde puede seleccionar la configuración que sea compatible con el tipo de maquina que posea y hacer clic, para copiarla y salvarla.

### Xylotex

Si usted tiene una maquina marca Xylotex, usted puede escapar las siguientes secciones para pasar directamente a la sección de Asistente de Configuración de Motores a Pasos ubicada en [Wizard](#cha:stepconf-wizard). EMC provee una configuración rápida para maquinas Xylotex.

### Información sobre la Maquina

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

### Información de los pines de salida

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

### Información Mecánica

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

![images/step-calc-inch-math.png]()

Example 2. Unidades mm

    Motor a Pasos             = 200 Pasos por revolución
    Controlador de Motor      =  8 micro pasos por paso
    Dientes del motor         =  30
    Dientes del tornillo guía =  90
    Paso del tornillo guía    =   5.00 mm por revolución

A partir de la información anterior, el tornillo guía se mueve 5.00 mm por vuelta. - El motor da 3 vueltas por una vuelta del tornillo guía. - El controlador necesita 8 micro pasos de entrada para hacer al motor dar un paso. - El controlador necesita 1600 pasos para hacer que el motor de una revolución. Por lo tanto la escala necesitada es:

![images/step-calc-mm-math.png]()

Asistente de configuracion Stepconf
-----------------------------------

<span id="cha:stepconf-wizard"></span>

Machinekit es capaz de controlar un vasto rango de diferentes tipos de maquinaria, utilizando diferentes interfaces de Hardware.

Stepconf es un programa que genera archivos de configuracion para Machinekit para un tipo especifico de maquina CNC: Aquellas que son controladas atravez de un *Puerto paralelo estandar*, y que son controladas utilizando las senales *Paso y Direccion*

Stepconf se instala automaticamente cuando instala Machinekit y se encuentra en el menu CNC.

Stepconf genera un archivo en el directorio emc2/config en el cual guarda las selecciones de cada configuracion que usted genere. Cuando se desea cambia algo, se necesita seleccionar el archivo que tenga el mismo numaero que la configuracion que desea modificar. La extencion del archivo es .stepconf.

El asistente Stepconf necesita al menos una resolucion de pantalla de 800 x 600 para que los botones de la parte baja de la pantalla sean visibles.

Instrucciones paso a paso
-------------------------

### Pagina de entrada<span id="sec:Entry-Page"></span>

![images/stepconf-config.png]()

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

### Informacion Basica<span id="sec:Basic-Information"></span>

![images/stepconf-basic.png]()

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

### Prueba de latencia<span id="sub:latency-test"></span>

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

![images/latency.png]()

Figure 3. Prueba de Latencia<span id="cap:Latency-Test"></span>

Latencia es cuanto le tomara a la PC detenerse en lo que esta haciendo y responder a una solicitud externa. En este caso, la solicitud el el *latido periodico* que sirve como referencia de tiempo para la genracion de los pulsos de paso. Entre menor sea la latencia, mas rapido se generaran los latidos, y mas rapidos y suabes seran los pulsos de paso.

La latencia es mucho mas importante que la velocidad del CPU. La velocidad del CPU no es el unico factor determinate en la latencia. Tahgetas madre, targetas de video, puertos USB, Problemas con SMI, y otra cantidad de coasas pueden afectar la latencia.

Troubleshooting SMI Issues (Machinekit.org Wiki)

Encuentre soluciones a algunos problemas de SMI comunes en Debian

<http://wiki.machinekit.org/cgi-bin/emcinfo.pl?FixingSMIIssues>

Los numeros importantes son el "max jitter". en el ejemplo de abajo 9075 nanosegundos, o 9.075 microsegundos, es el maximo retraso. Guarde este numero, y escrivalo en la caja Base Period Maximum Jitter.

Si el maximo retrazo es menor o se encuentra entre 15-20 microsegundos (15000-20000 nanosegundos), la computadora deveria de dar muy buenos resultados con la generacion de pulsos de pasos. Si la latencia maxima esta entre 30-50 microsegundos, se pueden seguir obteniendo buenos resultados, pero la tasa maxima de generacion de pulsos puede ser un poco desepcionante, especialmente si se usan micropasos o un tornillo con un paso muy fino. si los numeros son 100us o mas (100 000 nanosegundos), la PC no es una buena candidata para la generacion de pulsos de paso por software. Numeros arriva de 1 milisegundo (1 000 000 nanosegundos) significan que la PC no es una buena candidata para ejecutar Machinekit, sin importar si se usa generacion de pulsos de paso por software o no.

### Ajustes del puerto Paralelo<span id="sec:Parallel-Port-Setup"></span>

![images/stepconf-pinout.png]()

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

### Configuracion de los Ejes<span id="sec:Axis-Configuration"></span>

![images/stepconf-axis.png]()

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

#### Probar este Eje

![images/stepconf-test.png]()

Figure 6. Probar este Eje<span id="cap:Test-This-Axis"></span>

Con Stepconf es sencillo probar diferentes valores de aceleracion y velocidad.

##### Busqueda de Velocidad Maxima

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

##### Encontrando la maxima aceleracion

Con la velocidad maxima que se encontro en el paso anterior, introduca un valor de aceleracion a probar. Utilizando el mismo procedimiento antes descrito, redusca la aceleracion si en necesario. En esta prueba, es importante que la combinacion de Aceleracion y area de prueba permita a la maquina alcanzar la velocidad seleccionada. Una ves que se encuentre un valor de aceleracion en el cual la maquina no pierda pasos o se detenga durante la prueba, redusca el valor encontrado un 10% y utilice este nuevo valor como el valor de Aceleracion Maxima.

### Configuracion del Husillo<span id="sec:Spindle-Configuration"></span>

![images/stepconf-spindle.png]()

Figure 7. Pagina configuracion del Husillo<span id="cap:Spindle-Configuration-Page"></span>

Esta pagina solo aparece cuando la opcion *Spindle PWM* es seleccionada en la pagina de seleccion de las salidad 'Parallel Port Pinout

#### Control de la velocidad del Husillo<span id="spindle-speed-control"></span>

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

#### Determinacion de la Calibracion del Husillo<span id="determining-spindle-calibration"></span>

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

### Opciones de configuracion avanzada<span id="sec:Advanced-Configuration-Options"></span>

![images/stepconf-advanced.png]()

Figure 8. Configuracion avanzada<span id="cap:Advanced-Configuration"></span>

 Incluir Halui   
Esta opcion incluira la interface de usuario Halui. (Control remoto de los parametros de pantalla de la GUI) Vea el manual del integrador para mas detalles.

 Incluir pyVCP   
Esta opcion agrega el pnel base de pyVCP y un archivo simple para comenzar a trabajar en el. Vea el manual del integrador para mas detalles.

 Incluir ClassicLadder PLC   
Esta opcion agregara el ClassicLadder PLC (Programmable Logic Controller). Vea el manual del integrador para mas detalles.

### Terminando de configurar la Maquina<span id="sub:Machine-Configuration-Complete"></span>

Seleccione *Apply* para escrivir los archivos de configuracion. Mas tarde puede correrse el programa de configuracion Stepconf de nuevo y recuperar los valores que se introdugeron anteriormente.

### Carrera de Eje, Localizacion de los interruptores de inicio y la pocicion inicial<span id="sec:Axis-Travel-Home"></span>

Para cada eje, existe un rango limitado de carrera. El limite fisico de la carrera se conoce como *hard stop*.

ANtes de alcanzar el limite fisico *hard stop* existe un interruptor de limite *limit switch*. Si se encuentra el interruptor de limite durante la operacion normal, EMC apagara el controlador del eje. La distancia entre el limite fisico y el interruptor de limite Deve ser suficiente para permitir al eje sin energia detenerse.

Antes del interruptor de limite existe un limite suabe *soft limit*. Este es un limite determinado por programa despues de la rutina de inicializacion. Si un comando MDI o G exede el limite suabe, el comando no se ejecutara. Si un movimiento manual del eje exede el limite suabe, el movimiento es terminado en el limite suabe.

El interruptor de inicializacion *home switch* puede ser colocado en cualquier lugar de la carrera del eje entre los dos limites fisicos del eje. Mientras algun dispositivo externo no desactive los controladores de motor cuando el interruptor de limite es activado, uno de los interruptores de limite puede ser utilizado para la posicion de inicializacion.

La posicion cero *zero position* es la posicion en el eje que tienen el valor de 0 en el sistema coordenado de la maquina. Usualmente la posicion cero se encontrara dentro de los limites suabes. En los tornos, la opcion de velocidad de superficie constante requiere que la posicion *X=0* corresponda al centro de rotacion del husillo cuando no exista alguna compensacion en la herramienta.

La posicion de inizializacion es la posicion a la que el eje sera desplazado al final de la secuencia de inizializacion. Este valor deve de encontrarse dentro de los limites suabes. En particular la posicion de inizializacion nunca deve ser igual a un limite suabe.

#### Operacion sin interruptores de limite<span id="sub:Operating-without-Limit"></span>

Una maquina puede ser operada sin interruptores de limite. En ese caso, solo los limites suabes detendran al eje de alcanzar el limite fisico. Los limites suabes solo operan despues de que se a ejecutado la rutina de inizializacion.

#### Operacion sin limites de inizializacion<span id="sub:Operating-without-Home"></span>

Una maquina puede ser operada sin interruptores de inicializacion. Si la maquina tiene interruptores de limite pero no de inicializacion, es mejor utilizar un interruptor de limite como interruptor de inizializacion (ejemplo, seleccione la opcion *Minimum Limit + Home X* cuando configure el puerto en Stepconf). Si la maquina no tiene interruptores de ningun tipo, o si los interruptores no pueden ser utilizados como interruptores de inizializacion por cualquier otra razon, entonces la maquina devera ser inicializada a mano, o utilizando marcas en la bancada. la inicializacion a mano no es tan confiable como la inicializacion con interruptores, pero permite seguir utilizando los limites suabes.

#### Opciones de cableado de los interruptores de inicializacion y limite<span id="sub:Home-and-Limit"></span>

El cableado ideal deve de incluir un interruptor por señal. Sin embargo, el puerto paralelo del computador solo ofrece un total de 5 entradas, mientras que se necesitan 9 en una maquina de 3 ejes. Por lo tanto, Varios interruptores pueden ser cableados en conjunto en diversas formas para permitir utilizar menos entradas.

La siguiente figura muestra la idea general de cablear varios interruptores a una unica entrada. En el caso ilustrado, cuando un interruptor es precionado, El valor mandado atraves de la entrada va de un valor logico ALTO a BAJO. Sin embargo Machinekit espera un valor logico ALTO cuando un interruptor es precionado, Por lo tanto se devera seleccionar la opcion de invercion *Invert* cuando se configure la entrada del puerto paralelo en el Stepconf.

La resistencia de polarizacion mostrada en el diagrama fija la señal de entrada a ALTO Amenos que una conexion a tierra sea realizada, en tal caso la señal ira a BAJO. Sin la resistencia la entrada quedaria flotando y la entrada podria variar entre ALTO y BAJO cuando el circuito este abierto. Un balor tipoco de resistencia de polarizacion es de 47K.

Interruptores normalmente cerrados<span id="cap:Normally-Closed-Switches"></span>

Cableado de Interruptores Normalmente Cerrados en serie (diagrama simplificado)

![images/switch-nc-series.png]()

Interruptores normalmente abiertos<span id="cap:Normally-Open-Switches"></span>

Cableado de interruptores normalmente abiertos en paralelo (diagrama simplificado)

![images/switch-no-parallel.png]()

Las siguientes configuraciones de interruptores estan permitidas en Stepconf:

-   Interruptores de inizializacion combinados para todos los ejes

-   Interruptores de limite combinados para todos los ejes

-   Combinar ambos interruptores de limite para un eje

-   Combinar ambos interruptores de limite y el interruptor de inizializacion para un eje

-   Combinar un interruptor de limite y el interruptor de inicializacion de un eje

Mesa Configuration Wizard
-------------------------

PNCconf is made to help build configurations that utilize specific Mesa *Anything I/O* products.

It can configure closed loop servo systems or hardware stepper systems. It uses a similar *wizard* approach as Stepconf (used for software stepping, parallel port driven systems).

There are two trains of thought when using PNCconf:

One is to use PNCconf to always configure your system - if you decide to change options, reload PNCconf and allow it to configure the new options. This will work well if your machine is fairly standard and you can use custom files to add non standard features. PNCconf tries to work with you in this regard.

The other is to use PNCconf to build a config that is close to what you want and then hand edit everything to tailor it to your needs. This would be the choice if you need extensive modifications beyond PNCconf’s scope or just want to tinker with / learn about Machinekit

You navigate the wizard pages with the forward, back, and cancel buttons there is also a help button that gives some help information about the pages, diagrams and an output page.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Tip
</div></td>
<td align="left">PNCconf’s help page should have the most up to date info and has additional details.</td>
</tr>
</tbody>
</table>

Step by Step Instructions
-------------------------

![images/pncconf-splash.png]()

Figure 9. PnCConf Splash

### Create or Edit

This allows you to select a previously saved configuration or create a new one. If you pick *Modify a configuration* and then press next a file selection box will show. Pncconf preselects your last saved file. Choose the the config you wish to edit. It also allows you to select desktop shortcut / launcher options. A desktop shortcut will place a folder icon on the desktop that points to your new configuration files. Otherwise you would have to look in your home folder under machinekit/configs.

A Desktop launcher will add an icon to the desktop for starting your config directly. You can also launch it under Applications/cnc/emc2 and selecting your config name.

### Basic Machine Information

![images/pncconf-basic.png]()

Figure 10. PnCConf Basic

 Machine Basics   
If you use a name with spaces PNCconf will replace the spaces with underscore (as a loose rule Linux doesn’t like spaces in names) Pick an axis configuration - this selects what type of machine you are building and what axes are available. The Machine units selector allows data entry of metric or imperial units in the following pages.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Tip
</div></td>
<td align="left">Defaults are not converted when using metric so make sure they are sane values!</td>
</tr>
</tbody>
</table>

 Computer Response Time   
The servo period sets the heart beat of the system. Latency refers to the amount of time the computer can be longer then that period. Just like a railroad, Machinekit requires everything on a very tight and consistent time line or bad things happen. Machinekit requires and uses a *real time* operating system, which just means it has a low latency ( lateness ) response time when Machinekit requires its calculations and when doing Machinekits calculations it cannot be interrupted by lower priority requests (such as user input to screen buttons or drawing etc).

Testing the latency is very important and a key thing to check early. Luckily by using the Mesa card to do the work that requires the fastest response time (encoder counting and PWM generation) we can endure a lot more latency then if we used the parallel port for these things. The standard test in Machinekit is checking the BASE period latency (even though we are not using a base period). If you press the *test base period jitter* button, this launches the latency test window ( you can also load this directly from the applications/cnc panel ). The test mentions to run it for a few minutes but the longer the better. consider 15 minutes a bare minimum and overnight even better. At this time use the computer to load things, use the net, use USB etc we want to know the worst case latency and to find out if any particular activity hurts our latency. We need to look at base period jitter. Anything under 20000 is excellent - you could even do fast software stepping with the machine 20000 - 50000 is still good for software stepping and fine for us. 50000 - 100000 is really not that great but could still be used with hardware cards doing the fast response stuff. So anything under 100000 is useable to us. If the latency is disappointing or you get a bad hiccup periodically you may still be able to improve it.

Now we are happy with the latency and must pick a servo period. In most cases a servo period of 1000000 ns is fine ( that gives a 1 kHz servo calculation rate - 1000 calculations a second) if you are building a closed loop servo system that controls torque (current) rather then velocity (voltage) a faster rate would be better - something like 200000 (5 kHz calculation rate). The problem with lowering the servo rate is that it leaves less time available for the computer to do other things besides Machinekit’s calculations. Typically the display (GUI) becomes less responsive. You must decide on a balance. Keep in mind that if you tune your closed loop servo system then change the servo period you probably will need to tune them again.

 I/O Control Ports/Boards   
PNCconf is capable of configuring machines that have up to two Mesa boards and three parallel ports. Parallel ports can only be used for simple low speed (servo rate) I/O.

 Mesa   
You must choose at least one Mesa board as PNCconf will not configure the parallel ports to count encoders or output step or PWM signals. The mesa cards available in the selection box are based on what PNCconf finds for firmware on the systems. There are options to add custom firmware and/or *blacklist* (ignore) some firmware or boards using a preference file. If no firmware is found PNCconf will show a warning and use internal sample firmware - no testing will be possible. One point to note is that if you choose two PCI Mesa cards there currently is no way to predict which card is 0 and which is 1 - you must test - moving the cards could change their order. If you configure with two cards both cards must be installed for tests to function.

 Parallel Port   
Up to 3 parallel ports (referred to as parports) can be used as simple I/O. You must set the address of the parport. You can either enter the Linux parallel port numbering system (0,1,or 2) or enter the actual address. The address for an on board parport is often 0x0278 or 0x0378 (written in hexadecimal) but can be found in the BIOS page. The BIOS page is found when you first start your computer you must press a key to enter it (such as F2). On the BIOS page you can find the parallel port address and set the mode such as SPP, EPP, etc on some computers this info is displayed for a few seconds during start up. For PCI parallel port cards the address can be found by pressing the *parport address search* button. This pops up the help output page with a list of all the PCI devices that can be found. In there should be a reference to a parallel port device with a list of addresses. One of those addresses should work. Not all PCI parallel ports work properly. Either type can be selected as *in* (maximum amount of input pins) or *out* (maximum amount of output pins)

 GUI Frontend list   
This specifies the graphical display screens Machinekit will use. Each one has different option.

AXIS

-   fully supports lathes.

-   is the most developed and used frontend

-   is designed to be used with mouse and keyboard

-   is tkinter based so integrates PYVCP (python based virtual control panels) naturally.

-   has a 3D graphical window.

-   allows VCP integrated on the side or in center tab

TOUCHY

-   Touchy was designed to be used with a touchscreen, some minimal physical switches and a MPG wheel.

-   requires cycle-start, abort, and single-step signals and buttons

-   It also requires shared axis MPG jogging to be selected.

-   is GTK based so integrates GLADE VCP (virtual control panels) naturally.

-   allows VCP panels integrated in the center Tab

-   has no graphical window

-   look can be changed with custom themes

MINI

-   standard on OEM Sherline machines

-   does not use Estop

-   no VCP integration

TkMachinekit

-   hi contrast bright blue screen

-   separate graphics window

-   no VCP integration

### External Configuration

This page allows you to select external controls such as for jogging or overrides.

![images/pncconf-external.png]()

Figure 11. GUI External

If you select a Joystick for jogging, You will need it always connected for Machinekit to load. To use the analog sticks for useful jogging you probably need to add some custom HAL code. MPG jogging requires a pulse generator connected to a MESA encoder counter. Override controls can either use a pulse generator (MPG) or switches (such as a rotary dial). External buttons might be used with a switch based OEM joystick.

 Joystick jogging   
Requires a custom *device rule* to be installed in the system. This is a file that Machinekit uses to connect to LINUX’s device list. PNCconf will help to make this file.

*Search for device rule* will search the system for rules, you can use this to find the name of devices you have already built with PNCconf.

*Add a device rule* will allow you to configure a new device by following the prompts. You will need your device available.

*test device* allows you to load a device, see its pin names and check its functions with halmeter.

joystick jogging uses HALUI and hal\_input components.

 External buttons   
allows jogging the axis with simple buttons at a specified jog rate. Probably best for rapid jogging.

 MPG Jogging   
Allows you to use a Manual Pulse Generator to jog the machine’s axis.

MPG’s are often found on commercial grade machines. They output quadrature pulses that can be counted with a MESA encoder counter. PNCconf allows for an MPG per axis or one MPG shared with all axis. It allows for selection of jog speeds using switches or a single speed.

The selectable increments option uses the mux16 component. This component has options such as debounce and gray code to help filter the raw switch input.

 Overrides   
PNCconf allows overrides of feedrates and/or spindle speed using a pulse generator (MPG) or switches (eg. rotary).

### GUI Configuration

Here you can set defaults for the display screens, add virtual control panels (VCP), and set some Machinekit options..

![images/pncconf-gui.png]()

Figure 12. GUI Configuration

 Frontend GUI Options   
The default options allows general defaults to be chosen for any display screen.

AXIS defaults are options specific to AXIS. If you choose size , position or force maximize options then PNCconf will ask if it’s alright to overwrite a preference file (.axisrc). Unless you have manually added commands to this file it is fine to allow it. Position and force max can be used to move AXIS to a second monitor if the system is capable.

Touchy defaults are options specific to Touchy. Most of Touchy’s options can be changed while Touchy is running using the preference page. Touchy uses GTK to draw its screen, and GTK supports themes. Themes controls the basic look and feel of a program. You can download themes from the net or edit them yourself. There are a list of the current themes on the computer that you can pick from. To help some of the text to stand out PNCconf allows you to override the Themes’s defaults. The position and force max options can be used to move Touchy to a second monitor if the system is capable.

 VCP options   
Virtual Control Panels allow one to add custom controls and displays to the screen. AXIS and Touchy can integrate these controls inside the screen in designated positions. There are two kinds of VCPs - pyVCP which uses *Tkinter* to draw the screen and GLADE VCP that uses *GTK* to draw the screen.

 PyVCP   
PyVCPs screen XML file can only be hand built. PyVCPs fit naturally in with AXIS as they both use TKinter.

HAL pins are created for the user to connect to inside their custom HAL file. There is a sample spindle display panel for the user to use as-is or build on. You may select a blank file that you can later add your controls *widgets* to or select a spindle display sample that will display spindle speed and indicate if the spindle is at requested speed.

PNCconf will connect the proper spindle display HAL pins for you. If you are using AXIS then the panel will be integrated on the right side. If not using AXIS then the panel will be separate *stand-alone* from the frontend screen.

You can use the geometry options to size and move the panel, for instance to move it to a second screen if the system is capable. If you press the *Display sample panel* button the size and placement options will be honoured.

 GLADE VCP   
GLADE VCPs fit naturally inside of TOUCHY screen as they both use GTK to draw them, but by changing GLADE VCP’s theme it can be made to blend pretty well in AXIS. (try Redmond)

It uses a graphical editor to build its XML files. HAL pins are created for the user to connect to, inside of their custom HAL file.

GLADE VCP also allows much more sophisticated (and complicated) programming interaction, which PNCconf currently doesn’t leverage. (see GLADE VCP in the manual)

PNCconf has sample panels for the user to use as-is or build on. With GLADE VCP PNCconf will allow you to select different options on your sample display.

Under *sample options* select which ones you would like. The zero buttons use HALUI commands which you could edit later in the HALUI section.

Auto Z touch-off also requires the classicladder touch-off program and a probe input selected. It requires a conductive touch-off plate and a grounded conductive tool. For an idea on how it works see: [Here](http://wiki.linuxcnc.org/cgi-bin/wiki.pl?ClassicLadderExamples#Single_button_probe_touchoff)

Under *Display Options*, size, position, and force max can be used on a *stand-alone* panel for such things as placing the screen on a second monitor if the system is capable.

You can select a GTK theme which sets the basic look and feel of the panel. You Usually want this to match the frontend screen. These options will be used if you press the *Display sample button*. With GLADE VCP depending on the frontend screen, you can select where the panel will display.

You can force it to be stand-alone or with AXIS it can be in the center or on the right side, with Touchy it can be in the center.

 Defaults and Options   
-   Require homing before MDI / Running

    -   If you want to be able to move the machine before homing uncheck this checkbox.

-   Popup Tool Prompt

    -   Choose between an on screen prompt for tool changes or export standard signal names for a User supplied custom tool changer Hal file

-   Leave spindle on during tool change:

    -   Used for lathes

-   Force individual manual homing

-   Move spindle up before tool change

-   Restore joint position after shutdown

    -   Used for non-trivial kinematics machines

-   Random position toolchangers

    -   Used for toolchangers that do not return the tool to the same pocket. You will need to add custom HAL code to support toolchangers.

### Mesa Configuration

The Mesa configuration pages allow one to utilize different firmwares. On the basic page you selected a Mesa card here you pick the available firmware and select what and how many components are available.

![images/pncconf-mesa-config.png]()

Figure 13. Mesa Configuration

Parport address is used only with Mesa parport card, the 7i43. An onboard parallel port usually uses 0x278 or 0x378 though you should be able to find the address from the BIOS page. The 7i43 requires the parallel port to use the EPP mode, again set in the BIOS page. If using a PCI parallel port the address can be searched for by using the search button on the basic page.

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
<td align="left">Many PCI cards do not support the EPP protocol properly.</td>
</tr>
</tbody>
</table>

PDM PWM and 3PWM base frequency sets the balance between ripple and linearity. If using Mesa daughter boards the docs for the board should give recommendations

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Important
</div></td>
<td align="left">It’s important to follow these to avoid damage and get the best performance.</td>
</tr>
</tbody>
</table>

    The 7i33 requires PDM and a PDM base frequency of 6 mHz
    The 7i29 requires PWM and a PWM base frequency of 20 Khz
    The 7i30 requires PWM and a PWM base frequency of 20 Khz
    The 7i40 requires PWM and a PWM base frequency of 50 Khz
    The 7i48 requires UDM and a PWM base frequency of 24 Khz

Watchdog time out is used to set how long the MESA board will wait before killing outputs if communication is interrupted from the computer. Please remember Mesa uses *active low* outputs meaning that when the output pin is on, it is low (approx 0 volts) and if it’s off the output in high (approx 5 volts) make sure your equipment is safe when in the off (watchdog bitten) state.

You may choose the number of available components by deselecting unused ones. Not all component types are available with all firmware.

Choosing less then the maximum number of components allows one to gain more GPIO pins. If using daughter boards keep in mind you must not deselect pins that the card uses. For instance some firmware supports two 7i33 cards, If you only have one you may deselect enough components to utilize the connector that supported the second 7i33. Components are deselected numerically by the highest number first then down with out skipping a number. If by doing this the components are not where you want them then you must use a different firmware. The firmware dictates where, what and the max amounts of the components. Custom firmware is possible, ask nicely when contacting the Machinekit developers and Mesa. Using custom firmware in PNCconf requires special procedures and is not always possible - Though I try to make PNCconf as flexible as possible.

After choosing all these options press the *Accept Component Changes* button and PNCconf will update the I/O setup pages. Only I/O tabs will be shown for available connectors, depending on the Mesa board.

### Mesa I/O Setup

The tabs are used to configure the input and output pins of the Mesa boards. PNCconf allows one to create custom signal names for use in custom HAL files.

![images/pncconf-mesa-io2.png]()

Figure 14. Mesa I/O C2

On this tab with this firmware the components are setup for a 7i33 daughter board, usually used with closed loop servos. Note the component numbers of the encoder counters and PWM drivers are not in numerical order. This follows the daughter board requirements.

![images/pncconf-mesa-io3.png]()

Figure 15. Mesa I/O C3

On this tab all the pins are GPIO. Note the 3 digit numbers - they will match the HAL pin number. GPIO pins can be selected as input or output and can be inverted.

![images/pncconf-mesa-io4.png]()

Figure 16. Mesa I/O C4

On this tab there are a mix of step generators and GPIO. Step generators output and direction pins can be inverted. Note that inverting a Step Gen-A pin (the step output pin) changes the step timing. It should match what your controller expects.

### Parport configuration

![images/pncconf-parport.png]()

The parallel port can be used for simple I/O similar to Mesa’s GPIO pins.

### Axis Configuration

![images/pncconf-axis-drive.png]()

Figure 17. Axis Drive Configuration

This page allows configuring and testing of the motor and/or encoder combination . If using a servo motor an open loop test is available, if using a stepper a tuning test is available.

 Open Loop Test   
An open loop test is important as it confirms the direction of the motor and encoder. The motor should move the axis in the positive direction when the positive button is pushed and also the encoder should count in the postie direction. The axis movement should follow the Machinery’s Handbook <span class="footnote">
\["axis nomenclature" in the chapter "Numerical Control" in the "Machinery’s Handbook" published by Industrial Press.\]
</span> standards or AXIS graphical display will not make much sense. Hopefully the help page and diagrams can help figure this out. Note that axis directions are based on TOOL movement not table movement. There is no acceleration ramping with the open loop test so start with lower DAC numbers. By moving the axis a known distance one can confirm the encoder scaling. The encoder should count even without the amp enabled depending on how power is supplied to the encoder.

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
<td align="left">If the motor and encoder do not agree on counting direction then the servo will run away when using PID control.</td>
</tr>
</tbody>
</table>

Since at the moment PID settings can not be tested in PNCconf the settings are really for when you re-edit a config - enter your tested PID settings.

DAC scaling, max output and offset are used to tailor the DAC output.

 Compute DAC   
These two values are the scale and offset factors for the axis output to the motor amplifiers. The second value (offset) is subtracted from the computed output (in volts), and divided by the first value (scale factor), before being written to the D/A converters. The units on the scale value are in true volts per DAC output volts. The units on the offset value are in volts. These can be used to linearize a DAC.

Specifically, when writing outputs, the Machinekit first converts the desired output in quasi-SI units to raw actuator values, e.g., volts for an amplifier DAC. This scaling looks like: The value for scale can be obtained analytically by doing a unit analysis, i.e., units are \[output SI units\]/\[actuator units\]. For example, on a machine with a velocity mode amplifier such that 1 volt results in 250 mm/sec velocity, Note that the units of the offset are in machine units, e.g., mm/sec, and they are pre-subtracted from the sensor readings. The value for this offset is obtained by finding the value of your output which yields 0.0 for the actuator output. If the DAC is linearized, this offset is normally 0.0.

The scale and offset can be used to linearize the DAC as well, resulting in values that reflect the combined effects of amplifier gain, DAC non-linearity, DAC units, etc. To do this, follow this procedure:

-   Build a calibration table for the output, driving the DAC with a desired voltage and measuring the result:

<table>
<caption>Table 1. Output Voltage Measurements</caption>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><strong>Raw</strong></p></td>
<td align="left"><p><strong>Measured</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p>-10</p></td>
<td align="left"><p><strong>-9.93</strong></p></td>
</tr>
<tr class="odd">
<td align="left"><p>-9</p></td>
<td align="left"><p><strong>-8.83</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p><strong>-0.96</strong></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p><strong>-0.03</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p>9</p></td>
<td align="left"><p><strong>9.87</strong></p></td>
</tr>
<tr class="odd">
<td align="left"><p>10</p></td>
<td align="left"><p><strong>10.07</strong></p></td>
</tr>
</tbody>
</table>

-   Do a least-squares linear fit to get coefficients a, b such that meas=a\*raw+b

-   Note that we want raw output such that our measured result is identical to the commanded output. This means

    -   cmd=a\*raw+b

    -   raw=(cmd-b)/a

-   As a result, the a and b coefficients from the linear fit can be used as the scale and offset for the controller directly.

MAX OUTPUT: The maximum value for the output of the PID compensation that is written to the motor amplifier, in volts. The computed output value is clamped to this limit. The limit is applied before scaling to raw output units. The value is applied symmetrically to both the plus and the minus side.

**Tuning Test** The tuning test unfortunately only works with stepper based systems. Again confirm the directions on the axis is correct. Then test the system by running the axis back and forth, If the acceleration or max speed is too high you will lose steps. While jogging, Keep in mind it can take a while for an axis with low acceleration to stop. Limit switches are not functional during this test. You can set a pause time so each end of the test movement. This would allow you to set up and read a dial indicator to see if you are loosing steps.

**Stepper Timing** Stepper timing needs to be tailored to the step controller’s requirements. Pncconf supplies some default controller timing or allows custom timing settings . See [here](http://wiki.machinekit.org/cgi-bin/wiki.pl?Stepper_Drive_Timing) for some more known timing numbers (feel free to add ones you have figured out). If in doubt use large numbers such as 5000 this will only limit max speed.

**Brushless Motor Control** These options are used to allow low level control of brushless motors using special firmware and daughter boards. It also allows conversion of HALL sensors from one manufacturer to another. It is only partially supported and will require one to finish the HAL connections. Contact the mail-list or forum for more help.

![images/pncconf-scale-calc.png]()

Figure 18. Axis Scale Calculation

The scale settings can be directly entered or one can use the *calculate scale* button to assist. Use the check boxes to select appropriate calculations. Note that *pulley teeth* requires the number of teeth not the gear ratio. Worm turn ratio is just the opposite it requires the gear ratio. If your happy with the scale press apply otherwise push cancel and enter the scale directly.

![images/pncconf-axis-config.png]()

Figure 19. Axis Configuration

Also refer to the diagram tab for two examples of home and limit switches. These are two examples of many different ways to set homing and limits.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Important
</div></td>
<td align="left">It is very important to start with the axis moving in the right direction or else getting homing right is very difficult!</td>
</tr>
</tbody>
</table>

Remember positive and negative directions refer to the TOOL not the table as per the Machinists handbook.

On a typical knee or bed mill

-   when the TABLE moves out that is the positive Y direction

-   when the TABLE moves left that is the positive X direction

-   when the TABLE moves down that is the positive Z direction

-   when the HEAD moves up that is the positive Z direction

On a typical lathe

-   when the TOOL moves right, away from the chuck

-   that is the positive Z direction

-   when the TOOL moves toward the operator

-   that is the positive X direction. Some lathes have X

-   opposite (eg tool on back side), that will work fine but

-   AXIS graphical display can not be made to reflect this.

When using homing and / or limit switches Machinekit expects the HAL signals to be true when the switch is being pressed / tripped. If the signal is wrong for a limit switch then Machinekit will think the machine is on end of limit all the time. If the home switch search logic is wrong Machinekit will seem to home in the wrong direction. What it actually is doing is trying to BACK off the home switch.

Decide on limit switch location.

Limit switches are the back up for software limits in case something electrical goes wrong eg. servo runaway. Limit switches should be placed so that the machine does not hit the physical end of the axis movement. Remember the axis will coast past the contact point if moving fast. Limit switches should be *active low* on the machine. eg. power runs through the switches all the time - a loss of power (open switch) trips. While one could wire them the other way, this is fail safe. This may need to be inverted so that the HAL signal in Machinekit in *active high* - a TRUE means the switch was tripped. When starting Machinekit if you get an on-limit warning, and axis is NOT tripping the switch, inverting the signal is probably the solution. (use HALMETER to check the corresponding HAL signal eg. axis.0.pos-lim-sw-in X axis positive limit switch)

Decide on the home switch location.

If you are using limit switches You may as well use one as a home switch. A separate home switch is useful if you have a long axis that in use is usually a long way from the limit switches or moving the axis to the ends presents problems of interference with material. eg a long shaft in a lathe makes it hard to home to limits with out the tool hitting the shaft, so a separate home switch closer to the middle may be better. If you have an encoder with index then the home switch acts as a course home and the index will be the actual home location.

Decide on the MACHINE ORIGIN position.

MACHINE ORIGIN is what Machinekit uses to reference all user coordinate systems from. I can think of little reason it would need to be in any particular spot. There are only a few G codes that can access the MACHINE COORDINATE system.( G53, G30 and G28 ) If using tool-change-at-G30 option having the Origin at the tool change position may be convenient. By convention, it may be easiest to have the ORIGIN at the home switch.

Decide on the (final) HOME POSITION.

this just places the carriage at a consistent and convenient position after Machinekit figures out where the ORIGIN is.

Measure / calculate the positive / negative axis travel distances.

Move the axis to the origin. Mark a reference on the movable slide and the non-moveable support (so they are in line) move the machine to the end of limits. Measure between the marks that is one of the travel distances. Move the table to the other end of travel. Measure the marks again. That is the other travel distance. If the ORIGIN is at one of the limits then that travel distance will be zero.

 (machine) ORIGIN   
The Origin is the MACHINE zero point. (not the zero point you set your cutter / material at). Machinekit uses this point to reference everything else from. It should be inside the software limits. Machinekit uses the home switch location to calculate the origin position (when using home switches or must be manually set if not using home switches.

 Travel distance   
This is the maximum distance the axis can travel in each direction. This may or may not be able to be measured directly from origin to limit switch. The positive and negative travel distances should add up to the total travel distance.

 POSITIVE TRAVEL DISTANCE   
This is the distance the Axis travels from the Origin to the positive travel distance or the total travel minus the negative travel distance. You would set this to zero if the origin is positioned at the positive limit. The will always be zero or a positive number.

 NEGATIVE TRAVEL DISTANCE   
This is the distance the Axis travels from the Origin to the negative travel distance. or the total travel minus the positive travel distance. You would set this to zero if the origin is positioned at the negative limit. This will always be zero or a negative number. If you forget to make this negative PNCconf will do it internally.

 (Final) HOME POSITION   
This is the position the home sequence will finish at. It is referenced from the Origin so can be negative or positive depending on what side of the Origin it is located. When at the (final) home position if you must move in the Positive direction to get to the Origin, then the number will be negative.

 HOME SWITCH LOCATION   
This is the distance from the home switch to the Origin. It could be negative or positive depending on what side of the Origin it is located. When at the home switch location if you must move in the Positive direction to get to the Origin, then the number will be negative. If you set this to zero then the Origin will be at the location of the limit switch (plus distance to find index if used)

 Home Search Velocity   
Course home search velocity in units per minute.

 Home Search Direction   
Sets the home switch search direction either negative (ie. towards negative limit switch) or positive (ie. towards positive limit switch)

 Home Latch Velocity   
Fine Home search velocity in units per minute

 Home Final Velocity   
Velocity used from latch position to (final) home position in units per minute. Set to 0 for max rapid speed

 Home latch Direction   
Allows setting of the latch direction to the same or opposite of the search direction.

 Use Encoder Index For Home   
Machinekit will search for an encoder index pulse while in the latch stage of homing.

 Use Compensation File   
Allows specifying a Comp filename and type. Allows sophisticated compensation. See Manual.

 Use Backlash Compensation   
Allows setting of simple backlash compensation. Can not be used with Compensation File. See Manual.

![images/pncconf-diagram-lathe.png]()

Figure 20. AXIS Help Diagram

The diagrams should help to demonstrate an example of limit switches and standard axis movement directions. In this example the Z axis was two limit switches, the positive switch is shared as a home switch. The MACHINE ORIGIN (zero point) is located at the negative limit. The left edge of the carriage is the negative trip pin and the right the positive trip pin. We wish the FINAL HOME POSITION to be 4 inches away from the ORIGIN on the positive side. If the carriage was moved to the positive limit we would measure 10 inches between the negative limit and the negative trip pin.

### Spindle Configuration

If you select spindle signals then this page is available to configure spindle control.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Tip
</div></td>
<td align="left">Many of the option on this page will not show unless the proper option was selected on previous pages!</td>
</tr>
</tbody>
</table>

![images/pncconf-spindle-config.png]()

Figure 21. Spindle Configuration

This page is similar to the axis motor configuration page.

There are some differences:

-   Unless one has chosen a stepper driven spindle there is no acceleration or velocity limiting.

-   There is no support for gear changes or ranges.

-   If you picked a VCP spindle display option then spindle-at-speed scale and filter settings may be shown.

-   Spindle-at-speed allows Machinekit to wait till the spindle is at the requested speed before moving the axis. This is particularly handy on lathes with constant surface feed and large speed diameter changes. It requires either encoder feedback or a digital spindle-at-speed signal typically connected to a VFD drive.

-   If using encoder feedback, you may select a spindle-at-speed scale setting that specifies how close the actual speed must be to the requested speed to be considered at-speed.

-   If using encoder feedback, the VCP speed display can be erratic - the filter setting can be used to smooth out the display. The encoder scale must be set for the encoder count / gearing used.

-   If you are using a single input for a spindle encoder you must add the line: setp hm2\_7i43.0.encoder.00.counter-mode 1 (changing the board name and encoder number to your requirements) into a custom HAL file. See the Hostmot2 section on encoders for more info about counter mode.

### Advanced Options

This allows setting of HALUI commands and loading of classicladder and sample ladder programs. If you selected GLADE VCP options such as for zeroing axis, there will be commands showing. See the manual about info on HALUI for using custom halcmds. There are several ladder program options. The Estop program allows an external ESTOP switch or the GUI frontend to throw an Estop. It also has a timed lube pump signal. The Z auto touch-off is with a touch-off plate, the GLADE VCP touch-off button and special HALUI commands to set the current user origin to zero and rapid clear. The serial modbus program is basically a blank template program that sets up classicladder for serial modbus. See the classicladder section in the manual.

![images/pncconf-advanced.png]()

Figure 22. Advanced Options

### HAL Components

On this page you can add additional HAL components you might need for custom HAL files. In this way one should not have to hand edit the main HAL file, while still allowing user needed components.

![images/pncconf-hal.png]()

Figure 23. HAL Components

The first selection is components that pncconf uses internally. You may configure pncconf to load extra instances of the components for your custom HAL file.

Select the number of instances your custom file will need, pncconf will add what it needs after them.

Meaning if you need 2 and pncconf needs 1 pncconf will load 3 instances and use the last one.

 Custom Component Commands   
This selection will allow you to load HAL components that pncconf does not use. Add the loadrt or loadusr command, under the heading *loading command* Add the addf command under the heading *Thread command*. The components will be added to the thread between reading of inputs and writing of outputs, in the order you write them in the *thread command*.

### Advanced Usage Of PNCconf

PNCconf does its best to allow flexible customization by the user. PNCconf has support for custom signal names, custom loading of components, custom HAL files and custom firmware.

There are also signal names that PNCconf always provides regardless of options selected, for user’s custom HAL files With some thought most customizations should work regardless if you later select different options in PNCconf.

Eventually if the customizations are beyond the scope of PNCconf’s frame work you can use PNCconf to build a base config or use one of Machinekit’s sample configurations and just hand edit it to what ever you want.

 Custom Signal Names   
If you wish to connect a component to something in a custom HAL file write a unique signal name in the combo entry box. Certain components will add endings to your custom signal name:

Encoders will add &lt; customname &gt; +:

-   position

-   count

-   velocity

-   index-enable

-   reset

Steppers add:

-   enable

-   counts

-   position-cmd

-   position-fb

-   velocity-fb

PWM add:

-   enable

-   value

GPIO pins will just have the entered signal name connected to it

In this way one can connect to these signals in the custom HAL files and still have the option to move them around later.

 Custom Signal Names   
The Hal Components page can be used to load components needed by a user for customization.

 Loading Custom Firmware   
PNCconf searches for firmware on the system and then looks for the XML file that it can convert to what it understands. These XML files are only supplied for officially released firmware from the Machinekit team. To utilize custom firmware one must convert it to an array that PNCconf understands and add its filepath to PNCconf’s preference file. By default this path searches the desktop for a folder named custom\_firmware and a file named firmware.py.

The hidden preference file is in the user’s home file, is named .pncconf-preferences and require one to select *show hidden files* to see and edit it. The contents of this file can be seen when you first load PNCconf - press the help button and look at the output page.

Ask on the Machinekit mail-list or forum for info about converting custom firmware. Not all firmware can be utilized with PNCconf.

 Custom HAL Files   
There are four custom files that you can use to add HAL commands to:

-   custom.hal is for HAL commands that don’t have to be run after the GUI frontend loads. It is run after the configuration-named HAL file.

-   custom\_postgui.hal is for commands that must be run after AXIS loads or a standalone PYVCP display loads.

-   custom\_gvcp.hal is for commands that must be run after glade VCP is loaded.

-   shutdown.hal is for commands to run when Machinekit shuts down in a controlled manner.

Ejecutando Machinekit
---------------------

<span id="cha:running-emc"></span>

### invocación de Machinekit

Después de la instalación, Machinekit comienza como cualquier otro programa de Linux: Ejecutándolo desde la terminal con el comando `emc`, O seleccionandolo en el menu de aplicaciones → CNC menu.

### Selector de configuración

De manera determinada, el dialogo de seleccion de configuracion Se muestra cuando se ejecuta Machinekit por primera vez. Las configuraciones personalizadas se muestran en la parte superior de la lista, Seguida de ejemplos de configuraciones. Devido a que cada ejemplo de configuraciones es para un tipo diferente de interface de hardware. Casi ninguna funcionara sin el hardware espcifico instalado. Las configuraciones que están en la categoría "sim" se ejecutan completamente Sin ningun hardware conectado.

Figura [\[cap:Machinekit-Configuration-Selector\]](#cap:Machinekit-Configuration-Selector) muestra la apariencia De ventana de selección de configuración.

![images/configuration-selector.png]()

Figure 24. Machinekit seleccion de configuración<span id="cap:Machinekit-Configuration-Selector"></span>

De click en cualquiera de las configuraciones que se indican para mostrar información específica al respecto. De doble click en una configuración o haga click en aceptar para ejecutar la configuración. Seleccione "crear acceso directo en el escritorio" y luego de click en aceptar para añadir un icono en el escritorio de Debian para iniciar directamente esta configuración sin mostrar la pantalla de selección de configuración.

Cuan do se selecciona una configuracion de la seccion de muestras de configuracion, se creeara un a copia de la misma en el directorio emc/configs

### Los siguientes pasos en la configuración son

Después de encontrar el ejemplo de la configuración que utiliza la misma interface que el hardware de su máquina, guarde una copia en su directorio personal. Usted puede personalizar de acuerdo a los detalles d su maquina consulte el manual de integración de los temas de configuración.

Linux FAQ
---------

<span id="cha:linux-faq"></span>

Estos son algunos comandos básicos y técnicas para nuevos usuarios de linux. Información más completa se puede encontrar en la web o mediante las páginas del manual con el comando man.

### Acceso automático

Al instalar linuxCNC con el CD de Debian por defecto se tiene que iniciar sesión cada vez que encienda el ordenador. Para activar autentificacion automática valla a *System &gt; Administration &gt; Login Window*. Si se trata de una nueva instalación de la ventana de inicio de sesión puede tardar un segundo o tres para apareser. Usted tiene que tener la contraseña que utilizo para la instalación para acceder a la ventana de configuracion de inicio. En la pestaña seguridad marque habilitar acceso automático y elija un nombre de usuario de la lista (seleecione su nombre de usuario).

### Inicio automático

Para tener un inicio automático de linuxCNC con su configuración después de encender el equipo valla a *System &gt; Preferences &gt; Sessions &gt; Startup Applications*, seleccione la opcion agregar nuevo. Valla a su carpeta de configuración y seleccione el archivo .ini. Cuando el cuadro de dialogo selector de archivos se cierra, añadir emc y un espacio al frente de la ruta a su archivo .ini.

Ejemplo:

    emc /home/mill/emc2/config/mill/mill.ini

### Páginas de manual <span id="sec:Man-Pages"></span>

Las páginas del manual son generadas automáticamente en la mayoría de los casos. Páginas del manuales estan generalmente disponibles para la mayoría de los programas y comandos de linux.

Para abrir una página del manual utilice una terminar, la terminal se puede abrir usando la ruta *Applications &gt; Accessories &gt; Terminal*. Por ejemplo si quiere encontrar algo acerca del comando de busqueda "find" tecle:

    man find

Utilice los botones página hacia arriba y pagina hacia abajo para ver las páginas del manual y la tecla Q para salir de la visualizacion.

### Lista de módulos

A veces para soluciónar algun problemas que usted necesita obtener una lista de módulos que se encuentran cargados. En una ventana de terminal tecle:

    lsmod

Si desea enviar la salida de lsmod a un archivo de texto tecle:

    lsmod > mymod.txt

El archivo de texto resultante se colocara en el directorio de inicio si usted no cambio de directorio cuando abrio la terminal y será nombrado mymod.txt o como usted lo haya nombrado.

### Edición de archivos raíz <span id="sec:Editing-a-Root-File"></span>

Al abrir el explorador de archivos y ver que el propietario del archivo es el usuario raíz, se tienen que hacer algunos pasos adicionales para modificar el archivo. La edición de algunos archivos puede tener malos resultados. Tenga cuidado al editar los archivos de raíz, generalmente usted puede ver y abrir los archivos raíz en modo de *solo lectura*.

#### El camino de línea de comandos

Abrir *Applications &gt; Accessories &gt; Terminal*.

En la ventana de terminal tecle.

    sudo gedit

Abrir el archivo con el menu File &gt; Open &gt; Edit, proceda a editar.

#### Usando la interface grafica

    .Haga clic derecho sobre el escritorio y seleccione crear lansador
    .Escriba nombre en como editar sudo
    .Escriba 'gksudo "gnome-open %u"' como el comando y guarda el lansador
     en su escritorio.
    .Arrastré el archivo a su lansador para abrir y editar.

#### Acceso tipo super usuario

En ubuntu puede convertirse en super usuario tecleando "sudo -i" en una terminal devera de escribir su contraseña. Tenga cuidado porque usted puede dañar su instalacion si no sabe lo que esta haciendo.

### Comandos en la terminal <span id="sec:Terminal-Commands"></span>

#### Directorio de trabajo

Para encontrar la ruta del directorio actual de trabajo en la terminal tecle:

    pwd

#### Cambiar de directorios

Para subir un nivel en la terminal tecle:

    cd ..

Para subir dos niveles en la terminal tecle:

    cd ../..

Para desplazarse hacia abajo hacia el subdirectorio emc2/configs en la terminal tecle:

    cd emc2/configs

#### Listado e los archivos en un directorio

Para ver una lista de todos los archivos y subdirecciones en la terminal tecle:

    dir

ó

    ls

#### Encontrar un archivo

El comando encontrar puede ser un poco confuso para un usuario nuevo de linux. La sintaxis básica es la siguiente:

    find directorio-inicio parametros acciones

Por ejemplo para encontrar todos los archivos .ini en el directorio de EMC2 primero tiene que usar el comando pwd para ver el directorio. + abra una ventana terminal y escriba.

    pwd

y pwd podría devolver el siguiente resultado:

    /home/joe

Con esta información se pondrá el comando conjunto de esta manera:

    find /home/joe/emc2 -name \*.ini -print

El -name es el nombre del archivo que se busca y -print muestra el resultado en la ventana de terminal. El \\\*.ini indica devolver todos los archivos que tienen la extensión .ini. La diagonal se requiere para escapar los meta caracteres de la consola. sidesea mas informacion al respecto vea las paginas de man.

#### Búsqueda de texto

    grep -irl 'buscar' *

Este comando encuentra todos los archivos que contienen el texto *buscar* en el directorio actual y todos los subdirectorios por debajo de este, sin tener en cuenta el uso de mayusculas. El -i es para ignorar mayusculas y el -r es recursivo (incluir todos los subdirectorios en la búsqueda). La opcion -l retornara una lista de los nombres de archivo, si no se usa -l tambien se obtendra el texto donde fue encontrada la ocurrencia de lo buscado dentro de *buscar*. El \* es un comodín para buscar todos los archivos.

#### Mensaje de arranque

Para ver los mensajes de arranque usar "dmesg" en la ventana de comandos. Para guardar los mensajes de arranque en un archivo use el operador de redirección, de esta manera:

    dmesg > bootmsg.txt

El contenido de este archivo puede ser copiado y pegado en línea para compartir con la gente que le este intentando ayudar a diagnosticar un problema.

Para borrar el mensaje de buffer tecle:

    sudo dmesg -c

Esto puede ser útil de hacer justo antes del arranque de Machinekit, por lo que solo abra un registro de información relacionada con el lanzamiento actual de Machinekit.

Para encontrar la direccion de un puerto paralelo integrado use grep para filtrar la información producida por dmesg.

Después del arranque abrir una terminal y escriba:

    dmesg|grep parport

### Articulos convenientes

#### Iniciador de terminal

Si quiere añadir un iniciador de terminal en la barra del panel en la parte superior de la pantalla normalmente puede hacer clic derecho en el panel en la parte superior de la pantalla y seleccionar "añadir al panel". Seleccione lanzador de aplicación personalizado y agregar. Dele un nombre y use el comando gnome-terminal en la caja de comando.

### Problemas de Hardware

#### Información Hardware

Para encontrar que Hardware está conectado a la placa base en una ventana de terminal tecle.

    lspci -v

#### Resolución del monitor

Durante la instalación ubuntu intentara detectar la configuración del monitor. Si esto no funciona el sistema se instalara con un máximo de resolución 800x600.

Instrucciones para la fijación de este se encuentra aquí:

<https://help.ubuntu.com/community/FixVideoResolutionHowto>

### Paths

Paths relativos

Los phats relativos se basan en el directorio de arranque que contiene el archivo ini. Usar paths relativos puede facilitar la relocalizacion de archivos de configuracion pero requiere una buena comprencion de los especificadores de path linux.

       ./f0        es lo mismo que f0, e.g., un archivo llamado f0 en el directorio de arranque
       ../f1       se refiere a un archivo llamado f1 en el directorio padre
       ../../f2    se refiere a un archivo llamado f2 en el directorio padre del padre
       ../../../f3 etc.

Legal Section
-------------

### Copyright Terms

Copyright (c) 2000-2013 Machinekit.org

Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.1 or any later version published by the Free Software Foundation; with no Invariant Sections, no Front-Cover Texts, and one Back-Cover Text: "This Machinekit Handbook is the product of several authors writing for machinekit.io As you find it to be of value in your work, we invite you to contribute to its revision and growth." A copy of the license is included in the section entitled "GNU Free Documentation License". If you do not find the license you may order a copy from Free Software Foundation, Inc. 59 Temple Place, Suite 330, Boston, MA 02111-1307

### GNU Free Documentation License

GNU Free Documentation License Version 1.1, March 2000

Copyright © 2000 Free Software Foundation, Inc. 59 Temple Place, Suite 330, Boston, MA 02111-1307 USA Everyone is permitted to copy and distribute verbatim copies of this license document, but changing it is not allowed.

**0. PREAMBLE**

The purpose of this License is to make a manual, textbook, or other written document "free" in the sense of freedom: to assure everyone the effective freedom to copy and redistribute it, with or without modifying it, either commercially or noncommercially. Secondarily, this License preserves for the author and publisher a way to get credit for their work, while not being considered responsible for modifications made by others.

This License is a kind of "copyleft", which means that derivative works of the document must themselves be free in the same sense. It complements the GNU General Public License, which is a copyleft license designed for free software.

We have designed this License in order to use it for manuals for free software, because free software needs free documentation: a free program should come with manuals providing the same freedoms that the software does. But this License is not limited to software manuals; it can be used for any textual work, regardless of subject matter or whether it is published as a printed book. We recommend this License principally for works whose purpose is instruction or reference.

**1. APPLICABILITY AND DEFINITIONS**

This License applies to any manual or other work that contains a notice placed by the copyright holder saying it can be distributed under the terms of this License. The "Document", below, refers to any such manual or work. Any member of the public is a licensee, and is addressed as "you".

A "Modified Version" of the Document means any work containing the Document or a portion of it, either copied verbatim, or with modifications and/or translated into another language.

A "Secondary Section" is a named appendix or a front-matter section of the Document that deals exclusively with the relationship of the publishers or authors of the Document to the Document’s overall subject (or to related matters) and contains nothing that could fall directly within that overall subject. (For example, if the Document is in part a textbook of mathematics, a Secondary Section may not explain any mathematics.) The relationship could be a matter of historical connection with the subject or with related matters, or of legal, commercial, philosophical, ethical or political position regarding them.

The "Invariant Sections" are certain Secondary Sections whose titles are designated, as being those of Invariant Sections, in the notice that says that the Document is released under this License.

The "Cover Texts" are certain short passages of text that are listed, as Front-Cover Texts or Back-Cover Texts, in the notice that says that the Document is released under this License.

A "Transparent" copy of the Document means a machine-readable copy, represented in a format whose specification is available to the general public, whose contents can be viewed and edited directly and straightforwardly with generic text editors or (for images composed of pixels) generic paint programs or (for drawings) some widely available drawing editor, and that is suitable for input to text formatters or for automatic translation to a variety of formats suitable for input to text formatters. A copy made in an otherwise Transparent file format whose markup has been designed to thwart or discourage subsequent modification by readers is not Transparent. A copy that is not "Transparent" is called "Opaque".

Examples of suitable formats for Transparent copies include plain ASCII without markup, Texinfo input format, LaTeX input format, SGML or XML using a publicly available DTD, and standard-conforming simple HTML designed for human modification. Opaque formats include PostScript, PDF, proprietary formats that can be read and edited only by proprietary word processors, SGML or XML for which the DTD and/or processing tools are not generally available, and the machine-generated HTML produced by some word processors for output purposes only.

The "Title Page" means, for a printed book, the title page itself, plus such following pages as are needed to hold, legibly, the material this License requires to appear in the title page. For works in formats which do not have any title page as such, "Title Page" means the text near the most prominent appearance of the work’s title, preceding the beginning of the body of the text.

**2. VERBATIM COPYING**

You may copy and distribute the Document in any medium, either commercially or noncommercially, provided that this License, the copyright notices, and the license notice saying this License applies to the Document are reproduced in all copies, and that you add no other conditions whatsoever to those of this License. You may not use technical measures to obstruct or control the reading or further copying of the copies you make or distribute. However, you may accept compensation in exchange for copies. If you distribute a large enough number of copies you must also follow the conditions in section 3.

You may also lend copies, under the same conditions stated above, and you may publicly display copies.

**3. COPYING IN QUANTITY**

If you publish printed copies of the Document numbering more than 100, and the Document’s license notice requires Cover Texts, you must enclose the copies in covers that carry, clearly and legibly, all these Cover Texts: Front-Cover Texts on the front cover, and Back-Cover Texts on the back cover. Both covers must also clearly and legibly identify you as the publisher of these copies. The front cover must present the full title with all words of the title equally prominent and visible. You may add other material on the covers in addition. Copying with changes limited to the covers, as long as they preserve the title of the Document and satisfy these conditions, can be treated as verbatim copying in other respects.

If the required texts for either cover are too voluminous to fit legibly, you should put the first ones listed (as many as fit reasonably) on the actual cover, and continue the rest onto adjacent pages.

If you publish or distribute Opaque copies of the Document numbering more than 100, you must either include a machine-readable Transparent copy along with each Opaque copy, or state in or with each Opaque copy a publicly-accessible computer-network location containing a complete Transparent copy of the Document, free of added material, which the general network-using public has access to download anonymously at no charge using public-standard network protocols. If you use the latter option, you must take reasonably prudent steps, when you begin distribution of Opaque copies in quantity, to ensure that this Transparent copy will remain thus accessible at the stated location until at least one year after the last time you distribute an Opaque copy (directly or through your agents or retailers) of that edition to the public.

It is requested, but not required, that you contact the authors of the Document well before redistributing any large number of copies, to give them a chance to provide you with an updated version of the Document.

**4. MODIFICATIONS**

You may copy and distribute a Modified Version of the Document under the conditions of sections 2 and 3 above, provided that you release the Modified Version under precisely this License, with the Modified Version filling the role of the Document, thus licensing distribution and modification of the Modified Version to whoever possesses a copy of it. In addition, you must do these things in the Modified Version:

1.  Use in the Title Page (and on the covers, if any) a title distinct from that of the Document, and from those of previous versions (which should, if there were any, be listed in the History section of the Document). You may use the same title as a previous version if the original publisher of that version gives permission. B. List on the Title Page, as authors, one or more persons or entities responsible for authorship of the modifications in the Modified Version, together with at least five of the principal authors of the Document (all of its principal authors, if it has less than five). C. State on the Title page the name of the publisher of the Modified Version, as the publisher. D. Preserve all the copyright notices of the Document. E. Add an appropriate copyright notice for your modifications adjacent to the other copyright notices. F. Include, immediately after the copyright notices, a license notice giving the public permission to use the Modified Version under the terms of this License, in the form shown in the Addendum below. G. Preserve in that license notice the full lists of Invariant Sections and required Cover Texts given in the Document’s license notice. H. Include an unaltered copy of this License. I. Preserve the section entitled "History", and its title, and add to it an item stating at least the title, year, new authors, and publisher of the Modified Version as given on the Title Page. If there is no section entitled "History" in the Document, create one stating the title, year, authors, and publisher of the Document as given on its Title Page, then add an item describing the Modified Version as stated in the previous sentence. J. Preserve the network location, if any, given in the Document for public access to a Transparent copy of the Document, and likewise the network locations given in the Document for previous versions it was based on. These may be placed in the "History" section. You may omit a network location for a work that was published at least four years before the Document itself, or if the original publisher of the version it refers to gives permission. K. In any section entitled "Acknowledgements" or "Dedications", preserve the section’s title, and preserve in the section all the substance and tone of each of the contributor acknowledgements and/or dedications given therein. L. Preserve all the Invariant Sections of the Document, unaltered in their text and in their titles. Section numbers or the equivalent are not considered part of the section titles. M. Delete any section entitled "Endorsements". Such a section may not be included in the Modified Version. N. Do not retitle any existing section as "Endorsements" or to conflict in title with any Invariant Section.

If the Modified Version includes new front-matter sections or appendices that qualify as Secondary Sections and contain no material copied from the Document, you may at your option designate some or all of these sections as invariant. To do this, add their titles to the list of Invariant Sections in the Modified Version’s license notice. These titles must be distinct from any other section titles.

You may add a section entitled "Endorsements", provided it contains nothing but endorsements of your Modified Version by various parties—for example, statements of peer review or that the text has been approved by an organization as the authoritative definition of a standard.

You may add a passage of up to five words as a Front-Cover Text, and a passage of up to 25 words as a Back-Cover Text, to the end of the list of Cover Texts in the Modified Version. Only one passage of Front-Cover Text and one of Back-Cover Text may be added by (or through arrangements made by) any one entity. If the Document already includes a cover text for the same cover, previously added by you or by arrangement made by the same entity you are acting on behalf of, you may not add another; but you may replace the old one, on explicit permission from the previous publisher that added the old one.

The author(s) and publisher(s) of the Document do not by this License give permission to use their names for publicity for or to assert or imply endorsement of any Modified Version.

**5. COMBINING DOCUMENTS**

You may combine the Document with other documents released under this License, under the terms defined in section 4 above for modified versions, provided that you include in the combination all of the Invariant Sections of all of the original documents, unmodified, and list them all as Invariant Sections of your combined work in its license notice.

The combined work need only contain one copy of this License, and multiple identical Invariant Sections may be replaced with a single copy. If there are multiple Invariant Sections with the same name but different contents, make the title of each such section unique by adding at the end of it, in parentheses, the name of the original author or publisher of that section if known, or else a unique number. Make the same adjustment to the section titles in the list of Invariant Sections in the license notice of the combined work.

In the combination, you must combine any sections entitled "History" in the various original documents, forming one section entitled "History"; likewise combine any sections entitled "Acknowledgements", and any sections entitled "Dedications". You must delete all sections entitled "Endorsements."

**6. COLLECTIONS OF DOCUMENTS**

You may make a collection consisting of the Document and other documents released under this License, and replace the individual copies of this License in the various documents with a single copy that is included in the collection, provided that you follow the rules of this License for verbatim copying of each of the documents in all other respects.

You may extract a single document from such a collection, and distribute it individually under this License, provided you insert a copy of this License into the extracted document, and follow this License in all other respects regarding verbatim copying of that document.

**7. AGGREGATION WITH INDEPENDENT WORKS**

A compilation of the Document or its derivatives with other separate and independent documents or works, in or on a volume of a storage or distribution medium, does not as a whole count as a Modified Version of the Document, provided no compilation copyright is claimed for the compilation. Such a compilation is called an "aggregate", and this License does not apply to the other self-contained works thus compiled with the Document, on account of their being thus compiled, if they are not themselves derivative works of the Document.

If the Cover Text requirement of section 3 is applicable to these copies of the Document, then if the Document is less than one quarter of the entire aggregate, the Document’s Cover Texts may be placed on covers that surround only the Document within the aggregate. Otherwise they must appear on covers around the whole aggregate.

**8. TRANSLATION**

Translation is considered a kind of modification, so you may distribute translations of the Document under the terms of section 4. Replacing Invariant Sections with translations requires special permission from their copyright holders, but you may include translations of some or all Invariant Sections in addition to the original versions of these Invariant Sections. You may include a translation of this License provided that you also include the original English version of this License. In case of a disagreement between the translation and the original English version of this License, the original English version will prevail.

**9. TERMINATION**

You may not copy, modify, sublicense, or distribute the Document except as expressly provided for under this License. Any other attempt to copy, modify, sublicense or distribute the Document is void, and will automatically terminate your rights under this License. However, parties who have received copies, or rights, from you under this License will not have their licenses terminated so long as such parties remain in full compliance.

**10. FUTURE REVISIONS OF THIS LICENSE**

The Free Software Foundation may publish new, revised versions of the GNU Free Documentation License from time to time. Such new versions will be similar in spirit to the present version, but may differ in detail to address new problems or concerns. See <http://www.gnu.org/copyleft/>.

Each version of the License is given a distinguishing version number. If the Document specifies that a particular numbered version of this License "or any later version" applies to it, you have the option of following the terms and conditions either of that specified version or of any later version that has been published (not as a draft) by the Free Software Foundation. If the Document does not specify a version number of this License, you may choose any version ever published (not as a draft) by the Free Software Foundation.

**ADDENDUM**: How to use this License for your documents

To use this License in a document you have written, include a copy of the License in the document and put the following copyright and license notices just after the title page:

Copyright (c) YEAR YOUR NAME. Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.1 or any later version published by the Free Software Foundation; with the Invariant Sections being LIST THEIR TITLES, with the Front-Cover Texts being LIST, and with the Back-Cover Texts being LIST. A copy of the license is included in the section entitled "GNU Free Documentation License".

If you have no Invariant Sections, write "with no Invariant Sections" instead of saying which ones are invariant. If you have no Front-Cover Texts, write "no Front-Cover Texts" instead of "Front-Cover Texts being LIST"; likewise for Back-Cover Texts.

If your document contains nontrivial examples of program code, we recommend releasing these examples in parallel under your choice of free software license, such as the GNU General Public License, to permit their use in free software.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


