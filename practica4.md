# Practica 4 Navegación global con algoritmo GPP, plan como recurso

## Objetivo
El objetivo de esta practica es guiar un coche por un mapa de manera autónoma usando un algoritmo GPP (Gradient path planning), para ello el usuario selecciona un waypoint en el mapa y el coche debe de ir de manera autónoma y sin chocarse con ningún elemento del mapa hasta ese objetivo, después se tiene que volver a pedir otro waypoint.

## Problema planteado
En esta practica se plantea el problema de calcular un array de numpy correspondiente con el mapa, que contenga valores desde 0 en el destino, hasta n valor que alcance el algoritmo cuando alcance al vehículo con un pequeño offset, ademas se tiene que generar otro mapa que se va a sumar al anterior que añada un coste a las zonas cercanas a paredes u obstáculos, de esta manera se van a priorizar las zonas con menor valor y por tanto las mas alejadas de los obstáculos.

## Solución propuesta
Como solución se propone lo siguiente:
1. Generar dos mapas, un mapa va a tener valores que van a ir creciendo desde el destino hasta el vehiculo, para generar este mapa se va a utilizar el algoritmo BFS, que expande nodos hasta llegar al objetivo, por cada nodo (celda de numpy correspondiente con el mapa), que se expanda se suma 0.25, por tanto se genera un gradiente que guiara el coche, en este caso los obstáculos valen directamente 0 y se ignoran, ya que el destino tendrá el valor 0.25. Otro mapa va a contener el coste de acercarse a un obstáculo, para ello se va generar un mapa con BFS, que contiene el contorno de todos los obstáculos del mapa, después se aplica BFS con una profundidad determinada BFS a cada nodo (correspondiente a cada pixel que pertenece al contorno de un obstáculo o pared), de esta manera generamos un mapa que añade un coste a acercarse a los obstáculos, es importante una profundidad para que la repetida ejecución del algoritmo no sobrecargue el equipo utilizado. Por ultimo estos dos mapas se suman obteniendo un mapa con un gradiente y valores mas elevados en las zonas cercanas a obstáculos y paredes que al centro de cada calle del mapa.

Esquema aproximado acerca de la generación de estos mapas:
<img width="1764" height="930" alt="imagen" src="https://github.com/user-attachments/assets/dfab1c1c-549c-4fd4-ab0c-08e8eaeff9af" />

2. Después de generar el mapa mencionado anteriormente, ya se puede guiar el vehiculo hasta el destino, para ello lo que se hace es seleccionar en un rango el punto con el menor valor, de esta manera podemos generar un vector atractivo desde el vehiculo hasta ese punto de menor valor, después de llegar a ese punto se selecciona otro punto y así hasta que se llega al waypoint seleccionado, el coche evita obstáculos ya que cuanto mas cerca de las paredes mas valor tiene ese punto por tanto el coche siempre tiende a seleccionar los puntos que mas alejados estén de las paredes u obstáculos.

## Demostración
[![Demo Practica 4]()](https://youtu.be/ANnyiZcPF90)

## Problemas de la practica y soluciones
1. Para representar el mapa en el WebGUI, se necesitaba normalizar los datos para que estuvieran entre 0 y 255, al principio solo se imprimían todos los valores pero cuando imprimes un valor mayor a 255 se ve de color blanco sin ningún, bajando la calidad del mapa al dejar de dar información acerca de la expansión del gradiente en el mapa, para ello lo que se hace es normalizar los datos según se expande el gradiente y el mapa se va adaptando según el numero de nodos expandidos.

2. Otro problema que ha surgido ha sido modificar el campo de costes de los obstáculos, ya que al principio, tenia mucha profundidad y eso provocaba que el coche no pudiera pasar por pasillos estrechos, por eso el coche tiende a ir pegado a la pared.

3. Por ultimo, se ha necesitado buscar el punto de menor coste en un rango mayor a 1, ya que si se intenta buscar un punto inmediatamente continuo al coche provocaba oscilaciones y un movimiento muy brusco, pero al aumentar el radio, también el coche buscaba en su primera iteración los valores de menor coste y los valores 0 que quedaban de no expandir mas el mapa provocando que el coche se fuera del campo, para ello se ha añadido un offset que expande un poco mas el mapa y ahora el coche no busca en zonas inicializadas a 0 del mapa.

## Conclusiones
En conclusión en esta practica se realizado la integración y programación del algoritmo gradient path planning aplicado a la navegación global por un mapa de una ciudad, de un coche autónomo en la plataforma unibotics, en este proyecto se ha conseguido un resultado correcto, ya que el coche puede desplazarse de un waypoint a otro dentro del mapa a una velocidad aceptable y no acercándose demasiado a paredes o esquinas que puedan provocar que el vehiculo pueda chocarse.
