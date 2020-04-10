# Descripción del Modelo

## Introducción 

El uso de modelos matemáticos en epidemiología tiene una larga tradición. Los modelos más antiguos y más usados son conocidos como modelos tipo SIR. En estos modelos, la población se divide en tres compartimentos: susceptibles, infecciosos y recuperados. Y la evolución del número de individuos en cada compartimento se modela mediante ecuaciones diferenciales ordinarias, que pueden verse como ecuaciones de balance de flujos. Es decir, la rapidez de variación del número de individuos en cada compartimento es igual al flujo neto de individuos hacia el mismo; y los flujos netos dependen de los flujos de entrada y salida, así como de los flujos entre compartimentos.

Los modelos tipo SIR tienen la gran ventaja de ser muy simples. Son lo que podría llamarse modelos explicativos, que reproducen de manera cualitativa el comportamiento dinámico de diversas epidemias. Mediante el uso de este tipo de modelos ha sido posible entender fenómenos dinámicos como las bifurcaciones, y su importancia en un contexto epidemiológico. Por otra parte, una de las principales limitaciones de los modelos tipo SIR es que se basan en la ley de acción de masas, que a su vez supone un individuo puede interactuar con cualquier otro individuo de la población. En la vida real, esto no es cierto. Hay individuos que, por su estilo de vida y/o su profesión, entran en contacto rutinariamente con muchas personas; y por lo tanto representan un gran riesgo de contagio. Pero también hay individuos que son muy solitarios. 

En el otro extremo están los modelos basados en agentes. Estos modelos toman en cuenta a cada uno de los individuos de la población, y modelan su comportamiento dinámico mediante reglas preestablecidas. Estos modelos son en definitiva más realistas, pero también presentan desventajas. Una de ellas es que dependen de una gran cantidad de parámetros, que en algunos casos son difíciles de estimar a partir de datos reales. Por otra parte, requieren de una gran capacidad de cómputo para ser analizados.

Una alternativa que representa un buen compromiso entre los dos enfoques arriba descritos es modelar la evolución de una epidemia en una red, en la que los nodos representan a los individuos de la población, en tanto que los enlaces entre nodos representan las interacciones entre individuos, que pueden dan lugar a contagios. 

Existen varios algoritmos para construir redes que reproducen las características de la redes de interacciones sociales. Dos de los más usados son el modelo de Watts-Strogatz (WS) y el Barabási-Albert (AB). El modelo WS tiene la ventaja de generar redes graphs con características similares a las de las redes sociales como: una pequeña longitud promedio de trayectorias entre pares de nodos, y la formación de pequeños grupos de nodos altamente conectados entre sí (**clustering**). Por otra parte, tienen la desventaja de que la distribución de números de enlaces por nodo no obedece una ley de potencias (algo que sí presentan las redes sociales). En lo que respecta al modelo AB, este reproduce la ley de potencias para las distribuciones de nodos, pero no genera redes con un elevado nivel de clustering.

## Desarrollo del modelo

Consideremos una red de N nodos, que están representados por las variables ni (i = 1, 2 ... N). Las variables ni pueden tomar cuatro diferentes valores: S (susceptible de ser infectado), I (infeccioso), R (recuperado de la infección), y C (en cuarentena).

La evolución de una epidemia en la red se modela mediante el siguiente algoritmo tipo Montecarlo:
 
 1. Inicializar la red de manera que todos los nodos están en el estado S, excepto un número determinado de ellos (ni_0), elegidos al azar, que se consideran infecciosos (I).
2. Recorrer cada uno de los nodos de la red, y actualizar su estado de acuerdo a las siguientes reglas:
	* Si el estado del nodo es R o C, no hacer nada.
	* Si el estado del nodo es S, cambiar el estado a I con probabilidad beta mi_I, donde mi_I es el número de nodos de la red conectados con el presente nodo que son infecciosos.
	* Si el estado del nodo es I, cambiar el estado a R con probabilidad gamma.
3. Iterar al paso 2.

En el presente modelo elegí emplear redes del tipo AB en virtud de que las redes generadas presentan una distribución de enlaces similar a la de las redes sociales; cosa que ha demostrado ser relevante en epidemiología. Por otra parte, para paliar el bajo nivel de **clustering**, consideramos que cada nodo de la red representa no a un individuo, sino a un **cluster**. En el contexto de enfermedades respiratorias, muchos de estos **clusters** corresponden a los habitantes de un mismo domicilio. Por cuestión de simpleza, a estos les llamaremos familias.
