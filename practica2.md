# Practica 2 Control Visual con PID de un Fórmula1

## Objetivo
El objetivo de esta practica es programar un control de un coche de formula 1 con un pid, el coche recibe la imagen de la cámara y gracias a la librería opencv podemos obtener el punto medio de la linea roja que el coche tiene que seguir.

## Problema planteado
El problema planteado es que el coche tiene que seguir una linea roja pintada en el suelo, recorriendo el circuito en el menor tiempo posible. En la practica se proporcionan varios circuitos, el coche tiene que poder terminar todos los circuitos de acuerdo al sistema de dirección, en esta practica se ha desarrollado un código para cada sistema de dirección.

## Solución propuesta
Para solucionar el problema se ha implementado un control pid que corrige el error que hay desde el valor deseado (punto medio de la imagen) y el valor obtenido (valor que obtenemos del punto medio de la linea roja). 

Control normal:
Para este modelo se han implementado dos pid, uno para la dirección (w) y otro para la velocidad lineal (v), para configurar los dos pid, se ha ido probando el coche en el circuito principal hasta conseguir un control que haga que el coche no oscile (modificando la constante proporcional (kp), y cambiando la constante derivativa (kd) para aumentar la amortiguación), pero manteniendo la constante proporcional (kp), de manera que la dirección no sea demasiado brusca y se salga de la linea.

Control Ackermann:
Para este modelo se ha implementado otro sistema, en este caso solo se ha programado un solo pid para la dirección (w), y para reducir la velocidad lineal (v) en las rectas se ha aplicado la siguiente formula:   
velocidad lineal = velocidad maxima – k_v * e  
siendo el error(e) = punto de la imagen – punto medio de la imagen

Así se simplifica la regulación del pid y no es necesario estar configurando dos pid, ya que en Ackermann se complica mucho ese proceso de configuración de constantes. Al igual que en el control normal se ha ido probando el coche en el circuito principal hasta conseguir un control haga que el coche no oscile (modificando la constante proporcional (kp), y cambiando la constante derivativa (kd) para aumentar la amortiguación), pero manteniendo la constante proporcional (kp), de manera que la dirección no sea demasiado brusca y se salga de la linea.

## Implementación
Para implementar este programa se ha utilizado lo siguiente:
* Se ha utilizado la librería opencv, que a través de un filtro HSV nos proporciona una mascara que representa al imagen de esta manera
  * Si el color detectado esta en un rango determinado (en este caso interesa filtrar el color rojo que corresponde con la linea a seguir) la imagen muestra un pixel con el valor 255.
  * Si no coincide con ese color el pixel tiene el valor 0, teniendo esta matriz que coincide con la imagen obtenida, podemos obtener el punto medio de la linea a seguir. 

* Después de obtener el punto de la imagen que va a servir para calcular el error de la imagen respecto del punto medio de la imagen, calculamos los valores que nos arroja el pid respecto a ese error:
  * Se multiplica el error por la constante de proporcionalidad (kp), si aumenta la constante, mayor sera la corrección aplicada, puede producir oscilaciones.
  * Se multiplica el error por la constante de derivación (kd) y restando el error actual al error previo (aplicar una derivada en cada momento de tiempo), se amortigua la salida reduciendo oscilaciones y aumentando el amortiguamiento.
  * Se multiplica el error por la constante de integración (ki) y sumando el error a lo largo del tiempo, aumenta la corrección.

* Por ultimo se utiliza la salida del pid para controlar la dirección y la velocidad lineal, en el caso de la velocidad lineal se han usado dos mecanismos:
  * Para el control normal se han utilizado dos pid, uno controla la dirección del coche y con el otro pid se obtiene la salida y se resta a la velocidad máxima del coche, esto sirve para que el coche reduzca su velocidad en las curvas y así poder tomar las curvas del circuito sin descentrarse de la linea roja y tener que ejecutar la recuperación que hace que el coche pierda tiempo en la vuelta al circuito.
  * Para el control Ackermann, se ha utilizado un solo pid para controlar la dirección y un factor (K_v) que multiplica al error y se resta a la velocidad lineal máxima para calcular la velocidad del coche en las curvas. En este caso se ha cambiado a un factor que depende de la velocidad linear máxima y mínima para facilitar la configuración del pid de la dirección, ya que en el control Ackermann el ajuste de dos pid dificulta la configuración de las constantes.

## Demostración
* Control normal:  
[![Simple Circuit]()](https://youtube.com/shorts/8gnfV_YpA_g?feature=share)  
[![Montmelo Circuit]()](https://youtube.com/shorts/NaJGEC5E0pM?feature=share)  
[![Nurburgrind Circuit]()](https://youtube.com/shorts/uHb-ZOkOMrI?feature=share)  
[![Montreal Circuit]()](https://youtube.com/shorts/2ynZYwBt_B4?feature=share) 

* Control Ackermann:  
[![Simple Circuit]()](https://youtu.be/nVaIoJ5mg80)    

## Problemas de la practica y soluciones
En esta practica han surgido varios problemas, especialmente relacionados con el ajuste del pid y con el mecanismo que recupera la trayectoria del coche en el caso de que se pierda la linea:
* Problema del pid:
	* Para el control normal:  
Para ajustar el pid se han puesto todas las constantes a 0 y se ha ido modificando la constante proporcional (kp), hasta alcanzar un controlador para el cual el coche complete el circuito en un tiempo razonable y con el menor numero de oscilaciones posibles, luego se ha implementado un segundo controlador que nos sirve para reducir la velocidad del coche en las curvas, lo que hace menos probable que el coche pierda la linea en las curvas. La salida de este controlador se resta a la velocidad máxima y así garantizar que nunca se supere una velocidad determinada. Por ultimo se ha ido aumentando la velocidad y se han ido modificando las demás constantes hasta alcanzar un controlador que puede completar el “simple circuit” en menos de 1 minuto con el control normal.

	* Para el control Ackermann:  
Se ha intentado seguir la misma metodología que para el control normal, pero por simplificar se ha tenido que aplicar la siguiente formula:  
```text
v_lineal = v_max - |K_v * ω|
```
Se ha decidido cambiar el pid lineal por esta formula para simplificar la regulación del pid de la dirección ya que para el modelo Ackermann se complica mucho la regulación de dos pid. Incluso en este 	caso no se ha conseguido un control muy preciso y tarda unos 220 segundos en completar el “simple circuit”, que es bastante mas que en control normal, pero no se ha llegado a un consenso entre no 		oscilar en las rectas y girar en las curvas con la suficiente fuerza para que no se salga de la linea. De esta forma el sistema de recuperación se ejecuta el menor numero de veces posible ya 	que ralentiza demasiado el coche.

* Problema del sistema de recuperación:
	* Para el control normal: se ha implementado una condición que se ejecuta cada vez que el valor del punto detectado es 0, en ese caso se considera que no se esta detectando la linea por tanto, durante un tiempo determinado el coche retrocede y gira en el sentido contrario teniendo en cuenta en que lado de la imagen la linea roja ha desaparecido.

	* Para el control Ackermann: se ha implementado un sistema parecido al implementado en el control normal, pero en este caso no se ha decidido retroceder, en su lugar se ha decidido que el coche se frene y gire lentamente otra vez hacia la linea roja.

## Conclusiones
En conclusión en este proyecto se ha programado un formula 1 en la plataforma unibotics, para la programación de este coche se ha utilizado un pid que dirige el coche en el circuito. Ademas se han probado el control normal para los cuatro circuitos disponibles y el control Ackermann en el circuito simple y se ha observado que para el modelo normal no se ha requerido tantos ajustes como para el modelo Ackermann, el cual ha requerido cambiar el pid para la velocidad lineal por un sistema mas sencillo para simplificar el ajuste, no consiguiendo en Ackermann un resultado tan optimo como en el modelo normal.
