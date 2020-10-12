![](https://github.com/Democrito/Motor/blob/main/Motor_DC/img/esquema.png)

Control de Motor DC con encoder incremental para FPGA (Icestudio). A través de cualquier serial se le envía la posición absoluta (0..65535). Tiene una adaptación particular de control PID.

Proyecto funcional 100% y es estable pero tiene ciertos comportamiento que quiero afinar y queda pendiente para un futuro.

Falta documentar; mientras tanto voy poniendo información vital para al menos poder montar y funcionar.

La Alhambra II funciona con 3,3V pero también tolera sin problemas entradas de 5V. Para este proyecto no se requiere adaptadores de tensión en sus pines. Quizás el puente en H lo requiera, pero he probado dos modelos de puente en H y no he tenido ningún problema en usar 3,3V.

Para hacer ajustes se hace a través de las constantes **KP**, **fout**, **brake** y **us**. Siempre hay que utilizar números enteros positivos. Estas constantes son muy fáciles de configurar porque el rango es muy pequeño.

**KP:** Constante proporcional. Casi siempre va a ser **1** y es como está por defecto. Si pones **2** harás que el posicionamiento sea más rápido. Si es mayor de **2** suele carecer de sentido. Recomiendo dejarlo con el valor **1** y cuando ya lo tengas bien ajustado entonces puedes probar a poner el valor de **2** y ver el comportamiento.

**fout:** (fade out) Se trata de un factor que *sólo se aplica justo cuando se acerca a la meta designada* sin embargo es muy importante. Hace que el posicionamiento a la meta sea suave, decrementando progresivamente la velocidad. Aquí entra en juego una danza entre el control integral y derivativa, es decir, si cuando se aproxima a la meta va con demasiada velocidad lo que hace es frenar, y si por cualquier circunstancia se frenase en exceso lo que haría sería incrementar la fuerza del motor. Los valores recomendados estarían entre 2 y 4 (en rara ocasión hasta 8).

**brake:** Es el freno general tal y como lo haría un control PID convencional. Cuando las inercias son fuertes hemos de subir este valor. Valores posibles sería entre 4 y 16.

**us:** (micro-segundos) Es el tiempo de muestreo. Tal y como está debería de funcionar bien con encoders de 90 pulsos por revolución (ppr) o más resolución. Para encoder de muy baja resolución hay que aumentar este tiempo. Por ejemplo, para 94 ppr lo tengo a 4000 us y para 4 ppr puse 8000 us. También se puede deducir que para encoder de media-alta resolución el tiempo de muestro podría ser más bajo.

Como puedes comprobar el ratio de los valores son muy estrechos y fácilmente deducibles. Cuando hago los ajustes de *fout* y *brake* suelo utilizar *potencias de 2* (2, 4, 8, 16...) y sólo si es necesario entonces (por ejemplo, entre 2 y 8) busco si un valor intermedio mejora el funcionamiento (3, 4, 5, 6 y 7).





