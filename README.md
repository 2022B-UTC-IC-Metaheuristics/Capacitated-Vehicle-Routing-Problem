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

1. Definicion de Elementos: 
   1.1 Nodos (N)
   1.2 Matriz de Coordenadas
   1.3 Matriz de Demandas
   1.4 Capacidad
   1.5 Numero de Vehiculos
   1.6 Nodo Origen
2. Obtener solucion inicial
   2.1 Quitar nodo origen de N
   2.2 Revolver elementos de N
   2.3 Repartir elementos de N a cada ruta
3. Obtener solucion vecina
   3.1 Seleccionar 2 elementos de N
   3.2 Intercambiarlos
   3.2 Repartir elementos de N a cada ruta
4. Obtener costo global
   4.1 Otener costo de cada ruta
   4.2 Sumarlas para obtener el **costo global** 

### Ejemplo de una instancia:
NAME : toy.vrp  
COMMENT : toy instance>  
TYPE : CVRP  
DIMENSION : 6  
EDGE_WEIGHT_TYPE : EUC_2D  
CAPACITY : 30  
NODE_COORD_SECTION  
1 38 46  
2 59 46  
3 96 42  
4 47 61  
5 26 15  
6 66 6  
DEMAND_SECTION  
1 0  
2 16  
3 18  
4 1  
5 13  
6 8  
DEPOT_SECTION  
1  
-1  
EOF  

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

Nuestra propueta para solución inicial:

```python
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

En esta función se están utilizando variables globales como son el lugar de origen, las distancias, las demandas de los lugares y las capacidades del vehículo. Cada vez que un vehiculo excede su capacidad, se penaliza la solución para que no sea la óptima.

#### Instancias a ejecutar. 
Se encuentran en el documento adjunto.

### Lectura de instancias
Se proporciona el código en python para la lectura correcta de los archivos y almacenar los datos. Esto en una matriz en la cual cada renglón sigue el siguiente formato: [Nodo,CoordX,CoordY,Demanda]
```python
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
     

with open('./A-n33-k5.txt') as f:
    file = f.readlines()

string = convertListCharToString(file)

stringDimensions = "DIMENSION : "
dimensionsIndex  = string.find(stringDimensions) + len(stringDimensions)
dimension = ""

while(string[dimensionsIndex] != '\n'):
    dimension += string[dimensionsIndex]
    dimensionsIndex += 1

dimension = int(dimension)

data = getData(dimension)

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

stringCost = "DEMAND_SECTION"
initialIndex = string.find(stringCost) + len(stringCost) + 2

indicator = 0

i = 0
while(string[initialIndex] != 'D'):
    number = ""
    while(ord(string[initialIndex]) != 32 and string[initialIndex] != '\n'):
        number += string[initialIndex]
        initialIndex += 1
    #print(number)
    if(len(number) > 0):        
        if indicator == 0:
            indicator += 1
        elif indicator == 1:
            data[i][2] = int(number)
            indicator = 0     
            i += 1

    initialIndex +=1
print(data)
```

#### Funcion de crear vecinos

```python
def create_neighbor_solution(data, actual_solution, capacity):
    while(1):
        neighbor = copy.deepcopy(actual_solution)
        idx = random.randint(0, (len(data) - 2))

        i = 0
        chargedNode = 0
        for route in neighbor:
            for node in route:
                if(i == idx):
                    chargedNode = node
                i += 1
            if(chargedNode):
                route.remove(chargedNode)
                break

        idxRoute = random.randint(0, (len(neighbor) - 1))
        idxNode = random.randint(0, len(neighbor[idxRoute]))
        neighbor[idxRoute].insert(idxNode, chargedNode)

        demands = []
        for route in neighbor:
            demands.append(getDemand(data, route))

        isUnderDemand = True
        for demand in demands:
            if(demand > capacity):
                isUnderDemand = False
        
        if(isUnderDemand):
            return neighbor

```
Función objetivo:

```python
def euclidean_distance(point1, point2):
    return np.sqrt((point1[0] - point2[0])**2 + (point1[1] - point2[1])**2)

def CVRP_function(routes, data):
    distance = 0
    for i in np.arange(len(routes)):
        for j in np.arange(len(routes[i]) - 1):
            node1 = data[routes[i][j]][1]
            node2 = data[routes[i][j+1]][1]
            distance += euclidean_distance(node1, node2)
    return distance
```
Ejemplo practico para datos estaticos:

```python
# Datos de ejemplo
data = [
    [0, (38, 46), 0],
    [1, (59, 46), 16],
    [2, (96, 42), 18],
    [3, (47, 61), 1],
    [4, (26, 15), 13],
    [5, (66, 6), 8],
    [6, (70, 6), 8]
]

capacity = 30

# Crear solución inicial
initial_solution = create_first_solution(data, capacity)

# Imprimir la solución inicial
print("Solución inicial:")
for i, route in enumerate(initial_solution):
    print(f"Ruta {i+1}: {route}")

# Crear una solución vecina
neighbor_solution = create_neighbor_solution(data, initial_solution, capacity)

# Imprimir la solución vecina
print("\nSolución vecina:")
for i, route in enumerate(neighbor_solution):
    print(f"Ruta {i+1}: {route}")

# Calcular la función objetivo de la solución inicial
initial_distance = CVRP_function(initial_solution, data)
print(f"\nDistancia de la solución inicial: {initial_distance}")

# Calcular la función objetivo de la solución vecina
neighbor_distance = CVRP_function(neighbor_solution, data)
print(f"Distancia de la solución vecina: {neighbor_distance}")

```
