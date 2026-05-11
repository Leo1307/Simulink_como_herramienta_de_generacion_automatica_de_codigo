# Instalación de extensiones

## Paso 1: entrar a "Add-Ons"

![Add-Ons]()

## Paso 2: Buscar "Simulink Support Package for Arduino Hardware"

Una vez encontrado, presionar "install" para descargar el Simulink Support Package for Arduino Hardware

![Simulink Support Package for Arduino Hardware]()

## Paso 3: Configuración

❌ Error: Download failed / Permission denied / cannot open port
Este error ocurre cuando Linux no tiene permisos para comunicarse con la placa Arduino mediante el puerto serial.
Normalmente sucede aunque el código compile correctamente.

✅ Solución paso a paso
🔍 Paso 1: Verificar que el sistema detecte el Arduino
Conectar la placa Arduino y ejecutar:
bashls /dev/ttyACM*
✅ Resultado esperado
Debe aparecer algo similar a:
/dev/ttyACM0
❌ Si no aparece
Posibles causas:

Cable USB defectuoso
Cable solo de carga (sin datos)
Puerto USB con problemas
La placa no está conectada correctamente

🔧 Solución:

Desconectar y volver a conectar el Arduino
Probar otro puerto USB
Probar otro cable USB


🔐 Paso 2: Verificar permisos del usuario
Ejecutar:
bashid
✅ Resultado esperado
Debe aparecer el grupo:
dialout
Ejemplo:
groups=1000(usuario),20(dialout),...
❌ Si dialout NO aparece
Continuar con el siguiente paso.

🛠️ Paso 3: Agregar el usuario al grupo dialout
Ejecutar:
bashsudo usermod -aG dialout $USER
✅ Resultado esperado
El comando no mostrará errores.

🔄 Paso 4: Aplicar los cambios
Reiniciar sesión o reiniciar el sistema completo.
Luego volver a ejecutar:
bashid
✅ Resultado esperado
Ahora debe aparecer:
dialout

⚙️ Paso 5: Verificar permisos del puerto serial
Ejecutar:
bashls -l /dev/ttyACM0
✅ Resultado esperado
Debe aparecer algo similar a:
crw-rw---- 1 root dialout ...
Lo importante es:

Grupo: dialout
Permisos: rw


🔌 Paso 6: Verificar que ningún proceso esté usando el puerto
Ejecutar:
bashlsof /dev/ttyACM0
✅ Resultado esperado
No debe aparecer ninguna salida.
❌ Si aparece algún proceso
Significa que otro programa está usando el puerto serial.
🔧 Solución:
Cerrar cualquier programa que pueda estar usando el Arduino, por ejemplo:

MATLAB
Simulink
Monitor serial
IDEs de Arduino
