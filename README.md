_Jhon Trujillo, Juan Ospina y Heissen_
# **Apuntes tercer corte**
## **Semana 12**
### _1. Modulo de captura_
El módulo de captura es un bloque de hardware dentro del microcontrolador diseñado para medir el tiempo entre eventos o la duración de pulsos. Trabaja en estrecha colaboración con un temporizador (generalmente Timer1 o Timer3 en muchos microcontroladores PIC, por ejemplo, ademas en algunos casos el modulo de captura puede asociarse a eventos internos, por ejemplo en el Atmega16 el modulo de captura puede asociarse al conversor A/D
#### ¿Cómo funciona?
##### **1. Configuración del temporizador:** Antes de usar el modo de captura, se debe configurar un temporizador asociado. Este temporizador se ejecuta continuamente, incrementando su valor.
##### **2. Configuración del pin de entrada:** Se selecciona un pin de entrada específico del microcontrolador (por ejemplo, RC2/CCP1 en muchos PICs) que será monitoreado por el módulo de captura. Este pin debe configurarse como entrada.
##### **3. Detección del evento:** El módulo de captura se configura para detectar un tipo específico de evento en el pin de entrada. Estos eventos pueden ser:
##### **• Flanco de subida:** Cuando la señal en el pin pasa de un nivel bajo a un nivel alto (0 a 1).
##### **• Flanco de bajada:** Cuando la señal en el pin pasa de un nivel alto a un nivel bajo (1 a 0).
##### **• Múltiples flancos:** Algunos módulos más avanzados pueden capturar después de un cierto número de flancos (por ejemplo, 4 o 16 flancos de subida).
##### **4. Captura del valor del temporizador:** En el instante en que se detecta el evento configurado, el módulo de captura automáticamente copia el valor actual del temporizador (que es un registro de 16 bits, por ejemplo, TMR1) en un registro interno del módulo de captura (por ejemplo, CCPRxH:CCPRxL, donde "x" se refiere al número del módulo CCP, como CCP1 o CCP2). Esta copia se realiza de forma instantánea y precisa a nivel de hardware, lo que lo diferencia de las interrupciones normales donde el microcontrolador debe primero ejecutar la interrupción y luego leer el temporizador, lo que introduce un pequeño retardo.
##### **5. Generación de interrupción (opcional):** Una vez que el valor del temporizador ha sido capturado, el módulo de captura puede generar una interrupción. Esta interrupción notifica al microcontrolador que se ha producido una captura, permitiendo que el software lea el valor capturado y lo procese.
##### **6. Procesamiento del valor capturado:** El microcontrolador lee el valor almacenado en los registros del módulo de captura. Este valor representa el conteo del temporizador en el momento exacto del evento. Al analizar la diferencia entre dos valores capturados (por ejemplo, el valor capturado en un flanco de subida y el valor capturado en el siguiente flanco de subida), se puede determinar el período de una señal, o la duración de un pulso.
##### ![image](https://github.com/user-attachments/assets/bef765b6-72d1-47ac-a181-932f6b29365a)
### _2. Granuralidad del timer_
La granularidad del timer se refiere a la resolución o precisión mínima con la que un temporizador puede medir el tiempo. Es decir, es el tamaño del "paso" o "tick" individual del temporizador. En el contexto de un microcontrolador:
##### • Es el tiempo que transcurre entre dos incrementos consecutivos del contador del temporizador.
##### • Está directamente relacionada con la frecuencia de reloj que alimenta el temporizador y el prescaler configurado.
#### ¿Cómo funciona la Granularidad del Timer?
Su funcionamiento se describe en los siguentes 4 apartados:
##### **1. "System Clock Ticks" (Ticks del reloj del sistema):** Son los pulsos de reloj de la fuente de tiempo más rápida disponible para el microcontrolador. Estos pulsos son los más finos.
##### **2. "Timer Ticks" (Ticks del timer):** Estos son los incrementos reales del contador del temporizador. La distancia entre dos "timer ticks" es la granularidad. Si se usa un prescaler, varios "system clock ticks" son necesarios para producir un solo "timer tick". Por ejemplo, si el prescaler es 1:2, se necesitan 2 "system clock ticks" para que el "timer tick" se produzca y el "counter value" se incremente.
##### **3. "Counter Value" (Valor del contador):** Es el valor actual del registro del temporizador. Este valor solo cambia en los "timer ticks".
##### **4. "Timestamp" (Marca de tiempo):** Cuando ocurre un evento (como un cambio en una señal de entrada en un módulo de captura), el valor actual del "counter value" se "sella" o "captura" como una marca de tiempo.
#### ![image](https://github.com/user-attachments/assets/cd82eeb6-9172-482d-96c7-6fb9c70c171a)
### _3. Tienpos de lectura_
Son los intervalos y la secuencia de eventos que ocurren desde que un evento externo (como un flanco en un pin) es detectado hasta que el microcontrolador puede procesar su marca de tiempo asociada. Involucran:
#### **1. Ocurrencia del Evento (ev1, ev2):** El momento físico en el que la señal de entrada cambia.
#### **2. Captura de la Marca de Tiempo (TS1, TS2):** El hardware del módulo de captura toma una "instantánea" del valor del temporizador en el momento exacto del evento. Esta es una operación atómica y muy precisa.
#### **3. Generación de Interrupción (set IF):** Una vez que la marca de tiempo es capturada, el módulo establece un flag de interrupción (por ejemplo, CCPxIF).
#### **4. Llamada a la Rutina de Servicio de Interrupción (call ISR):** El microcontrolador detiene su ejecución principal y salta a la ISR si las interrupciones están habilitadas y el flag de interrupción está activo.
#### **5. Limpieza del Flag de Interrupción (clear IF):** Dentro de la ISR, el programador debe borrar manualmente el flag de interrupción para indicar que el evento ha sido atendido y permitir futuras interrupciones del mismo módulo.
#### **6. Lectura del Valor Capturado (read TSx):** Dentro de la ISR, el microcontrolador lee el valor almacenado en los registros del módulo de captura (por ejemplo, CCPRx).
#### ![image](https://github.com/user-attachments/assets/a91fb71f-7a6a-4b30-8519-904ed028b7de)
### _4. Modulo de comparacion_
En el contexto de los microcontroladores (como el PIC18F4550), el módulo de comparación es otro modo de operación de los versátiles módulos CCP (Captura/Comparación/PWM). A diferencia del modo de captura, que registra el valor de un temporizador cuando ocurre un evento externo, el modo de comparación se encarga de generar un evento cuando el valor de un temporizador coincide con un valor predefinido por el usuario.Es decir, basicamente es un hardware dentro del microcontrolador que monitoriza continuamente el valor de un temporizador (generalmente Timer1 o Timer3 en muchos PICs) y lo compara con un valor de referencia almacenado en un registro específico (el registro CCPRx). Cuando estos dos valores coinciden, el módulo puede:
##### 1. Generar una interrupción.
##### 2. Actuar directamente sobre un pin de salida.
##### 3. Reiniciar el temporizador asociado.
#### ¿Cómo funciona el Módulo de Comparación?
El funcionamiento del módulo de comparación se basa en una comparación constante entre el temporizador y un valor de umbral.
##### **1. Configuración del Temporizador:**
##### • Al igual que en el modo de captura, se debe configurar un temporizador (Timer1 o Timer3 en el PIC18F4550) para que cuente a una velocidad deseada. Este temporizador debe estar en funcionamiento.
##### • Los bits T3CCP2 y T3CCP1 en el registro T3CON se utilizan para seleccionar cuál temporizador (Timer1 o Timer3) será la fuente para los módulos CCP1 y CCP2.
##### **2. Configuración del Valor de Comparación:**
##### • El usuario carga un valor de 16 bits en el registro CCPRx (formado por CCPRxH y CCPRxL). Este es el valor con el que se comparará el temporizador.
##### • Por ejemplo, si Timer1 está contando y se configura CCPR1 a 1000, el módulo de comparación esperará a que Timer1 llegue a 1000.
##### **3. Configuración del Pin de Salida CCP (Opcional, pero común):**
##### •. El pin asociado al módulo CCP (RC2 para CCP1, RC1 o RB3 para CCP2) debe configurarse como salida digital en el registro TRISC o TRISB si se desea que el módulo actúe sobre este pin.
##### **4. Configuración del Modo de Comparación:**
##### • El registro CCPxCON (por ejemplo, CCP1CON o CCP2CON) es fundamental. Los bits CCPxM<3:0> definen el modo de comparación y la acción a realizar cuando ocurre una coincidencia:
###### • 1000: Generar interrupción cuando TMRx (temporizador) coincide con CCPRx. El pin CCPx no se ve afectado.
###### • 1001: Setear (poner a nivel alto) el pin CCPx en la coincidencia.
###### • 1010: Limpiar (poner a nivel bajo) el pin CCPx en la coincidencia.
###### • 1011: Generar interrupción y setear el pin CCPx en la coincidencia.
###### • 1100: Generar interrupción y limpiar el pin CCPx en la coincidencia.
###### • 1101: Generar interrupción y alternar el estado del pin CCPx en la coincidencia.
###### • 1110: Generar interrupción y reiniciar el Timerx (Timer1 o Timer3) en la coincidencia.
###### • 1111: Generar interrupción, reiniciar el Timerx y alternar el estado del pin CCPx en la coincidencia.
##### **5. Funcionamiento Continuo:**
##### • El hardware del módulo de comparación compara constantemente el valor del temporizador (TMRx) con el valor en el registro CCPRx.
##### • Cuando TMRx == CCPRx (coincidencia):
###### • La acción configurada en CCPxCON se ejecuta (por ejemplo, cambiar el estado del pin CCPx, generar una interrupción).
###### • El bit de bandera de interrupción CCPxIF (en PIR1) se establece a '1'.
##### **6. Manejo de Interrupción (Opcional):**
##### • Si se configuró para generar una interrupción, el microcontrolador saltará a la ISR.
##### • Dentro de la ISR, el programador debe borrar el bit CCPxIF para permitir futuras interrupciones de comparación.
##### • Si el modo configurado incluye reiniciar el temporizador, el temporizador se reinicia automáticamente en la coincidencia, lo que facilita la generación de eventos periódicos. Si no, el temporizador seguirá contando y eventualmente desbordará.
##### ![image](https://github.com/user-attachments/assets/82ec42a4-3d49-4050-91ce-f2704d163741)
### _5. Modulo PWM_
El módulo PWM (Pulse Width Modulation), o Modulación por Ancho de Pulso, es una técnica muy utilizada en microcontroladores para generar una señal digital de onda cuadrada cuya relación entre el tiempo que está en estado alto (ON) y el tiempo que está en estado bajo (OFF) es variable. Esto permite simular una salida analógica o controlar la potencia entregada a una carga de manera eficiente. Es decir, es un periférico de hardware especializado dentro del microcontrolador que se encarga de crear automáticamente señales PWM en uno o más pines de salida. Estas señales tienen dos características principales:
##### **1. Frecuencia (Período):** Es el inverso del período y se refiere a la rapidez con la que la onda cuadrada se repite. Esta es constante en una señal PWM.
##### **2. Ciclo de Trabajo (Duty Cycle):** Es el porcentaje del período total en el que la señal se mantiene en estado alto. Este es el parámetro que se varía para controlar la potencia o el valor "analógico" equivalente.
#### ¿Cómo funciona el Módulo PWM?
El funcionamiento del módulo PWM se basa principalmente en la interacción de un temporizador, un registro de período y un registro de ciclo de trabajo. En los microcontroladores PIC, el módulo PWM suele estar asociado con el Timer2. Eston son los pasos y registros clave involucrados en el PIC18F4550:
##### 1. Selección y Configuración del Temporizador (Timer2):
##### • El módulo PWM en el PIC18F4550 utiliza el Timer2 como su base de tiempo. El Timer2 es un temporizador de 8 bits.
##### • Se configura el Timer2 con un prescaler adecuado (T2CON bits T2CKPS) y se habilita.
##### • **Registro PR2:** Este registro (8 bits) define el período de la señal PWM. El Timer2 cuenta desde 0 hasta el valor de PR2. Una vez que TMR2 alcanza PR2, se reinicia a 0 y el ciclo PWM se completa.
##### 2. Configuración del Ciclo de Trabajo (Duty Cycle):
##### • Se utiliza un registro de 10 bits para definir el ancho del pulso alto. En el PIC18F4550, este valor se carga en los registros CCPRxL (los 8 bits menos significativos) y los dos bits menos significativos de CCPxCON (DCxB1:DCxB0, bits 5 y 4).
##### • El valor del ciclo de trabajo se compara continuamente con el contador del Timer2.
##### • Cuando el Timer2 se incrementa de 0, el pin de salida PWM se pone en ALTO.
##### • Cuando el Timer2 alcanza el valor del ciclo de trabajo (CCPRxL + DCxB1:DCxB0), el pin de salida PWM se pone en BAJO.
##### 3. Configuración del Pin de Salida PWM:
##### • El pin asociado al módulo CCP (RC2 para CCP1, RC1 o RB3 para CCP2) debe configurarse como salida digital en el registro TRISC o TRISB.
##### • El módulo CCP se encarga de alternar el estado de este pin de acuerdo con la lógica de PWM.
##### 4. Configuración del Modo PWM:
##### • El registro CCPxCON (por ejemplo, CCP1CON o CCP2CON) es crucial. Los bits CCPxM<3:0> deben configurarse para el modo PWM:
###### • 11xx: Estos bits activan el modo PWM. Los bits 'xx' son los dos bits menos significativos del ciclo de trabajo.
##### 5. Funcionamiento Automático:
##### • Una vez configurado, el módulo PWM opera de forma autónoma a nivel de hardware. El microcontrolador solo necesita cargar los valores de PR2 y del ciclo de trabajo (CCPRxL y CCPxCON bits DCxB) una vez, o cambiarlos dinámicamente para variar la señal PWM.
##### • El Timer2 cuenta hasta PR2, reinicia, y en cada ciclo, el pin PWM se mantiene en alto durante el tiempo definido por el ciclo de trabajo y en bajo el resto del período.
##### ![image](https://github.com/user-attachments/assets/c642f423-47a4-41bd-bd58-36f21f98f0fe)
##### ![image](https://github.com/user-attachments/assets/c5c389c4-a0a8-4d28-96b7-637dbc2e3caa)
### _6.Ciclo util_
El ciclo útil, o Duty Cycle (DC), es una medida que describe la proporción de tiempo que una señal de pulso está "encendida" (ON) durante un período completo. Se expresa generalmente como un porcentaje.
#### ![image](https://github.com/user-attachments/assets/ad39df30-269a-4993-95d1-8fbfd857b1f6)
La imagen muestra la fórmula y tres ejemplos visuales:
#### ![image](https://github.com/user-attachments/assets/71788300-4257-4090-b1c2-44372998f046)
O, de manera equivalente:
#### ![image](https://github.com/user-attachments/assets/25713133-a3be-4ef8-a63f-9570dc32e5b6)
Donde:
#### • _TON_ es el tiempo que el pulso está en estado "alto" (ON).
#### • _TOFF_es el tiempo que el pulso está en estado "bajo" (OFF).
#### • T es el período total de la señal (T=_TON_ + _TOFF_).
Ejemplos Visuales:
##### • 10% Duty Cycle: El pulso está en estado "ON" por un tiempo muy corto en comparación con el período total. La señal pasa la mayor parte del tiempo en estado "OFF".
##### • 50% Duty Cycle: El pulso está en estado "ON" exactamente la mitad del tiempo y en estado "OFF" la otra mitad. Esto genera una onda cuadrada simétrica.
##### • 90% Duty Cycle: El pulso está en estado "ON" durante la mayor parte del período total y solo un corto tiempo en estado "OFF".
#### Funcionamiento en un PIC (Módulo PWM):
En un microcontrolador PIC, el módulo PWM te permite generar estas señales. Tú configuras:
##### • **La frecuencia del PWM:** Esto determina el período T de la señal.
##### • **El ciclo útil:** Esto determina el valor de _TON_ dentro de ese período. El PIC ajustará automáticamente el tiempo que el pin de salida está en alto.
El PWM se utiliza para controlar la potencia entregada a una carga (como motores, LEDs, fuentes de alimentación conmutadas) de manera eficiente, ya que la potencia se varía cambiando el tiempo ON/OFF en lugar de la tensión o corriente de forma lineal.
### _7.Valor promedio_
El valor promedio de una señal periódica, como la señal PWM, es el valor constante que produciría el mismo efecto que la señal variable si se aplicara durante el mismo período. Para una señal de pulso rectangular (como la que genera el PWM), el valor promedio es una forma de determinar el "equivalente" de CD (Corriente Directa) de la señal pulsante.
#### ![image](https://github.com/user-attachments/assets/6c341cf8-d23f-41d0-9725-ec2cbee5ec03)
La imagen detalla cómo se calcula el valor promedio (V _AV_) para una señal de pulso con amplitud A:
#### ![image](https://github.com/user-attachments/assets/2cac6833-c5d4-4bbe-ba87-5853be683eeb)
Donde v(t) es la función que describe la señal en el tiempo y T es el período. Al desarrolar la integral el resultado final es:
#### ![image](https://github.com/user-attachments/assets/bb29fd45-6311-4f95-9fcb-787de5b769c6)
#### Relación con el Duty Cycle:
Si observas, la expresión _TON_/T, es precisamente el ciclo útil (DC) expresado como una fracción (no como porcentaje), por lo tanto:
#### ![image](https://github.com/user-attachments/assets/cb4ab9d7-25ae-4f53-abaa-800b960f2234)
Donde DC _fraccional_ es el ciclo útil dividido por 100.
#### Funcionamiento en un PIC (Módulo PWM):
El valor promedio es crucial porque es lo que efectivamente "siente" la carga conectada al pin PWM.
##### • Cuando controlas la velocidad de un motor DC con PWM, lo que estás haciendo es variar el valor promedio de la tensión aplicada al motor. Un ciclo útil del 50% aplica una tensión promedio que es la mitad de la tensión máxima (A), lo que hace que el motor gire a una velocidad media. Un ciclo útil del 90% aplica una tensión promedio cercana a la máxima, y el motor gira casi a máxima velocidad.
##### • De manera similar, para controlar el brillo de un LED, un ciclo útil alto resulta en un brillo mayor (más valor promedio de corriente), y un ciclo útil bajo en un brillo menor.
### _8.Ejemplo módulo CCP_
Para configurar correctamente el modúlo CCP, debemos:
#### 1. **Configurar el periodo del PWM:** Este paso establece el período de la señal PWM, lo que a su vez define su frecuencia. El período se controla mediante el registro PR2, que es el límite superior del Timer2.
##### ![image](https://github.com/user-attachments/assets/3921eaec-3c23-4070-a37b-066d1b12c69e)
#### 2. **Configurar ciclo util:** Este paso define la duración del pulso "ON" dentro de cada período del PWM, es decir, el Duty Cycle. El módulo PWM en los PICs típicamente ofrece una resolución de 10 bits para el ciclo útil. Estos 10 bits se almacenan en dos lugares: los 8 bits menos significativos en el registro CCPRxL (donde x es el número del módulo CCP, e.g., CCP1) y los 2 bits más significativos en un par de bits dentro del registro CCPxCON.
##### ![image](https://github.com/user-attachments/assets/9665195f-664b-49a2-9dd1-5a2a1aab8b4a)
#### 3. **Configurar los pines:** Para que el pin de salida del PWM funcione correctamente, debe configurarse como una salida digital. Los pines de los microcontroladores PIC tienen registros de dirección (TRISx) que controlan si un pin es entrada (1) o salida (0).
##### ![image](https://github.com/user-attachments/assets/cbe2c518-aa04-4e5b-a062-24a70231ecfc)
#### 4. **Configurar el prescaler:** Este paso configura el pre-escalador del Timer2, que es esencial para controlar la velocidad a la que el Timer2 incrementa y, por lo tanto, la frecuencia final del PWM.
##### ![image](https://github.com/user-attachments/assets/850fe1ee-81fd-45cc-b4dc-d92994f9ae37)
#### 5. **Configurar el modo PWM:** Este es el paso final para habilitar el módulo PWM y ponerlo en funcionamiento.
##### ![image](https://github.com/user-attachments/assets/c3c6331b-18fc-441d-a9ec-3086c627e5f1)

## **Semana 13**
### _1. Comunicacion serial_
La comunicación serial es un método de transferencia de datos en el que la información se envía un bit a la vez, de forma secuencial, a través de un único canal o línea de datos. Es lo opuesto a la comunicación paralela, donde varios bits se envían simultáneamente a través de múltiples líneas.
#### ¿Qué es la Comunicación Serial?
Imagina que tienes una palabra para enviar. En la comunicación paralela, enviarías todas las letras de la palabra al mismo tiempo (si tuvieras suficientes manos o cables). En la comunicación serial, enviarías la primera letra, luego la segunda, luego la tercera, y así sucesivamente, una detrás de otra, por el mismo "conducto". Este método es extremadamente común en electrónica y sistemas embebidos debido a varias ventajas:
##### • **Menor número de cables:** Solo se necesita una línea de datos (o muy pocas, como una de datos y una de reloj) para transmitir la información, lo que reduce la complejidad del cableado, el costo y el tamaño.
##### • **Mayor distancia:** La menor cantidad de cables y la naturaleza secuencial de la transmisión hacen que la comunicación serial sea más robusta para distancias largas, ya que hay menos problemas de sincronización entre múltiples líneas.
##### • **Menor susceptibilidad al ruido:** Menos líneas significan menos oportunidades para la interferencia.
#### ¿Cómo funciona la Comunicación Serial?
Aunque existen muchos protocolos de comunicación serial (UART/USART, SPI, I2C, USB, Ethernet, RS-232, RS-485, etc.), el principio fundamental es el mismo: los bits se envían uno tras otro. Para que la comunicación sea exitosa, tanto el emisor como el receptor deben estar de acuerdo en ciertas reglas o parámetros.
##### ![image](https://github.com/user-attachments/assets/0c0b4e68-9cc2-4809-8ffc-0d9ffb864c44)

### _2. Comunicacion paralela_
La comunicación paralela es un método de transferencia de datos en el que múltiples bits de información se envían simultáneamente a través de varias líneas o canales de datos. A diferencia de la comunicación serial, donde los bits se envían uno a la vez de forma secuencial, en la comunicación paralela se transmiten grupos de bits (por ejemplo, un byte completo, 8 bits) en un solo ciclo de reloj.

#### ¿Qué es la Comunicación Paralela?
Para entenderlo de forma sencilla, volvamos al ejemplo de enviar una palabra:
##### • **Comunicación Serial:** Enviarías cada letra una por una, a través del mismo conducto.
##### • **Comunicación Paralela:** Si la palabra tiene 8 letras y tienes 8 conductos, puedes enviar las 8 letras al mismo tiempo, cada una por un conducto diferente.
Este método fue muy común en las primeras computadoras y en interfaces internas de dispositivos debido a su velocidad inherente en distancias cortas.
#### ¿Cómo funciona la Comunicación Paralela?
Para que la comunicación paralela funcione, se requieren los siguientes elementos básicos:
##### 1. **Múltiples Líneas de Datos:** Se necesita una línea de datos dedicada por cada bit que se desea transmitir simultáneamente. Por ejemplo, para transmitir un byte (8 bits) en paralelo, se requieren al menos 8 líneas de datos.
##### 2. **Líneas de Control (Opcional, pero común):** Además de las líneas de datos, a menudo se utilizan una o más líneas de control para gestionar el flujo de datos y la sincronización entre el emisor y el receptor. Ejemplos de líneas de control incluyen:
##### • **STROBE (Estroboscópica):** Una señal del emisor para indicar al receptor que los datos en las líneas están listos para ser leídos.
##### • **ACK (Acknowledge - Reconocimiento):** Una señal del receptor al emisor para confirmar que los datos han sido recibidos.
##### • **BUSY (Ocupado):** Una señal del receptor para indicar que no puede aceptar más datos por el momento.
##### • **RESET:** Para inicializar la comunicación.
##### 3. **Sincronización:** En la comunicación paralela, es crucial que todos los bits de un grupo lleguen al receptor al mismo tiempo y que el receptor sepa cuándo leerlos. Esto se logra a menudo con una señal de reloj común o con señales de control de "handshaking" (apretón de manos) que coordinan el emisor y el receptor.
#### Proceso Básico de Transmisión Paralela:
##### 1. Preparación de Datos: El emisor coloca el conjunto de bits (por ejemplo, un byte) en las líneas de datos correspondientes.
##### 2. Señalización de Disponibilidad: El emisor activa una línea de control (como STROBE) para indicar al receptor que los datos están disponibles.
##### 3. Lectura de Datos: El receptor, al detectar la señal STROBE, lee simultáneamente todos los bits presentes en las líneas de datos.
##### 4. Reconocimiento (Handshaking - Opcional): El receptor puede enviar una señal de ACK al emisor para confirmar que los datos han sido recibidos. Esto permite al emisor saber cuándo puede enviar el siguiente conjunto de datos.
##### ![image](https://github.com/user-attachments/assets/89fbbe81-7cce-4b50-a8ed-78df0a184a34)

### _3. Modos de transmision_
Los modos de transmisión describen la dirección en la que la información puede fluir entre dos dispositivos o puntos en un sistema de comunicación.
#### 1. Simplex
##### ![image](https://github.com/user-attachments/assets/2891788c-bd45-4e9e-aa17-39f720079c5a)
##### • Descripción: Es un modo de comunicación unidireccional, lo que significa que los datos fluyen en una sola dirección, desde el transmisor (Tx) hacia el receptor (Rx), y nunca en la dirección opuesta.
##### • Funcionamiento: Un dispositivo siempre actúa como emisor y el otro siempre como receptor. No hay capacidad para que el receptor envíe una respuesta o datos de vuelta al emisor.
##### • Ventajas: Es el modo más simple y económico de implementar, ya que solo se necesita una dirección de flujo de datos.
##### • Desventajas: No permite retroalimentación ni comunicación bidireccional.
#### 2. Half Duplex (Medio Dúplex)
##### ![image](https://github.com/user-attachments/assets/7eee6f93-c7f1-455e-825f-df7173e77e77)
##### • Descripción: Es un modo de comunicación bidireccional, pero no simultánea. Los datos pueden fluir en ambas direcciones entre dos dispositivos (Tx/Rx), pero solo uno puede transmitir a la vez; el otro debe esperar su turno para recibir y luego responder.
##### • Funcionamiento: Los dispositivos se turnan para enviar y recibir. Existe un proceso de "negociación" o control para determinar qué dispositivo tiene acceso al medio de transmisión en un momento dado. Si ambos intentaran transmitir al mismo tiempo, se produciría una colisión y los datos se corromperían.
##### • Ventajas: Permite la comunicación bidireccional utilizando un solo par de líneas de transmisión (o un solo canal), lo que lo hace más eficiente en el uso de recursos que el Full Duplex para ciertas aplicaciones.
##### • Desventajas: La velocidad efectiva de transferencia de datos es menor que en Full Duplex porque los dispositivos tienen que esperar su turno. Puede haber latencia debido a la negociación del acceso al medio.
#### 3. Full Duplex (Dúplex Completo)
##### ![image](https://github.com/user-attachments/assets/88beea3e-ce1b-4014-9391-f0e164c683c4)
##### • Descripción: Es el modo de comunicación más avanzado, permitiendo la transferencia de datos bidireccional y simultánea. Ambos dispositivos (Tx/Rx) pueden transmitir y recibir datos al mismo tiempo, sin interrupción.
##### • Funcionamiento: Requiere dos canales de comunicación separados o una técnica de multiplexación (como la división de tiempo o frecuencia) que permita el envío y la recepción concurrente. Esto generalmente implica un par de líneas para la transmisión en una dirección y otro par para la transmisión en la dirección opuesta.
##### • Ventajas: Ofrece la mayor velocidad de transferencia de datos efectiva, ya que no hay tiempos de espera para que los dispositivos se turnen. Es ideal para aplicaciones que requieren un flujo constante de datos en ambas direcciones.
##### • Desventajas: Requiere más recursos (más cables o canales de transmisión más complejos) en comparación con Simplex y Half Duplex.
La elección del modo de transmisión depende de los requisitos de la aplicación en términos de dirección del flujo de datos, velocidad, complejidad del cableado y costo.
### _4. Tipos de comunicación_
#### Comunicación Síncrona:
##### • Requiere una señal de reloj común (o mecanismos de reloj embebidos en los datos) entre el emisor y el receptor para sincronizar la transferencia de bits.
Los datos se envían en bloques o tramas grandes.
##### • Ventajas: Más rápida y eficiente para grandes volúmenes de datos, ya que no hay bits de inicio/parada por cada byte.
##### • Desventajas: Más compleja de implementar debido a la necesidad de sincronización de reloj, puede requerir más líneas.
##### • Ejemplos: SPI, I2C, USB (con sus complejidades), Ethernet, bus PCIe.
#### Comunicación Asíncrona:
##### • No requiere una señal de reloj compartida. Cada paquete de datos (generalmente un byte) se enmarca con bits de inicio y parada para que el receptor sepa cuándo empiezan y terminan los datos.
##### • Ambos dispositivos deben estar configurados a la misma velocidad de transmisión (baud rate) para que el receptor pueda muestrear los bits correctamente.
##### • Ventajas: Más sencilla de implementar (solo una línea de datos y GND), flexible para diferentes velocidades.
##### • Desventajas: Menos eficiente para grandes volúmenes de datos debido al overhead de los bits de inicio/parada en cada byte.
##### • Ejemplos: UART (RS-232), comunicación en serie con terminales.
### _5. Topología de comunicación_
#### 1. Topología de Bus
##### • En una topología de bus, hay un único canal de comunicación compartido (el "bus" o cable principal) al que todos los dispositivos están directamente conectados. Piensa en ello como una autopista de un solo carril por donde todos los vehículos (datos) tienen que pasar.
##### • Múltiples dispositivos: La clave es que más de un dispositivo puede estar conectado a este mismo medio compartido. Esto es eficiente en términos de cableado porque no necesitas un cable separado para cada par de dispositivos.
##### • Sistema de direccionamiento: Debido a que el medio es compartido, cuando un dispositivo transmite datos, esos datos viajan a lo largo de todo el bus y son "vistos" por todos los demás dispositivos. Por lo tanto, es esencial un sistema de direccionamiento. Esto significa que cada paquete de datos debe incluir la dirección del dispositivo de destino. Cada dispositivo en el bus debe monitorear el bus y, al recibir un paquete, verificar si la dirección de destino coincide con la suya. Si coincide, procesa el paquete; de lo contrario, lo ignora.
##### • Acceso al medio: Como el bus es un recurso compartido, se necesitan protocolos de acceso al medio (como CSMA/CD en las antiguas redes Ethernet de bus) para evitar colisiones (cuando dos o más dispositivos intentan transmitir al mismo tiempo, corrompiendo los datos). Si una colisión ocurre, los dispositivos deben detectar, detener la transmisión, esperar un tiempo aleatorio y reintentar.
#### 2. Topología Punto a Punto
##### • En una topología punto a punto, la comunicación se establece directamente entre exactamente dos dispositivos. Hay un enlace de comunicación exclusivo y dedicado entre ellos. Es como una línea telefónica directa entre dos personas.
##### • Comunicación exclusiva: No hay otros dispositivos conectados al mismo canal en ese segmento específico. La comunicación es solo entre esos dos extremos.
##### • No se requiere direccionamiento: Dado que solo hay dos participantes en la conexión, y cada uno sabe quién es el otro extremo, no es necesario incluir direcciones de destino en los paquetes de datos. El dispositivo que recibe los datos sabe automáticamente que provienen del único otro dispositivo al que está conectado en ese enlace punto a punto. Esto simplifica el protocolo y reduce el tamaño de los paquetes de datos.
##### • Ejemplos:
###### • Una conexión de módem tradicional a Internet.
###### • Un cable USB conectando una computadora a una impresora.
###### • Un sensor conectado directamente a un pin de microcontrolador (por ejemplo, con SPI o I2C si es solo un dispositivo en el bus).
###### • La comunicación entre la CPU y la memoria RAM (cada chip de RAM tiene una conexión dedicada o un conjunto de líneas dedicadas a la CPU en ese contexto específico).
### _6. Arquitectura de comunicación_
#### 1. Maestro-Esclavo (Master-Slave)
##### • En una arquitectura Maestro-Esclavo, existe una jerarquía clara. Un dispositivo es designado como el Maestro, y todos los demás dispositivos son Esclavos.
##### • Rol del Maestro: El dispositivo Maestro tiene el control total de la comunicación. Es el único que puede iniciar (comenzar) una comunicación, enviando comandos, solicitando datos o indicando a los esclavos qué hacer. También es quien generalmente termina la comunicación.
##### • Rol del Esclavo: Los dispositivos Esclavo solo pueden responder a las peticiones o comandos del Maestro. No pueden iniciar una comunicación por sí mismos; simplemente esperan a que el Maestro se dirija a ellos o les pida información.
##### • Flujo de Control: Esta arquitectura ofrece un flujo de datos muy controlado y predecible. Dado que solo el Maestro puede iniciar, se evitan conflictos o colisiones por múltiples dispositivos intentando hablar al mismo tiempo (a menos que el protocolo del Maestro lo permita y lo gestione adecuadamente).
##### • Ejemplos:
###### • SPI (Serial Peripheral Interface): Típicamente tiene un Maestro (por ejemplo, un microcontrolador) que controla uno o más Esclavos (por ejemplo, sensores, memorias EEPROM). El Maestro genera la señal de reloj e inicia las transferencias de datos.
###### • I2C (Inter-Integrated Circuit): Aunque puede tener múltiples Maestros, en un momento dado solo un Maestro está activo, controlando a varios Esclavos. El Maestro inicia la comunicación enviando una condición de inicio y la dirección del esclavo.
###### • Modbus (RTU/ASCII): Un protocolo industrial común donde un Maestro (por ejemplo, un PLC o una HMI) "consulta" (polls) a múltiples dispositivos Esclavo (por ejemplo, sensores inteligentes, actuadores).
#### 2. Todos Iguales (Peer-to-Peer / Any Equal)
En una arquitectura "Todos Iguales" o Punto a Punto (también conocida como distribuida o descentralizada), no hay un dispositivo Maestro único. Todos los dispositivos conectados al medio de comunicación tienen el potencial de iniciar la comunicación.
##### • Inicio de la Comunicación: Cualquier dispositivo puede empezar a enviar datos siempre y cuando el canal de comunicación (el "medio") no esté siendo utilizado por otro dispositivo en ese momento.
##### • Administración del Medio: Dado que varios dispositivos pueden querer iniciar la comunicación, es fundamental que exista un mecanismo para administrar el acceso al medio compartido. Esto es crucial para prevenir colisiones de datos y asegurar una comunicación ordenada. Esta "forma para administrar el medio" suele implicar:
###### • Detección/Evitación de Colisiones: Protocolos que detectan cuando dos dispositivos intentan transmitir simultáneamente (lo que resulta en una colisión) y cómo manejarla (por ejemplo, esperando un tiempo aleatorio y retransmitiendo).
###### • Paso de Testigo (Token Passing): Se pasa un "testigo" (un mensaje especial) alrededor de la red. Solo el dispositivo que tiene el testigo puede transmitir.
###### • Detección de Portadora (Carrier Sense): Los dispositivos "escuchan" el medio primero para ver si está libre antes de transmitir.
##### • Ejemplos:
###### • Ethernet estándar (CSMA/CD en medios compartidos): Los dispositivos "escuchan" el cable. Si está libre, transmiten. Si se detecta una colisión, detienen la transmisión y reintentan. (Las redes Ethernet modernas con switches actúan más como una colección de enlaces punto a punto full-duplex, pero el principio subyacente para medios compartidos es de igualdad de pares).
###### • Wi-Fi (CSMA/CA): Los dispositivos inalámbricos "escuchan" las ondas. Si están libres, transmiten. Si están ocupadas, esperan. También utilizan mecanismos de evitación de colisiones.
###### • CAN Bus (Controller Area Network): Los dispositivos transmiten cuando el bus está libre, y la resolución de colisiones se maneja por prioridad de mensaje.
###### • Redes peer-to-peer descentralizadas: Como algunas redes de intercambio de archivos o redes blockchain, donde todos los nodos tienen capacidades iguales para iniciar acciones.
### _7. Condiciones eléctricas_
#### 1. Terminación Sencilla (Single Ended):
##### • En la transmisión single-ended, la señal se envía a través de un único hilo conductor y la referencia de voltaje para esa señal es una tierra común compartida por el emisor y el receptor. La información se codifica como la diferencia de voltaje entre el hilo de la señal y esta tierra común.
##### • Problema con el ruido: A largas distancias, la tierra común puede no ser realmente "común" en ambos extremos. Las corrientes de retorno de otros circuitos, los campos electromagnéticos y las resistencias en el propio cable de tierra pueden hacer que el potencial de tierra varíe a lo largo del cable. Esto significa que el "0V" del emisor podría ser diferente al "0V" del receptor. Cualquier ruido inducido en el hilo de señal se suma directamente a la señal, alterando el voltaje y dificultando que el receptor distinga la señal original del ruido.
##### • Ejemplos: Comunicación paralela simple (como muchos pines GPIO de microcontroladores), RS-232 a cortas distancias.
#### 2. Diferencial: En la transmisión diferencial, la señal no se refiere a una tierra común, sino a la diferencia de voltaje entre dos hilos separados. Un hilo lleva la señal original (por ejemplo, V+) y el otro hilo lleva la señal invertida (V-), o simplemente una señal de referencia que no es tierra. El receptor mide la diferencia entre estos dos hilos.
##### • Ventaja contra el ruido: La gran ventaja de este método es su inmunidad al ruido. Si un campo electromagnético o una interferencia externa induce ruido en el cable, es muy probable que afecte a ambos hilos de la pareja diferencial de manera similar (lo que se conoce como "ruido en modo común"). Dado que el receptor solo se preocupa por la diferencia entre los dos hilos, el ruido que se suma por igual a ambos hilos se cancela o se reduce drásticamente, dejando la señal original intacta.
##### • Ejemplos: RS-485, CAN Bus, Ethernet (par trenzado), USB, HDMI, LVDS. Estos protocolos son ideales para comunicación a alta velocidad y/o largas distancias donde la integridad de la señal es crítica.
### _8. SCI (Serial communication Interface)_
Un SCI es un bloque de hardware dentro del microcontrolador diseñado para:
##### 1. Convertir datos paralelos en datos seriales para la transmisión.
##### 2. Convertir datos seriales en datos paralelos para la recepción.
Esto permite que el microcontrolador se comunique con otros dispositivos (como otros microcontroladores, PCs, sensores, módulos GPS, Bluetooth, etc.) utilizando solo dos líneas de datos (más una tierra común), lo que simplifica el cableado y reduce los costos.
#### ¿Cómo funciona SCI?
El funcionamiento del módulo SCI se basa en el principio de la comunicación serial asíncrona, similar al UART:
##### 1. Líneas de Comunicación:
##### • TxD (Transmit Data): Pin de salida del microcontrolador por donde se envían los datos.
##### • RxD (Receive Data): Pin de entrada del microcontrolador por donde se reciben los datos.
Además, se necesita una tierra (GND) común entre los dispositivos para establecer una referencia de voltaje.
##### 2. Configuración de la Velocidad (Baud Rate):
##### • Tanto el dispositivo emisor como el receptor deben estar configurados para operar a la misma velocidad de transmisión (baud rate). Esta velocidad define cuántos bits se transmiten o reciben por segundo (bits por segundo, bps).
##### • El SCI tiene un registro de divisor de baudios (baud rate generator) que se configura para generar la velocidad deseada a partir del reloj del sistema del microcontrolador.
##### 3. Formato de Trama (Frame Format):
##### • Los datos se envían en tramas o "frames", que son pequeños paquetes de bits. Cada trama generalmente consta de:
###### • Bit de Inicio (Start Bit): Un bit lógico '0' que indica el comienzo de una nueva trama. Esto ayuda al receptor a sincronizarse con el emisor.
###### • Bits de Datos: Los bits que representan la información real que se va a transmitir (comúnmente 7, 8 o 9 bits, siendo 8 bits lo más habitual). Se envían de forma secuencial, generalmente el bit menos significativo (LSB) primero.
###### • Bit de Paridad (Parity Bit - Opcional): Un bit adicional utilizado para la detección de errores. Puede ser par (si el número de '1's en los bits de datos es par) o impar (si es impar). El receptor verifica este bit para asegurar que los datos no se corrompieron durante la transmisión.
###### • Bit(s) de Parada (Stop Bit(s)): Uno o dos bits lógicos '1' que marcan el final de la trama. Proporcionan un período de inactividad para que el receptor procese el byte recibido antes de que llegue el siguiente.
###### • Estado Inactivo (Idle State): Cuando no se transmiten datos, la línea TxD/RxD se mantiene en estado alto (lógico '1').
##### 4. Transmisión (TX):
##### • Cuando el microcontrolador desea enviar un byte de datos, escribe ese byte en el registro de datos de transmisión (TXREG) del SCI.
##### • El hardware del SCI toma este byte paralelo, añade automáticamente el bit de inicio, el bit de paridad (si está configurado) y los bits de parada.
##### • Luego, serializa estos bits y los envía uno por uno a través del pin TxD a la velocidad configurada.
##### • Mientras un byte se está transmitiendo, el CPU puede continuar con otras tareas. Una vez que el registro de transmisión esté vacío, puede cargarse con el siguiente byte. A menudo, hay un registro de desplazamiento (shift register) que maneja la serialización mientras el TXREG puede cargarse con el siguiente byte.
##### 5. Recepción (RX):
##### • El SCI monitorea continuamente el pin RxD. Cuando detecta un cambio de un estado alto a un estado bajo (el bit de inicio), sabe que se está recibiendo una nueva trama.
##### • Utiliza el baud rate configurado para muestrear los bits entrantes en el momento adecuado dentro de cada "intervalo de bit".
##### • Los bits seriales (datos, paridad, parada) se ensamblan en un registro de desplazamiento de recepción.
##### • Una vez que se ha recibido una trama completa, el byte de datos se transfiere al registro de datos de recepción (RCREG).
##### • El módulo SCI también puede establecer banderas de error (por ejemplo, error de framing si el bit de parada no es el esperado, o error de overrun si un nuevo byte llega antes de que el anterior haya sido leído del RCREG).
##### 6. Interrupciones (Opcional, pero común):
##### • Para facilitar el manejo de los datos, el SCI puede generar interrupciones cuando:
###### • Un byte ha sido completamente transmitido (registro de transmisión vacío).
###### • Un byte ha sido completamente recibido (registro de recepción lleno).
###### • Se ha detectado un error.
##### • Las interrupciones permiten al microcontrolador ser notificado de estos eventos sin tener que estar constantemente "sondeando" los registros, liberando al CPU para otras tareas.
##### ![image](https://github.com/user-attachments/assets/3542372a-228f-4140-baa4-8c720b09fa27)
### _9. Parámetros configurables en el módulo UART_
Cuando se configura un módulo UART (o SCI) en un microcontrolador, hay varios parámetros que se deben establecer para asegurar que la comunicación sea correcta y compatible entre el emisor y el receptor. Estos parámetros definen cómo se codifican, transmiten y decodifican los datos. Aquí están los parámetros configurables más importantes en un módulo UART:
#### 1. Velocidad de Transmisión (Baud Rate):
##### • ¿Qué es? Es la velocidad a la que los bits individuales se transmiten por segundo, medida en bits por segundo (bps).
##### • ¿Cómo se configura? Se establece típicamente escribiendo un valor en un registro específico del módulo UART (por ejemplo, un registro SPBRG/SPBRGH en PICs, o registros UBRR en AVRs, o diferentes registros en ARM). Este valor, junto con la frecuencia del reloj del microcontrolador y la configuración de ciertos bits de control, determina la velocidad de baudios.
##### • Importancia: ¡Es el parámetro más crítico! El emisor y el receptor DEBEN estar configurados a la misma velocidad de baudios para que la comunicación sea inteligible. Si no coinciden, se recibirán datos corruptos.
##### • Valores comunes: 9600, 19200, 38400, 57600, 115200 bps, etc.
#### 2. Número de Bits de Datos (Data Bits / Word Length):
##### • ¿Qué es? Define cuántos bits de datos se transmiten en cada "carácter" o "palabra". Es el número de bits que representan la información útil.
##### • ¿Cómo se configura? Se establece mediante bits de control en los registros de configuración del UART (por ejemplo, un bit RX9/TX9 en PICs para habilitar 9 bits, o un campo UCSZn en AVRs).
##### • Valores comunes: 8 bits es el más frecuente. También se pueden usar 5, 6, 7 o 9 bits, dependiendo del microcontrolador y el protocolo.
#### 3. Bits de Parada (Stop Bits):
##### • ¿Qué es? Son bits adicionales (siempre lógicos '1') que se envían al final de cada trama de datos para indicar el final de la transmisión del carácter y proporcionar un tiempo de inactividad para que el receptor pueda procesar el byte.
##### • ¿Cómo se configura? Se establece mediante un bit en el registro de control del UART (por ejemplo, un bit STOP_SEL en algunos módulos, o un campo USBS en AVRs).
##### • Valores comunes: 1 bit de parada es el más común. También pueden configurarse 1.5 o 2 bits de parada, especialmente en sistemas más antiguos o con relojes menos precisos.
##### • Importancia: El receptor utiliza el bit de parada para resincronizarse para el próximo bit de inicio.
#### 3. Bit de Paridad (Parity Bit):
##### • ¿Qué es? Un bit opcional que se añade a la trama de datos para la detección simple de errores. No corrige errores, solo los detecta.
##### • ¿Cómo se configura? Se establecen bits en los registros de control del UART para:
###### • Habilitar/Deshabilitar Paridad: Un bit para activar o desactivar la funcionalidad de paridad.
###### • Tipo de Paridad: Par (El bit de paridad se establece de tal manera que el número total de '1's en los bits de datos MÁS el bit de paridad sea par), impar (El bit de paridad se establece de tal manera que el número total de '1's en los bits de datos MÁS el bit de paridad sea impar)
##### • Importancia: Permite al receptor verificar si un bit se invirtió durante la transmisión. Si el recuento de '1's no coincide con el tipo de paridad esperado, se detecta un error.
##### • Valores comunes: Sin paridad (None), Par (Even), Impar (Odd).
### _10. USART (Universal Synchronous Asynchronous transmission)_
Un módulo USART es un bloque de hardware dentro de un microcontrolador que ofrece la flexibilidad de configurar la comunicación serial en dos modos principales:
##### 1. Modo Asíncrono: Funciona exactamente como una UART. No requiere una señal de reloj compartida entre los dispositivos. La sincronización se logra mediante bits de inicio y parada, y ambos extremos deben acordar la velocidad de baudios. Ideal para comunicaciones con terminales, módulos GPS, Bluetooth, etc.
##### 2. Modo Síncrono: Requiere una señal de reloj compartida (generada por uno de los dispositivos) para sincronizar la transferencia de bits. No utiliza bits de inicio/parada, lo que lo hace más eficiente para la transferencia de datos en bloques. Ideal para comunicaciones rápidas entre microcontroladores o con periféricos cercanos como sensores o memorias seriales que usan protocolos como SPI o I2C (aunque los USARTs no implementan SPI o I2C directamente, sus modos síncronos pueden ser la base para implementar protocolos similares).
#### ¿Cómo funciona USART?
El funcionamiento del USART varía ligeramente según el modo de operación que se configure:
##### A. Modo Asíncrono (UART):
Funciona exactamente como una UART, como se explicó anteriormente.
##### • Pines: Utiliza los pines TX (transmisión) y RX (recepción).
##### • Transmisión:
###### • CPU escribe byte en el registro de datos de transmisión (TXREG).
###### • Hardware añade bit de inicio, bits de datos (5-9 bits), bit de paridad (opcional) y bit(s) de parada.
###### • Los bits se transmiten secuencialmente al pin TX a la velocidad de baudios configurada.
##### • Recepción:
###### • Receptor monitorea pin RX en busca de un flanco de bajada (bit de inicio).
###### • Sincroniza y muestrea los bits de datos a la velocidad de baudios.
###### • Reensambla los bits en un byte y lo coloca en el registro de datos de recepción (RXREG).
###### • Puede detectar errores de framing, overrun y paridad.
##### • Generador de Baud Rate: Un componente clave que genera la señal de reloj interna para la transmisión y muestreo a la velocidad de baudios deseada.
##### • Interrupciones: Genera interrupciones para TX listo, RX completo y errores.
##### B. Modo Síncrono:
En este modo, el USART utiliza una señal de reloj externa para sincronizar la transferencia de datos. No hay bits de inicio/parada, lo que hace la transmisión más eficiente en cuanto a "overhead".
##### • Pines: Utiliza los pines TX (datos) y CK (reloj). El pin RX no se usa en este modo si la comunicación es solo simplex síncrona, pero se usa para comunicación bidireccional síncrona.
##### • Tipos de Operación Síncrona:
###### • Maestro Síncrono (Master Synchronous): El USART genera la señal de reloj en su pin CK. Controla la velocidad de la comunicación.
###### • Esclavo Síncrono (Slave Synchronous): El USART acepta una señal de reloj externa en su pin CK. Se sincroniza con el reloj generado por otro dispositivo (el maestro).
##### • Transmisión Síncrona (ej. en modo Maestro):
###### • CPU escribe un byte en TXREG.
###### • El USART mueve el byte a un registro de desplazamiento.
###### • Los bits de datos se sacan del pin TX sincronizados con los pulsos de reloj generados en el pin CK.
###### • No hay bits de inicio o parada; se envía solo el byte de datos.
###### • La comunicación suele ser bidireccional si ambos dispositivos tienen líneas de datos de transmisión y recepción, pero la sincronización de reloj es única.
##### • Recepción Síncrona (ej. en modo Esclavo):
###### • El USART monitorea su pin RX y el pin CK.
###### • Cuando se detectan pulsos de reloj en CK, el USART muestrea el estado del pin RX en el flanco adecuado del reloj (subida o bajada, configurable).
###### • Los bits muestreados se mueven a un registro de desplazamiento.
###### • Una vez que se han recibido todos los bits del byte, el byte se coloca en RXREG.
###### • No hay bits de error de framing o overrun en el mismo sentido que en asíncrono, ya que la sincronización es provista por el reloj.
### _11. EUSART (Enhanced Universal synchronous asynchronous receiver transmitter)_
Como su nombre lo indica ("Enhanced" significa "Mejorado"), es una versión avanzada y más completa de un módulo USART estándar. Hereda todas las funcionalidades de un USART normal (comunicación asíncrona como UART y comunicación síncrona), pero añade características y modos operativos adicionales que lo hacen más potente y flexible. Es un periférico de comunicación serial que soporta tanto la comunicación asíncrona (UART) como la síncrona, pero con capacidades mejoradas como:
##### • Generación de Baud Rate de alta resolución: Permite establecer velocidades de baudios más precisas.
##### • Modo de 9 bits para direccionamiento: Facilita la comunicación en redes multipunto.
##### • Detección automática de baud rate (Auto-Baud Detect): Permite que el receptor se adapte automáticamente a la velocidad de transmisión del emisor.
##### • Mayor flexibilidad en la operación síncrona: A menudo con más opciones para la polaridad y fase del reloj.
##### • Manejo de interrupciones mejorado: Con más banderas de estado y opciones de interrupción.
##### • Soporte para modos de baja potencia: Permite operar con menor consumo de energía en ciertos estados.
#### ¿Cómo funciona EUSART?
El funcionamiento básico del EUSART es el mismo que el de un USART, pero con las características adicionales que se detallan a continuación. Se configura a través de registros de control específicos del PIC, como TXSTA (Transmitter Status and Control Register), RCSTA (Receiver Status and Control Register), SPBRG y SPBRGH (Special Baud Rate Generator High/Low Register).
#### Modos de Operación Principales:
##### 1. Modo Asíncrono (UART Mejorado):
##### • Pines: Utiliza TX/CK (para transmisión) y RX/DT (para recepción).
##### • Funcionamiento básico: Idéntico a un UART: envío y recepción de bits en serie con bits de inicio y parada, a una velocidad de baudios acordada.
##### • Características Mejoradas:
###### • Generador de Baud Rate de 16 bits (SPBRG y SPBRGH): Permite una mayor precisión en el cálculo de la velocidad de baudios y soporta un rango más amplio de velocidades.
###### • Auto-Baud Detect (ABD): Una característica clave. Si está habilitada, el EUSART puede detectar automáticamente la velocidad de baudios de una trama de entrada (típicamente midiendo la duración del bit de inicio o de una secuencia de caracteres conocida). Esto es útil cuando no se conoce la velocidad del dispositivo externo.
###### • Modo de 9 bits (9-bit mode): Permite usar el noveno bit como un bit de dirección/control. En redes multipunto, un maestro puede enviar una dirección con el noveno bit a '1' para "despertar" a un esclavo específico, y luego enviar datos normales con el noveno bit a '0'. Los esclavos pueden ignorar las tramas con el noveno bit a '0' hasta que sean direccionados.
###### • Mayor profundidad del búfer de recepción (a veces): Algunos EUSARTs pueden tener búferes FIFO más grandes para la recepción, reduciendo la probabilidad de errores de overrun.
##### 2. Modo Síncrono (USART Síncrono Mejorado):
##### • Pines: Utiliza TX/CK (para datos de transmisión o recepción) y RX/DT (para datos de recepción o transmisión) y el pin CK para la señal de reloj.
##### • Funcionamiento básico: Los bits se transmiten o reciben en serie, sincronizados con una señal de reloj externa (generada por el maestro o provista por el esclavo). No hay bits de inicio/parada.
##### • Características Mejoradas:
###### • Modos Maestro/Esclavo: Al igual que el USART estándar, puede funcionar como maestro (generando el reloj) o como esclavo (utilizando un reloj externo).
###### • Polaridad y Fase de Reloj: Ofrece opciones configurables para la polaridad del reloj (estado inactivo alto o bajo) y la fase del reloj (en qué flanco se muestrean/cambian los datos), lo que permite la compatibilidad con una variedad de dispositivos síncronos, incluyendo algunos que usan protocolos como SPI (aunque no implementa SPI directamente, puede simularlo con software).
#### Registros de Configuración Clave en PICs (Ejemplo):
###### • TXSTA (Transmit Status and Control Register):
###### • TXEN: Habilitar transmisión.
###### • SYNC: Selecciona el modo asíncrono (0) o síncrono (1).
###### • BRGH: Habilitar baud rate de alta velocidad (afecta el cálculo del baud rate).
###### • TX9/TX9D: Habilitar transmisión de 9 bits y el noveno bit de datos.
###### • TRMT: Bandera de "registro de desplazamiento de transmisión vacío".
##### • RCSTA (Receive Status and Control Register):
###### • SPEN: Habilitar el puerto serie (activa los pines TX/RX para el EUSART).
###### • CREN: Habilitar recepción continua (en modo asíncrono).
###### • RX9/RX9D: Habilitar recepción de 9 bits y el noveno bit recibido.
###### • FERR: Bandera de error de framing.
###### • OERR: Bandera de error de overrun.
###### • ADDEN: Habilitar detección de dirección (para el modo de 9 bits).
##### • SPBRG y SPBRGH (Baud Rate Generator Registers):
###### • Registros de 8 bits que combinados forman un valor de 16 bits para el generador de baud rate.
##### • BAUDCON (Baud Rate Control Register):
###### • ABDEN: Habilitar detección automática de baud rate.
###### • SCKP: Control de polaridad del reloj síncrono.
###### • BRG16: Habilitar el generador de baud rate de 16 bits.
### _12. Ejemplo de comunicacion en 2 pic´s_
Para lograr una comunicacion efectiva entre dos microcontroladores, se debem seguir los siguientes pasos:
#### Para el transmisor:
##### 1. Definir la frecuencia del oscilador
##### ![image](https://github.com/user-attachments/assets/dff7f236-4c67-4528-995a-91ea974c53ba)
##### 2. Establecer las funciones a usar
##### ![image](https://github.com/user-attachments/assets/113bdd57-e002-409a-95d7-eb63c4f6c5a4)
##### 3. Configurar los pines
##### ![image](https://github.com/user-attachments/assets/30c13eae-e11c-4a91-aa3d-c89954be81ff)
##### 4. Configurar la tasa de baudios
##### ![image](https://github.com/user-attachments/assets/c53d6b64-761a-4b7f-a080-b46bf193a7db)
##### 5. Configurar la comunicacion sincrona
##### ![image](https://github.com/user-attachments/assets/00952d5f-2b9f-4764-a4f3-28c02a432a2e)
##### 6. Configurar si se transmiten 8 o 9 bits
##### ![image](https://github.com/user-attachments/assets/3931bd6b-dee4-40f5-91a0-00d1947a03b9)
##### 7. Habilitar la transmision
##### ![image](https://github.com/user-attachments/assets/b34419b6-66ae-4ef7-965f-4b767f309f1a)
##### 8. Configurar emitir x bits
##### ![image](https://github.com/user-attachments/assets/160470da-213f-43b8-90ab-d19d95718338)
##### 9. Establecer funcion principal
##### ![image](https://github.com/user-attachments/assets/c5020989-753d-4636-b400-efea69dafd46)
#### Para el receptor:
Segun el datasheet, los primeros pasos son los mismos
##### 1. Configurar la tasa de baudios
##### ![image](https://github.com/user-attachments/assets/1a854f4a-cd93-4cad-add2-15d465eb8ef7)
##### 2. Configurar la comunicacion sincrona
##### ![image](https://github.com/user-attachments/assets/2a4eddab-f020-4ace-94ab-947cad506f13)
##### 3. Configurar si se transmiten 8 o 9 bits
##### ![image](https://github.com/user-attachments/assets/0a0051c2-0cb8-44d7-afb0-9c28aa301b8a)
##### 4. Habilitar la transmision
##### ![image](https://github.com/user-attachments/assets/932aeb41-50b9-477c-8a41-7ca21186c8fa)
##### 5. Configurar recibir x bits
##### ![image](https://github.com/user-attachments/assets/c205f5ca-f8b7-4a19-a549-8f0531fbc600)
##### 6. Establecer funcion principal
##### ![image](https://github.com/user-attachments/assets/bd92e659-2e68-4f57-8299-811d2c4ceec2)

## **Semana 14**
### _1. SPI (Serial peripherial interface)_
El SPI (Serial Peripheral Interface), que significa "Interfaz Periférica en Serie", es un protocolo de comunicación serie síncrono y full-duplex (bidireccional simultáneo) muy popular, desarrollado por Motorola a mediados de la década de 1980. Es ampliamente utilizado para la comunicación de corta distancia entre un microcontrolador (el "maestro") y uno o más periféricos (los "esclavos"), como sensores, memorias flash, DACs, ADCs, pantallas LCD, etc. SPI es una interfaz de comunicación que permite a un dispositivo central (el Maestro) controlar y comunicarse con uno o más dispositivos periféricos (los Esclavos). Se caracteriza por:
##### • Síncrono: La comunicación está sincronizada por una señal de reloj compartida generada por el Maestro. Esto elimina la necesidad de bits de inicio y parada, haciendo la comunicación más eficiente que la asíncrona (UART).
##### • Full-Duplex: La transmisión y recepción de datos ocurren simultáneamente en dos líneas de datos separadas. Esto significa que mientras el Maestro envía un bit de datos, puede recibir un bit de datos al mismo tiempo.
##### • Basado en Maestro-Esclavo: Siempre hay un dispositivo Maestro que inicia y controla la comunicación, y uno o más dispositivos Esclavo que responden al Maestro.
#### ¿Cómo funciona SPI?
El protocolo SPI utiliza un mínimo de cuatro líneas lógicas para la comunicación entre el Maestro y los Esclavos:

##### 1. SCLK (Serial Clock) / SCK: Línea de reloj serie. Esta señal es generada por el Maestro y es utilizada por todos los dispositivos (Maestro y Esclavos) para sincronizar el envío y la recepción de bits. Los datos se desplazan en cada flanco de esta señal.
##### 2. MOSI (Master Out, Slave In) / SDO (Serial Data Out en el Maestro): Línea de datos por la que el Maestro envía datos al Esclavo. Los datos fluyen desde el Maestro hacia el Esclavo.
##### 3. MISO (Master In, Slave Out) / SDI (Serial Data In en el Esclavo): Línea de datos por la que el Esclavo envía datos al Maestro. Los datos fluyen desde el Esclavo hacia el Maestro. SS (Slave Select) / CS (Chip Select) / nCS (Not Chip Select): Línea de selección de esclavo. Esta línea es generada por el Maestro y se utiliza para seleccionar un Esclavo específico con el que el Maestro desea 
##### 4. comunicarse. Normalmente, cada Esclavo tiene su propia línea SS/CS dedicada. Un nivel lógico bajo (LOW) en esta línea suele activar al Esclavo.
#### Proceso de Comunicación SPI:
##### 1. Selección del Esclavo: El Maestro comienza la comunicación poniendo la línea SS (Chip Select) del Esclavo con el que desea comunicarse a un estado activo (generalmente LOW). Todos los demás Esclavos cuyas líneas SS estén en HIGH permanecerán inactivos.
##### 2. Generación de Reloj: El Maestro comienza a generar pulsos en la línea SCLK.
##### 3. Transmisión de Datos (simultánea): En cada pulso de reloj (flanco de subida o bajada, según la configuración de la polaridad y fase del reloj):
##### • El Maestro coloca un bit de datos en la línea MOSI.
##### • El Esclavo lee un bit de datos de la línea MOSI.
##### • Simultáneamente, el Esclavo coloca un bit de datos en la línea MISO.
##### • El Maestro lee un bit de datos de la línea MISO.
##### • Esto significa que los datos se intercambian en ambas direcciones (full-duplex) en cada ciclo de reloj.
##### 4. Finalización de la Comunicación: Una vez que se han intercambiado todos los bits necesarios (típicamente 8 bits para un byte, pero puede ser cualquier número de bits), el Maestro detiene la generación de pulsos de reloj y pone la línea SS/CS del Esclavo de nuevo a un estado inactivo (generalmente HIGH). Esto indica al Esclavo que la transmisión ha terminado y libera el bus para que el Maestro pueda seleccionar otro Esclavo o realizar otra operación.
##### ![image](https://github.com/user-attachments/assets/ca3a6586-5039-4cf6-a5ed-9c53c633c913)

### _2. MSSP (Master Synchronous Serial Port)_
El módulo MSSP (Master Synchronous Serial Port), que significa "Puerto Serie Síncrono Maestro", es un periférico de hardware encontrado en muchos microcontroladores PIC de Microchip (y en otros microcontroladores bajo diferentes nombres). Su función principal es proporcionar un hardware dedicado para la comunicación serie síncrona, específicamente a través de los protocolos SPI (Serial Peripheral Interface) e I2C (Inter-Integrated Circuit).  Es un módulo versátil que permite al microcontrolador actuar como maestro o esclavo en dos de los protocolos de comunicación serie síncronos más populares:
##### 1. SPI (Serial Peripheral Interface): Comunicación full-duplex de alta velocidad, ideal para conectar a memorias EEPROM, sensores, DACs, ADCs, y otros microcontroladores. Utiliza cuatro líneas (MOSI, MISO, SCK, SS).
##### 2. I2C (Inter-Integrated Circuit): Comunicación bidireccional multipunto de dos hilos, ideal para conectar a sensores, RTCs, EEPROMs, y otros microcontroladores en un bus compartido. Utiliza dos líneas (SDA, SCL).
#### ¿Cómo funciona el MSSP?
El funcionamiento del MSSP depende del modo en que se configure (SPI o I2C).
##### 1. Funcionamiento en Modo SPI (Serial Peripheral Interface)
En el modo SPI, el MSSP gestiona la transmisión y recepción de datos de forma síncrona utilizando cuatro líneas principales:
##### • SDO (Serial Data Out) / MOSI (Master Out Slave In): Línea de datos del maestro al esclavo.
##### • SDI (Serial Data In) / MISO (Master In Slave Out): Línea de datos del esclavo al maestro.
##### • SCK (Serial Clock): Línea de reloj. Generada por el maestro (en modo maestro) o recibida por el esclavo (en modo esclavo).
##### • SS (Slave Select) / CS (Chip Select): Línea de selección de esclavo. El maestro la usa para seleccionar un esclavo específico con el que quiere comunicarse.
### _3. Transmisión modo maestro_
La Transmisión en modo maestro es un concepto fundamental en las arquitecturas de comunicación Maestro-Esclavo, especialmente en protocolos serie síncronos como SPI e I2C, donde un dispositivo toma el control central de la comunicación. La Transmisión en Modo Maestro se refiere a la capacidad y rol de un dispositivo (el "Maestro") para iniciar, controlar y terminar la comunicación con uno o varios dispositivos Esclavos. En este modo, el Maestro es el que dicta las reglas del flujo de datos, la temporización y a qué Esclavo se dirige. Según la información proporcionada, el Maestro es el "único habilitado para iniciar y terminar la comunicación".
#### ¿Cómo Funciona la Transmisión en Modo Maestro?
El funcionamiento exacto varía ligeramente entre protocolos (como SPI e I2C), pero los principios generales son los siguientes:
##### 1. Inicio de la Comunicación:
##### • Selección del Esclavo (específico para SPI): Si hay múltiples Esclavos en el bus (como en SPI), el Maestro activa la línea de selección de esclavo (SS o CS) específica del Esclavo con el que quiere comunicarse. Esto "despierta" a ese Esclavo y le indica que esté listo para recibir o enviar datos.
##### • Condición de Inicio (específico para I2C): En I2C, el Maestro genera una "condición de inicio" en el bus, que es una secuencia específica de cambio de niveles en las líneas de datos (SDA) y reloj (SCL) para indicar que una nueva transacción va a comenzar.
##### • Envío de Dirección (específico para I2C): En I2C, después de la condición de inicio, el Maestro envía la dirección del Esclavo con el que quiere comunicarse, junto con un bit que indica si la operación será de lectura o escritura.
##### 2. Control del Reloj (en protocolos síncronos como SPI/I2C):
##### • El Maestro es el responsable de generar la señal de reloj (SCK en SPI, SCL en I2C) que sincroniza toda la transferencia de bits. La velocidad de este reloj es configurable por el Maestro.
##### • Los Esclavos simplemente utilizan esta señal de reloj para saber cuándo deben muestrear los datos que llegan o cuándo deben colocar sus propios datos en la línea.
##### • 3. Transmisión y Recepción de Datos:
##### • Una vez establecido el enlace con el Esclavo (seleccionado o direccionado), el Maestro comienza a enviar o recibir datos.
##### • Transmisión de Datos (Maestro a Esclavo): El Maestro coloca los bits de datos en la línea de datos de salida (MOSI en SPI, SDA en I2C) y los desplaza uno por uno, sincronizado con el reloj.
##### • Recepción de Datos (Esclavo a Maestro): Simultáneamente, el Maestro lee los bits de datos de la línea de datos de entrada (MISO en SPI, SDA en I2C), también sincronizado con el reloj. En protocolos full-duplex como SPI, esto ocurre al mismo tiempo que la transmisión.
##### • Gestión del Flujo: El Maestro determina cuántos bytes o bits se van a transferir y en qué orden.
##### 3. Terminación de la Comunicación:
##### • Una vez que la transacción ha finalizado, el Maestro es el que termina la comunicación.
##### • Liberación del Esclavo (específico para SPI): El Maestro desactiva la línea de selección de esclavo (SS o CS) de ese Esclavo, indicándole que ignore el bus hasta la próxima selección.
##### • Condición de Parada (específico para I2C): En I2C, el Maestro genera una "condición de parada", que es otra secuencia específica de cambio de niveles en SDA y SCL para liberar el bus para futuras transacciones.
##### ![image](https://github.com/user-attachments/assets/754f832f-dfe6-46ff-9710-5fe38ac1c1f5)

### _4.Modo esclavo _
El modo esclavo es un concepto fundamental en las arquitecturas de comunicación Maestro-Esclavo, especialmente en protocolos de comunicación serial síncrona como SPI e I2C. En este modo, un dispositivo (el "Esclavo") se somete al control de un dispositivo "Maestro" y solo puede actuar en respuesta a las directivas del Maestro. El modo esclavo describe el rol de un dispositivo que no puede iniciar la comunicación por sí mismo. Su función principal es responder a las solicitudes o comandos enviados por un dispositivo Maestro. En la jerarquía Maestro-Esclavo, el Esclavo es el componente pasivo que espera instrucciones.
#### ¿Cómo Funciona el Modo Esclavo?
El funcionamiento del modo esclavo depende del protocolo de comunicación específico, pero los principios generales son los siguientes:
##### 1. Espera Pasiva:
##### • El dispositivo Esclavo se mantiene en un estado de espera, monitoreando las líneas de comunicación para detectar una señal del Maestro.
##### • En SPI: El Esclavo espera a que su línea de selección de esclavo (SS o CS) sea activada por el Maestro (generalmente un nivel bajo).
##### • En I2C: El Esclavo monitorea las líneas de datos (SDA) y reloj (SCL) en busca de una condición de inicio y su dirección específica enviada por el Maestro.
##### 2. Sincronización (en protocolos síncronos):
##### • Una vez seleccionado o direccionado, el Esclavo se sincroniza con la señal de reloj generada por el Maestro (SCK en SPI, SCL en I2C). El Esclavo no genera el reloj, sino que lo utiliza para saber cuándo leer o colocar los datos en las líneas.
##### 3. Transmisión y Recepción de Datos (en respuesta al Maestro):
##### • Recepción de Datos (Maestro a Esclavo): Cuando el Maestro envía datos por su línea de salida de datos (MOSI en SPI, SDA en I2C), el Esclavo los lee y los almacena en su búfer de recepción, sincronizado con el reloj del Maestro.
##### • Transmisión de Datos (Esclavo a Maestro): Si el Maestro solicita datos (a través de un comando o un bit de lectura/escritura en la dirección I2C), el Esclavo coloca los datos solicitados en su línea de salida de datos (MISO en SPI, SDA en I2C), también sincronizado con el reloj del Maestro.
##### • El Esclavo nunca inicia la transferencia de datos por sí mismo; siempre es en respuesta a una solicitud del Maestro.
##### 4. Confirmación (ACK/NACK - específico para I2C):
##### • En I2C, después de recibir un byte (ya sea una dirección o un dato), el Esclavo tiene la capacidad de enviar un bit de Acknowledge (ACK) al Maestro para confirmar que ha recibido el byte correctamente. Si no puede procesar el byte, puede enviar un NACK. En SPI, no existe un mecanismo de reconocimiento incorporado.
##### 5. Finalización de la Comunicación:
##### • El Esclavo vuelve a su estado pasivo de espera cuando el Maestro termina la comunicación.
##### • En SPI: El Maestro desactiva la línea de selección de esclavo (SS o CS).
##### • En I2C: El Maestro genera una condición de parada.
##### ![image](https://github.com/user-attachments/assets/be774020-336f-4acc-8b44-7e50af910a6f)

## **Semana 15**
### _1. Inter-IC bus_
El Inter-IC bus, o I2C, sirve para permitir la comunicación entre varios componentes integrados (chips) en una placa de circuito impreso, utilizando solo dos líneas de comunicación bidireccionales:
##### • SDA (Serial Data Line): La línea por donde se transmiten y reciben los datos. Es una línea bidireccional.
##### • SCL (Serial Clock Line): La línea de reloj, que sincroniza la transferencia de datos. Es generada por el maestro, pero los esclavos pueden "estirar" el reloj para pausar la comunicación si necesitan más tiempo para procesar los datos.
#### Sirve para:
##### • Comunicación de bajo costo y baja velocidad: Es ideal para conectar sensores, memorias EEPROM, convertidores analógico-digital (ADC) y digital-analógico (DAC), módulos RTC (Real Time Clock), y otros periféricos que no requieren una muy alta velocidad de datos, pero sí una configuración sencilla y un bajo número de pines.
##### • Reducción de pines: Su principal ventaja es que utiliza solo dos cables para comunicar múltiples dispositivos, lo que simplifica el diseño de la placa y reduce el número de pines necesarios en el microcontrolador.
##### • Capacidad Multimaestro/Multiesclavo: Permite que haya varios dispositivos Maestros en el mismo bus, aunque solo uno puede estar activo en un momento dado.
#### ¿Cómo funciona el Inter-IC bus (I2C)?
El funcionamiento del I2C se basa en un sistema de direccionamiento y un protocolo bien definido:
##### 1. Condición de Inicio (Start Condition): Para iniciar una comunicación, un dispositivo Maestro genera una condición de inicio. Esto ocurre cuando la línea SDA pasa de ALTO a BAJO mientras la línea SCL está en ALTO. Todos los dispositivos en el bus detectan esta condición, que indica el inicio de una nueva transacción.
##### 2. Direccionamiento de Esclavo: Después de la condición de inicio, el Maestro envía la dirección de 7 o 10 bits del dispositivo Esclavo con el que desea comunicarse. Junto con la dirección, se envía un bit de lectura/escritura (R/W), que indica si el Maestro va a leer datos del Esclavo o a escribir datos en él.
##### 3. ACK/NACK (Acknowledge/Non-Acknowledge): El dispositivo Esclavo cuya dirección coincide con la enviada por el Maestro responde con un bit de ACK (Acknowledge). Esto significa que el Esclavo ha reconocido su dirección y está listo para la comunicación. Si ningún Esclavo responde o el Esclavo no está listo, el Maestro recibe un NACK.
##### 4. Transferencia de Datos: Una vez que el Esclavo ha sido direccionado y ha enviado un ACK, comienza la transferencia de datos:
##### • Escritura (Maestro a Esclavo): El Maestro envía bytes de datos, uno por uno. Después de cada byte, el Esclavo debe enviar un ACK para indicar que lo ha recibido correctamente.
##### • Lectura (Esclavo a Maestro): Si el Maestro ha solicitado una lectura, el Esclavo comienza a enviar bytes de datos al Maestro. Después de cada byte recibido, el Maestro debe enviar un ACK al Esclavo para indicarle que está listo para el siguiente byte. Para indicar al Esclavo que es el último byte a leer, el Maestro envía un NACK.
##### 5. Condición de Parada (Stop Condition): Una vez que la transferencia de datos ha finalizado, el Maestro genera una condición de parada. Esto ocurre cuando la línea SDA pasa de BAJO a ALTO mientras la línea SCL está en ALTO. Esto libera el bus para que otros dispositivos puedan usarlo.
##### ![image](https://github.com/user-attachments/assets/074d2588-0f34-47b6-817d-ae1a4c6ee16d)
### _2. Transmisión de datos I2C_
La transmisión de datos en I2C (Inter-Integrated Circuit) funciona de una manera estructurada y controlada, utilizando solo dos líneas bidireccionales: SDA (Serial Data Line) y SCL (Serial Clock Line).
#### 1. Estados del Bus:
##### • Ambas líneas, SDA y SCL, son de drenador abierto y requieren resistencias pull-up externas para mantenerlas en un estado lógico ALTO cuando están inactivas o no están siendo controladas activamente por ningún dispositivo.
##### • Cuando el bus está inactivo (libre), ambas líneas SDA y SCL están en ALTO.
#### 2. Condición de Inicio (START Condition):
##### • La transmisión de datos siempre comienza con una condición de inicio generada por el dispositivo Maestro.
##### • Esta condición se define como un flanco de bajada (transición de ALTO a BAJO) en la línea SDA, mientras la línea SCL se mantiene en ALTO.
##### • Todos los dispositivos en el bus detectan esta condición, lo que indica el inicio de una nueva transacción.
#### 3. Envío de Dirección del Esclavo y Bit de Lectura/Escritura:
##### • Después de la condición de inicio, el Maestro envía la dirección de 7 o 10 bits del dispositivo Esclavo con el que desea comunicarse.
##### • Inmediatamente después de la dirección, el Maestro envía un bit de Lectura/Escritura (R/W):
###### • Si R/W es '0', indica que el Maestro va a escribir datos en el Esclavo.
###### • Si R/W es '1', indica que el Maestro va a leer datos del Esclavo.
###### • Estos bits (dirección + R/W) se transmiten en serie por la línea SDA, sincronizados por los pulsos de SCL generados por el Maestro.
#### 4. Bit de Reconocimiento (ACK/NACK - Acknowledge/Non-Acknowledge):
##### • Después de que el Maestro ha transmitido la dirección y el bit R/W (9 bits en total), el bus se libera brevemente para un bit de reconocimiento.
##### • El dispositivo Esclavo cuya dirección coincide con la enviada por el Maestro debe tirar la línea SDA a BAJO durante el noveno pulso de SCL para enviar un bit de ACK (Acknowledge). Esto le indica al Maestro que el Esclavo está presente, ha reconocido su dirección y está listo para la comunicación.
##### • Si ningún Esclavo responde con un ACK, o el Esclavo direccionado no puede procesar la solicitud, la línea SDA permanece en ALTO (o es tirada a ALTO por una resistencia pull-up), lo que se interpreta como un NACK (Non-Acknowledge). El Maestro debe manejar este NACK adecuadamente, quizás reintentando o terminando la transacción.
#### 5. Transferencia de Datos (Byte a Byte):
##### • Una vez que el Esclavo ha enviado un ACK, comienza la fase de transferencia de datos real, byte a byte.
##### • Si el Maestro está Escribiendo en el Esclavo:
###### • El Maestro coloca 8 bits de datos en la línea SDA.
###### • El Esclavo lee esos 8 bits.
###### • Después de cada byte, el Esclavo debe enviar un ACK (tirando SDA a BAJO) para confirmar que ha recibido el byte correctamente y que está listo para el siguiente.
##### • Si el Maestro está Leyendo del Esclavo:
###### • El Esclavo coloca 8 bits de datos en la línea SDA.
###### • El Maestro lee esos 8 bits.
###### • Después de cada byte, el Maestro debe enviar un ACK (tirando SDA a BAJO) al Esclavo para indicarle que ha recibido el byte y que está listo para el siguiente.
###### • Cuando el Maestro ha recibido todos los bytes que necesita, envía un NACK (dejando SDA en ALTO) al Esclavo en lugar de un ACK. Esto le indica al Esclavo que es el último byte a leer y que la transmisión de datos por parte del Esclavo debe finalizar.
#### 6. Condición de Parada (STOP Condition):
##### • Una vez que toda la transferencia de datos ha finalizado (y el último byte ha sido ACK/NACKeado), el Maestro genera una condición de parada.
##### • Esta condición se define como un flanco de subida (transición de BAJO a ALTO) en la línea SDA, mientras la línea SCL se mantiene en ALTO.
##### • La condición de parada libera el bus, permitiendo que otros Maestros (si los hay) o el mismo Maestro inicien nuevas transacciones.
##### ![image](https://github.com/user-attachments/assets/ec0a7be3-54cc-4efb-9f95-510a2708a722)
### _3. Manejo de velocidad desde el esclavo_
El "manejo de velocidad desde el esclavo" en el contexto de la comunicación serial síncrona se refiere a la capacidad de un dispositivo esclavo para influir o controlar temporalmente la velocidad de la transmisión de datos, a pesar de que el maestro es el que genera la señal de reloj. Esto es crucial en escenarios donde el esclavo necesita más tiempo para procesar datos o prepararse para la siguiente transferencia. El mecanismo principal que permite esto es el "Clock Stretching" (Estiramiento del Reloj), y es una característica distintiva del protocolo I2C. SPI no tiene un mecanismo equivalente nativo para que el esclavo controle el reloj.
#### ¿Cómo funciona el "Clock Stretching" (Estiramiento del Reloj) en I2C?
El estiramiento del reloj permite a un dispositivo esclavo (o incluso a otro maestro en un entorno multi-maestro) mantener la línea SCL (Serial Clock Line) en BAJO después de que el Maestro haya liberado la línea.
##### 1. El Maestro genera pulsos de reloj (SCL): Durante una transacción I2C, el Maestro es quien genera la señal de reloj en la línea SCL para sincronizar la transferencia de datos.
##### 2. El Maestro libera la línea SCL: Después de un cierto tiempo (generalmente el tiempo de "bajo" del pulso de reloj), el Maestro libera la línea SCL (es decir, deja de tirar de ella a BAJO, permitiendo que la resistencia pull-up la eleve a ALTO).
##### 3. El Esclavo tira SCL a BAJO (Estiramiento): Si el Esclavo necesita más tiempo para procesar el byte que acaba de recibir, o para preparar el siguiente byte a enviar, tira la línea SCL a BAJO y la mantiene así.
##### 4. El Maestro espera: El Maestro, que esperaba que SCL subiera a ALTO para continuar con el siguiente pulso de reloj (o para muestrear datos), detecta que SCL permanece en BAJO. El Maestro debe esperar pasivamente hasta que la línea SCL vuelva a subir a ALTO.
##### 5. El Esclavo libera SCL: Una vez que el Esclavo ha terminado su procesamiento interno y está listo para continuar, libera la línea SCL. La resistencia pull-up la eleva de nuevo a ALTO.
##### 6. La Comunicación se Reanuda: El Maestro detecta que SCL ha vuelto a ALTO y puede continuar generando los siguientes pulsos de reloj y la transferencia de datos.
##### ![image](https://github.com/user-attachments/assets/6692ed7d-e97a-4fc5-9d81-b475e16aaa49)

### _5. Configuracion I2C en el PIC_
#### 1. Configurar puertos TRIS
#### 2. Configurar velocidad
#### 3. Configurar modo maestro 
#### 4. Cargar registro SSPAD para lograr la velocidad
#### 5. Enviar señal start
#### 6. Enviar dirección
#### 7. Enviar acknowledge (opcional)
#### 8. Enviar dato
Funciones
##### ![image](https://github.com/user-attachments/assets/cfa8f950-511b-4c5b-a50e-2594ff81ed0e)
Funciones Tx y Rx
##### ![image](https://github.com/user-attachments/assets/7e6f701d-49df-463a-8346-06968712406b)
Funciones Acknnowledgement
##### ![image](https://github.com/user-attachments/assets/37f659da-3cfd-44bc-ae22-4394dcccbfad)
Funciones flujo de datos
#####ñ ![image](https://github.com/user-attachments/assets/d3dc578e-e3c3-4379-85e9-c176af1461af)
Función de inicialización
##### ![image](https://github.com/user-attachments/assets/9b44af06-91df-41ed-a6fa-dc72be5ea4ab)
Programa principal
##### ![image](https://github.com/user-attachments/assets/5c5cea98-26a1-439a-8d81-452d1e67ef0a)
## CONCLUSION
La comunicación en microcontroladores es un ecosistema diverso y estratégico, que va desde la transmisión fundamental de bits hasta complejas arquitecturas de red, donde la elección y configuración adecuadas son cruciales para la funcionalidad y fiabilidad de los sistemas embebidos. A lo largo de este semetre, hemos explorado los siguientes puntos clave que fundamentan esta conclusión:
### 1. Fundamentos de la Transmisión de Datos:
#### • La comunicación se clasifica por la dirección del flujo (Simplex, Half Duplex, Full Duplex) y la sincronización (Asíncrona/UART/SCI/EUSART, Síncrona/SPI/I2C/MSSP).
#### • La velocidad (baud rate), el número de bits de datos, los bits de parada y la paridad son parámetros esenciales y deben coincidir entre los dispositivos para una comunicación asíncrona efectiva.
### 2. Organización de la Comunicación:
#### • Las topologías (Bus, Punto a Punto) definen la disposición física y lógica de los dispositivos, impactando directamente en el cableado, la escalabilidad y la complejidad.
#### • Las arquitecturas (Maestro-Esclavo, Todos Iguales) dictan la jerarquía y el control sobre quién inicia y gestiona la comunicación, lo que a su vez determina la necesidad de mecanismos de direccionamiento y gestión de acceso al medio.
### 3. Integridad y Temporización de la Señal:
#### • Las condiciones eléctricas (Terminación Sencilla vs. Diferencial) son vitales para la robustez de la señal. La transmisión diferencial es superior para distancias largas y entornos ruidosos debido a su inmunidad al ruido en modo común.
#### • La granularidad del temporizador y la gestión de los tiempos de lectura son críticas para sistemas en tiempo real. Un temporizador con baja granularidad y una lectura lenta pueden llevar a la pérdida de eventos o a mediciones imprecisas, lo que resalta la importancia de ISRs eficientes y búferes de hardware.
### 4. Periféricos de Comunicación en Microcontroladores:
#### • Los módulos de hardware como UART/SCI/EUSART permiten la comunicación serial asíncrona, con el EUSART ofreciendo mejoras significativas como la detección automática de baudios y modos de 9 bits para redes multipunto.
#### • El módulo MSSP es fundamental para implementar protocolos seriales síncronos como SPI e I2C, delegando gran parte de la complejidad a hardware, lo que libera recursos del CPU y mejora la fiabilidad y velocidad.
#### • SPI es ideal para comunicación rápida y full-duplex punto a punto o con pocos esclavos (usando múltiples líneas SS), sin mecanismos de reconocimiento integrados.
#### • I2C es perfecto para redes multimaestro/multiesclavo con pocos pines, ofreciendo direccionamiento, ACK/NACK y la capacidad única de estiramiento del reloj para que los esclavos gestionen su velocidad de procesamiento.
En esencia, la comunicación en microcontroladores es un campo donde la interoperabilidad es clave. Elegir el protocolo, la topología y la arquitectura adecuados, junto con una configuración precisa de los parámetros de hardware, son pasos determinantes para construir sistemas embebidos eficientes, fiables y capaces de interactuar con el mundo exterior de forma exitosa. La comprensión profunda de cada uno de estos aspectos permite a los diseñadores optimizar el rendimiento, el consumo de energía y la complejidad del software.
## REFERENCIAS
### https://aulas.ecci.edu.co/mod/resource/view.php?id=217972
### https://aulas.ecci.edu.co/mod/resource/view.php?id=217974
### https://aulas.ecci.edu.co/mod/resource/view.php?id=217977
### https://aulas.ecci.edu.co/mod/resource/view.php?id=217979
### Datasheet pic18f4550: https://www.microchip.com/
