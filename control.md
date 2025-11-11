sistemas de control.md 2025-11-07
2 / 10
Sensor: Mide la variable controlada, como la temperatura, la posición o la velocidad. La medición debe
ser precisa para que el controlador ajuste correctamente la señal de control.
Controlador: Se encarga de calcular y enviar la señal de control adecuada al actuador, basándose en la
retroalimentación obtenida del sensor.
Actuador: El actuador ejecuta la señal de control, modificando la planta. Esto podría ser un motor, una
válvula o un calentador, entre otros.
Modelado de Sistemas y Funciones de Transferencia
Todo sistema físico (una bomba, un horno, un motor, una válvula) responde con cierto retardo o dinámica
ante una señal de entrada. Para poder diseñar un controlador, necesitamos una representación matemática
que describa cómo responde ese sistema.
El modelado es el proceso de crear una representación abstracta de un sistema físico o proceso, utilizando
herramientas matemáticas. Este modelo permite predecir cómo se comportará el sistema bajo diferentes
condiciones y es fundamental para diseñar el controlador. EL modelado nos permite:
1. Permite predecir la respuesta temporal del sistema ante una acción de control o una perturbación
(cambio externo).
2. Diseño: Es la base matemática para diseñar, analizar y sintonizar un controlador (como el PID).
3. Simulación: Permite probar el controlador en un entorno virtual antes de implementarlo en el sistema
físico real.
Hay tres formas basicas que podemso expresar el modelado:
Ecuaciones Integro Diferenciales (ED): Son la forma original del modelo, basadas en leyes físicas (Leyes
de Newton para sistemas mecánicos, Leyes de Kirchhoff para eléctricos, balances de energía o masa
para químicos).La transformada de Laplace nos permite convertir esas ecuaciones integro-diferenciales
en expresiones algebraicas, más fáciles de manipular y entender.
Transformada de Laplace:
$L{f(t)}=F(s)=>\int_0^\infty f(t)^{-st}dt$
Ejemplo: Para un sistema masa-resorte-amortiguador, se aplica la Segunda Ley de Newton para
obtener una ED de segundo orden.
Función de Transferencia $G(s)$ es la representación más usada en control clásico. Se obtiene aplicando
la Transformada de Laplace a la ecuación diferencial lineal.$$G(s) = \frac{\text{Salida}(s)}{\text{Entrada}
(s)}$$
En el diagrama de bloques, esta función representa al Proceso/Planta.
Con el modelo obtenido podemos estudiar las prespuesta temporal del sistema. La respuesta temporal de un
sistema es el comportamiento de su salida (Variable a Controlada) a lo largo del tiempo, luego de que se le
aplica una señal de entrada específica. Es la forma en que el sistema reacciona dinámicamente. Para el análisis,
se usan entradas de prueba estándar, siendo la más común la Entrada Escalón Unitario.
A esta respuesta temporal la analizaremos en dos partes:
1. Respuesta Transitoria
