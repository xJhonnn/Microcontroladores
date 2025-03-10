_Jhon Trujillo y Juan Ospina_
# **Apuntes primer corte**
## **Semana 1**
### _1. ¿Que es un micropocesador?_
el microprocesador es un componente fundamental que permite que los dispositivos electrónicos procesen información y realicen tareas complejas.
### _2. Componentes de un microprocesador_
Un micropocesador tiene dos componentes principales
#### **• UC:** Unidad de control
#### **• ALU:** Unidad aritmético lógica
![image](https://github.com/user-attachments/assets/59f04ce5-6956-4e32-8ffc-728cd130ea43)
### _3. Funcionamiento de un microprocesador_
#### **• ALU:** Ejecuta operaciones matemáticas y lógicas,tTiene 2 entradas, la unidad de control selecciona el tipo de operación a realizar, tiene una salida y muestra información adicional sobre el resultado.
![image](https://github.com/user-attachments/assets/ad03dcbb-4cf4-495b-997d-cc0da551e0b7)
#### **• Archivo de registro:** Son registros de propósito general, acumuladores, índice.
![image](https://github.com/user-attachments/assets/b5fe991a-1fe4-4b04-9357-45ddc8024dc5)
#### **• Unidad de control:** La unidad de control habilta los bits necesarios para realizar las operaciones.
![image](https://github.com/user-attachments/assets/88b803b8-9e77-4279-b042-70ce74549672)
### _4. Set de instrucciones_
#### **• CISC (Complex Instructions set computer):** La unidad de control descompone las instrucciones en microinstrucciones que permiten ejecutar operaciones mucho mas complejas.
#### **• RISC (Reduced instruction set computer):** La unidad de control habilita directamente los pines necesarios para llevar a cabo una instrucción.
### _5. Arquitectura_
#### **• Von Neumann:** Tanto la memoria de programa como la memoria de datos comparten almacenamiento por lo tanto usan el mismo bus. Se presentan caso de cuello de botella debido a esto.
#### **• Harvard:** El programa y los datos estan en diferentes espacios de memoria lo cual hace mas eficiente el rendimiento, pero exige el uso de 2 buses separados y dos chips de memoria.
### _6. ¿Que es un microcontrolador?_
Es un dispositivo programable con capacidad de ejecutar operaciones, tareas y procesos a gran velocidad, lo que permite su uso en aplicaciones en tiempo real, como sensores, sistemas remotos, automatismos, sistemas de control en máquinas y aplicaciones industriales. En síntesis el microcontrolador es una pequeña computadora utilizada para aplicaciones puntuales, esto quiere decir que el microcontrolador debe incluir ciertas unidades fundamentales y comunes en cualquier computadora, estas unidades son:
#### • CPU
#### • MEMORIA RAM
#### • MEMORIA ROM
#### • ENTRADAS Y SALIDAS
### _7. Diagrama lógico del micrcontrolador_
#### Un microcontrolador integra diferentes dispositivos en un solo circuito integrado
![image](https://github.com/user-attachments/assets/f9370307-1b2e-4881-9848-3812093d7ae0)
### _8. Aplicaciones de los microcontroladores_
#### **•Robótica:** Muy usados en subsistemas específicos de control (extremidades, facciones del rostro, soportes prensiles, etc.)
#### **•Equipamiento informático:** impresoras, scanners, copiadoras… Sistemas portátiles y autónomos
#### **•Sector automotriz:** control centralizado de puertas y ventanas, climatizadores, inyección, alarmas, etc.
#### **•Sector doméstico:** integrado en los sistemas de televisores, lavarropas, microondas, heladeras, videos, etc.
#### **•Sector industrial:** Controladores industriales, Automatización
### _9. Clasificacion microcontroladores_
Los microcontroladores tienen una clasificación similar a la de los microprocesadores, es decir, se clasifican de acuerdo a la longitud de palabra desde los 4bits, 8bits, 16bits y 32bits.
## **Semana 2 y semana 3**
### _1. Señales digitales y analogas_
#### **Analogas:** Una señal analógica es una señal continua que varía en amplitud o frecuencia a lo largo del tiempo. Puede tomar cualquier valor dentro de un rango determinado.
##### Caracteristicas: 
##### • Es continua, lo que significa que no hay saltos o interrupciones en la señal.
##### • Puede representar una gama infinita de valores.
##### • Es susceptible al ruido y la distorsión, lo que puede degradar la calidad de la señal.
![image](https://github.com/user-attachments/assets/fb48d89f-88ac-4b54-8ee5-78e37b5f2f53)
#### **Digitales:** Una señal digital es una señal discreta que representa información mediante valores numéricos, generalmente 0 y 1 (sistema binario).
##### Caracteristicas: 
##### • Es discreta, lo que significa que la señal solo puede tomar valores específicos.
##### • Es menos susceptible al ruido y la distorsión.
##### • Permite el procesamiento y almacenamiento de información de forma eficiente.
![image](https://github.com/user-attachments/assets/46110871-2342-4dd3-b21f-fc3e76cd69c9)
### _2. Puertos de entrada y salida_
Los puertos de entrada/salida son interfaces que permiten a un dispositivo digital, como un microcontrolador, comunicarse con el mundo exterior. Estos puertos proporcionan la capacidad de:
#### Recibir señales eléctricas (entradas):
##### • Permiten que el microcontrolador detecte el estado de dispositivos externos, como botones, sensores o interruptores.
##### • Por ejemplo, un puerto de entrada puede leer el estado de un sensor de temperatura para determinar si la temperatura ha alcanzado un cierto umbral.
#### Enviar señales eléctricas (salidas):
##### • Permiten que el microcontrolador controle dispositivos externos, como LEDs, motores o relés.
##### • Por ejemplo, un puerto de salida puede encender un LED o activar un motor.
### _3. Que significa que sean I/O digitales_
Cuando hablamos de I/O digitales (Entrada/Salida digitales), nos referimos a la capacidad de un dispositivo electrónico, como un microcontrolador, para interactuar con el mundo exterior utilizando señales que representan valores discretos, típicamente "encendido" o "apagado", o 1 y 0. Aquí te explico qué significa esto en detalle:
#### Características de las I/O Digitales:
##### Valores Discretos:
###### • Las I/O digitales manejan señales que solo pueden tener dos estados definidos: alto (1) o bajo (0). Esto contrasta con las I/O analógicas, que pueden manejar un rango continuo de valores.
###### • "Alto" y "bajo" generalmente se refieren a niveles de voltaje específicos. Por ejemplo, en un sistema de 5V, "alto" podría ser cercano a 5V y "bajo" cercano a 0V.
##### Encendido/Apagado:
###### • En términos prácticos, las I/O digitales se utilizan para controlar dispositivos que tienen estados de encendido/apagado, como LEDs, relés o interruptores.
###### • También se utilizan para leer el estado de sensores digitales que proporcionan salidas binarias.
##### Comunicación Binaria:
###### • Las I/O digitales son fundamentales para la comunicación binaria, que es la base de la electrónica digital y la informática.
###### • Permiten la transferencia de datos en forma de secuencias de 1s y 0s.
![image](https://github.com/user-attachments/assets/5f49e74e-ddcd-498e-8457-5a3ffc1091f1)
### _4. Componentes típicos de los puertos digitales_
#### • Data direction register (DDR): Es un registro que almacena la configuración del Puerto. Si se quiere usar como entrada o como salida
#### • Port Register (PORT): Este registro almacena el equivalente lógico de los niveles de tensión que se encuentran en los pines de conexión eléctrica
#### • Port Input Register (PIN): Generalmente es solo de lectura y contiene el estado actual de todos los pines
#### • En la mayoría de microcontroladores es posible accesar estos registros modificando el byte completo o solo uno de los bit. Por ejemplo el PIC
### _5. Entradas digitales_
Las entradas digitales son pines o terminales en un dispositivo electrónico, como un microcontrolador, que permiten recibir señales eléctricas que representan información binaria (0 o 1). Estas señales se utilizan para detectar el estado de dispositivos externos o para recibir datos digitales.
#### Características Clave
##### Niveles Lógicos:
##### • Las entradas digitales operan con niveles lógicos, que representan estados binarios.
##### • Un nivel lógico alto (1) generalmente corresponde a un voltaje cercano al voltaje de alimentación (por ejemplo, 5V o 3.3V).
##### • Un nivel lógico bajo (0) generalmente corresponde a un voltaje cercano a 0V (tierra).
##### Estados Discretos:
##### • Las entradas digitales solo pueden detectar dos estados distintos: alto o bajo.
##### • Esto significa que no pueden medir valores intermedios o continuos.
##### Versatilidad:
##### • Las entradas digitales pueden utilizarse para una amplia variedad de aplicaciones, como:
##### • Leer el estado de interruptores, botones o sensores digitales.
##### • Recibir datos digitales de otros dispositivos.
##### • Detectar eventos o condiciones específicas.
#### Funcionamiento
Cuando una señal eléctrica se aplica a una entrada digital, el microcontrolador o dispositivo electrónico compara el voltaje de la señal con un umbral predefinido. Si el voltaje está por encima del umbral, la entrada se considera alta (1). Si el voltaje está por debajo del umbral, la entrada se considera baja (0).
### _6. Muestreo digital_
El muestreo digital es el proceso de convertir una señal analógica continua en una secuencia de muestras discretas. En otras palabras, se toman mediciones de la señal analógica a intervalos regulares de tiempo.
#### Proceso de Muestreo
##### 1. Señal Analógica:
Se parte de una señal analógica, que es una señal continua que varía en amplitud a lo largo del tiempo.
##### 2. Muestreo:
Se toman muestras de la señal analógica a intervalos regulares de tiempo. Cada muestra representa el valor de la amplitud de la señal en un instante específico.
#### ![image](https://github.com/user-attachments/assets/e8264323-b658-4703-96a9-6fb3096564cd)
##### 3. Frecuencia de Muestreo:
La frecuencia de muestreo es el número de muestras que se toman por segundo y se mide en hercios (Hz).
##### 4. Cuantificación:
Después del muestreo, cada muestra se cuantifica, lo que significa que se le asigna un valor numérico discreto. La cuantificación introduce un error de cuantificación, que es la diferencia entre el valor real de la muestra y el valor cuantificado.
#### ![image](https://github.com/user-attachments/assets/8a1ed18f-1eb9-4aff-bd20-0a237703b442)
##### 5. Codificación:
Finalmente, los valores cuantificados se codifican en un formato digital, generalmente utilizando un sistema binario.
#### ![image](https://github.com/user-attachments/assets/675ae657-d433-47ed-8ac8-770eeb458e67)
### _7. Resistencias de pull_
Las resistencias de pull-up y pull-down son resistencias utilizadas en circuitos digitales para asegurar que una entrada digital tenga un nivel lógico definido cuando no está conectada a una fuente de señal activa.
#### Resistencia de pull-up:
##### • Conecta la entrada digital a un voltaje alto (VCC o 5V, por ejemplo).
##### • Cuando la entrada no está conectada a nada, la resistencia la mantiene en un estado lógico alto.
##### • Se utiliza para que una entrada digital tenga un estado por defecto alto.
#### Resistencia de pull-down:
##### • Conecta la entrada digital a tierra (GND o 0V).
##### • Cuando la entrada no está conectada a nada, la resistencia la mantiene en un estado lógico bajo.
##### • Se utiliza para que una entrada digital tenga un estado por defecto bajo.
### _8. Salidas digitales_
Las salidas digitales son pines o terminales en un dispositivo electrónico, como un microcontrolador, que permiten enviar señales eléctricas que representan información binaria (0 o 1). Estas señales se utilizan para controlar dispositivos externos o para enviar datos digitales a otros dispositivos.
#### Características Clave
##### Niveles Lógicos:
##### • Las salidas digitales generan niveles lógicos, que representan estados binarios.
##### • Un nivel lógico alto (1) generalmente corresponde a un voltaje cercano al voltaje de alimentación (por ejemplo, 5V o 3.3V).
##### • Un nivel lógico bajo (0) generalmente corresponde a un voltaje cercano a 0V (tierra).
##### Estados Discretos:
##### • Las salidas digitales solo pueden generar dos estados distintos: alto o bajo.
##### • Esto significa que no pueden generar valores intermedios o continuos.
##### Versatilidad:
##### • Las salidas digitales pueden utilizarse para una amplia variedad de aplicaciones, como:
##### • Encender y apagar LEDs.
##### • Controlar relés o transistores para activar dispositivos de mayor potencia.
##### • Enviar señales de control a otros circuitos digitales.
##### • Transmitir datos digitales a otros dispositivos.
#### Funcionamiento
Cuando un microcontrolador o dispositivo electrónico configura una salida digital en un nivel lógico alto (1), el pin de salida se conecta a la fuente de alimentación, generando un voltaje alto. Cuando la salida se configura en un nivel lógico bajo (0), el pin de salida se conecta a tierra, generando un voltaje bajo.
### _9. Salidas analogicas_
Las salidas analógicas son pines o terminales en un dispositivo electrónico, como un microcontrolador, que permiten generar señales eléctricas que varían continuamente en amplitud. Estas señales se utilizan para controlar dispositivos externos que requieren señales analógicas, como motores, altavoces o luces.
#### Características Clave
##### • Valores Continuos:
##### Las salidas analógicas pueden generar una gama continua de valores de voltaje o corriente dentro de un rango determinado.
##### Esto contrasta con las salidas digitales, que solo pueden generar dos estados discretos (alto o bajo).
##### • Señales Variables:
##### Las salidas analógicas se utilizan para generar señales que varían en amplitud o frecuencia a lo largo del tiempo.
##### Esto permite controlar dispositivos que requieren señales variables, como motores o altavoces.
##### • Versatilidad:
##### Las salidas analógicas pueden utilizarse para una amplia variedad de aplicaciones, como: Control de velocidad de motores, control de intensidad de luces, generación de señales de audio, Control de temperatura.
#### Funcionamiento
En algunos microcontroladores, las salidas analogicas se generan mediante un módulo DAC (Convertidor Digital-Analógico). Este módulo convierte un valor digital en una señal analógica. En muchos microcontroladores, las salidas analogicas se simulan mediante la técnica PWM (Modulación por Ancho de Pulsos). La PWM genera una señal cuadrada donde se varia el ancho del pulso para simular un voltaje analógico promedio.
### _10. Salida “analógica_” por PWM
PWM es una técnica que modula el ancho de un pulso digital para simular una señal analógica. En lugar de generar una señal analógica continua, el PWM genera una señal cuadrada con un ciclo de trabajo variable.
#### Ciclo de trabajo:
##### • El ciclo de trabajo es la proporción del tiempo en que la señal está en estado alto (encendido) en comparación con el tiempo total del período de la señal.
##### • Se expresa como un porcentaje.
##### • Un ciclo de trabajo del 0% significa que la señal está siempre en estado bajo (apagado).
##### • Un ciclo de trabajo del 100% significa que la señal está siempre en estado alto (encendido).
#### ¿Cómo simula PWM una señal analógica?
##### Promedio de voltaje:
##### • Al variar el ciclo de trabajo de la señal PWM, se varía el voltaje promedio de la señal.
##### • Un ciclo de trabajo más alto resulta en un voltaje promedio más alto.
##### • Un ciclo de trabajo más bajo resulta en un voltaje promedio más bajo.
##### • Este voltaje promedio es lo que se utiliza para simular una señal analógica.
##### Frecuencia de PWM:
##### • La frecuencia de la señal PWM determina la rapidez con la que se repite el ciclo de trabajo.
##### • Una frecuencia alta permite simular señales analógicas con mayor precisión.
##### • Una frecuencia baja puede causar parpadeo en dispositivos como LEDs.
#### Aplicaciones de PWM
##### • Control de velocidad de motores:
Al variar el ciclo de trabajo de la señal PWM, se varía la velocidad del motor.
##### • Control de intensidad de LEDs:
Al variar el ciclo de trabajo de la señal PWM, se varía la intensidad del LED.
##### • Generación de señales de audio:
El PWM se puede utilizar para generar señales de audio, aunque generalmente se requiere un filtro de paso bajo para suavizar la señal.
##### • Control de servomotores:
El PWM se utiliza para controlar la posición de los servomotores.
### _11. Conversión Análogica a digital_
La conversión analógica a digital (ADC) es el proceso de transformar una señal analógica continua en una representación digital discreta. Esto permite que dispositivos digitales, como microcontroladores o computadoras, procesen y almacenen señales analógicas.
#### Proceso de Conversión ADC
##### **1. Muestreo:**
##### • Se toman muestras de la señal analógica a intervalos regulares de tiempo.
##### • La frecuencia de muestreo determina cuántas muestras se toman por segundo.
##### • El teorema de Nyquist-Shannon establece que la frecuencia de muestreo debe ser al menos el doble de la frecuencia más alta presente en la señal analógica para evitar la pérdida de información.
##### ![image](https://github.com/user-attachments/assets/26c5291f-34b7-4531-af80-1b504450a363)
##### **2. Cuantificación:**
##### • Cada muestra se cuantifica, lo que significa que se le asigna un valor numérico discreto.
##### • La cuantificación introduce un error de cuantificación, que es la diferencia entre el valor real de la muestra y el valor cuantificado.
##### • La resolución del ADC determina cuántos niveles de cuantificación están disponibles.
##### ![image](https://github.com/user-attachments/assets/fe84eb62-e2c6-4ac9-a427-f7763a0b6f90)
##### **3. Codificación:**
##### • Los valores cuantificados se codifican en un formato digital, generalmente utilizando un sistema binario.
##### • El número de bits utilizados para la codificación determina la resolución del ADC.
##### ![image](https://github.com/user-attachments/assets/58239684-2437-4f71-9f8d-597d66ad805e)
#### Características Clave de un ADC
##### **1. Resolución:**
##### • El número de bits utilizados para representar la señal analógica.
##### • Una resolución más alta significa que se pueden representar más niveles de cuantificación, lo que resulta en una representación más precisa de la señal analógica.
##### ![image](https://github.com/user-attachments/assets/a2d0f44b-4688-4b09-88cd-f2a23f2a7a28)
##### **2. Frecuencia de muestreo:**
##### • El número de muestras que se toman por segundo.
##### • Una frecuencia de muestreo más alta permite capturar señales analógicas con frecuencias más altas.
##### **3. Rango de entrada:**
##### • El rango de voltajes que el ADC puede medir.
##### • El rango de entrada debe ser adecuado para la señal analógica que se está midiendo.
### _12. Conversión diferencial/bipolar_
#### Conversión Diferencial:
##### La conversión diferencial es una técnica de medición que utiliza dos entradas analógicas para medir la diferencia de voltaje entre ellas. En lugar de medir el voltaje de una sola entrada en relación con una referencia común (como tierra), se mide la diferencia entre dos entradas.
#### Conversión Bipolar:
##### La conversión bipolar es la capacidad de un ADC para medir señales que pueden tener valores positivos y negativos. Un ADC unipolar solo puede medir señales positivas.
### _13. Calculo del tiempo de adquisición_
El cálculo del tiempo de adquisición en un ADC (Convertidor Analógico a Digital) es fundamental para asegurar que la señal analógica se convierta correctamente en una representación digital.
#### Factores que influyen en el tiempo de adquisición:
##### _• Capacitancia de la fuente de señal (Cs):_ La capacitancia de la fuente de señal afecta la rapidez con la que el condensador de muestreo del ADC puede cargarse.
##### _• Resistencia de la fuente de señal (Rs):_ La resistencia de la fuente de señal también afecta la rapidez con la que el condensador de muestreo puede cargarse.
##### _• Capacitancia del condensador de muestreo (Csh):_ La capacitancia del condensador de muestreo del ADC determina cuánto tiempo tarda en cargarse.
##### _• Tiempo de establecimiento del amplificador de muestreo y retención:_ El amplificador de muestreo y retención necesita tiempo para estabilizarse antes de que se pueda realizar la conversión.
##### ![image](https://github.com/user-attachments/assets/12ad7853-6c4c-429f-bd87-624d4ff91a11)
#### Cálculo del tiempo de adquisición:
El tiempo de adquisición se puede calcular utilizando la siguiente fórmula:
##### $• Tacq = n * τ$
#### Donde:
##### • Tacq: Tiempo de adquisición.
##### • n: Número de constantes de tiempo requeridas para la carga del condensador de muestreo (generalmente 6-8).
##### • τ: Constante de tiempo, calculada como $$τ = Rs * (Cs + Csh)$$.
#### Pasos para calcular el tiempo de adquisición:
##### _1. Determinar los valores de Rs, Cs y Csh:_ Estos valores se pueden obtener de las hojas de datos de los componentes o mediante mediciones.
##### _2. Calcular la constante de tiempo (τ):_ Utilizar la fórmula $$τ = Rs * (Cs + Csh).$$
##### _3. Determinar el número de constantes de tiempo (n):_ Este valor depende de la precisión requerida para la conversión. Generalmente, se utilizan 6-8 constantes de tiempo para una precisión de 10-12 bits.
##### _4. Calcular el tiempo de adquisición (Tacq):_ Utilizar la fórmula $$Tacq = n * τ$$.
##### ![image](https://github.com/user-attachments/assets/a0e427e0-3dea-4f98-97d9-d537b4deb84b)
## **Semana 4**
### _1. Interrupciones_
Las interrupciones son eventos que interrumpen la ejecución normal de un programa en un microcontrolador o procesador. Permiten que el microcontrolador responda a eventos externos o internos de manera rápida y eficiente, sin necesidad de estar constantemente revisando (polling) el estado de esos eventos.
#### Características Clave
##### 1. Eventos Asíncronos:
Las interrupciones son eventos asíncronos, lo que significa que pueden ocurrir en cualquier momento, independientemente del flujo principal del programa.
##### 2. Rutinas de Servicio de Interrupción (ISR):
##### • Cuando ocurre una interrupción, el microcontrolador salta a una rutina de servicio de interrupción (ISR), que es un bloque de código específico diseñado para manejar ese evento.
##### • Después de que la ISR se completa, el microcontrolador regresa al flujo principal del programa.
##### 3. Prioridad:
Algunos microcontroladores permiten asignar prioridades a las interrupciones, lo que significa que las interrupciones de mayor prioridad pueden interrumpir las ISR de menor prioridad.
##### 4. Habilitación/Deshabilitación:
##### • Las interrupciones pueden habilitarse o deshabilitarse mediante registros de control.
##### • Esto permite controlar qué eventos pueden interrumpir el flujo principal del programa.
#### Tipos de Interrupciones
##### • Interrupciones Externas:
Ocurren cuando un evento externo, como un cambio en un pin de entrada o una señal de un sensor, activa una interrupción.
##### • Interrupciones Internas:
Ocurren cuando un evento interno, como un temporizador que alcanza un valor predefinido o un error en la conversión ADC, activa una interrupción.
#### Ventajas de las Interrupciones
##### • Respuesta Rápida:
Las interrupciones permiten que el microcontrolador responda rápidamente a eventos críticos.
##### • Eficiencia:
Las interrupciones evitan la necesidad de revisar constantemente el estado de los eventos, lo que ahorra tiempo de procesamiento.
##### • Multitarea:
Las interrupciones permiten que el microcontrolador realice múltiples tareas simultáneamente.
### _2. Bits IE e IF_
#### Bits IE (Interrupt Enable):
##### Función:
##### • Los bits IE se utilizan para habilitar o deshabilitar interrupciones específicas.
##### • Cada fuente de interrupción (por ejemplo, un periférico, un pin externo) tiene su propio bit IE correspondiente.
##### • Cuando un bit IE está establecido (generalmente a 1), la interrupción correspondiente está habilitada y puede ocurrir.
##### • Cuando un bit IE está borrado (generalmente a 0), la interrupción correspondiente está deshabilitada y no ocurrirá.
##### Uso:
##### • Los bits IE permiten al programador controlar qué eventos pueden interrumpir la ejecución del programa principal.
##### • Esto es útil para priorizar interrupciones y evitar que eventos no críticos interrumpan tareas importantes.
##### • En los microcontroladores PIC, estos bits se encuentran generalmente dentro de registros de control específicos de cada periférico o en el registro INTCON.
#### Bits IF (Interrupt Flag)
##### Función:
##### • Los bits IF son indicadores que se establecen automáticamente cuando ocurre una interrupción.
##### • Cada fuente de interrupción tiene su propio bit IF correspondiente.
##### • Cuando ocurre una interrupción, el bit IF correspondiente se establece (generalmente a 1).
##### • El bit IF se borra generalmente por el software dentro de la rutina de servicio de interrupción (ISR) para indicar que la interrupción ha sido manejada.
##### Uso:
##### • Los bits IF permiten al programador determinar qué fuente de interrupción activó la ISR.
##### • Esto es útil cuando varias fuentes de interrupción comparten la misma ISR.
##### • Es muy importante limpiar los bits IF dentro de la ISR para evitar que la interrupción se repita inmediatamente.
##### • En los microcontroladores PIC, estos bits se encuentran generalmente dentro de registros de control específicos de cada periférico o en el registro INTCON.
#### Relación entre IE e IF
##### • Los bits IE controlan si una interrupción puede ocurrir.
##### • Los bits IF indican si una interrupción ha ocurrido.
##### • Cuando ocurre un evento que podría generar una interrupción, el microcontrolador verifica el bit IE correspondiente.
##### • Si el bit IE está habilitado, la interrupción ocurre y el bit IF correspondiente se establece.
### _3. Control de la interrupción_
El control de interrupciones en microcontroladores PIC implica habilitar y deshabilitar interrupciones, así como manejar las banderas de interrupción. Aquí te presento los pasos y registros clave involucrados:
#### 1. Habilitación de Interrupciones Globales
##### Registro INTCON:
##### • El bit GIE (Global Interrupt Enable) en el registro INTCON controla si las interrupciones están habilitadas globalmente.
##### • Para habilitar todas las interrupciones habilitadas individualmente, se debe establecer GIE = 1.
##### • Para deshabilitar todas las interrupciones, se debe establecer GIE = 0.
#### 2. Habilitación de Interrupciones Periféricas/Específicas
##### Registros de Control de Periféricos:
##### • Cada periférico (por ejemplo, temporizadores, UART, ADC) tiene sus propios registros de control que contienen bits de habilitación de interrupción (IE).
##### • Por ejemplo, el bit TMR0IE en el registro INTCON habilita la interrupción del temporizador 0.
##### • Para habilitar una interrupción específica, se debe establecer el bit IE correspondiente a 1.
##### • Para deshabilitar una interrupción específica, se debe borrar el bit IE correspondiente a 0.
#### 3. Manejo de Banderas de Interrupción
##### Registros de Control de Periféricos:
##### • Cada periférico tiene sus propios registros de control que contienen banderas de interrupción (IF).
##### • Por ejemplo, el bit TMR0IF en el registro INTCON indica si ha ocurrido una interrupción del temporizador 0.
##### • Cuando ocurre una interrupción, el bit IF correspondiente se establece automáticamente a 1.
##### Rutina de Servicio de Interrupción (ISR):
##### • Dentro de la ISR, es crucial borrar el bit IF correspondiente para indicar que la interrupción ha sido manejada.
##### • Si no se borra el bit IF, la interrupción podría ocurrir repetidamente.
#### 4. Rutina de Servicio de Interrupción (ISR)
##### • Vector de Interrupción:
Cuando ocurre una interrupción, el microcontrolador salta a un vector de interrupción específico, que es una dirección de memoria que contiene el inicio de la ISR.
##### • ISR:
La ISR es una función especial que se ejecuta cuando ocurre una interrupción.Dentro de la ISR, se debe realizar lo siguiente:
###### • Guardar el contexto (registros) si es necesario.
###### • Identificar la fuente de la interrupción (mediante los bits IF).
###### • Realizar la tarea correspondiente.
###### • Borrar el bit IF correspondiente.
###### • Restaurar el contexto (registros) si se guardó.
###### • Regresar de la interrupción (mediante la instrucción RETFIE).
##### ![image](https://github.com/user-attachments/assets/0878f39b-72e2-462f-971f-a0f70e4330de)
### _4. Prioridad de interrupciones_
La prioridad de interrupciones es un mecanismo que permite a un microcontrolador o procesador manejar múltiples interrupciones de diferentes niveles de importancia. Cuando varias interrupciones ocurren simultáneamente, el microcontrolador utiliza la prioridad para determinar cuál interrupción debe ser atendida primero.
#### ¿Por qué es necesaria la prioridad de interrupciones?
##### Eventos críticos:
Algunas interrupciones pueden ser más críticas que otras. Por ejemplo, una interrupción que indica un fallo de energía puede ser más importante que una interrupción que indica que un usuario ha presionado un botón.
##### Manejo de múltiples periféricos:
Los microcontroladores modernos tienen muchos periféricos que pueden generar interrupciones. La prioridad de interrupciones permite al microcontrolador manejar estas interrupciones de manera eficiente.
##### Sistemas en tiempo real:
En sistemas en tiempo real, es crucial que las interrupciones críticas se atiendan rápidamente. La prioridad de interrupciones garantiza que esto suceda.
#### ¿Cómo funciona la prioridad de interrupciones?
##### Niveles de prioridad:
###### • A cada fuente de interrupción se le asigna un nivel de prioridad.
###### • Los niveles de prioridad pueden ser numéricos (por ejemplo, 0-7) o simbólicos (por ejemplo, alta, media, baja).
##### Arbitraje:
###### • Cuando ocurren varias interrupciones simultáneamente, el microcontrolador utiliza un mecanismo de arbitraje para determinar cuál interrupción debe ser atendida primero.
###### • El mecanismo de arbitraje generalmente selecciona la interrupción con el nivel de prioridad más alto.
##### Interrupciones anidadas:
###### • En algunos microcontroladores, una interrupción de alta prioridad puede interrumpir la ejecución de una rutina de servicio de interrupción (ISR) de baja prioridad.
#### Implementación en Microcontroladores PIC
###### • Algunos microcontroladores PIC tienen un sistema de prioridad de interrupciones de dos niveles (alta y baja).
###### • Se utilizan registros de control específicos para habilitar la prioridad de interrupciones y asignar niveles de prioridad a las fuentes de interrupción.
###### • Es importante consultar la hoja de datos del microcontrolador PIC específico para obtener información detallada sobre la implementación de la prioridad de interrupciones.
### _5. Interrupciones externas_
Las interrupciones externas son eventos que ocurren fuera del microcontrolador y que pueden interrumpir la ejecución del programa principal. En los microcontroladores PIC, las fuentes de interrupción externa más comunes son:
#### 1. Interrupción por el pin RB0/INT (INT0):
##### • Este pin puede configurarse para generar una interrupción cuando ocurre un cambio de nivel lógico (flanco de subida o de bajada).
##### • Es útil para detectar eventos como pulsaciones de botones o señales de sensores.
#### 2. Interrupción por cambio lógico en el puerto B (RB4:RB7):
##### • Los pines RB4 a RB7 pueden configurarse para generar una interrupción cuando ocurre un cambio de nivel lógico en cualquiera de ellos.
##### • Esto es útil para detectar eventos como cambios en el estado de interruptores o sensores conectados al puerto B.
#### 3. Interrupciones por los pines RB1/INT1 y RB2/INT2:
##### • En algunos microcontroladores PIC, los pines RB1 y RB2 también pueden configurarse como fuentes de interrupción externa (INT1 e INT2).
##### • Al igual que el pin RB0/INT, estos pines pueden configurarse para generar interrupciones en flancos de subida o de bajada.
#### Configuración de Interrupciones Externas
Para utilizar interrupciones externas en un microcontrolador PIC, es necesario configurar los siguientes registros:
##### 1. Registro INTCON:
##### • Contiene los bits de habilitación de interrupciones globales (GIE) y periféricas (PEIE), así como las banderas de interrupción (IF) para las interrupciones externas.
##### • También contiene los bits de habilitación de interrupciones externas (INTE) y de cambio lógico en el puerto B (RBIE).
##### 2. Registro OPTION_REG:
Contiene el bit INTEDG, que permite seleccionar el flanco de interrupción (subida o bajada) para el pin RB0/INT.
##### 3. Registros de control de periféricos:
Algunos periféricos pueden tener registros de control adicionales que afectan el comportamiento de las interrupciones externas.
## **Conclusión**
A lo largo de este corte , hemos explorado la esencia de los sistemas digitales y su interacción con el mundo analógico, centrando nuestra atención en los microcontroladores y sus capacidades.
#### 1. La dualidad analógica-digital: Hemos comprendido que el mundo que nos rodea es fundamentalmente analógico, caracterizado por señales continuas que varían en amplitud y frecuencia. Sin embargo, el procesamiento, almacenamiento y transmisión eficientes de información requieren una representación digital, discreta y binaria. Aquí es donde entran en juego los microcontroladores, dispositivos que actúan como puentes entre estos dos dominios.
#### 2. Conversión y simulación: La conversión analógica a digital (ADC) se erige como un proceso crucial, permitiendo a los microcontroladores "leer" el mundo analógico mediante muestreo, cuantificación y codificación de señales. Por otro lado, la modulación por ancho de pulsos (PWM) emerge como una técnica ingeniosa para simular salidas analógicas a partir de señales digitales, abriendo un abanico de posibilidades para el control de dispositivos.
#### 3. Puertos y señales Los puertos de entrada/salida (E/S) digitales se revelan como las puertas de comunicación del microcontrolador con el exterior, permitiendo la lectura de señales discretas (entradas) y el control de dispositivos (salidas). Las resistencias de pull-up y pull-down, a su vez, aseguran la estabilidad de las señales digitales, evitando estados indeterminados.
#### Interrupciones: Las interrupciones se presentan como eventos asíncronos que interrumpen el flujo normal del programa, permitiendo al microcontrolador responder rápidamente a eventos críticos sin necesidad de un sondeo constante. La prioridad de interrupciones garantiza que los eventos más importantes se atiendan primero, optimizando la capacidad de respuesta del sistema.
#### 4. Control y configuración: El control de interrupciones implica la habilitación y deshabilitación de interrupciones globales y periféricas, así como el manejo de banderas de interrupción dentro de las rutinas de servicio de interrupción (ISR). La configuración adecuada de los registros de control es esencial para el funcionamiento correcto de las interrupciones.
En conclusión los microcontroladores, con sus capacidades de conversión analógica-digital, simulación de salidas analógicas, puertos de E/S y manejo de interrupciones, se han convertido en pilares fundamentales de la electrónica moderna. Su versatilidad y eficiencia los hacen indispensables en una amplia gama de aplicaciones, desde sistemas embebidos hasta dispositivos inteligentes y el Internet de las cosas (IoT). La comprensión profunda de estos conceptos es esencial para el diseño y desarrollo de sistemas electrónicos robustos y eficientes.
## Referencias
#### https://aulas.ecci.edu.co/mod/resource/view.php?id=217938
#### https://aulas.ecci.edu.co/mod/resource/view.php?id=217940
#### https://aulas.ecci.edu.co/mod/resource/view.php?id=217945
#### https://aulas.ecci.edu.co/mod/resource/view.php?id=217948
#### Datasheet pic18f4550: https://www.microchip.com/
