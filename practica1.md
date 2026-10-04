

# Practica 1: Navegación pseudoaleatoria con FSM en una aspiradora de gama baja

## Objetivo

El objetivo de esta practica es programar la lógica de control de un robot aspiradora de baja gama, esto quiere decir que no esta localizado en el plano y su exploración se hará pseudoaleatoriamente.

## Problema planteado

El problema que plantea la practica es programar un robot aspiradora sin la localización en el plano, para ello es necesario explorar el mapa moviéndose de forma pseudoaleatoria hasta explorar el mapa completamente.

## Solución propuesta
La solución propuesta para realizar este ejercicio es que el robot siga la siguiente secuencia:

1. El robot avanza en espiral hasta acercarse un obstáculo.  
2. El robot retrocede al detectar un obstáculo durante 1 segundos.    
3. El robot gira un angulo generado de manera pseudoaleatoria.  
4. El robot avanza en linea recta si no hay espacio.  
5. Si hubiera espacio el robot volvería a girar en espiral.  

## Implementación

Para implementar la siguiente secuencia se ha utilizado una maquina de estados, en cada estado se ha definido la lógica de programación y que condición se tiene que cumplir para pasar al siguiente estado.

## Demostración

[demo_practica1_1.webm](https://github.com/user-attachments/assets/6dda0386-936f-43c4-a616-b6483b090156)

[demo_practica1_2.webm](https://github.com/user-attachments/assets/d5e0a6ba-d02f-4f94-a0d7-fc43bd7c1853)

[demo_practica1_3.webm](https://github.com/user-attachments/assets/429e612f-19b5-413d-ad23-339ae3210dbc)

## Problemas de la practica y soluciones

Durante el desarrollo de la practica han surgido diversos problemas, para los cuales se ha podido encontrar una solución:

1. Para usar el lidar como detector de obstáculos, primero implemente una función que me devolvía la distancia del valor asociado al indice 90 en el array proporcionado por el lidar, el problema que había es que solo se detectaba el frente y si daba el robot en un costado se quedaba atascado, para solucionar este problema decidí obtener el valor mínimo y compararlo con un valor constante, si el valor mínimo es menor a 0,25 se considera que el robot ha chocado con algo y por tanto se tiene que ejecutar la secuencia de recuperación.  

2. Para aumentar la zona por donde ha pasado el robot se ha decidido cambiar el sistema de avance en una linea recta a una espiral, para poder cubrir mas superficie, después para que el robot pueda salir de alguna zona donde se quede atascado, se ha decidido que el robot avance en linea recta si no hay espacio y si hay una zona grande el robot gira en espiral. 

3. Un problema que ha sido complicado solucionar ha sido el patrón que el robot debe seguir, se han hecho pruebas con un algoritmo que hace que el robot avance y cuando se choca gira y otro que hace lo mismo pero en vez de ir en linea recta gira en espiral, la solución ha sido unir los dos patrones y activar un patrón u otro dependiendo del caso que se de en ese momento. 

## Conclusiones
En conclusion en este trabajo se ha programado un robot aspirador de gama baja, mejorando el algoritmo añadiendo funcionalidades, hasta encontrar unas constantes y unos algoritmos capaces de limpiar la mayor parte de la habitación posible. En la demo grabada el robot llega hasta el 54,76%, pero por probabilidad el robot deberia limpiar mas zona de la habitacion pero no se puede saber el tiempo que tardaria en completar la misión.
