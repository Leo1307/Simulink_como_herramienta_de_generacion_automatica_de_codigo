# Instalación de extensiones

## Paso 1: entrar a "Add-Ons"

![Add-Ons](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/0.png)

## Paso 2: Buscar "Simulink Coder"

Una vez encontrado, seleccionelo y presionar "install" para descargar el Simulink Coder.

![Simulink Coder](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Simulink%20coder.png)

En caso de aparecer algun error ejecute lo siguiente en la terminal

```bash
sudo chown -R $USER:$USER /usr/local/MATLAB/R2026a

chmod -R u+rw /usr/local/MATLAB/R2026a
```
Y vuelva a intalar simulink coder

## Paso 3: Buscar "Simulink Support Package for Arduino Hardware"

Una vez encontrado, seleccionelo y presionar "install" para descargar el Simulink Support Package for Arduino Hardware.

![Simulink Support Package for Arduino Hardware](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/1.png)

## Paso 4: Configuración y verificación de compatibilidad con arduino

Salga de Add-Ons y vaya a la terminal principal de matlab, a un costado a la izquierda esta un simbolo de Add-Ons, presione allí. En installed, busque Simulink Support Package for Arduino Hardware, presione click derecho y luego setup

![configuración de Simulink Support Package for Arduino Hardware](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/3.png)

Conecte su Arduino y presione "Next" hasta llegar a la pestsña de test(Matlab reconoce automaticamente el puerto de su arduino).

![Arduino](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/4.png) 

Presione test conection, este comprabara que la comunicación entre matlab y el arduino sean correctas.

En este punto puede suceder una de dos cosas.

1. ❌ Error: Download failed / Permission denied / cannot open port
Este error ocurre cuando Linux no tiene permisos para comunicarse con la placa Arduino mediante el puerto serial.
Normalmente sucede aunque el código compile correctamente.

Solución 1
    Verificar que el sistema detecte el Arduino

    Conectar la placa Arduino y ejecutar:
    ```bash
    bashls /dev/ttyACM*
    ```
    Resultado esperado
    Debe aparecer algo similar a:
    ```
    /dev/ttyACM0
    ```
    Si no aparece las posibles causas son:

    * Cable USB defectuoso
    * Cable solo de carga 
    * Puerto USB con problemas
    * La placa no está conectada correctamente

Solución 2

    Conceder permisos del usuario

    Ejecutar:

    ```bash
    bashsudo usermod -aG dialout $USER
    ```

    Luego reiniciar sesión o reiniciar el sistema completo.

    Si ejecuta

    ```bash
    bashid
    ```
    Debe aparecer:

    dialout

2. Paso ambas pruebas

![Test completo](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/7.png) 

## Paso 5: Simulink

Abra simulink y un nuevo blank model

![Simulink](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/8.png) 
![Blank Model](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/9.png)

Presione Model Setting

![Model Setting](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/10.png)

Vaya a "Hardware implementation" y en "Harware board" seleccione su arduino, tambien tenemos que aasegurarnos de que el puerto donde esta conectado el arduino este seleccionado.

![Hardware implementation](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/11.png)

En solver coloque lo siguiente

![Solver](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/12.png)

Presione Library Browser, aqui se encuentran las principales herramientas encargadas de la comunicación con arduino.

![Library](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/13.png)

Tambien puede dar doble click en la pantalla grande y buscar cada uno de los bloques a continuación (conectelos como se muestra en la imagen).

![Circuito](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Circuito.png)

Los bloques se llaman
* Clock
* Matlab funtion
* Digital output
* Terminator
* Cast

En el matlab funtion de una salida se realiza una traducción de un mensaje a codigo morse con un led, en el de dos salidas se usan dos leds (pegue los codigos en cada uno).

Una salida
```bash
function y = morse_arduino(t)
% Genera señal Morse en tiempo real para Arduino.
% Mensaje fijo (cambiable en el código).
persistent message unit
if isempty(message)
    message = 'LA AREPA ES DE VENEZUELA'; % <-- Cambia aquí tu frase
    unit = 0.5;                            % duración de un punto (segundos)
end
y = morse_signal(t, message, unit);
end

function y = morse_signal(t, msg, unit)
% Calcula si el LED debe estar encendido (1) en el instante t
% recorriendo el mensaje carácter por carácter.
% Si t está más allá del mensaje completo, devuelve 0.

% Tabla de códigos Morse (A-Z, 0-9) con tamaño fijo
CHARS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
CODES = ['.-   '; '-... '; '-.-. '; '-..  '; '.    '; '..-. '; ...
         '--.  '; '.... '; '..   '; '.--- '; '-.-  '; '.-.. '; ...
         '--   '; '-.   '; '---  '; '.--. '; '--.- '; '.-.  '; ...
         '...  '; '-    '; '..-  '; '...- '; '.--  '; '-..- '; ...
         '-.-- '; '--.. '; '-----'; '.----'; '..---'; '...--'; ...
         '....-'; '.....'; '-....'; '--...'; '---..' ; '----.'];
LENS  = [2 4 4 3 1 4 3 4 2 4 3 4 2 2 3 4 4 3 3 1 3 4 3 4 4 4 ...
         5 5 5 5 5 5 5 5 5 5];

t_elapsed = 0.0;
n = length(msg);

for i = 1:n
    ch = msg(i);
    if ch == ' '
        % Espacio entre palabras: 7 unidades de silencio
        t_elapsed = t_elapsed + 7 * unit;
    else
        % Buscar código Morse del carácter
        idx = 0;
        for k = 1:36
            if CHARS(k) == ch
                idx = k;
                break;
            end
        end
        if idx == 0
            continue; % carácter no soportado, ignorar
        end
        code = CODES(idx, :);
        lenCode = LENS(idx);
        
        % Recorrer cada símbolo del código (punto o raya)
        for s = 1:lenCode
            if code(s) == '.'
                onDur = 1 * unit;
            else
                onDur = 3 * unit;
            end
            % ¿Está t dentro de este segmento encendido?
            if t >= t_elapsed && t < t_elapsed + onDur
                y = 1;
                return;
            end
            t_elapsed = t_elapsed + onDur;
            
            % Apagado entre símbolos (si no es el último)
            if s < lenCode
                offDur = 1 * unit;
                t_elapsed = t_elapsed + offDur;
            end
        end
        
        % Espacio entre letras (3 unidades) si la siguiente letra no es espacio
        if i < n && msg(i+1) ~= ' '
            t_elapsed = t_elapsed + 3 * unit;
        end
    end
end

% Si llegamos aquí, t está más allá del mensaje
y = 0;
end
```

Dos salidas
```bash
function [y_dot, y_dash] = morse_arduino(t)
% Genera dos señales Morse en tiempo real para Arduino.
% - y_dot: salida para el LED de puntos   (pin 13)
% - y_dash: salida para el LED de rayas   (pin 12)

persistent message unit
if isempty(message)
    message = 'LA AREPA ES DE VENEZUELA'; % <-- Cambia aquí tu frase
    unit = 0.5;                            % duración de un punto (segundos)
end
[y_dot, y_dash] = morse_signal_doble(t, message, unit);
end

function [y_dot, y_dash] = morse_signal_doble(t, msg, unit)
% Recorre el mensaje y devuelve las dos señales según el tipo de símbolo.
%   y_dot: 1 si t está en el encendido de un punto, 0 en caso contrario.
%   y_dash: 1 si t está en el encendido de una raya, 0 en caso contrario.

% Tablas de códigos Morse (A‑Z, 0‑9) con dimensiones fijas
CHARS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
CODES = ['.-   '; '-... '; '-.-. '; '-..  '; '.    '; '..-. '; ...
         '--.  '; '.... '; '..   '; '.--- '; '-.-  '; '.-.. '; ...
         '--   '; '-.   '; '---  '; '.--. '; '--.- '; '.-.  '; ...
         '...  '; '-    '; '..-  '; '...- '; '.--  '; '-..- '; ...
         '-.-- '; '--.. '; '-----'; '.----'; '..---'; '...--'; ...
         '....-'; '.....'; '-....'; '--...'; '---..' ; '----.'];
LENS  = [2 4 4 3 1 4 3 4 2 4 3 4 2 2 3 4 4 3 3 1 3 4 3 4 4 4 ...
         5 5 5 5 5 5 5 5 5 5];

t_elapsed = 0.0;
n = length(msg);

for i = 1:n
    ch = msg(i);
    if ch == ' '
        % Espacio entre palabras: 7 unidades de silencio
        t_elapsed = t_elapsed + 7 * unit;
    else
        % Buscar índice del carácter
        idx = 0;
        for k = 1:36
            if CHARS(k) == ch
                idx = k;
                break;
            end
        end
        if idx == 0
            continue; % carácter no soportado
        end
        code = CODES(idx, :);
        lenCode = LENS(idx);

        % Recorrer cada símbolo del código
        for s = 1:lenCode
            if code(s) == '.'
                onDur = 1 * unit;
                tipo  = 'punto';   % para identificar la salida
            else
                onDur = 3 * unit;
                tipo  = 'raya';
            end

            % Verificar si t está dentro de este encendido
            if t >= t_elapsed && t < t_elapsed + onDur
                if strcmp(tipo, 'punto')
                    y_dot = 1; y_dash = 0;
                else
                    y_dot = 0; y_dash = 1;
                end
                return;
            end
            t_elapsed = t_elapsed + onDur;

            % Silencio entre símbolos (1 unidad), excepto tras el último
            if s < lenCode
                offDur = 1 * unit;
                % Si t cae en este silencio, ambas salidas deben estar apagadas
                if t >= t_elapsed && t < t_elapsed + offDur
                    y_dot = 0; y_dash = 0;
                    return;
                end
                t_elapsed = t_elapsed + offDur;
            end
        end

        % Espacio entre letras (3 unidades), si la siguiente letra no es espacio
        if i < n && msg(i+1) ~= ' '
            % Comprobar si t cae en este silencio entre letras
            if t >= t_elapsed && t < t_elapsed + 3 * unit
                y_dot = 0; y_dash = 0;
                return;
            end
            t_elapsed = t_elapsed + 3 * unit;
        end
    end
end

% Fuera del mensaje: todo apagado
y_dot = 0;
y_dash = 0;
end
```

Para observar el código generado haga click en embedded coder en apps

![Coder](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Coder.png)

Para generar y cargar el codigo en arduino vaya a Harware y presione "Build, Deploy & Start"

![Build, Deploy & Start](https://github.com/Leo1307/Simulink_como_herramienta_de_generacion_automatica_de_codigo/blob/main/Fig/Build%2C%20Deploy%20%26%20Start.png)


