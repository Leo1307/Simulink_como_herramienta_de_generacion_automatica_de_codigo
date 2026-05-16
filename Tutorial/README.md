# Tutorial

## Paso 1: Abrir una nueva "Blank Model"

Entre a matlab abriendo una terminal y usando el comando 
```bash
matlab
```
Haga click en simulink y luego en "Blank Model"

![Blank Model](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/9.png)

## Paso 2: Añade los bloques a usar

Puede añadir los bloques de dos maneras.
1. Desde "Library"
2. Dando doble click y escribiendo el nombre del bloque

Los bloques a usar son los siguientes:

* 1 Clock
* 2 Constant
* 4 switch
* 3 Cast
* 1 AND
* 3 Digital output
* 1 Scope
* 1 Math function (Mod)

![Constant](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Constant.png)

![switch](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Switch.png)

![AND](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/AND.png)

![Cast](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Cast%20to%20Single.png)

![Digital output y Scope](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Digital%20output%20y%20scope.png)

![Mod y Clock](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Mob%20y%20Clock.png)
  
## Paso 3: Configuración y conección de bloques

Una vez teniendo todos los bloques procedemos a configurar cada uno

* 1 Clock = Se queda igual
* 2 Constant = Un bloque tiene que tener un valor de 1 y el otro en 23
* 4 switch = 3 tienen que estar en:
    Criteria for passing first input: u2 >= Threshold
    con:
    - Threshold: 10
    - Threshold: 10
    - Threshold: 13
    - 
    Y el ultimo switch debe estas en Criteria for passing first input: u2 > Threshold con Threshold: 13
* 3 Cast = Deben de estar en Output data type:  single
* 1 AND = Se queda igual
* 3 Digital output = Los pines debe de estar en
    - 11
    - 12
    - 13
* 1 Scope = entre a scope, luego otra vez a scope -> setting -> number of inputs ports: 3
* 1 Math function (Mod) = Math function debe de estar en function: mod

Finalmente conecte de la siguiente manera

![Circuito](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Circuito_Tutorial.png)

Nota: El orden de switches es el mismo de arriba hacia a bajo como se menciona en la configuración, es decir, los dos de abajo son de "Threshold: 13" y especialmente el ultimo es el de "u2 > Threshold".

## Paso 3: Simulación
  El objetivo de esta simulación es el reproducir el conportamiento de un semaforo. Por lo cual si introducimos un Stop time de 40 seg y conectando todo tal cual como se ve en la imagen, al darle a "RUN" obtendremos casi 2 ciclos de repetición, donde se observa como al inicio esta presente el color verde por 10 seg, luego el amarillo por 3 seg, el rojo por 10 seg y se repite hasta terminar la simulación.

![Stop time](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Stop%20time.png)

![Comportamiento](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Comportamiento.png)

## Paso 4: Arduino

Finalmente, conectamos el Arduino y realizamos la misma configuración vista en clase.

Presionando model settings, se colocan las siguientes configuraciones:

![Model Setting](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/10.png)

Vaya a "Hardware implementation" y en "Harware board" seleccione su arduino, tambien tenemos que aasegurarnos de que el puerto donde esta conectado el arduino este seleccionado.

![Hardware implementation](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/11.png)

En solver coloque lo siguiente

![Solver](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/12.png)

Para observar el código generado haga click en embedded coder en apps

![Coder](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Coder.png)

Para generar y cargar el codigo en arduino vaya a Harware y presione "Build, Deploy & Start"

![Build, Deploy & Start](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Build%2C%20Deploy%20%26%20Start.png)


  
