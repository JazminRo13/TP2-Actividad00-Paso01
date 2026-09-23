## Implementación del modelo Sensor

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

## Depuración del modelo Sensor

Se realizó la depuración del proyecto mediante STM32CubeIDE con el objetivo de verificar el correcto funcionamiento del modelo `Sensor`.

Luego de varias ejecuciones de `app_update()`, se observaron los valores almacenados en `task_sensor_dta_list[0]` para las condiciones de botón liberado y botón presionado.

Los valores obtenidos fueron:

| Condición de B1 | `tick` | `state` | `event` |
|---|---:|---|---|
| Botón liberado | 0 | `ST_BTN_UP` | `EV_BTN_UP` |
| Botón presionado | 0 | `ST_BTN_DOWN` | `EV_BTN_DOWN` |

Cuando el botón B1 se encuentra liberado, el modelo permanece en el estado `ST_BTN_UP` y presenta el evento `EV_BTN_UP`. Al presionar el botón B1, el estado cambia a `ST_BTN_DOWN` y el evento a `EV_BTN_DOWN`.

En ambas condiciones se observó un valor de `tick = 0`. Considerando que la tarea `Sensor` se ejecuta mediante un esquema temporizado *Update by Time Code* con un período de 1 ms, la unidad asociada al contador `tick` es el milisegundo (ms).

De esta manera, mediante la depuración se verificó el cambio de estado y de evento del modelo `Sensor` en función de la condición del botón B1.
ck.
