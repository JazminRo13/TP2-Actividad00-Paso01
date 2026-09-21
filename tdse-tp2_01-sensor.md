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
