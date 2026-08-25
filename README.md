
El sistema convierte la deformación mecánica generada por el peso del paciente en una señal eléctrica mediante una galga extensiométrica configurada en un puente de Wheatstone. Esta señal, inicialmente del orden de los milivoltios, es acondicionada y digitalizada por un módulo **HX711** (ADC de 24 bits), para luego ser procesada por un **Arduino Nano**. Finalmente, el peso escalado y procesado se transmite vía I2C a una **Raspberry Pi 5** (configurada como maestro), encargada de concentrar todas las variables biomédicas del paciente.

1.  **Celda de Carga (Galga Extensiométrica):** Modelo DYLY-10B-200KG (Tipo S, 2.0 mV/V).
2.  **Módulo Acondicionador:** Amplificador de instrumentación y ADC HX711.
3.  **Microcontrolador (Esclavo):** Arduino Nano.
4.  **Computadora Monoplaca (Maestro):** Raspberry Pi 5.
5.  **Estructura:** Planchas de acero de 45x46 cm y resortes industriales.

El repositorio está organizado con los siguientes archivos:

`Balanza_codigofinal_comunicacionI2C_esclavo.ino`: Código principal del Arduino Nano. Realiza la lectura del HX711, aplica el factor de escala y transmite los datos por I2C a la Raspberry Pi.
*`codigo_maestro_rphi5.c`: Script en C para la Raspberry Pi 5. Actúa como maestro en el bus I2C (dirección `0x08`), solicitando y recibiendo el valor del peso.
`codigo_para_obtener_escala.ino`: Código de utilidad utilizado en la fase de calibración para obtener el factor de escala bruto dividiendo la lectura cruda entre un peso conocido.
`celda_de_carga`: Archivos complementarios relacionados a la estructura o modelado del sensor.
`DATASHEET HX711.pdf`: Hoja de datos técnica del módulo ADC HX711.
`DIAGRAMA DEL CIRCUITO`: Esquema de conexiones eléctricas (ruteo, placa PCB y borneras).
`INFORME FINAL.pdf`: Documentación detallada del proyecto (fundamento teórico, ecuaciones de nodos, calibración y conclusiones).
`VIDEOS DE APOYO`: Enlaces y referencias a material audiovisual para comprender el uso del HX711 y la comunicación I2C.

1. Subir el archivo `codigo_para_obtener_escala.ino` al Arduino Nano.
2. Abrir el Monitor Serial (9600 baudios).
3. Seguir las instrucciones en pantalla, coloca un peso conocido cuando se te indique y anota el valor crudo obtenido.
4. Dividir ese valor crudo entre el peso real (en kg) para obtener tu factor de escala.
5. Abrir `Balanza_codigofinal_comunicacionI2C_esclavo.ino`.
6. Actualizar la función `celda.set_scale(TU_FACTOR_DE_ESCALA);` con el valor calculado en el paso anterior.
7. Cargar el código en el Arduino Nano.
8. Asegurarse de tener habilitado el bus I2C en la Raspberry Pi (`sudo raspi-config`).
9. Compilar el archivo en C:
   ```bash
   gcc codigo_maestro_rphi5.c -o leer_peso
   ```
10. Ejecuta el archivo para solicitar la lectura del sensor:
   ```bash
   ./leer_peso
   ```
