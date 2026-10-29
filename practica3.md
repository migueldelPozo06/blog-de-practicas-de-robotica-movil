# Practica 3 Navegación local esquivando obstáculos con algoritmo VFF

## Objetivo
EL objetivo de esta practica es navegar con un coche de formula 1 por un circuito utilizando navegación local y el algoritmo VFF para esquivar los obstáculos y acercarse al waypoint siguiente.

## Problema planteado
En esta practica se plantea el problema de implementar en python una aproximación del algoritmo VFF (Virtual Force Field), que consiste en tener un vector atractivo al waypoint siguiente y un vector repulsivo desde el obstáculo a esquivar, posteriormente se suman estos vectores obteniendo un vector resultante que el coche debe seguir, ademas se tienen que eliminar los mínimos locales que presenta este algoritmo, que consiste en recuperarse del caso en el que los dos vectores tienen mismo modulo y mismo angulo con diferencia pi radianes resultando en que el vector resultante vale 0 y quedándose el coche parado en el sitio.

* Esquema del algoritmo VFF
<img width="852" height="493" alt="imagen" src="https://github.com/user-attachments/assets/282d9b9c-43a3-4f34-a7b0-ba1bdaa12862" />


## Solución propuesta
Como solución se ha propuesto obtener el vector atractivo al siguiente waypoint, obteniendo las coordenadas relativas desde el coche (0,0) hasta el waypoint, una vez se tiene el vector atractivo, se obtiene el vector repulsivo sumando todos los vectores repulsivos que se obtienen del láser, así obtenemos el vector repulsivo de todos los obstáculos y paredes que hay en ese momento. Para controlar el coche que solo recibe velocidad lineal (v) y angular (w), se tienen que hacer los siguientes cálculos:

* Respecto a la velocidad lineal: Se establece una velocidad lineal máxima y se multiplica por un factor que depende del angulo relativo entre el vector resultante y la recta media frontal del coche.

* Respecto a la velocidad angular: Se multiplica el angulo relativo  entre el vector resultante y la recta media frontal del coche que coincide con el error que queremos corregir, y se multiplica por una constante K_V, gracias a este mecanismo el coche siempre tiende a minimizar el error.

Para evitar mínimos locales y choques con las paredes, se establecen ciertas velocidades mínimas que el coche debe cumplir en ciertos casos, estos casos van desde estar a cierta distancia de un obstáculo o estar demasiado cerca de una pared porque el waypoint esta muy lejos.

## Demostración
[![Demo Practica 3]()](https://youtu.be/MP38WjewwBU)

## Problemas de la practica y soluciones
En esta practica han surgido varios problemas que se han ido solucionando gradualmente:
* Uno de los problemas mas complicados de resolver ha sido el de los mínimos locales, para solucionarlo se ha necesitado una función que obtiene la distancia frontal mínima, luego se han definido tres constantes, con tres velocidades, en cada ciclo se comprueba si la distancia al obstáculo esta en alguna de esas tres constantes se reduce la velocidad, están colocados de manera gradual de tal manera que a menor distancia, menor velocidad, esto soluciona el problema de los mínimos locales ya que se presentan en cercanía a un obstáculo, por tanto al acercarse se establece una velocidad lineal que no depende del vector resultante, lo que rompe el mínimo local esquivando el obstáculo y siguiendo el camino propuesto.

* Otro problema, a pesar de que no es muy común, es que el coche no frena lo suficiente y se choca con un obstáculo, para ello se ha añadido una recuperación, que hace que el coche retroceda devolviendo el control al bucle principal y esquivando el obstáculo.

* Por ultimo un problema existente es que hay waypoints que puede que estén mas lejos que otros, cuando esta cerca no hay problemas, pero cuando el waypoint es lejano, el coche tiende a chocarse con las paredes, por tanto se ha añadido un sistema que reduce la velocidad y controla al coche, con el coste de mantener el coche mas lento de los debido y muy pegado a la pared pero evita que se choque con obstáculos o con la pared con ese coste añadido de perder un poco de tiempo en completar el circuito.

## Conclusiones
En conclusión, en este proyecto se ha desarrollado un coche de formula 1 que navega por el circuito con navegación local en la plataforma unibotics, al contrario que en la practica 2 que se utilizaba la cámara para obtener la posición de la linea roja y seguirla con un pid, en este caso solo se ha utilizado el lidar y el waypoint siguiente que el coche tiene que acercarse, proporcionado por la librería WebGUI. En este proyecto se ha conseguido un buen resultado, consiguiendo un tiempo de aproximadamente 1 minuto en recorrer el circuito simple, no consiguiendo reducir unas pequeñas oscilaciones que se presentan sobretodo en rectas.
