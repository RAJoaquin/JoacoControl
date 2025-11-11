# JoacoControl

![ECEA Logo](ecea-logo.png)
<!-- Si tenés el logo, ponelo en la misma carpeta y se verá -->

# Sistemas de Control

*Título:* "Introducción a los sistemas de control"  
*Creado:* 07/11/2025  
*Versión:* 0.001  
*Publicado:* no  
*Autor:* Gabriel A. Vásquez H.

## 1. ¿Qué es el control?

*Definición de control:*  
El control es un proceso de modificación y ajuste de la entrada de un sistema para obtener una *salida deseada. Se basa en reducir o eliminar la discrepancia entre el valor real de una variable (temperatura, velocidad, presión, nivel, etc.) y el valor deseado o **referencia (SP)*.

### Sistema en lazo abierto
- La señal de control se aplica *sin retroalimentación*.
- Ventaja: simple y rápido.
- Desventaja: no se adapta a perturbaciones.
- Ejemplo: sistema de riego por temporizador.

### Sistema en lazo cerrado
- La señal de control se ajusta según la *retroalimentación* de la salida.
- Mejora la precisión y corrige automáticamente.
- Ejemplo: termostato de calefacción.

### Elementos de un sistema de control
- *Planta*: proceso físico a controlar (motor, tanque, horno, etc.).
- *Sensor*: mide la variable controlada (PV).
- *Controlador*: calcula la señal de control (PID).
- *Actuador*: ejecuta la orden (válvula, motor, calentador).

## Modelado de Sistemas y Funciones de Transferencia

Todo sistema físico responde con cierta *dinámica* ante una entrada. Necesitamos un modelo matemático para predecir y diseñar controladores.

*Ventajas del modelado:*
1. Predecir respuesta temporal ante escalón o perturbación.
2. Diseñar y sintonizar controladores (PID).
3. Simular antes de implementar.

### Formas de modelado
- *Ecuaciones integro-diferenciales* (leyes físicas).
- *Transformada de Laplace* → álgebra:

$$ \mathcal{L}\{f(t)\} = F(s) = \int_0^\infty f(t)e^{-st}dt $$

- *Función de transferencia* $G(s)$ (la más usada):

$$ G(s) = \frac{Y(s)}{U(s)} $$

### Respuesta temporal (entrada escalón unitario)
1. *Respuesta transitoria*
   - Tiempo de subida $t_r$: 10 % → 90 %
   - Sobrepaso máximo $M_p$
   - Tiempo de pico $t_p$

2. *Respuesta en estado estacionario*
   - Error en estado estacionario $e_{ss}$
   - Tiempo de asentamiento $t_s$ (±2 % o ±5 %)

## Sistemas de Primer Orden

Ecuación diferencial:

$$ a_1 \frac{dy(t)}{dt} + a_0 y(t) = b_0 u(t) $$

Ejemplo tanque:

$$ A \frac{dh(t)}{dt} = k_1 \alpha(t-\Theta) - k_2 h(t) $$

Función de transferencia:

$$ G(s) = \frac{H(s)}{\alpha(s)} = \frac{K}{\tau s + 1} e^{-\Theta s} $$

Respuesta al escalón $AK$:

$$ h(t) = AK \left(1 - e^{-t/\tau}\right) \quad (t \geq \Theta) $$

![Respuesta primer orden](primer_orden.png)

- $\tau$: constante de tiempo → 63.2 % del valor final
- Tiempo de asentamiento: $t_s \approx 4\tau$ (2 %) o $5\tau$ (1 %)

*Polo:* $s = -1/\tau$
- Cerca del origen → respuesta lenta
- Lejos → respuesta rápida

*Teorema del valor final:*

$$ h_{ss} = \lim_{t \to \infty} h(t) = AK $$

## Sistema realimentado (lazo cerrado)

1. *Error:* $e(t) = SP - PV(t)$
2. *Retroalimentación:* mide $PV$ y la devuelve.
3. *Acción de control:* $u(t)$ generada por el PID.

### Polos, ceros y estabilidad
- Estable → todos los polos con $\Re(s) < 0$
- Inestable → algún polo con $\Re(s) > 0$
- Oscilatorio → polos imaginarios puros

## Control PID – Ejemplo: Tanque de nivel

![Diagrama tanque](tanque.png)

- $a_e$: apertura válvula entrada (manipulada)
- $a_s$: apertura válvula salida (perturbación fija)
- Realimentación unitaria

![Lazo cerrado PID](pid_tanque.png)

### Control Proporcional (P)

$$ u(t) = K_p e(t) $$
$$ a_e = K_p (H_r - H) $$

- Cuanto mayor $K_p$ → rampa más empinada
- *Banda proporcional:* $BP = \frac{100}{K_p}\%$

Función de transferencia en lazo cerrado (planta $P(s) = \frac{K}{\tau s + 1}$):

$$ T(s) = \frac{K_p P(s)}{1 + K_p P(s)} \quad \Rightarrow \quad T_{ss} = \frac{K_p K}{1 + K_p K} $$

*Conclusión:* Control P *NO elimina* el error en estado estacionario.

*Solución industrial:* *BIAS* (término constante)

![Control P con Bias](p_bias.png)

---
