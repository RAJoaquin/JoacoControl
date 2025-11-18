# RESUMEN COMPLETO – Sistemas Embebidos (MEF, I2C, SPI, RTOS)

## 1) Máquinas de Estados Finitos (MEF)

### ¿Qué es un modelo?
Un modelo es una **representación simplificada** de un sistema. Debe ser:
- Abstracto
- Comprensible
- Preciso
- Predictivo
- Económico

En sistemas embebidos, los modelos también se usan para generar código.

### Máquina de Estados Finitos (MEF)
Una MEF describe **sistemas cuyo comportamiento depende de**:
- Eventos actuales
- Eventos pasados

Se usa para:
- Teclados
- Displays
- Alarmas
- Menús
- Interfaces con el usuario

#### Componentes de una MEF:
- Estados
- Entradas
- Salidas
- Transiciones

### Tipos de MEF
#### Moore
- Salida depende solo del estado.
- Ejemplo: semáforo.


![[Pasted image 20251117215257.png]]
  
Detector de flanco: la salida es la XOR de las dos entradas más recientes.

#### Mealy
- Salida depende de estado actual + entrada.
- Ejemplo: movimiento de un robot.

![[Pasted image 20251117215311.png]]

Detector de flanco: la salida es la XOR de las dos entradas más recientes.


### Implementación típica en C
```c
typedef enum {ESTADO_INICIAL, 
			  ESTADO_1, 
			  ESTADO_2, 
			  ESTADO_N} estadoMEF_t;
			  
estadoMEF_t = EstadoActual; //variable de estado actual
tick_t tInicio;
// prototipo de funciones 
void InicializarMEF(void);
void ActualizarMEF(void);

void InicializarMEF(void) {
    EstadoActual = ESTADO_INICIAL;
}

void ActualizarMEF(void) {
    switch(EstadoActual) {
        case ESTADO_INICIAL:
            if(condicion) EstadoActual = ESTADO_N;
        break;
    }
}
```

---
## 2) RTOS (Real Time Operative Sistem)– Sistemas Operativos Embebidos

#### clasificación según su funcionalidad 

- Nano-Kernel -> maneja interrupciones y despacho de tareas 
- Micro-Kernel -> maneja interrupciones, planificación y despacho de tareas
- Kernel -> maneja interrupciones, planificación, comunicación y despacho de tareas 
- Executive -> maneja interrupciones, planificación, comunicación y despacho de tareas, también brinda soporte a archivos y discos + gestión de almacenamiento.
- Operating System -> sistema completo, incluye interfaz de usuario y shell 


### Arquitecturas

#### Time Triggered (Disparadas por Tiempo)
- Basada en ticks periódicos del timer.
- Determinista; ideal para tareas periódicas.
- Se la denomina RTI (Real Time Interrupt)
- Cada interrupcion es indica que hay que ejecutar una tarea
- 

#### Event Triggered (Reactivas)
- Basada en interrupciones.
- Menos predecible, reactiva a eventos asíncronos.
- Si hay eventos que ocurren durante la ejecucion de una tarea y deben ejecutar otra 

![[Pasted image 20251117235316.png]]

- Si se requiere ejecutar mas de una tarea a la vez (Sistemas Disparados por Tiempo)

![[Pasted image 20251117235402.png]]



#### 

### Tiempo real
- Hard RT: **plazos estrictos**; fallo tiene consecuencias graves.
- Soft RT: **tolera retrasos** leves; ejemplo: interfaces de usuario.

![[Pasted image 20251117235613.png]]

### Sistemas cooperativos
- Las tareas no se interrumpen entre sí.
- Deben ser **no bloqueantes** y terminar antes del siguiente tick.

![[Pasted image 20251118000132.png]]


![[Pasted image 20251118002236.png]]

### Sistemas apropiativos (preemptive)
- El planificador de tareas interrumpe las tareas y realiza context switch.
- Más complejo pero ofrece mejor control temporal.

![[Pasted image 20251118001857.png]]

Más allá que la tarea se interrumpa, se guardan los valores que se tenían antes de ejecutarse

![[Pasted image 20251118002210.png]]

### Planificadores
- Ejecutivo cíclico: planificación por tabla; simple y predecible.
- FPS (Fixed Priority Scheduling): prioridades fijas; rate
  monotonic = periodos mayores → prioridad menor.

### ciclo de vida de una tarea 

| Atributo | Descripcion |
|-----------|---------------|
| Period | intervalo de tiempo que indica cuan seguido se debería iniciar una tarea |
| Release Time | momento en el tiempo el cual la tarea se vuelve disponible para la ejecución |
| Deadline |Momento en el tiempo el cual la tarea debería estar completada |
| Execution Time| Tiempo que tarda la tarea en ejecutarse (peor caso)|

![[Pasted image 20251118003630.png]]

---

## 3) Comunicación I2C

### Características
- 2 líneas: SDA (datos), SCL (clock)
- Master–Slave
- Velocidades típicas: 100 kHz, 400 kHz, 1 MHz
- Muy usado por sensores, memorias y pantallas (estandarizado)

### Hardware

![[Pasted image 20251118004002.png]]

### Direccionamiento
- Cada dispositivo está direccionado individualmente por software
- El primer byte tras START contiene la dirección del slave y el siguiente bit R/W (0 = write, 1 = read).
- Direccionamiento de 7 bits 
	- ![[Pasted image 20251118093441.png]]
- Direccionamiento de 10 bits
	- ![[Pasted image 20251118093516.png]]

### START / STOP
Las condiciones de START y STOP se detectan cuando SDA cambia mientras SCL está en alto.

### ACK / NACK
- En el noveno pulso de reloj (9° bit) el receptor debe poner SDA en bajo para confirmar (ACK).
- Si no hay ACK → NACK → el master debe abortar o reintentar.
- Hay que programarlo manualmente

![[Pasted image 20251118093724.png]]


### Clock Stretching
El esclavo puede mantener SCL en bajo para **solicitar más tiempo**, por ejemplo si está procesando una conversión ADC o escribiendo memoria interna.

![[Pasted image 20251118094057.png]]

### Arbitraje
Si dos masters inician al mismo tiempo, el arbitraje se resuelve comparando el valor en SDA mientras SCL está en alto.

![[Pasted image 20251118093922.png]]

### Debug
Usar un analizador lógico para ver tramas I2C y detectar errores.

```c

i2cStart ();  // inicio de comunicación                          
err = i2cWriteByte (byteAEnviar*1*); // enviar un byte al esclavo  
 if (err) {_mensaje de error_}; // si hay error informa  
dato = i2cReadByte (); // leer un byte del esclavo                            
i2cReadAck (); // Envía ACK para leer más de un byte                                      
i2cReadNoAck () ; // Envía NACK cuando es el ultimo byte  
i2cStop (); // Finaliza la comunicación I2C

```


---



## 4) Comunicación SPI

### Características
- Bus serie síncrono, full duplex.
- Muy rápido (hasta ~10 Mbps).
- No tiene ACK/NACK ni direccionamiento estándar.
- Permite controlar casi cualquier dispositivo digital que acepte un flujo de bits serie regulado por reloj
- Se puede implementar simplemente utilizando un shift register o registro de desplazamiento serie
![[Pasted image 20251118103625.png]]

### Líneas
- SCK / SCLK : Reloj en serie
- SDO / MOSI : Salida de datos en serie. Master -> Slave (datos)
- SDI / MISO : Entrada de datos en serie. Slave -> Master (datos)
- CS / SS : Selección del chip. Chip Select (activo bajo)

### Modos SPI
Definidos por CPOL (polaridad) y CPHA (fase): modos 0..3.

### Ventajas / Desventajas
- Ventajas: 
	- Velocidad 
	- Simplicidad.
	- Soporta multiples slaves
- Desventajas: 
	- Más cables 
	- Un CS por dispositivo 
	- Sin control de flujo.

### Guia basica para conversacion SPI

```c

PIN_nCS = 0; // Habilita el esclavo  
spiWriteByte (byteAEnviar*1*); // Envía Comando de lectura  
spiReadByte (); //Descarta lo recibido  
spiWriteByte (0x00); // genera el clock  
dato = spiReadByte (); // recibe el byte del esclavo  
PIN_nCS = 1; // Termina la comunicación con el esclavo

```
---

# Ejercicio 13
## Arrancamos por los #include

```c
#include <xc.h>
#include <stdint.h>
#include <uart.h>
#include <user.h>
#include <tick.h>

```

## definimos los estados con #typedef

```c

typedef enum {
	Desactivado,
	Activado,
	Sonando,
	Error} EstadoMEF_t;
	
EstadoMEF_t = EstadoActual;

```


## iniciamos la maquina de estados

```c

voidActualizarMEF (void){
	static tick_t tInicio;
	uint8_t Letra;

```

## Switch case (Desactivado)

```c

switch(EstadoActual){
		case Desactivado:
			if (uartReadByte(&Letra) == 1){
				if (Letra == 'A'){
					if(Pin_Tec1 == 1){
						Pin_Led_V = 0;
						EstadoActual = Activo; 
					}else {
						Pin_Led_V = 0;
						Pin_Led_R = 1;
						EstadoActual = Error;
					}
				}
			}break;

```

## Switch case (activado)

```c
case Activado:
			if (Pin_Tec1 == 0){
				Pin_Led_AM = 1;
				tInicio = tickRead();
				EstadoActual = Sonando;
			}break;

```

## Switch case (Sonando)

```c
case Sonando:
			if (uartReadByte(&Letra)){
				if (Letra == 'd' || tickRead() = tInicio > 1000){
					Pin_Led_AM = 0;
					Pin_Led_V = 1;
					EstadoActual = Desactivado;
				}
			}break;

```

## Switch case (Error)

```c
case Error:
			if (Pin_Tec2 == 0){
			Pin_Led_V = 1;
			Pin_Led_R = 0,
			EstadoActual = Desactivado;
			}break;

```

## Switch case (Default)

```c
default:
			InicioMEF();	

```



## Nos quedaría algo así 
```c

#include <xc.h>
#include <stdint.h>
#include <uart.h>
#include <user.h>
#include <tick.h>

typedef enum {
	Desactivado,
	Activado,
	Sonando,
	Error} EstadoMEF_t;
	
EstadoMEF_t = EstadoActual;

voidActualizarMEF (void){
	static tick_t tInicio;
	uint8_t Letra;
	switch(EstadoActual){
		case Desactivado:
			if (uartReadByte(&Letra) == 1){
				if (Letra == 'A'){
					if(Pin_Tec1 == 1){
						Pin_Led_V = 0;
						EstadoActual = Activo; 
					}else {
						Pin_Led_V = 0;
						Pin_Led_R = 1;
						EstadoActual = Error;
					}
				}
			}break;
		case Activado:
			if (Pin_Tec1 == 0){
				Pin_Led_AM = 1;
				tInicio = tickRead();
				EstadoActual = Sonando;
			}break;
		case Sonando:
			if (uartReadByte(&Letra)){
				if (Letra == 'd' || tickRead() = tInicio > 1000){
					Pin_Led_AM = 0;
					Pin_Led_V = 1;
					EstadoActual = Desactivado;
				}
			}break;
		case Error:
			if (Pin_Tec2 == 0){
			Pin_Led_V = 1;
			Pin_Led_R = 0,
			EstadoActual = Desactivado;
			}break;
		default:
			InicioMEF();	
	}
}

```
