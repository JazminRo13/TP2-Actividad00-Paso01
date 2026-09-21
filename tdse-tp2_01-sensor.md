## Paso 03 - Implementación del modelo Sensor

Se modificó el código fuente del modelo Sensor para implementar el
diagrama de estados correspondiente al botón `BTN_A`, asociado al
pulsador `B1 USER (Blue)` de la placa.

El modelo Sensor utiliza los eventos `EV_BTN_UP` y `EV_BTN_DOWN` para
representar, respectivamente, el botón liberado y presionado.

La máquina de estados está compuesta por cuatro estados:

- `ST_BTN_UP`: el botón se encuentra liberado.
- `ST_BTN_FALLING`: estado de transición utilizado para verificar la
  pulsación del botón.
- `ST_BTN_DOWN`: el botón se encuentra presionado.
- `ST_BTN_RISING`: estado de transición utilizado para verificar la
  liberación del botón.

Se modificó el archivo `task_sensor_attribute.h` para incorporar los
cuatro estados del modelo Sensor.

Además, se modificó `task_sensor.c` para inicializar el modelo en
`ST_BTN_UP` e implementar las transiciones del diagrama de estados.

Los estados `ST_BTN_FALLING` y `ST_BTN_RISING` utilizan la variable
`tick` para temporizar la validación de los cambios de estado del
pulsador. Una vez confirmada una pulsación o liberación, el modelo
Sensor envía al modelo System los eventos correspondientes mediante
`put_event_task_system()`.

## Paso 04 - Depuración del modelo Sensor

Se realizó la depuración del proyecto
`tdse-tp2_01-model_integration` para verificar el funcionamiento
del modelo Sensor.

La tarea `task_sensor` se ejecuta con un período de actualización
de 1 ms.

Se observaron durante la depuración los valores de
`task_sensor_dta_list[0]`:

- `state`: estado actual de la máquina de estados.
- `event`: estado detectado del botón B1.
- `tick`: contador utilizado para la temporización del antirrebote.

### Valores observados

| Condición de B1 | state | event | tick |
|---|---|---|---|
| Botón liberado | ST_BTN_UP | EV_BTN_UP | ... |
| Transición al presionar | ST_BTN_FALLING | EV_BTN_DOWN | ... |
| Botón presionado | ST_BTN_DOWN | EV_BTN_DOWN | ... |
| Transición al liberar | ST_BTN_RISING | EV_BTN_UP | ... |

Unidad temporal de `tick`: 1 ms por incremento/decremento de tick.
