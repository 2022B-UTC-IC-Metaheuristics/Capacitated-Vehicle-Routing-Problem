# The Capacited Vehicle Routing Problem

#### Descripción del problema.
El problema de ruta de vehículos capacitados (CVRP) es un VRP en el que los vehículos con capacidad de carga limitada tienen que recoger o entregar artículos en varios lugares. Los artículos tienen una cantidad, como el peso o el volumen, y los vehículos tienen una capacidad máxima que pueden transportar. *El problema es recoger o entregar los artículos con el menor coste posible, sin exceder nunca la capacidad de los vehículos.*
#### Aplicaciones
Los problemas de routeo son muy comunes en tareas relacionadas en transporte relacionadas con la logistica o distribución, esto porque su resultado da un recorrido que es considerado como óptimo dadas distintas variables y costos determinados en su planteamiento.

#### ¿Qué vamos a necesitar para un problema?
Nosotros tomamos en consideración algunos elementos, como los listados a continuación.
* Tamaño de la matriz:  Matriz cuadrada que nos delimitará el área de la ciudad en donde se encuentran los lugares para entregar o recoger. Esta debe ser cuadrada.
* Distancias en forma matricial: Cada renglón representa la distancia
desde ese punto con respecto a los demás. La distancia a el mismo nodo es 0.
* Número de Vehiculos: El número máximo de vehiculos en ese problema especifico.
* Nodo de origen: El número de nodo seleccionado como el origen. En caso de ser omitido, el nodo de origen pasa a ser el nodo 0.
* Demandas de cada destino: Vector en el que cada elemento es el número de objetos a entregar o recibir en el lugar i. Si el número de demanda es negativo se va a recoger, si es positivo se va a entregar.
* Capacidades de cada vehiculo: Vector en el que cada elemento es la carga máxima para el vehiculo i. No puede existir una carga negativa.

#### Ejemplo
Se tienen las siguientes ubicaciones, numeradas del 0 al 16, donde la ubicación 0 es la salida. En la parte inferior derecha se tiene la cantidad que se va a recoger.
 ![Diagrama del ejemplo](https://developers.google.com/optimization/images/routing/cvrp.svg)

 Para este problema se van a tener un total de 4 vehiculos con una carga máxima de 15 unidades cada uno.

*El problema es encontrar una asignación de rutas que sean las más cortas en distancia y que el total de carga de un vehículo nunca sobrepase su capacidad de carga.*

#### Solución
La solución propuesta al problema es dividirlo en segmentos pequeños, que cada uno de los vehiculos va a recorrer. Cada vehiculo ahora se convierte en un problema del agente viajero y se visitian las ciudades en base al menor costos de transporte de productos.
El esquema de la solución queda igual al siguiente:

 ![Diagrama del ejemplo](https://developers.google.com/optimization/images/routing/vrpgs_solution.svg)

#### Modelo y restricciones
El modelo matematico de la función del CVRP está definida de la siguiente forma: 

![Formulación matematica](https://repository.uaeh.edu.mx/scige/boletin/sahagun/n10/multimedia/a2/a2_2.jpg)

Donde
* A: Capacidad de cada vehículo
* V: Número máximo de vehículos
* Fij: Flujo del producto desde el nodo  a  
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

En esta función se están utilizando variables globales como cual es el lugar origen, las distancias, las demandas de los lugares y las capacidades del vehículo. Cada vez que un vehiculo excede su capacidad, se penaliza la solución para que no sea la optima.

Para la generación de una *solución inicial* utilizamos la siguiente función:
```python
def generate_initial_solution(self, solution):
	global vis
	global vehicles
	global locations
	for i in range(vehicles):
		y = rd.randrange(locations)+1
		while vis[y]!=0:
			y = rd.randrange(locations)+1
		vis[y] = i
		solution[i].append(y)
	for i in range(locations):
		y = rd.randrange(vehicles)
		if vis[i]==0:
			vis[i] = y
		solution[y].append(i)
	return solution
```

Para la generación de una solución vecina de una forma aleatoria utilizamos las siguiente función:
```python
def generate_neighbor(self, solution):
  global locations
  global vehicles
  global vis
  x = rd.randrange(vehicles+1)
  y = rd.randrange(locations+1)
  z = vis[y]
  pos = 0
  for i in solution[z]:
    if(i==y):
      break
    ++pos
  np.delete(solution[z],pos)
  solution[x].append(y)
  return solution
```
