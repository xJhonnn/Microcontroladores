_Jhon Trujillo y Juan Ospina_
# **Apuntes segundo corte**
## **Semana 7**
### _1. Efecto rebote_
se refiere a las múltiples transiciones rápidas de un estado lógico a otro (por ejemplo, de alto a bajo y viceversa) que ocurren cuando se acciona un interruptor mecánico o un pulsador.
Cuando un contacto mecánico se cierra o se abre, no lo hace de forma instantánea y limpia. En su lugar, las superficies de contacto pueden vibrar y rebotar entre sí varias veces durante un corto período de tiempo (típicamente unos pocos milisegundos). Esto genera una serie de pulsos rápidos que el microcontrolador puede interpretar erróneamente como múltiples accionamientos del interruptor, aunque el usuario solo lo haya presionado o soltado una vez.
#### ![image](https://github.com/user-attachments/assets/a1716a2d-f4db-4d1a-83c1-f49f0c707fb1)
#### Soluciones para el efecto rebote
##### **• Solucion por hardware:** Se utiliza un circuito RC conectado al interruptor para filtrar las transiciones rápidas de voltaje causadas por el rebote. El capacitor se carga y descarga a través de la resistencia, suavizando la señal y proporcionando una transición más limpia al microcontrolador.
#### ![image](https://github.com/user-attachments/assets/930b79c4-0f6b-41b8-ac8e-62c552c57011)
##### **• Solucion por software:** Una de las formas de resolver el problema por software es con una técnica de “cambio de estado demorado”. Esta técnica consiste en cambiar el estado de la entrada conectada al pulsador un cierto tiempo después que se haya detectado el primer cambio e ignorando todo cambio intermedio que haya. Ese tiempo debe ser superior al de establecimiento de la llave.
#### ![image](https://github.com/user-attachments/assets/6d33be0c-c6d6-4758-ba6d-3faebebe4edc)
### _2. Teclado matricial_
Los teclados matriciales son una forma económica y eficiente de implementar una interfaz con múltiples botones utilizando un número reducido de pines en un microcontrolador. En lugar de conectar cada botón individualmente a un pin, los botones se organizan en una matriz de filas y columnas. Su funcionamiento se ejecuta cuando se presiona un botón, se cierra el circuito entre una fila y una columna específicas. El microcontrolador puede detectar qué botón se ha presionado escaneando secuencialmente las filas o columnas y leyendo el estado de las otras.
#### Tipos de teclados:
##### **• De membrana:** Sus caracteristicas principales es que son delgados, flexibles y generalmente más económicos. Consisten en varias capas de plástico con pistas conductoras. Al presionar una tecla, se hace contacto entre las capas. Suelen ser resistentes al polvo y salpicaduras. Unas de sus ventajas es que son de bajo costo, diseño delgado, cierta resistencia ambiental y no presentan efecto rebote. Sus desventajas son que su Tacto es menos definido, menor durabilidad en comparación con los de botón mecánico.
##### ![image](https://github.com/user-attachments/assets/655952dd-0c02-4ec0-8fc2-b2d965144cfe)
##### **• Mecanicos:** Sus caracteristicas principales son que utilizan botones individuales con un mecanismo de pulsación más tradicional. Pueden ser de plástico o goma y ofrecen un tacto más táctil y a menudo más duradero. Unas de sus ventajas es que son de Tacto más definido, mayor durabilidad (dependiendo de la calidad de los interruptores). Sus desventajas son que Generalmente más costosos y voluminosos que los de membrana.
#### Funcionamiento:
Para el correcto funcionamiento del teclado matricial debera ser configurado su pseudocodigo de la siguiente manera:
##### **• Activar resistencias de pull-up:** En un teclado matricial, las filas o las columnas (generalmente las filas) se conectan a través de resistencias de pull-up a la alimentación (VCC). Esto asegura que, en estado de reposo (cuando ningún botón está presionado), las líneas correspondientes se mantengan en un estado lógico alto (1). Al presionar un botón, se conecta una fila a una columna que está siendo activamente puesta a un estado lógico bajo (0) por el microcontrolador, permitiendo la detección de la pulsación.
##### **• Activar interrupciones externas detección estado:** Se configuran los pines conectados a las filas (o columnas, dependiendo del diseño) del teclado para que generen una interrupción externa al microcontrolador cuando su estado lógico cambie. En este caso, la interrupción se configuraría para dispararse al detectar un cambio de estado, probablemente una transición de alto a bajo, lo que indicaría que un botón ha sido presionado y, por lo tanto, una fila ha sido conectada a una columna que está en bajo.
##### **• Activar interrupciones globales:** Se habilitan las interrupciones a nivel global en el microcontrolador. Esto permite que las interrupciones externas configuradas en el paso anterior puedan ser reconocidas y atendidas por la unidad central de procesamiento (CPU).
##### **• Repetir:** Se entra en un bucle principal donde se realiza el proceso de escaneo del teclado.
##### **• "Mover" un 0 lógico por los pines conectados a las columnas del teclado:** Dentro del bucle, el microcontrolador activa secuencialmente cada una de las columnas del teclado, poniendo una a la vez en un estado lógico bajo (0), mientras que las demás se mantienen en un estado lógico alto (o en un estado de alta impedancia, dependiendo de la implementación).
##### **• Si se presenta interrupción:** Mientras el microcontrolador está escaneando las columnas, si un botón es presionado, la fila correspondiente se conectará a la columna que actualmente está en bajo. Esto provocará un cambio de estado en el pin de la fila, lo que a su vez generará una interrupción externa (configurada en el paso 2).
##### **• Identificar la Tecla pulsada:** Cuando ocurre una interrupción, la rutina de servicio de interrupción (ISR, Interrupt Service Routine) se ejecuta. Dentro de esta rutina, el microcontrolador debe determinar qué tecla específica fue presionada. Esto se logra verificando qué fila generó la interrupción y cuál columna estaba activa (en estado bajo) en el momento de la interrupción. La combinación de la fila y la columna identifica de manera única el botón presionado.
##### **• Codificar la tecla:** Una vez identificada la fila y la columna del botón presionado, se debe traducir esta información a un código o valor que represente la tecla específica (por ejemplo, un carácter ASCII para un teclado alfanumérico). Esto generalmente se realiza utilizando una tabla de consulta (lookup table) almacenada en la memoria del microcontrolador que mapea las combinaciones de fila y columna a los caracteres o funciones correspondientes.
#### Configuracion de entradas:
#### ![image](https://github.com/user-attachments/assets/ee137004-3b72-4eaa-9cd4-2b02c4838fe7)
#### Configuracion de interrupcion: 
#### ![image](https://github.com/user-attachments/assets/c8edc90d-b627-484c-b5ed-852e690bd046)
#### Configuracion de interrupciones:
#### ![image](https://github.com/user-attachments/assets/75e76364-c023-4f87-a12f-607b9e9224bb)
#### Desplazamiento del 0 entre bits:
#### ![image](https://github.com/user-attachments/assets/808007a6-fad5-47a8-a455-7b1c12d89132)
#### Interrupcion
#### ![image](https://github.com/user-attachments/assets/f3af452e-c54f-45aa-93d8-50c7d98e3e74)

## **Semana 8**
### _1. Modulo temporizador_
El módulo temporizador (también conocido como timer o contador) es un componente fundamental dentro de un microcontrolador (MCU) y en muchos otros sistemas electrónicos digitales. Su función principal es contar eventos o medir intervalos de tiempo con precisión.
#### Funciones y Características Comunes de los Módulos Temporizadores:
##### **• Contador de Eventos:** Pueden configurarse para contar la ocurrencia de eventos externos, como pulsos en un pin específico. Esto es útil para medir la frecuencia de una señal o el número de veces que algo sucede.
##### **• Medición de Intervalos de Tiempo:** Al contar los ciclos de un reloj interno o externo, pueden medir con precisión la duración de un evento o el intervalo entre dos eventos. Esto es crucial para tareas como medir el ancho de un pulso, la duración de una señal o implementar retardos precisos.
##### **• Generación de Señales PWM (Pulse Width Modulation):** Muchos temporizadores avanzados pueden generar señales PWM, que son ondas cuadradas con un ciclo de trabajo variable. Estas señales son esenciales para controlar la velocidad de motores, la intensidad de LEDs, o generar señales analógicas aproximadas.
##### **• Generación de Retardos (Delays):** Al configurar el temporizador para contar hasta un valor específico y luego generar una interrupción, se pueden implementar retardos precisos en el software.
##### **• Captura de Entradas (Input Capture):** Algunos temporizadores tienen la capacidad de registrar el valor del contador en el momento en que ocurre un evento específico en un pin de entrada (por ejemplo, un flanco ascendente o descendente). Esto permite medir con precisión el tiempo de llegada de una señal.
##### **• Comparación de Salidas** (Output Compare): Pueden compararse el valor actual del contador con un valor predefinido. Cuando la comparación es verdadera, se puede generar una acción en un pin de salida, como cambiar su estado o generar una interrupción. Esto se utiliza para generar señales con temporización precisa o para activar eventos en momentos específicos.
##### **• Generación de Interrupciones:** Al alcanzar un valor predefinido (desbordamiento del contador o coincidencia con un valor de comparación), el temporizador puede generar una interrupción, permitiendo que el microcontrolador ejecute una rutina específica en momentos precisos sin necesidad de un sondeo constante.
##### **• Prescaler:** Muchos temporizadores incluyen un prescaler, que es un divisor de frecuencia para la señal de reloj que alimenta el contador. Esto permite extender el rango de tiempo que el temporizador puede medir o generar.
### _2. Modulo contador_
El módulo contador dentro de un microcontrolador (MCU) es esencialmente un subconjunto o una configuración específica del módulo temporizador, aunque a veces pueden existir módulos dedicados únicamente a la función de conteo. Su función principal es registrar y contar la ocurrencia de eventos discretos.
Un módulo contador incrementa su valor interno cada vez que recibe un pulso o un evento de señal específico. Este pulso o evento puede provenir de una fuente interna (como el reloj del sistema) o de una fuente externa (como una señal en un pin de entrada) y puede configurarse para contar hasta un valor predefinido. Una vez alcanzado este valor (conocido como valor de "overflow"), el contador puede reiniciarse automáticamente a cero, generar una interrupción o detener el conteo, dependiendo de su configuración.
#### Funciones y Características Típicas de un Módulo Contador:
##### **• Fuente de Conteo Seleccionable:** Puede configurarse para contar pulsos del reloj interno del microcontrolador o eventos externos detectados en un pin específico.
##### **• Dirección de Conteo:** Puede contar de forma ascendente (incrementando el valor) o descendente (decrementando el valor).
##### **• Valor de Precarga:** Permite iniciar el conteo desde un valor diferente de cero.
##### **• Valor de Comparación (en algunos casos):** Al igual que los temporizadores, algunos contadores pueden tener registros de comparación para generar acciones (como cambiar un pin de salida o generar una interrupción) cuando el valor del contador alcanza un valor específico.
##### **• Generación de Interrupciones:** Al alcanzar el valor de overflow o un valor de comparación, el contador puede generar una interrupción para alertar al microcontrolador.
##### **• Prescaler (en algunos casos):** Puede incluir un prescaler para dividir la frecuencia de la señal de entrada y así extender el rango de conteo.
##### **• Modos de Operación:** Pueden tener diferentes modos de operación, como conteo único (se detiene al alcanzar el overflow) o conteo continuo (se reinicia automáticamente).
### _3. Tamaño bus vs resolución timer_
la resolución de un timer define la precisión con la que puede medir o generar intervalos de tiempo, mientras que el tamaño de su contador determina el rango máximo de tiempo que puede manejar con esa resolución.
### _4. Sistema de reloj_
El sistema de reloj en un sistema electrónico digital, especialmente en microcontroladores (MCUs), microprocesadores y otros circuitos integrados complejos, es el componente fundamental que proporciona la señal de temporización precisa y sincronizada para todas las operaciones internas del dispositivo. Actúa como el "latido" del sistema, coordinando la ejecución de instrucciones, la transferencia de datos y el funcionamiento de los diferentes módulos y periféricos.
#### Componentes Principales de un Sistema de Reloj:
##### **• Fuente de Reloj (Oscilador):** Es el componente que genera la señal de reloj base.
##### **• Divisores de Frecuencia (Prescalers):** Permiten generar múltiples señales de reloj con frecuencias más bajas a partir de la señal de reloj principal. Esto es útil para alimentar diferentes módulos del sistema que pueden operar a velocidades distintas.
##### **• Multiplicadores de Frecuencia (Phase-Locked Loops - PLLs):** Permiten generar señales de reloj con frecuencias más altas que la frecuencia del oscilador base. Los PLLs utilizan retroalimentación para bloquear la fase de un oscilador controlado por voltaje (VCO) a la fase de una señal de referencia (la señal del oscilador).
##### **• Selector de Reloj:** En sistemas más complejos, puede haber múltiples fuentes de reloj disponibles. Un selector de reloj permite elegir cuál de estas fuentes se utilizará como la señal de reloj principal del sistema o para alimentar periféricos específicos.
##### **• Distribución del Reloj:** La señal de reloj generada debe distribuirse de manera eficiente y sincronizada a todos los componentes que la necesitan dentro del chip o sistema.

### _5. Sistema de reloj interno_
El sistema de reloj interno en un microcontrolador (MCU) es un circuito oscilador que está integrado directamente dentro del propio chip del microcontrolador. A diferencia de los sistemas de reloj externos, que utilizan componentes como cristales de cuarzo o resonadores cerámicos conectados a pines externos del MCU, el reloj interno no requiere componentes externos para generar la señal de temporización básica.
#### Características Principales de los Sistemas de Reloj Internos:

##### **• Integración:** La principal característica es que el oscilador está construido dentro del silicio del microcontrolador.
##### **• Simplicidad:** No se necesitan componentes externos para su funcionamiento básico, lo que simplifica el diseño de la placa de circuito impreso (PCB) y reduce el costo.
##### **• Menor Precisión y Estabilidad:** En general, los osciladores internos suelen ser menos precisos y estables en frecuencia que los osciladores externos con cristal de cuarzo. Su frecuencia puede variar más con la temperatura, el voltaje de alimentación y las variaciones en el proceso de fabricación del chip.
##### **• Variedad de Tipos:** Los tipos comunes de osciladores internos incluyen: Osciladores RC Internos y Osciladores basados en otros principios
##### **• Frecuencia Configurable:** Muchos microcontroladores permiten configurar la frecuencia del reloj interno a través de registros de software. Esto puede permitir optimizar el consumo de energía (ejecutando a frecuencias más bajas cuando no se necesita alto rendimiento) o ajustar la velocidad de procesamiento.
##### **• Uso como Fuente de Reloj por Defecto:** A menudo, el oscilador interno se configura como la fuente de reloj predeterminada del microcontrolador al encenderse. El software puede luego optar por cambiar a una fuente de reloj externa si se requiere mayor precisión.
##### **• Calibración:** Algunos microcontroladores ofrecen mecanismos de calibración para mejorar la precisión del oscilador interno, aunque generalmente no alcanzan el nivel de un cristal externo.

### _6. Prescaler_
El prescaler (o preescalador) es un divisor de frecuencia utilizado comúnmente en los módulos de temporizador y contador dentro de microcontroladores (MCUs) y otros sistemas digitales. Su función principal es reducir la frecuencia de la señal de reloj de entrada antes de que llegue al contador principal del módulo.
#### ¿Cómo funciona?
Un prescaler es básicamente un contador que se incrementa con cada pulso de la señal de reloj de entrada. Cuando este contador interno alcanza un valor predefinido (el "valor de preescalado"), se genera un único pulso que se envía al contador principal del temporizador/contador, y el prescaler se reinicia.
#### Ejemplo:
##### Si tienes una señal de reloj de entrada de 1 MHz y configuras un prescaler con un valor de 8, el contador principal del temporizador/contador solo recibirá un pulso por cada 8 pulsos del reloj de entrada. Esto significa que la frecuencia efectiva que "ve" el contador principal será de 1MHz/8 = 125kHz
#### ¿Para qué se utiliza el prescaler?
El prescaler se utiliza por varias razones importantes:
##### **• Extender el Rango de Conteo/Temporización:** Los temporizadores y contadores tienen un tamaño de registro finito (por ejemplo, 8 bits, 16 bits, 32 bits). Esto limita el valor máximo que pueden alcanzar antes de desbordarse. Al reducir la frecuencia del reloj de entrada con un prescaler, se aumenta el tiempo que tarda el contador principal en alcanzar su valor máximo. Esto permite medir intervalos de tiempo más largos o contar un mayor número de eventos sin desbordamiento.
##### **• Generar Intervalos de Tiempo Más Largos con Resoluciones Fijas:** En aplicaciones donde se necesitan retardos o intervalos de tiempo largos con una cierta resolución, el prescaler permite lograr esto utilizando un contador de tamaño limitado.
##### **• Optimizar el Uso de Interrupciones:** Al extender el tiempo antes del desbordamiento, se reduce la frecuencia con la que se generan las interrupciones del temporizador/contador, lo que puede liberar recursos del microcontrolador para otras tareas.
#### Valores de Preescalado Comunes:
Los prescalers suelen ofrecer una variedad de factores de división, que a menudo son potencias de 2 (por ejemplo, 1, 2, 4, 8, 16, 32, 64, 128, 256) o múltiplos de 10 en algunos casos. El valor de preescalado se configura a través de registros específicos del módulo temporizador/contador en el microcontrolador.
#### Esquema logico
#### ![image](https://github.com/user-attachments/assets/ad909c74-6eb2-4a54-96f9-2c2439c95338)
### _7. Granularidad_
la granularidad se refiere al nivel de detalle o la menor unidad con la que se pueden controlar, medir o configurar los diferentes recursos y funcionalidades del microcontrolador. Es la "finura" con la que se puede interactuar con el hardware y el software del MCU.
#### Ejemplo 
####![image](https://github.com/user-attachments/assets/0a175591-c362-4543-bf64-dc6bf209b541)
### _8. Señal externa_
las señales externas son voltajes o corrientes eléctricas que provienen de fuentes fuera del propio dispositivo o circuito integrado. Estas señales interactúan con el dispositivo a través de sus pines de entrada/salida (I/O) y son fundamentales para que el dispositivo pueda percibir y responder al mundo exterior o comunicarse con otros componentes.
#### Características Clave de las Señales Externas:
##### **• Origen Externo:** La fuente de la señal no está contenida dentro del chip del microcontrolador u otro dispositivo que la recibe.
##### **• Interacción a través de Pines:** Las señales externas se conectan al dispositivo a través de sus pines físicos.
##### **• Variedad de Naturalezas:** Pueden ser de naturaleza analógica (voltajes o corrientes que varían continuamente) o digital (niveles de voltaje discretos que representan estados lógicos, típicamente alto y bajo).
##### **• Propósitos Diversos:** Sirven para una amplia gama de propósitos, como proporcionar información al dispositivo, recibir comandos, suministrar energía o permitir la comunicación con otros dispositivos.
#### Tipos Comunes de Señales Externas:
##### • Señales de Entrada Digital
##### • Señales de Entrada Analógica
##### • Señales de Salida Digital
##### • Señales de Salida Analógica
##### • Señales de Comunicación
##### **• Señales de Alimentación:** Aunque a veces se distinguen de las "señales de datos o control", la alimentación eléctrica externa (voltaje y corriente) es esencial para el funcionamiento del dispositivo.
##### **• Señales de Referencia:** Pueden ser voltajes de referencia precisos utilizados por el ADC o DAC para realizar conversiones exactas.
##### **• Señales de Reloj Externo:** En algunos casos, un microcontrolador puede utilizar una señal de reloj generada por un circuito externo (por ejemplo, un cristal de cuarzo conectado a pines específicos) para su funcionamiento.

### _9. Timers_
El microcontrolador PIC18F4550 cuenta con varios módulos de temporizador/contador versátiles, que son esenciales para una amplia gama de aplicaciones, desde la generación de retardos precisos hasta la implementación de PWM y la captura de eventos. A continuación, se detallan los timers disponibles en este microcontrolador:
#### **1. Timer0:**
##### **• Tamaño: Puede configurarse como un temporizador/contador de 8 o 16 bits.
##### **• Fuente de Reloj: Puede usar el reloj interno (Fosc/4) o una señal de reloj externa en el pin RA4/T0CKI.
##### **• Prescaler: Dispone de un prescaler programable con opciones de división desde 1:2 hasta 1:256.
##### **• Interrupción: Puede generar una interrupción por desbordamiento (cuando el contador regresa de su valor máximo a 0).
##### **• Aplicaciones Comunes: Generación de retardos, conteo de eventos externos, base de tiempo para otras funciones.
#### **2. Timer1:**
##### **• Tamaño: Temporizador/contador de 16 bits.
##### **• Fuente de Reloj: Puede usar el reloj interno (Fosc/4), una señal de reloj externa en el pin RC0/T1OSO y RC1/T1OSI (para el oscilador de baja potencia de 32 kHz), o el reloj interno dividido por un prescaler.
##### **• Prescaler: Dispone de un prescaler programable con opciones de división de 1:1, 1:2, 1:4 u 1:8.
##### **• Modo de Sincronización del Reloj Externo: Puede sincronizar la entrada del reloj externo.
##### **• Oscilador de Baja Potencia de 32 kHz: Puede utilizar un cristal externo de 32.768 kHz para aplicaciones de tiempo real o de bajo consumo.
##### **• Interrupción: Puede generar una interrupción por desbordamiento.
##### **• Aplicaciones Comunes: Temporización precisa, reloj de tiempo real (RTC) con el oscilador de baja potencia, captura de eventos.
#### **3. Timer2:**
##### **• Tamaño: Temporizador de 8 bits.
##### **• Fuente de Reloj: Siempre utiliza el reloj interno (Fosc/4) dividido por un prescaler.
##### **• Prescaler: Dispone de un prescaler programable con opciones de división de 1:1, 1:4 o 1:16.
##### **• Postscaler: Cuenta con un postscaler programable con opciones de división desde 1:1 hasta 1:16. El postscaler incrementa un contador interno después de que el Timer2 desborda un cierto número de veces antes de generar una interrupción o activar otra función.
##### **• Utilizado para PWM: Es el temporizador base para el módulo PWM (Pulse Width Modulation) del CCP (Capture/Compare/PWM) en modo PWM.
##### **• Interrupción: Puede generar una interrupción por desbordamiento.
##### **• Aplicaciones Comunes: Generación de señales PWM, base de tiempo para control de periféricos.
#### **4. Timer3:**
##### **• Tamaño:** Temporizador/contador de 16 bits.
##### **• Fuente de Reloj:** Puede usar el reloj interno (Fosc/4) o una señal de reloj externa en el pin RC2/T3CKI.
##### **• Prescaler:** Dispone de un prescaler programable con opciones de división de 1:1, 1:2, 1:4 u 1:8.
##### **• Modo de Sincronización del Reloj Externo:** Puede sincronizar la entrada del reloj externo.
##### **• Utilizado para Captura/Comparación/PWM:** Es otro temporizador base para los módulos CCP en modo Captura o Comparación.
##### **• Interrupción:** Puede generar una interrupción por desbordamiento.
##### **• Aplicaciones Comunes:** Captura de eventos, comparación de salidas, generación de PWM, temporización precisa.
#### **5. Timer4:**
##### **• Tamaño:** Temporizador de 8 bits.
##### **• Fuente de Reloj:** Siempre utiliza el reloj interno (Fosc/4) dividido por un prescaler.
##### **• Prescaler:** Dispone de un prescaler programable con opciones de división de 1:1, 1:4 o 1:16.
##### **• Postscaler:** Cuenta con un postscaler programable con opciones de división desde 1:1 hasta 1:16.
##### **• Utilizado para PWM:** Es el temporizador base para el módulo PWM del CCP en modo PWM (si el PIC18F4550 tiene múltiples módulos CCP).
##### **• Interrupción:** Puede generar una interrupción por desbordamiento.
##### **• Aplicaciones Comunes:** Generación de señales PWM, base de tiempo para control de periféricos.
### _10. Timer 0_
El Timer0 es uno de los módulos de temporizador/contador versátiles disponibles en el microcontrolador PIC18F4550. Su función principal es contar eventos o medir intervalos de tiempo, y puede configurarse de varias maneras para adaptarse a diferentes necesidades de la aplicación.
#### Características Principales del Timer0 en el PIC18F4550:
##### **• Tamaño Configurable:** Puede operar como un temporizador/contador de 8 bits o de 16 bits. Esta flexibilidad permite elegir entre un rango de conteo más pequeño (0-255 para 8 bits) o uno más grande (0-65535 para 16 bits), afectando el tiempo máximo que puede medir o el número máximo de eventos que puede contar antes de desbordarse.
##### **• Fuente de Reloj Seleccionable:** Reloj Interno (Fosc/4) o Señal de Reloj Externa (T0CKI)
##### **• Prescaler Programable:** El Timer0 incluye un prescaler, que es un divisor de frecuencia para la señal de reloj de entrada. Esto permite reducir la frecuencia que llega al contador del Timer0, extendiendo así el tiempo que tarda en desbordarse y permitiendo medir intervalos de tiempo más largos o contar un mayor número de eventos. Las opciones de división del prescaler suelen ser: 1:2, 1:4, 1:8, 1:16, 1:32, 1:64, 1:128 y 1:256.
##### **• Modo Temporizador:** Se incrementa con cada pulso de la fuente de reloj seleccionada (generalmente el reloj interno). Se utiliza para medir intervalos de tiempo.
##### **• Modo Contador:** Se incrementa con cada flanco ascendente o descendente (configurable) de la señal aplicada al pin T0CKI. Se utiliza para contar eventos externos.
##### **• Interrupción por Desbordamiento:** Cuando el contador del Timer0 alcanza su valor máximo (255 en modo de 8 bits o 65535 en modo de 16 bits) y se incrementa nuevamente, ocurre un desbordamiento. Este desbordamiento puede generar una interrupción, permitiendo que el microcontrolador ejecute una rutina de servicio de interrupción (ISR) para realizar alguna acción en intervalos de tiempo regulares o después de contar un cierto número de eventos.
##### **• Registros de Control:** El funcionamiento del Timer0 se configura mediante varios registros de control, como: T0CON (Timer0 Control Register), TMR0L (Timer0 Register Low Byte) y TMR0H (Timer0 Register High Byte), INTCON (Interrupt Control Register) y PIE1 (Peripheral Interrupt Enable Register 1).
#### Logica del temporizador
#### ![image](https://github.com/user-attachments/assets/e2cb98e5-b5d7-42c3-8b67-36da61844ae2)
### _11. Funcionamiento Timer 0_
Para el correcto funcionamiento del Timer 0 debera ser configurado su pseudocodigo de la siguiente manera:
#### 1. Habilitar las interrupciones globales y del timer: Para que el microcontrolador pueda responder a los eventos generados por el temporizador (como un desbordamiento), es necesario habilitar las interrupciones a dos niveles. Primero, se deben habilitar las interrupciones globales en el microcontrolador, lo que permite que cualquier interrupción configurada pueda ser reconocida por la unidad central de procesamiento (CPU). Segundo, se debe habilitar específicamente la interrupción asociada al temporizador que se está utilizando (por ejemplo, la interrupción por desbordamiento del Timer0). Esto se realiza mediante la configuración de bits específicos en los registros de control de interrupciones del microcontrolador (como los registros INTCON y PIE1 en el PIC18F4550). Una vez habilitadas, cuando el temporizador alcance la condición para generar una interrupción, el flujo normal del programa se suspenderá y se ejecutará una rutina especial llamada rutina de servicio de interrupción (ISR) diseñada para manejar ese evento.
#### 2. Configurar el temporizador: Este paso implica establecer los parámetros de funcionamiento del temporizador según las necesidades de la aplicación. Esto incluye:
##### **• Seleccionar el modo de operación:** Determinar si el temporizador funcionará como un temporizador (para medir intervalos de tiempo) o como un contador (para contar eventos externos).
##### **• Seleccionar la fuente de reloj:** Elegir si el temporizador utilizará el reloj interno del microcontrolador o una señal de reloj externa aplicada a un pin específico.
##### **• Configurar el prescaler:** Establecer el factor de división del prescaler para ajustar la frecuencia de la señal de reloj que alimenta el contador del temporizador, lo que afecta la resolución y el rango de temporización.
##### **• Seleccionar el modo de 8 o 16 bits (si aplica):** Para temporizadores como el Timer0, elegir la resolución del contador.
##### **• Configurar otros parámetros específicos del temporizador:** Esto podría incluir la selección del flanco de conteo para el modo contador, la habilitación de funcionalidades especiales, etc. La configuración se realiza escribiendo los valores apropiados en los registros de control del temporizador (como el registro T0CON para el Timer0).
#### 3. Cargar el valor inicial del registro TMRx: Los registros TMRx (donde 'x' es el número del temporizador, por ejemplo, TMR0L y TMR0H para el Timer0) almacenan el valor actual del contador del temporizador. Para iniciar la temporización desde un valor específico diferente de cero, o para controlar el período de tiempo antes de que ocurra un desbordamiento, se puede cargar un valor inicial en estos registros. Por ejemplo, si se desea generar una interrupción después de un cierto intervalo de tiempo, se puede cargar un valor inicial de manera que el temporizador alcance su valor máximo y desborde después del tiempo deseado.
#### 4. Iniciar la operación de temporización: Una vez que las interrupciones están habilitadas, el temporizador está configurado y (opcionalmente) se ha cargado un valor inicial, el último paso es iniciar la operación del temporizador. Esto generalmente se hace activando un bit de habilitación específico en el registro de control del temporizador. Una vez iniciado, el contador del temporizador comenzará a incrementarse (o decrementarse, dependiendo de la configuración) con cada pulso de la fuente de reloj configurada. La temporización o el conteo de eventos se llevarán a cabo hasta que el temporizador alcance su valor máximo (desbordamiento) o hasta que se detenga explícitamente mediante la modificación del bit de habilitación.
#### Declaracion variables y funciones 
#### ![image](https://github.com/user-attachments/assets/c9599356-69e5-464a-b5ed-a8119f43640f)
#### Inicializacion de puertos
#### ![image](https://github.com/user-attachments/assets/fbfad76e-ec32-44a0-bf1a-ae01a36f9b88)
#### Configuracion general timer
#### ![image](https://github.com/user-attachments/assets/4fbde362-ec3d-4d69-a9c8-8c023702e8bd)
#### Programa main 
#### ![image](https://github.com/user-attachments/assets/15ce3bf5-ef1a-48c3-acc1-c56b750dd379)
#### Funcion interrupcion
#### ![image](https://github.com/user-attachments/assets/71517e6f-6963-4d5a-9d3c-71016a76d1c6)
## **Semana 9**
### _1. Pantalla LCD_
Una pantalla LCD es un tipo de módulo de visualización de cristal líquido (LCD) muy popular y ampliamente utilizado en proyectos de electrónica, especialmente con microcontroladores. Su popularidad se debe a su bajo costo, facilidad de uso y la disponibilidad de una interfaz de control estandarizada.
#### Características Clave:
##### **• Visualización de Caracteres:** Están diseñadas principalmente para mostrar caracteres alfanuméricos (letras, números, símbolos) y algunos símbolos gráficos predefinidos. No están diseñadas para mostrar gráficos arbitrarios complejos.
##### **• Organización de Filas y Columnas:** Vienen en diferentes configuraciones de filas y columnas, siendo las más comunes: 16x2: 16 caracteres por fila y 2 filas; y 20x4: 20 caracteres por fila y 4 filas.
##### **• Interfaz Paralela de 4 u 8 Bits:** Se comunican con el microcontrolador o sistema de control a través de una interfaz paralela. Esta interfaz puede configurarse para usar 4 bits de datos o 8 bits de datos, además de varios pines de control. El modo de 4 bits requiere menos pines del microcontrolador, pero la transferencia de datos es más lenta.
##### **• Pines de Control:** Además de los pines de datos, típicamente tienen los siguientes pines de control: RS (Register Select), RW (Read/Write) y E (Enable).
##### **• Contraste Ajustable:** Suelen tener un pin para ajustar el contraste de la pantalla mediante un potenciómetro externo.
##### **• Retroiluminación (Opcional):** Muchos módulos HD44780 incluyen una retroiluminación LED (generalmente en color blanco, azul o verde) para mejorar la visibilidad en condiciones de poca luz. Esta retroiluminación suele tener pines separados para la alimentación.
##### **• Juego de Caracteres Estándar:** El controlador HD44780 tiene un juego de caracteres estándar incorporado, que incluye letras ASCII mayúsculas y minúsculas, números, signos de puntuación y algunos símbolos especiales. También permite definir algunos caracteres personalizados por el usuario.
### _2. Pantallas dot-matrix_
Las pantallas dot-matrix (también conocidas como pantallas de matriz de puntos) son dispositivos de visualización digital que utilizan una matriz de puntos individuales (píxeles) para formar caracteres, símbolos, gráficos e incluso imágenes. Cada punto en la matriz puede encenderse o apagarse individualmente para crear el patrón deseado.
#### Características Clave:
##### **• Matriz de Puntos:** La característica definitoria es la organización de los elementos de visualización en una matriz (filas y columnas) de puntos individuales.
##### **• Control Individual de Puntos:** Cada punto dentro de la matriz puede ser controlado independientemente de los demás.
##### **• Flexibilidad de Visualización:** Al controlar los puntos individualmente, se pueden mostrar una amplia variedad de caracteres, símbolos personalizados y gráficos básicos.
##### **• Variedad de Tecnologías:** Las pantallas dot-matrix pueden utilizar diferentes tecnologías para generar la luz o controlar los píxeles: LED (Diodo Emisor de Luz), LCD (Pantalla de Cristal Líquido), OLED (Diodo Orgánico Emisor de Luz), E-ink (Tinta Electrónica)
##### **• Diferentes Tamaños y Resoluciones: Las pantallas dot-matrix vienen en una amplia gama de tamaños y resoluciones, desde pequeñas matrices de 5x7 puntos para mostrar caracteres individuales hasta grandes paneles con miles de puntos para mostrar gráficos e imágenes más complejas.
##### **• Interfaz de Control: Requieren una interfaz electrónica para controlar el estado de cada punto de la matriz. Esta interfaz puede ser paralela o serial, dependiendo del tamaño y la complejidad de la pantalla.
#### ![image](https://github.com/user-attachments/assets/4faa24cc-8f26-4e70-a9b0-59b54551a41b)
### _3. Memoria de la LCD_
La memoria de una pantalla LCD Hitachi HD44780 (y otras pantallas LCD de caracteres basadas en controladores compatibles) está organizada principalmente para almacenar los caracteres que se van a mostrar y para almacenar patrones de caracteres definidos por el usuario. No es una memoria de propósito general para almacenar grandes cantidades de datos arbitrarios como la RAM de un microcontrolador. La memoria principal de estas LCD se divide en tres áreas importantes:
#### 1. DDRAM: Para almacenar los códigos de los caracteres que se están mostrando.
#### 2. CGROM: Para almacenar las formas predefinidas de los caracteres.
#### 3. CGRAM: Para permitir la definición y almacenamiento de caracteres personalizados por el usuario.

### _4. Señales de alimentacion LCD_
Las señales de alimentación de una pantalla LCD Hitachi HD44780 (y módulos compatibles) son los pines que se encargan de suministrar la energía eléctrica necesaria para su funcionamiento. Típicamente, estas pantallas tienen al menos tres pines dedicados a la alimentación:
#### • VCC (o VDD): Este pin es la conexión para el voltaje de alimentación positivo. El voltaje de operación estándar para la mayoría de las pantallas LCD HD44780 es de +5V DC. Sin embargo, algunos módulos pueden operar a +3.3V DC, por lo que es crucial consultar la hoja de datos específica del módulo LCD que estés utilizando para conocer su voltaje de alimentación requerido. Conectar un voltaje incorrecto puede dañar la pantalla de forma permanente.
#### • GND (o VSS): Este pin es la conexión para tierra (Ground) o el voltaje de alimentación negativo (0V). Es el punto de referencia para el voltaje de alimentación positivo.
#### • Contraste: Para ello, se debe instalar un potenciómetro de unos 10KΩ 
#### ![image](https://github.com/user-attachments/assets/03273fc4-a279-4842-a88b-a473a214409a)

### _5. Señales de control LCD_
Las señales de control de una pantalla LCD Hitachi HD44780 son los pines que se utilizan para comunicar comandos e información de control desde el microcontrolador (o cualquier otro sistema de control) hacia la pantalla LCD. Estos pines permiten al microcontrolador decirle a la pantalla qué hacer, como inicializarla, borrarla, mover el cursor, y, lo más importante, cuándo y dónde mostrar los datos. Los pines de control esenciales en una LCD HD44780 son:
####  Pin 4 RS (Register Select):
##### • Este pin se utiliza para seleccionar el tipo de información que se está enviando a la LCD.
##### • Cuando RS está en LOW (0 lógico), los datos en el bus de datos (D0-D7 o D4-D7 en modo de 4 bits) se interpretan como un comando para el controlador de la LCD. Los comandos incluyen instrucciones como borrar la pantalla, mover el cursor a una posición específica, configurar el modo de visualización, etc.
##### • Cuando RS está en HIGH (1 lógico), los datos en el bus de datos se interpretan como datos de carácter que se van a mostrar en la pantalla en la posición actual del cursor.
#### Pin 5 RW (Read/Write):
##### • Este pin controla la dirección de la transferencia de datos entre el microcontrolador y la LCD.
##### • Cuando RW está en LOW (0 lógico), se está escribiendo información (comando o dato de carácter) desde el microcontrolador a la LCD.
##### • Cuando RW está en HIGH (1 lógico), se está leyendo información desde la LCD hacia el microcontrolador. Esto se utiliza principalmente para verificar el estado de ocupado del controlador (si la LCD está lista para recibir más comandos o datos) o para leer el contenido de la DDRAM o CGRAM.
##### • En la mayoría de las aplicaciones donde solo se escribe a la LCD, el pin RW se conecta permanentemente a tierra (GND). Esto simplifica la interfaz y ahorra un pin del microcontrolador, ya que la lectura del estado de ocupado a menudo se implementa mediante un pequeño retardo de tiempo en el software.
#### Pin 6 E (Enable):
Este pin se utiliza para sincronizar la transferencia de datos entre el microcontrolador y la LCD. La LCD solo lee los datos de los pines de datos y RS cuando el pin E está en un cierto estado (típicamente durante una transición de HIGH a LOW o un pulso HIGH).

### _6. Señales de datos LCD_
Las señales de datos de una pantalla LCD Hitachi HD44780 son los pines que se utilizan para transferir la información real (comandos o datos de caracteres) desde el microcontrolador (o sistema de control) hacia la pantalla LCD. Estas señales son las "carreteras" por donde viajan las instrucciones y los códigos de los caracteres que se van a mostrar. La interfaz de datos de la LCD HD44780 puede configurarse para operar en dos modos: modo de 8 bits o modo de 4 bits. La elección del modo afecta el número de pines del microcontrolador necesarios para la comunicación y la velocidad de transferencia de datos.
#### 1. Modo de 8 Bits:
##### • En este modo, se utilizan ocho pines de datos, típicamente denominados D0, D1, D2, D3, D4, D5, D6 y D7.
##### • **Transferencia de Datos:** Cada byte de información (ya sea un comando o el código ASCII de un carácter) se transfiere a la LCD en una sola operación, utilizando estos ocho pines simultáneamente.
##### • **Velocidad:** La transferencia de datos es más rápida en el modo de 8 bits, ya que se envía un byte completo a la vez.
##### • **Requisitos de Pines:** Requiere un total de 8 pines del microcontrolador para la transferencia de datos, además de los pines de control (RS, RW, E) y alimentación.
#### 2. Modo de 4 Bits:
##### • En este modo, solo se utilizan cuatro pines de datos, típicamente D4, D5, D6 y D7. Los pines D0, D1, D2 y D3 generalmente no se conectan.
##### • **Transferencia de Datos:** Cada byte de información se divide en dos "nibbles" (grupos de 4 bits) y se transfiere secuencialmente. Primero se envían los 4 bits más significativos (MSB) y luego los 4 bits menos significativos (LSB), utilizando los mismos cuatro pines de datos.
##### • **Velocidad:** La transferencia de datos es más lenta en comparación con el modo de 8 bits, ya que se requieren dos operaciones para enviar un solo byte.
##### • **Ahorro de Pines:** La principal ventaja de este modo es que requiere solo 4 pines del microcontrolador para la transferencia de datos, lo que puede ser importante en proyectos con limitaciones de pines.
##### • **Inicialización:** La LCD debe inicializarse correctamente en modo de 4 bits mediante una secuencia específica de comandos.
### _7. DDRAM (Display data RAM)
La DDRAM (Display Data RAM) en una pantalla LCD Hitachi HD44780 es una memoria de acceso aleatorio (RAM) dentro del controlador de la LCD que almacena los códigos de los caracteres que se van a mostrar en la pantalla. Imagínala como una tabla o un mapa donde cada celda corresponde a una posición de carácter en la pantalla (una combinación de fila y columna). El valor almacenado en cada celda de la DDRAM es el código del carácter (generalmente ASCII o un código del juego de caracteres interno) que se mostrará en esa ubicación.
#### Puntos Clave sobre la DDRAM:
##### • **Almacenamiento de Caracteres a Mostrar:** La función principal de la DDRAM es mantener los códigos de los caracteres que el microcontrolador quiere que se visualicen en la pantalla.
##### • **Correspondencia con la Pantalla:** Cada dirección de memoria en la DDRAM se mapea a una posición física específica en la pantalla LCD (fila y columna). La forma exacta en que se realiza este mapeo depende de la configuración de filas y columnas del módulo LCD (por ejemplo, 16x2, 20x4).
##### • **Acceso de Lectura y Escritura:** El microcontrolador puede escribir en la DDRAM para especificar qué carácter se debe mostrar en una determinada posición. También puede leer desde la DDRAM para determinar qué carácter está actualmente almacenado en una posición específica.
##### • **Direccionamiento:** La DDRAM se direcciona mediante un sistema de filas y columnas. El controlador HD44780 tiene un contador de direcciones interno que se puede configurar mediante comandos. Cuando se escriben datos a la DDRAM, este contador de direcciones se incrementa automáticamente (o decrementa, según la configuración del modo de entrada).
##### • **Tamaño y Organización:** El tamaño de la DDRAM es lo suficientemente grande para almacenar todos los caracteres que se pueden mostrar simultáneamente en la pantalla, y a menudo tiene espacio adicional. Por ejemplo, en una pantalla 16x2, aunque solo se visualizan 32 caracteres a la vez, la DDRAM podría tener capacidad para almacenar 80 caracteres (40 por línea internamente). Este espacio adicional se utiliza para implementar funciones como el desplazamiento de texto (scrolling).
##### • **Volátil:** Como es RAM, el contenido de la DDRAM se pierde cuando se apaga la alimentación de la pantalla LCD.
#### Cómo se Utiliza la DDRAM:
##### 1. Seleccionar la Posición: El microcontrolador envía un comando a la LCD para establecer la dirección de la DDRAM donde se desea escribir el carácter. Esto determina la fila y la columna donde aparecerá el carácter en la pantalla.
##### 2. Escribir el Código del Carácter: El microcontrolador envía el código del carácter (por ejemplo, el código ASCII de una letra) a la LCD mientras el pin RS (Register Select) está en HIGH (indicando datos). Este código se almacena en la ubicación de la DDRAM previamente seleccionada.
##### 3. Visualización Automática: El controlador HD44780 lee continuamente la DDRAM y, para cada código de carácter almacenado, busca la forma del carácter correspondiente en la CGROM (o CGRAM si es un carácter definido por el usuario) y lo muestra en la matriz de puntos de la pantalla en la posición asociada a esa dirección de la DDRAM.
### _8. CGRAM (Character Generator RAM)_
La CGRAM (Character Generator RAM) en una pantalla LCD Hitachi HD44780 es una memoria de acceso aleatorio (RAM) dentro del controlador de la LCD que permite al usuario definir y almacenar hasta 8 caracteres personalizados o gráficos pequeños. A diferencia de los caracteres predefinidos almacenados en la CGROM (Character Generator ROM), los caracteres creados en la CGRAM pueden ser diseñados por el programador para mostrar símbolos, iconos o formas específicas que no están incluidas en el juego de caracteres estándar de la pantalla.
#### Puntos Clave sobre la CGRAM:
##### **• Definición de Caracteres Personalizados:** Su función principal es permitir la creación de hasta 8 caracteres gráficos definidos por el usuario.
##### **• Matriz de Puntos:** Cada carácter personalizado se define como una matriz de puntos. En las pantallas HD44780, la matriz típica para un carácter es de 5x8 píxeles. Esto significa que para cada uno de los 8 caracteres, se pueden especificar los estados (encendido o apagado) de los 40 puntos individuales que forman el carácter.
##### **• Almacenamiento Temporal:** Como es RAM, los caracteres definidos en la CGRAM se almacenan temporalmente y se pierden cuando se apaga la alimentación de la pantalla LCD. Si se necesitan estos caracteres de forma persistente, el programa debe redefinirlos cada vez que se enciende la pantalla.
##### **• Direccionamiento:** La CGRAM tiene direcciones específicas donde se pueden escribir los patrones de bits para cada una de las 8 posibles definiciones de caracteres. Cada carácter ocupa un bloque de 8 bytes en la CGRAM (uno para cada fila de 5 píxeles).
##### **• Acceso de Escritura:** El microcontrolador escribe los patrones de bits que definen la forma de cada carácter personalizado en las direcciones correspondientes de la CGRAM utilizando comandos específicos.
##### **• Visualización:** Una vez que un carácter personalizado ha sido definido en la CGRAM, se le asigna un código de carácter específico (generalmente del 0 al 7). Para mostrar este carácter en la pantalla, el microcontrolador simplemente escribe este código en la DDRAM (Display Data RAM) en la posición deseada. El controlador de la LCD reconocerá este código como un carácter definido por el usuario y buscará su patrón en la CGRAM para mostrarlo.
#### Cómo se Utiliza la CGRAM:
##### 1. Seleccionar la Dirección de la CGRAM: El microcontrolador envía un comando a la LCD para establecer la dirección de la CGRAM donde se va a definir un nuevo carácter. Esta dirección se calcula en función del número del carácter personalizado que se va a definir (0 al 7).
##### 2. Escribir el Patrón del Carácter: Para cada una de las 8 filas del carácter (en una matriz de 5x8), el microcontrolador envía un byte de datos a la LCD. Los 5 bits menos significativos de cada byte representan el estado de los 5 píxeles de esa fila (1 para encendido, 0 para apagado). Los 3 bits más significativos generalmente se ignoran. Se deben enviar 8 bytes secuencialmente para definir completamente un carácter de 5x8.
##### 3. Mostrar el Carácter Definido: Una vez que el patrón del carácter está escrito en la CGRAM, se le asigna un código (0-7). Para mostrar este carácter en la pantalla, el microcontrolador
### _9. CGROM (Character Generator ROM)_
La CGROM (Character Generator ROM) en una pantalla LCD Hitachi HD44780 es una memoria de solo lectura (ROM) dentro del controlador de la LCD que contiene las formas predefinidas de los caracteres que la pantalla puede mostrar. Imagínala como una biblioteca permanente de los glifos (representaciones visuales) de las letras, números, signos de puntuación y símbolos estándar que la LCD puede mostrar directamente.
#### Puntos Clave sobre la CGROM:
##### **• Almacenamiento de Caracteres Predefinidos:** Su función principal es almacenar de forma permanente los patrones de matriz de puntos (típicamente 5x8 o 5x10 píxeles por carácter) para el juego de caracteres estándar de la LCD.
##### **• Memoria de Solo Lectura:** El programador no puede escribir ni modificar el contenido de la CGROM. Las formas de los caracteres están fijas y definidas por el fabricante del controlador HD44780.
##### **• Acceso por Código de Carácter:** Cuando el microcontrolador envía un código de carácter específico (generalmente un código ASCII o un código del juego de caracteres interno) a la DDRAM (Display Data RAM), el controlador HD44780 utiliza este código como una dirección o índice para buscar la forma del carácter correspondiente en la CGROM.
##### **• Generación de la Visualización:** Una vez que el controlador localiza el patrón de bits del carácter en la CGROM, utiliza esta información para activar los píxeles apropiados en la matriz de puntos de la pantalla LCD, mostrando así el carácter deseado en la ubicación correspondiente.
##### **• Juego de Caracteres Estándar:** El contenido de la CGROM incluye típicamente el juego de caracteres ASCII básico (letras mayúsculas y minúsculas, números, signos de puntuación) y algunos símbolos adicionales específicos del fabricante. Pueden existir ligeras variaciones en el juego de caracteres entre diferentes clones del controlador HD44780.
### _10. Registros internos_
Los registros internos de una pantalla LCD con controlador Hitachi HD44780 son ubicaciones de memoria dentro del propio chip controlador de la LCD que se utilizan para configurar su funcionamiento y controlar la visualización. El microcontrolador se comunica con la LCD escribiendo valores en estos registros (para dar comandos o enviar datos) o leyendo valores de ellos (para obtener el estado). Estos registros no son directamente accesibles por el programador mediante direcciones de memoria convencionales como la DDRAM o la CGRAM. En cambio, se accede a ellos indirectamente a través de los pines de control RS (Register Select) y el bus de datos (D0-D7 o D4-D7). Los principales registros internos del controlador HD44780 son:
#### 1. Registro de Instrucción (IR - Instruction Register):
##### • Este registro es donde el microcontrolador escribe los comandos para controlar la función de la LCD.
##### • Se accede a este registro cuando el pin RS está en LOW (0 lógico) durante una operación de escritura.
##### • Los comandos incluyen acciones como: Borrar la pantalla (Clear Display), Retornar el cursor al inicio (Return Home), Configurar el modo de entrada (Entry Mode Set), Controlar el encendido/apagado de la pantalla, el cursor y el parpadeo (Display Control), Mover el cursor o desplazar la pantalla (Cursor or Display Shift), Establecer la función (Function Set) para configurar el tamaño de la interfaz de datos (4 u 8 bits), el número de líneas y la fuente de la matriz de caracteres, Establecer la dirección de la CGRAM (Set CGRAM Address), Establecer la dirección de la DDRAM (Set DDRAM Address), Leer el estado de ocupado (Busy Flag) y la dirección del contador (cuando RW está en HIGH y RS está en LOW)
#### 2. Registro de Datos (DR - Data Register):
##### • Este registro se utiliza para escribir los datos de los caracteres que se van a mostrar en la DDRAM o para leer los datos desde la DDRAM o la CGRAM.
##### • Se accede a este registro cuando el pin RS está en HIGH (1 lógico) durante una operación de escritura (para enviar datos a la DDRAM) o durante una operación de lectura (para leer datos desde la DDRAM o CGRAM, si se utiliza el pin RW).
##### • Cuando se escribe un byte en el DR, este byte se almacena en la DDRAM en la dirección actual del contador de direcciones, y el contador de direcciones se incrementa (o decrementa) automáticamente según la configuración del modo de entrada.
##### • Cuando se lee desde el DR, se obtiene el byte de datos de la dirección actual de la DDRAM o CGRAM.
### _11. Busy Flag (BF)_    
El Busy Flag (Bandera de Ocupado) es un bit de estado interno dentro del controlador Hitachi HD44780 de una pantalla LCD que indica si el controlador está actualmente realizando una operación interna y no está listo para aceptar nuevos comandos o datos. La función principal del Busy Flag es sincronizar la comunicación entre el microcontrolador y la LCD. Las operaciones internas de la LCD, como borrar la pantalla, mover el cursor o escribir datos en la memoria, toman un cierto tiempo para completarse. El Busy Flag permite al microcontrolador determinar cuándo la LCD ha terminado su tarea anterior y está lista para recibir la siguiente instrucción o dato.
#### Cómo Funciona:
##### **• Estado Ocupado (Busy Flag = 1):** Cuando la LCD está realizando una operación interna, el Busy Flag se pone a un estado lógico alto (1). Mientras la bandera esté en este estado, el microcontrolador no debe intentar enviar nuevos comandos o datos a la LCD, ya que podrían ser ignorados o causar un comportamiento inesperado.
##### **• Estado Libre (Busy Flag = 0):** Una vez que la operación interna se ha completado, el controlador de la LCD pone el Busy Flag a un estado lógico bajo (0). Esto indica al microcontrolador que la LCD está lista para recibir el siguiente comando o dato.
#### Cómo se Lee el Busy Flag:
Para verificar el estado del Busy Flag, el microcontrolador debe realizar una operación de lectura desde la LCD. Esto se hace de la siguiente manera:
##### **• Configurar el pin RS en LOW (0 lógico):** Esto selecciona el registro de instrucción (IR), donde se encuentra el estado del Busy Flag.
##### **• Configurar el pin RW en HIGH (1 lógico):** Esto habilita la operación de lectura desde la LCD.
##### **• Leer el valor del bus de datos (D0-D7):** El bit más significativo del bus de datos, D7, contiene el valor del Busy Flag.
##### **• Esperar a que D7 sea LOW (0 lógico):** El microcontrolador debe leer repetidamente el valor de D7 hasta que sea LOW, lo que indica que la LCD está lista.
##### **• Regresar el pin RW a LOW (0 lógico):** Para futuras operaciones de escritura.

### _12. Address Counter (AC)_
La función principal del Address Counter es mantener la ubicación de la siguiente operación de transferencia de datos, simplificando el proceso de escribir múltiples caracteres secuencialmente o leer datos de diferentes posiciones de la memoria de la LCD.
#### Cómo Funciona:
##### 1. Establecimiento de la Dirección: El valor del Address Counter se puede establecer directamente por el microcontrolador mediante el envío de comandos específicos al Registro de Instrucción (IR):
###### **• Set DDRAM Address:** Este comando carga un valor en el AC para apuntar a una posición específica en la Display Data RAM (DDRAM), donde se almacenan los caracteres que se muestran en la pantalla. Esto permite al microcontrolador controlar en qué fila y columna aparecerá el siguiente carácter escrito.
###### **• Set CGRAM Address:** Este comando carga un valor en el AC para apuntar a una posición específica en la Character Generator RAM (CGRAM), donde se definen los caracteres personalizados por el usuario. Esto permite al microcontrolador escribir los patrones de bits que forman los caracteres personalizados.
##### 2. Incremento/Decremento Automático: Después de cada operación de escritura o lectura en la DDRAM o CGRAM a través del Registro de Datos (DR), el valor del Address Counter se incrementa o decrementa automáticamente, dependiendo del modo de entrada establecido previamente mediante el comando "Entry Mode Set".
###### **• Incremento:** Si el modo de entrada está configurado para incremento, después de escribir o leer un dato, el AC se incrementa en 1, apuntando a la siguiente posición de memoria. Esto es útil para escribir una cadena de caracteres secuencialmente en la pantalla sin tener que establecer la dirección para cada carácter individualmente.
###### **• Decremento:** Si el modo de entrada está configurado para decremento, el AC se decrementa en 1 después de cada operación.
###### **• Desplazamiento del Display:** El comando "Entry Mode Set" también controla si la pantalla completa se desplaza (a la izquierda o a la derecha) después de cada escritura. Si el desplazamiento está habilitado, el efecto visual es que el siguiente carácter aparece en una posición diferente, aunque el AC se siga incrementando o decrementando.
##### 3. Lectura del Contador de Direcciones: El microcontrolador también puede leer el valor actual del Address Counter desde la LCD. Para hacer esto:
###### • Se configura el pin RS en LOW (para seleccionar el Registro de Instrucción).
###### • Se configura el pin RW en HIGH (para habilitar la lectura).
###### • El valor del AC se encuentra en los 7 bits menos significativos del bus de datos (D0-D6). El bit más significativo (D7) contendrá el estado del Busy Flag.
### _13. Inicialización LCD por reset interno_
La inicialización por reset interno de una pantalla LCD Hitachi HD44780 (o compatible) se refiere al proceso automático que el controlador de la LCD realiza inmediatamente después de que se le aplica alimentación. Este proceso está diseñado para poner la LCD en un estado funcional predeterminado sin necesidad de comandos explícitos enviados por el microcontrolador. Sin embargo, es crucial entender que la inicialización por reset interno por sí sola puede no ser suficiente o completamente confiable para asegurar el correcto funcionamiento de la LCD en todas las situaciones. La hoja de datos del HD44780 especifica una secuencia de inicialización por software que generalmente se recomienda seguir siempre para garantizar una configuración adecuada y consistente.
#### ![image](https://github.com/user-attachments/assets/9e7d62f5-1d76-4448-ab93-b415d6953eaa)

### _14. Inicialización por instrucciones_
La inicialización de una pantalla LCD Hitachi HD44780 (o compatible) por instrucciones es el método recomendado y más confiable para configurar la pantalla para su correcto funcionamiento. Consiste en enviar una secuencia específica de comandos (instrucciones) al controlador de la LCD inmediatamente después de encenderla. Esta secuencia asegura que la LCD esté en el modo de interfaz deseado (4 u 8 bits), configurada para el número correcto de líneas, con la fuente de caracteres adecuada y con la visualización controlada según las necesidades de la aplicación.
#### Secuencia Típica de Inicialización por Instrucciones (Ejemplo para Modo de 4 Bits y 2 Líneas):
La secuencia exacta puede variar ligeramente dependiendo del fabricante del módulo LCD y las necesidades específicas de la aplicación, pero la siguiente es una secuencia común para inicializar una LCD en modo de 4 bits y 2 líneas:
##### 1. Retardo después de la alimentación: Esperar un tiempo suficiente después de aplicar la alimentación para que la LCD se estabilice (generalmente 15 ms o más, consultar la hoja de datos).
##### 2. Secuencia de inicialización en modo de 8 bits (para asegurar la transición a 4 bits): Enviar tres veces el comando 0x30 o 0x38 (Function Set para modo de 8 bits) con pequeños retardos entre cada envío (ej: 4.1 ms, 100 µs). Esto asegura que la LCD entre en un estado conocido, incluso si inicialmente estaba en modo de 4 bits debido a condiciones de alimentación.
##### 3. Cambio al modo de 4 bits: Enviar el comando 0x20 o 0x28 (Function Set para modo de 4 bits). Generalmente se envía una sola vez.
##### 4. Configuración de la función (modo de 4 bits, número de líneas, fuente de caracteres): Enviar el comando 0x28 (4 bits, 2 líneas, fuente de caracteres de 5x8 puntos). Para una pantalla de 1 línea, se usaría 0x20. Para una fuente de 5x10 puntos (si es compatible), se podría usar 0x2C.
##### 5. Control de la pantalla (encender la pantalla, cursor, parpadeo): Enviar el comando 0x0C (Display ON, Cursor OFF, Blink OFF). Puedes usar otras opciones como 0x0E (Cursor ON) o 0x0F (Cursor ON, Blink ON) según lo desees.
##### 6. Modo de entrada (dirección del cursor, desplazamiento de la pantalla): Enviar el comando 0x06 (Incrementar la dirección del cursor, no desplazar la pantalla). 0x04 decrementaría la dirección. 0x07 o 0x05 habilitarían el desplazamiento.
##### 7. Borrar la pantalla: Enviar el comando 0x01 (Clear Display). Esto también posiciona el cursor en la dirección de inicio (0,0).
##### 8. Retorno al inicio: Enviar el comando 0x02 (Return Home). Mueve el cursor a la posición inicial.
#### Ejemplo 
##### ![image](https://github.com/user-attachments/assets/81d83fc1-0296-450d-8097-e24fac981530)
### _15. Set de instrucciones_
Las instrucciones o comandos que pueden ser utilizados por un microcontrolador o microprocesador externo para programar el LCD se pueden agrupar en cuatro tipos:
#### 1. Instrucciones para establecer funciones del LCD como el formato del display o la longitud de los datos.
#### 2. Instrucciones para direccionar la RAMs internas
#### 3. Instrucciones para transferir datos desde/a las RAMs internas.
#### 4. Otras Instrucciones.
## **Conclusión**
### A lo largo de este corte comenzamos comprendiendo el efecto rebote en interruptores y sus soluciones en microcontroladores. Luego, analizamos los teclados matriciales y sus diversas tipologías. Nos adentramos en el funcionamiento y la utilidad de los módulos temporizadores y contadores, diferenciando su propósito y la influencia del tamaño del bus frente a la resolución del timer.
### Exploramos la vital función del sistema de reloj, tanto interno como externo, y el concepto de granularidad en el control de los recursos del microcontrolador. Finalmente, nos sumergimos en detalle en las pantallas LCD Hitachi HD44780, abarcando sus señales de alimentación, control y datos, la organización y función de sus memorias (DDRAM, CGROM, CGRAM), sus registros internos, el crucial Busy Flag y el Address Counter, culminando en la comprensión de los procesos de inicialización por reset interno e instrucciones.
### En definitiva, este corte nos ha proporcionado una visión integral de estos componentes y conceptos, sentando una base firme para comprender y trabajar con sistemas embebidos y periféricos de visualización como la popular LCD HD44780. La interconexión de estos elementos, desde la gestión del tiempo y la entrada de datos hasta la visualización de información, ilustra la complejidad y la riqueza del diseño de sistemas electrónicos digitales.
## Referencias
#### [https://aulas.ecci.edu.co/mod/resource/view.php?id=217938](https://aulas.ecci.edu.co/mod/resource/view.php?id=217957)
#### [https://aulas.ecci.edu.co/mod/resource/view.php?id=217940](https://aulas.ecci.edu.co/mod/resource/view.php?id=217961)
#### [https://aulas.ecci.edu.co/mod/resource/view.php?id=217945](https://aulas.ecci.edu.co/mod/resource/view.php?id=217964)
#### Datasheet TECLADO MATRICIAL DEMEMBRANA 4X4 OKY0272: https://agelectronica.lat/pdfs/textos/O/OKY0272.PDF
#### Datasheet LCD Hitachi HD44780: https://www.alldatasheet.com/datasheet-pdf/pdf/63673/HITACHI/HD44780.html
#### Datasheet pic18f4550: https://www.microchip.com/
