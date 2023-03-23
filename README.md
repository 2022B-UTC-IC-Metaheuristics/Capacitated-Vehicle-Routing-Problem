# The Capacited Vehicle Routing Problem

#### Descripción del problema.
El problema de ruta de vehículos capacitados (CVRP) es un VRP en el que los vehículos con capacidad de carga limitada tienen que recoger o entregar artículos en varios lugares. Los artículos tienen una cantidad, como el peso o el volumen, y los vehículos tienen una capacidad máxima que pueden transportar. *El problema es recoger o entregar los artículos con el menor coste posible, sin exceder nunca la capacidad de los vehículos.*
#### Aplicaciones
Los problemas de routeo son muy comunes en tareas relacionadas en transporte relacionadas con la logistica o distribución, esto porque su resultado da un recorrido que es considerado como óptimo dadas distintas variables y costos determinados en su planteamiento.

#### ¿Qué vamos a necesitar para un problema?
Nosotros tomamos en consideración algunos elementos, como los listados a continuación.
* Tamaño de la matriz:  Matriz cuadrada que nos delimitará el área de la ciudad en donde se encuentran los lugares para entregar o recoger.
* Distancias en forma matricial: Cada renglón representa la distancia
desde ese punto con respecto a los demás. La distancia a el mismo nodo es 0.
* Número de Vehiculos: El máximo de vehiculos en este problema es especifico.
* Nodo de origen: El número del nodo seleccionado como el origen. En caso de ser omitido, el nodo de origen pasa a ser el nodo 0.
* Demandas de cada destino: Vector en el que cada elemento es el número de objetos a entregar o recibir en el lugar i. Si el número de demanda es negativo se va a recoger, si es positivo se va a entregar.
* Capacidades de cada vehiculo: Vector en el que cada elemento es la carga máxima para el vehiculo i. No puede existir una carga negativa.

#### ¿Cómo se representa una solución? 
Se representa por un arreglo de sub arreglos, en el que cada subarreglo nos dice el camino que tomará el camión en esa posición. En caso de que el camión no sea utilizado, no se mostrará nada.
Ejemplo: 
[[0,3,4,0],[0,1,2,0]]

#### Ejemplo
Se tienen las siguientes ubicaciones, numeradas del 0 al 16, donde la ubicación 0 es la salida. En la parte inferior derecha de cada una se tiene la cantidad que se va a recoger.
 ![Diagrama del ejemplo](https://developers.google.com/optimization/images/routing/cvrp.svg)

 Para este problema se van a tener un total de 4 vehiculos con una carga máxima de 15 unidades cada uno.

*El problema es encontrar una asignación de rutas que sean las más cortas en distancia y que el total de carga de un vehículo nunca sobrepase su capacidad de carga.*

#### Solución
La solución propuesta al problema es dividirlo en segmentos pequeños que cada uno de los vehiculos va a recorrer. Cada vehiculo ahora se convierte en un problema del agente viajero y se visitan las ciudades en base al menor costo de transporte de productos.
El esquema de la solución queda igual al siguiente:

 ![Diagrama del ejemplo](https://developers.google.com/optimization/images/routing/vrpgs_solution.svg)

#### Modelo y restricciones
El modelo matematico de la función del CVRP está definida de la siguiente forma: 

![Formulación matematica](https://repository.uaeh.edu.mx/scige/boletin/sahagun/n10/multimedia/a2/a2_2.jpg)

Donde
* ${A:}$ Capacidad de cada vehículo 
* ${V}$: Número máximo de vehículos
* ${F_{ij}}$: Flujo del producto desde el nodo a  
* Z: Costo total de transportación
* di: Demanda en el nodo
* cij: Costo de recorrer la distancia entre el nodo  al nodo
* N: Número de nodos


Nuestra *función objetivo* para la evalución de la solución queda de la siguiente forma:

```python
def f(self, solution):
	global origin
	global distances
	global demands
	global capacities
	cost = 0
	for i in range(vehicles):
		fill = 0
		ant = origin
		for j in M[i]:
			cost += distances[ant][j]
			ant = j
			fill += demands[j]
		cost += distances[j][origin]
		if(fill>capacities[i]):
			cost += 1e10
	return cost
```

En esta función se están utilizando variables globales como son el lugar de origen, las distancias, las demandas de los lugares y las capacidades del vehículo. Cada vez que un vehiculo excede su capacidad, se penaliza la solución para que no sea la óptima.

#### Instancias a ejecutar. 
Se encuentran en el documento adjunto. 
