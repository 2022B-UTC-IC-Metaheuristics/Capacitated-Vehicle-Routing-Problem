# The Capacited Vehicle Routing Problem

#### Descripción del problema.

El Capacitated Vehicle Routing Problem (CVRP) es un problema de optimización combinatoria que se enfoca en encontrar la ruta más eficiente para un conjunto de vehículos que deben cumplir con la entrega de bienes o servicios a un conjunto de clientes, tomando en cuenta las restricciones de capacidad de los vehículos.

En este problema, se tiene un conjunto de vehículos, cada uno con una capacidad máxima de carga. Además, se cuenta con un conjunto de clientes que deben ser atendidos, cada uno con una demanda específica de bienes o servicios. El objetivo es planificar la ruta que debe seguir cada vehículo para atender a todos los clientes, de manera que se minimice la distancia recorrida por los vehículos y se respeten las restricciones de capacidad de carga de cada uno.

La resolución del problema CVRP es compleja debido a la gran cantidad de posibles combinaciones de rutas que se pueden generar. Además, la capacidad limitada de los vehículos y las restricciones de tiempo pueden hacer que la planificación de rutas sea aún más desafiante. 

#### Aplicaciones

Este problema es común en logística, donde es necesario planificar rutas de entrega eficientes para minimizar los costos de transporte y asegurar que todos los clientes sean atendidos en el tiempo previsto. Algunos de los ejemplos más claros son:
* La recolección de residuos
* El transporte de pasajeros.
* La programación de la producción en la industria manufacturera.
* Entregas de paqueterías.

#### ¿Qué vamos a necesitar para un problema?
Nosotros tomamos en consideración algunos elementos, como los listados a continuación.
* Demandas de cada destino: Vector en el que cada elemento es el número de objetos a entregar o recibir en el lugar i. Si el número de demanda es negativo se va a recoger, si es positivo se va a entregar.
* Capacidades de cada vehiculo: Vector en el que cada elemento es la carga máxima para el vehiculo i. No puede existir una carga negativa.
* Distancias en forma matricial: Cada renglón representa la distancia
desde ese punto con respecto a los demás. La distancia a el mismo nodo es 0.

#### ¿Cómo se representa una solución? 
Se representa por un arreglo de sub arreglos, en el que cada subarreglo nos dice el camino que tomará el camión en esa posición. En caso de que el camión no sea utilizado, no se mostrará nada.
Ejemplo: 
[[0,7,1,4,3,0],[0,8,6,2,5,0],[0,12,11,15,13,0],[0,9,10,16,14,0]]


 ![Diagrama del ejemplo](https://i.postimg.cc/v8XP8ggT/png.gif)

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
* ${V:}$ Número máximo de vehículos
* ${F_{ij}:}$ Flujo del producto desde el nodo a  
* ${Z:}$ Costo total de transportación
* ${d_{i}:}$ Demanda en el nodo
* ${c_{ij}:}$ Costo de recorrer la distancia entre el nodo  al nodo
* ${N:}$ Número de nodos


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

#### Alternativa de solución secuencial
A continuación se muestra una solución alternativa al problema de CVRP aunque no es tan preciso eficiente.

```python
import numpy as np

# función para calcular la distancia entre dos nodos
def calcular_distancia(nodo1, nodo2):
    return np.sqrt((nodo1[0] - nodo2[0])**2 + (nodo1[1] - nodo2[1])**2)

# función para encontrar el camino óptimo
def encontrar_camino_optimo(nodos, capacidad_vehiculos, n_vehiculos):
    n_nodos = len(nodos)
    distancia_minima = float("inf")
    for i in range(n_nodos):
        for j in range(n_nodos):
            if i != j:
                demanda_total = sum([nodos[k][2] for k in range(n_nodos)])
                if demanda_total > capacidad_vehiculos * n_vehiculos:
                    continue
                rutas = [[] for _ in range(n_vehiculos)]
                ruta_actual = 0
                capacidad_actual = capacidad_vehiculos
                distancia_actual = 0
                visitados = set()
                visitados.add(i)
                visitados.add(j)
                rutas[ruta_actual].append(i)
                while len(visitados) < n_nodos:
                    ultimo_nodo = rutas[ruta_actual][-1]
                    mejor_distancia = float("inf")
                    mejor_nodo = None
                    for k in range(n_nodos):
                        if k not in visitados:
                            distancia = calcular_distancia(nodos[ultimo_nodo], nodos[k])
                            if nodos[k][2] > capacidad_actual:
                                continue
                            if distancia < mejor_distancia:
                                mejor_distancia = distancia
                                mejor_nodo = k
                    if mejor_nodo is not None:
                        rutas[ruta_actual].append(mejor_nodo)
                        visitados.add(mejor_nodo)
                        distancia_actual += mejor_distancia
                        capacidad_actual -= nodos[mejor_nodo][2]
                    else:
                        ruta_actual += 1
                        capacidad_actual = capacidad_vehiculos
                        rutas[ruta_actual].append(i)
                if distancia_actual < distancia_minima:
                    distancia_minima = distancia_actual
                    mejor_ruta = rutas
    return mejor_ruta, distancia_minima

# Ejemplo de uso
#coordenadaX,coordenadaY,demanda
nodos = np.array([[0, 0, 0], [3, 0, 1], [6, 0, 2], [0, 4, 3], [3, 4, 4], [6, 4, 5]])
capacidad_vehiculos = 5
n_vehiculos = 2
mejor_ruta, distancia_minima = encontrar_camino_optimo(nodos, capacidad_vehiculos, n_vehiculos)
print("La distancia mínima es:", distancia_minima)
print("Las rutas óptimas son:")
for ruta in mejor_ruta:
    print(ruta)
```
