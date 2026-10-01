# Practica 1: Robot aspiradora de baja gama

## Objetivo

El objetivo de esta practica es programar la logica de control de un robot aspiradora de baja gama, esto quiere decir que no esta localcizado en el plano y su exploracion sera pseudoaleatoria.

## Problema planteado

El problema que plantea la practica es programar un robot aspiradora sin la localizacion en el plano, para ello es necesario explorar el mapa moviendose de forma pseudoaleatoria hasta explorar el mapa completamente.

## Solucion propuesta
La solucion propuesta para realizar este ejercicio es que el robot siga la siguiente secuencia:

1º El robot avanza en espiral hasta chocarse con un obstaculo.  
2º El robot retrocede al detectar un obstaculo durante 2 segundos.    
3º El robot gira un angulo generado de manera pseudoaleatoria.  
4º El robot vuelve a girar en espiral.  
5º Cada 5 obstaculos el robot va en linea recta para salir de una situacion en la que haya podido entrar en bucle.  

## Implementación

Para implementar la siguiente secuencia se ha utilizado una maquina de estados, en cada estado se ha definido la logica de programacion y que condicion se tiene que cumplir para pasar al siguiente estado.

## Demostracion



## Problemas de la practica y soluciones

Durante el desarrollo de la practica han surgido diversos problemas, para los cuales se ha podido encontrar una solucion:

1º Para usar el lidar como detector de obstaculos, primero implemente una funcion que me devolvia la distancia del valor asociado al indice 90 en el array proporcionado por el lidar, el problema que habia es que solo se detectaba el frente y si daba el robot en un costado se quedaba atascado, para solucionar este problema decidi obtener el valor minimo y compararlo con un valor constante, si el valor minimo es menor a 0,25 se considera que el robot ha chocado con algo y por tanto se tiene que ejecutar la secuencia de recuperacion.  

2º Para aumentar la zona por donde ha pasado el robot se ha decidido cambiar el sistema de avance de uno recto a una espiral, para poder cubrir mas superficie, despues para que el robot pueda salir de alguna zonda donde se quede atascado, se ha decidido que cada 5 obstaculos detectados el robot avance recto hasta el siguiente obstaculo. 
3º 

## Conclusiones
