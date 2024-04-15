# CVRP: The Capacited Vehicle Routing Problem :car:
---
### Contexto Historico :book:

El **Problema de Ruta de Vehículo con Restricción de Capacidad** es un clásico en el campo de la optimización combinatoria, especialmente en logística y transporte. Tiene sus raíces en los estudios de distribución de mercancías y planificación de rutas que datan de la década de 1950, pero se formalizó por primera vez en la literatura académica en la década de 1960.

Pertenece a la categoría de problemas NP-hard, lo que significa que no se conoce un algoritmo eficiente que pueda resolver instancias de tamaño arbitrario en tiempo polinómico. Por lo tanto, se utilizan enfoques heurísticos y metaheurísticos para encontrar soluciones aproximadas en un tiempo razonable.

---
### Descripción del Problema :bulb:

Supongamos que la agencia "Envíos Rápidos" tiene un almacén central desde el cual distribuye paquetes a diferentes clientes en una ciudad. La empresa cuenta con una flota de vehículos con capacidad limitada para realizar las entregas. La agencia tiene que entregar paquetes a cierto numero de clientes en la ciudad, donde cada uno tiene una demanda específica de paquetes que debe ser entregada.

En este caso, el CVRP se aplicaría de la siguiente manera:

**Datos:** :bar_chart:
- Almacén central: Punto de partida y llegada de los vehículos.
- Clientes: Puntos de entrega de paquetes.
- Demandas: Cantidad de paquetes que cada cliente necesita.
- Capacidad de los vehículos: Número máximo de paquetes que pueden transportar.
  
**Objetivo:**  :dart:
Minimizar la distancia total recorrida por los vehículos.

**Restricciones:** :no_entry_sign:
- Los vehículos no pueden exceder su capacidad máxima de carga.
- Cada cliente debe recibir su cantidad específica de paquetes.
- Cada cliente debe ser visitado exactamente una sola vez.
- Cada vehículo debe regresar al almacén central al finalizar su ruta.
  
Dado este contexto, podemos entender que el **CVRP** busca determinar la mejor manera (encontrar rutas) de distribuir mercancías/paquetes desde un almacén central a una serie de clientes, utilizando una flota de vehículos con capacidad limitada, de tal forma que logre **minimizar la distancia total recorrida de todas las rutas.**

---
### Elementos :hammer_and_wrench:

Para la implementacion del CVRP debemos tener en consideración algunos elementos, como los listados a continuación:

**1. Nodos:** Representan los puntos de entrega de mercancías (casas/clientes), incluyendo el almacén central. Estos nodos estan numerados desde 0, 1, 2, 3, ... hasta el numero de nodos indicados. Estan incluidos en las siguientes matrices:

**2. Matriz de Coordenadas:** Cada renglón posee el número del nodo, seguido de sus coordenadas en X,Y. Por ejemplo: (2, 12, 10), indicando que la casa/cliente No.2 esta ubicada en las coordenadas (12,10) del vecindario.
  
**3. Matriz de Demandas:** Cada renglon contiene el numero del nodo, seguido de respectiva demanda. Por ejemplo: (8, 2), indicando que la casa/cliente No.8 requiere de 2 paquetes a entregar.
  
**4. Capacidad:** Los vehículos tienen la **misma** capacidad máxima de carga.

**5. Numero de Vehiculos.**

**6. Nodo Origen:** Representa el numero de nodo que se tomara como la bodega/punto de partida. Usualmente, se toma el primer nodo (No.1).

---
### Formulacion Matemática :1234:

El modelo matematico de la función del CVRP está definida de la siguiente forma: 

**Función Objetivo:** MINIMIZAR EL COSTO (DISTANCIAS) DE VIAJE DE TODOS LOS VEHÍCULOS
$$\text{Minimizar } \sum_{i \in N} \sum_{j \in N, j \neq i} c_{ij} \cdot x_{ij}$$

**Restricciones:**
- Cada cliente debe ser visitado exactamente una vez:
  $$\sum_{j \in N, j \neq i} x_{ij} = 1, \quad \forall i \in \{2, \ldots, n\}$$
- Cada vehículo debe salir exactamente una vez del almacén central y regresar al mismo:
  $$\sum_{i \in N, i \neq 1} x_{i1} = 1$$
  $$\sum_{j \in N, j \neq 1} x_{1j} = 1$$
- Capacidad de los vehículos (la misma):
  $$\sum_{i \in N} q_i \cdot x_{ij} \leq Q, \quad \forall j \in \{2, \ldots, n\}$$

Donde:
- N = {1,2,…,n}: Conjunto de nodos (Nodo No.1 es el almacen y el resto los clientes).
- $c_{ij}$: Costo de viajar desde el nodo \(i\) al \(j\).
- $q_{i}$: Demanda del cliente \(i\).
- Q : Capacidad de los vehiculos.

**Variables de Decisión:** 
- $x_{ij}$: Variable binaria, 1 indica que se viaja directamente del nodo ***i*** al ***j***, 0 caso contrario.
- $u_{i}$: Paquetes entregados al nodo ***i***.

---
### Aplicaciones :computer:

Este problema es común en logística, donde es necesario planificar rutas de entrega eficientes para minimizar los costos de transporte y asegurar que todos los clientes sean atendidos en el tiempo previsto. Algunos de los ejemplos más claros son:
- La recolección de residuos
- El transporte de pasajeros.
- Entregas de paqueterías.
- Servicios de Emergencia.

 ![Diagrama del ejemplo](https://raw.githubusercontent.com/Chuchito-Boy/images/main/imagen3.avif)

### Ejemplificacion

Supongamos que tenemos los siguientes datos:

N = 6 (Cantidad de nodos: 16 clientes y 1 bodega).
Cantidad de vehiculos = 2 (con este dato, sabemos que seran 4 rutas a encontrar).
Capacidad = 10 (la misma para todos los vehiculos).
Nodo Origen = 1 (el primer nodo sera la bodega).

Matriz de Coordenadas:

```
1 3 3  
2 2 4  
3 1 3  
4 6 4  
5 2 2  
6 5 5 
```
Matriz de Demandas:

```
1 0  
2 5  
3 3  
4 3  
5 1  
6 7  
```
Con estos datos, podemos ejemplificar con el siguiente grafo:
 ![Diagrama del ejemplo](https://github.com/Chuchito-Boy/images/blob/main/imagen1.png)

Donde los nodos estan enumerados del 1-6 con su respectiva demananda al lado.

CVRP nos dice que tenemos que construir una ruta para cada vehiculo de forma aleatoria permutable, es decir, asignar todos los nodos a las rutas, de tal forma que no se repitan. 

Para ello, seguiremos los siguientes pasos: 

1. Del conjunto `N = { 1, 2, 3, 4, 5, 6}` , el cual contiene todos los nodos enumerados, eliminamos el nodo bodega (nodo 1) quedando asi:

`N = { 2, 3, 4, 5, 6}`.

2. Posteriormente, revolvemos el arreglo N para comenzar a distribuir en las rutas.

`N = { 2, 3, 6, 5, 4}`

3. Para la construccion de las rutas, nos apoyaremos de arreglos auxiliares que representaran el recorrido de cada vehiculo (en este caso seran 2).

`Ruta 1 = []`
`Ruta 2 = []`

4. Comenzaremos asignando el primer nodo de N a la Ruta 1, el segundo a la Ruta 2 y asi de forma intercalada hasta repartir todos. Quedando las rutas de esta forma:

`Ruta 1 = [2, 6, 4]`
`Ruta 2 = [3, 5]`

**Nota:** Las rutas comienzan y finalizan en el nodo origen.

Graficamente tenemos la primer propuesta de rutas:
 ![Diagrama del ejemplo con rutas](https://github.com/Chuchito-Boy/images/blob/main/imagen2.png)

5. Como siguiente paso, debemos calcular las distancias recorridas en cada ruta con la formula de la **distancia euclideana**.

Ejemplo: Ruta 1,
| Nodo inicial | Nodo Destino | Costo |
|-----------|------|--------|
| 1      | 2   | Costo 1 |
| 2     | 6   | Costo 2  |
| 6    | 4   | Costo 3 |
| 4    | 1   | Costo 4 |

6. Sumamos los costos para obtener el total de la Ruta 1 y de igual forma para la Ruta 2. Finalmente, sumamos ambos para obtener el **costo global**, el cual es el que se desea minizar.

### Generacion de solucion vecina

Para generar una solucion vecina, basta con selecionar de forma aleatoria 2 nodos del conjunto N (el ultimo obtenido) e intercambiarlos. En el caso del ejemplo, tenemos que el ultimo conjunto N obtenido es `N = { 2, 3, 6, 5, 4}`. Al seleccionar aletoriamente 2 elementos: `2`, `6` y realizar el intercambio obtenemos el nuevo conjunto `N = { 6, 3, 2, 5, 4}`

### Metodología

Una vez abordado los conceptos y ejemplos necesarios, resumiremos los pasos a seguir para la resolucion de un problema CVRP:

1. Definición de Elementos:
   - 1.1 Nodos (N).
   - 1.2 Matriz de Coordenadas.
   - 1.3 Matriz de Demandas.
   - 1.4 Capacidad.
   - 1.5 Número de Vehículos.
   - 1.6 Nodo Origen.
2. Obtener solución inicial:
   - 2.1 Quitar nodo origen de N.
   - 2.2 Revolver elementos de N.
   - 2.3 Repartir elementos de N a cada ruta.
3. Obtener solución vecina:
   - 3.1 Seleccionar 2 elementos de N.
   - 3.2 Intercambiarlos.
   - 3.3 Repartir elementos de N a cada ruta.
4. Obtener costo global:
   - 4.1 Obtener costo de cada ruta.
   - 4.2 Sumarlas para obtener el **costo global** a minimizar.

### Ejemplo de Instancias:

A continuacion se mostrara la estructura de una instancia en particular; indicando los elementos escenciales para su posterior implementacion:

NAME : toy.vrp  
COMMENT : toy instance>  
TYPE : CVRP  
**DIMENSION : 6**  
EDGE_WEIGHT_TYPE : EUC_2D  
**CAPACITY : 30** 
**NODE_COORD_SECTION**  
1 38 46  
2 59 46  
3 96 42  
4 47 61  
5 26 15  
6 66 6  
**DEMAND_SECTION** 
1 0  
2 16  
3 18  
4 1  
5 13  
6 8  
**DEPOT_SECTION**  
1  
-1  
EOF  


### Implementacion Python

Lectura de instancias:

```python
import numpy as np
import random
import copy
import math
# Funciones de lectura de instancias
def getData(lenData):
    data = []
    for i in np.arange(lenData):
        data.append([])
        data[i].append(-1)
        data[i].append([-1,-1])
        data[i].append(-1)
    return data

def convertListCharToString(listChar):
    new = ""
    for x in listChar:
        new += x
    return new

def read_instance(filename):
    with open(filename) as f:
        file = f.readlines()

    string = convertListCharToString(file)
    dimension = extract_dimension(string)
    data = getData(dimension)
    read_coordinates(string, data)
    read_demands(string, data)
    capacity = extract_capacity(string)
    return data, capacity

def extract_capacity(string):
    stringCapacity = "CAPACITY : "
    capacityIndex  = string.find(stringCapacity) + len(stringCapacity)
    capacity = ""
    while(string[capacityIndex] != '\n'):
        capacity += string[capacityIndex]
        capacityIndex += 1
    return int(capacity)


def extract_dimension(string):
    stringDimensions = "DIMENSION : "
    dimensionsIndex  = string.find(stringDimensions) + len(stringDimensions)
    dimension = ""
    while(string[dimensionsIndex] != '\n'):
        dimension += string[dimensionsIndex]
        dimensionsIndex += 1
    return int(dimension)

def read_coordinates(string, data):
    stringCord = "NODE_COORD_SECTION" 
    initialIndex = string.find(stringCord) + len(stringCord) + 2

    indicator = 0
    i = 0
    while(string[initialIndex] != 'D'):
        number = ""
        while(ord(string[initialIndex]) != 32 and string[initialIndex] != '\n'):
            number += string[initialIndex]
            initialIndex += 1
        
        if(len(number) > 0):        
            if indicator == 0:
                data[i][0] = (int(number) - 1)
                indicator += 1
            elif indicator == 1:
                data[i][1][0] = int(number)
                indicator += 1      
            elif indicator == 2:
                data[i][1][1] = int(number)
                indicator = 0
                i += 1

        initialIndex +=1

def read_demands(string, data):
    stringCost = "DEMAND_SECTION"
    initialIndex = string.find(stringCost) + len(stringCost) + 2

    indicator = 0

    i = 0
    while(string[initialIndex] != 'D'):
        number = ""
        while(ord(string[initialIndex]) != 32 and string[initialIndex] != '\n'):
            number += string[initialIndex]
            initialIndex += 1
        if(len(number) > 0):        
            if indicator == 0:
                indicator += 1
            elif indicator == 1:
                data[i][2] = int(number)
                indicator = 0     
                i += 1
        initialIndex +=1
```

Suponiendo que se desea leer la instancia3.txt, la carga de datos es la siguiente:

`Datos:  [[0, [27, 93], 0], [1, [33, 27], 16], [2, [29, 39], 2], [3, [7, 81], 7], [4, [1, 59], 11], [5, [49, 9], 9], [6, [21, 53], 17], [7, [79, 89], 21], [8, [81, 83], 23], [9, [85, 11], 10], [10, [45, 9], 6], [11, [7, 65], 19], [12, [95, 27], 18], [13, [81, 85], 20], [14, [37, 81], 13], [15, [69, 69], 5], [16, [15, 95], 11], [17, [89, 75], 24], [18, [33, 93], 2], [19, [57, 83], 3], [20, [11, 95], 1], [21, [3, 57], 5], [22, [45, 11], 20], [23, [43, 61], 23], [24, [35, 43], 24], [25, [19, 83], 18], [26, [83, 69], 19], [27, [85, 77], 2], [28, [19, 39], 17], [29, [83, 87], 17], [30, [1, 13], 9], [31, [15, 39], 11], [32, [83, 17], 2], [33, [41, 97], 6], [34, [31, 61], 9], [35, [59, 69], 5], [36, [29, 15], 9], [37, [93, 83], 2], [38, [63, 97], 14], [39, [65, 57], 19], [40, [15, 69], 11], [41, [31, 97], 21], [42, [57, 9], 20], [43, [85, 37], 21], [44, [21, 29], 18], [45, [53, 11], 48], [46, [15, 77], 1], [47, [41, 69], 17], [48, [45, 17], 42], [49, [13, 25], 2], [50, [63, 57], 4], [51, [95, 5], 24], [52, [55, 91], 18], [53, [3, 31], 21], [54, [47, 7], 11], [55, [61, 69], 9], [56, [85, 35], 18], [57, [89, 81], 22], [58, [45, 47], 9], [59, [65, 93], 23]] `



#### Creacion de solucion inicial

```python
# Funciones principales
def create_first_solution(data, capacity):
    totalDemand = getTotalDemand(data)
    totalCars = math.ceil(totalDemand / capacity)

    while(1):
        routes = []
        for i in np.arange(totalCars):
            routes.append([])
        demands = []

        lista = list(range(1, len(data)))
        random.shuffle(lista)

        for i in np.arange(len(data) - 1):
            index = random.randint(0, (totalCars - 1))
            routes[index].append(lista[i])

        for route in routes:
            demands.append(getDemand(data, route))
        
        isUnderDemand = True
        for demand in demands:
            if(demand > capacity):
                isUnderDemand = False

        if(isUnderDemand):
            break

    return routes

```

La salida es la siguiente:

`Initial solution:  [[31, 58, 3, 40, 59, 13, 5], [26, 22, 10, 57, 38, 2], [1, 39, 47, 12, 27, 23], [6, 7, 54, 46, 45], [29, 35, 8, 49, 48], [51, 50, 21, 56, 43, 20, 25], [28, 32, 33, 16, 55, 36, 44, 30, 34], [52, 14, 24, 42, 11, 37], [19, 41, 4, 15, 9, 17, 53, 18]]`

#### Creacion de solucion vecina

```python
def create_neighbor_solution(data, actual_solution, capacity):
    while True:
        neighbor = copy.deepcopy(actual_solution)
        idx = random.randint(0, (len(data) - 2))

        i = 0
        chargedNode = 0
        for route in neighbor:
            for node in route:
                if i == idx:
                    chargedNode = node
                i += 1
            if chargedNode:
                route.remove(chargedNode)
                break

        idxRoute = random.randint(0, (len(neighbor) - 1))
        idxNode = random.randint(0, len(neighbor[idxRoute]))
        neighbor[idxRoute].insert(idxNode, chargedNode)

        return neighbor
```

La salida es la siguiente:

`Datos: Neighbor solution:  [[31, 58, 3, 40, 59, 13, 5], [26, 22, 10, 57, 38, 2], [1, 39, 47, 12, 27, 23], [6, 7, 54, 46, 19, 45], [29, 35, 8, 49, 48], [51, 50, 21, 56, 43, 20, 25], [28, 32, 33, 16, 55, 36, 44, 30, 34], [52, 14, 24, 42, 11, 37], [41, 4, 15, 9, 17, 53, 18]]`


#### Funcion objetivo

```python
def CVRP_function(routes, data, capacity):
    distance = 0
    penalty = 0
    for i in np.arange(len(routes)):
        route_demand = getDemand(data, routes[i])
        excess_demand = max(route_demand - capacity, 0)
        penalty += excess_demand * capacity
        for j in np.arange(len(routes[i]) - 1):
            node1 = data[routes[i][j]][1]
            node2 = data[routes[i][j+1]][1]
            distance += euclidean_distance(node1, node2)
    return distance + penalty

```

#### Funciones auxiliares

```python
def euclidean_distance(point1, point2):
    return np.sqrt((point1[0] - point2[0])**2 + (point1[1] - point2[1])**2)

def getTotalDemand(data):
    totalDemand = 0
    for element in data:
        totalDemand += element[2]
    return totalDemand

def getDemand(data, route):
    demand = 0
    for node in route:
        demand += data[node][2]
    return demand


```
