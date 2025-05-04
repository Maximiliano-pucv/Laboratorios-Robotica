# Laboratorios-Robotica

## Laboratorio 1

### Integrantes
1. Maximiliano Esteban Miño Silva
2. Tomás Abdiel Pérez Jeria
3. Francisca Javiera Silva Castro
4. Matías Eliseo Villegas Muñoz


### Preguntas

**Parte 1: Identificación de componentes y configuración**

1. **¿Qué función cumplen los sensores, actuadores y controladores en el robot?**  
* **Sensores** 

  Estos son los elementos que se encargan de capturar la información del mundo exterior y del propio robot, es decir, son aquellos que perciben el entorno o el estado interno del robot, para luego convertir esta información en señales eléctricas que puedan ser interpretadas por el controlador. 

* **Actuadores** 

  Son los dispositivos que transforman señales eléctricas en movimiento o acción física. Estos reciben señales de control y las traducen en acciones físicas que le permiten al robot desplazarse, levantar objetos o manipular elementos.

* **Controladores** 

  En sí, los controladores son la parte central del robot, ya que son aquellos que reciben, interpretan y procesan la información de los sensores, aplican algoritmos de control y generan señales de salida para los actuadores, que luego ejecutan las acciones 

2. **¿Cómo se puede estimar la velocidad sin encoders?**  
   Para poder estimar la velocidad sin la necesidad de encoders, se puede hacer uso de otros sensores, como el sensor IMU, el cuál contiene un acelerómetro para medir la aceleración lineal y un giroscopio para detectar rotaciones, que son componentes útiles para poder  estimar la velocidad. Además del IMU, también se puede hacer uso de  algoritmos de navegación autónoma como SLAM, para estimar el desplazamiento del robot.

   Por otro lado, cuando no se disponen de sensores físicos, otra forma de estimar la velocidad es en base al comportamiento del robot, a través de la medición de variables como distancia y tiempo transcurrido, obteniendo las velocidades angulares de cada rueda o la velocidad lineal del robot en un momento dado., 

3. **¿Cómo afecta la falta de encoders a la precisión del movimiento?**  
   La falta de encoders afecta de forma negativa la precisión del movimiento de un robot al impedir una estimación exacta de la posición y velocidad. Esto puede generar movimientos irregulares, errores de orientación y una mayor acumulación de errores. Además, al no contar con una retroalimentación directa, el sistema pierde la capacidad de corregir en tiempo real lo que limita la ejecución precisa de maniobras y reduce la efectividad de algoritmos de navegación autónoma. 

   

4. **¿Qué es PWM y cómo ayuda a controlar la velocidad de los motores?**  
   PWM es una técnica que permite controlar la potencia entregada a un dispositivo eléctrico, como un motor, sin tener que variar el voltaje de manera analógica, debido a que convierte una señal digital en una señal analógica simulada, para así controlar la velocidad de un motor o la salida de potencia de un circuito. Este se basa en el uso de una señal digital que alterna rápidamente entre los estados encendido y apagado a una frecuencia fija.  

   Para el control de la velocidad de los motores, PWM permite controlar la velocidad de un motor variando cuánto tiempo recibe energía en cada ciclo, sin necesidad de cambiar el voltaje de alimentación, ya que este responde al promedio del voltaje recibido, lo que hace que la velocidad del motor se pueda ajustar de forma precisa y eficiente. 

5. **¿Cómo afecta el control de velocidad a la precisión de la navegación sin encoders?**  
   Sin encoders, el sistema no puede verificar si el motor se mueve en la velocidad deseada. Sin embargo, aunque el PWM permite regular la cantidad de energía enviada al motor, no garantiza que el motor responda correctamente. 

   Si bien el control de velocidad mediante PWM permite ajustar la energía suministrada a los motores, sin encoders no es posible confirmar si el motor se está moviendo realmente a la velocidad deseada. Esto puede llevar a que surjan errores en la posición del robot, especialmente en trayectorias largas o en tareas que requieren alta precisión, ya que no hay forma de corregir desviaciones. En consecuencia, la navegación se vuelve menos precisa y confiable.

**Parte 2: Cinemática y Dinámica de Robots Móviles usando un IMU** 

1.  **¿Cómo se calcula la velocidad del robot sin encoders usando PWM?**  
   	El control mediante PWM permite ajustar la velocidad de rotación de un motor de corriente continua al variar el ciclo de trabajo de una señal digital. Sin embargo, PWM no proporciona una medición directa de la velocidad, ya que este actúa como un mecanismo de control. 

   En el caso de estimar la velocidad del robot sin encoders, una opción es utilizar los datos proporcionados por un sensor IMU, o más bien el acelerómetro que este contiene, ya que puede medir la aceleración lineal del robot en diferentes ejes. En base a estas mediciones, es posible realizar una integración de la aceleración respecto al tiempo, lo que permite obtener una estimación de la velocidad lineal del robot. Sin embargo, es necesario realizar una calibración adecuada del sensor y utilizar técnicas de filtrado para reducir los errores acumulativos, como el ruido. 

2. **¿Cómo factores afectan la trayectoria y velocidad del robot al cambiar los intervalos de tiempo?**  
   Cambiar los intervalos de tiempo, usados para actualizar el control, influye directamente en aspectos fundamentales del movimiento del robot:

* La precisión de integración, esto se refiere a que si los intervalos llegan a ser muy grandes al integrar la aceleración, puede darse el caso que el error acumulado sea mayor, afectando la estimación de velocidad y posición.    
* Capacidad de reacción, los intervalos de tiempo más cortos permiten que el robot reaccione con mayor rapidez a cambios en el entorno o en el movimiento del robot.  
* Estabilidad del control, al trabajar con intervalos de tiempo que puedan ser cambiados, el control hacia el robot puede volverse inestable.  
* Aumento de latencia en la corrección de la trayectoria al introducir intervalos más largos de tiempo.   
* Estimación de variables, como la velocidad, que depende de mediciones de posición/distancia en función del tiempo puede no ser tan precisa.


3.  **¿Cuáles son las ventajas y desventajas de usar un IMU para ajustar la dirección en lugar de encoders?**

	Las ventajas y desventajas que tiene el uso de un IMU, en lugar de encoders, para ajustar la dirección son:

Ventajas del IMU:

* Medición de orientación: el IMU, al incluir giroscopio y acelerómetro, permite detectar giros y cambios de dirección en los ejes X,Y,Z aunque las ruedas patinen, es decir, no tengan tracción.  
* No depende de un contacto con el suelo, otorgando una mayor flexibilidad en su ubicación y aplicación.  
* Tiene un diseño compacto y sin partes adicionales, lo que lo hace más fácil de integrar en otros sistemas.

Desventajas del IMU:

* Deriva: los giroscopios acumulan error con el paso del tiempo si no existe un método de corrección externa, como la fusión sensorial.   
* Ruido: los acelerómetros y giroscopios son sensibles al ruido y a las vibraciones, siendo más difícil de interpretar de forma directa  
* Errores de integración: para lograr obtener la posición o velocidad se requiere integrar las señales, las cuales contienen ruido, que puede llevar a generar errores importantes a largo plazo si no se aplica un filtrado adecuado  
    
4. **¿Qué efecto tiene la inclinación o el giro en el movimiento del robot, y cómo se corrige con el IMU?**  
   La inclinación o el giro no deseado del robot pueden afectar de forma significativa en el desplazamiento de este. Por ejemplo, una inclinación puede cambiar la distribución de peso sobre las ruedas, alterando la tracción y provocando derrapes o desvíos. Asimismo, un giro inesperado modifica la orientación del robot respecto a su trayectoria planeada, lo que afecta a tareas como el seguimiento de líneas o la navegación autónoma en espacios delimitados.

   En cuanto al IMU, la corrección que este realiza es detectar los cambios en tiempo real mediante el acelerómetro y el giroscopio. Para aumentar la precisión, se puede utilizar un filtro como Kalman o el filtro complementario, los cuales combinan las señales de ambos sensores para obtener una estimación más estable y confiable de la orientación. En base a esto, el sistema de control compara la orientación actual del robot con la orientación deseada y ajusta la potencia de los motores corrigiendo la trayectoria. Además, el IMU también permite estimar la pendiente del terreno y puede ajustar el esfuerzo de los motores para compensar la inclinación y mantener una velocidad constante. 
