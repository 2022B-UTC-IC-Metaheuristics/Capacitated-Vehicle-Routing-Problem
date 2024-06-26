# CVRP: The Capacited Vehicle Routing Problem :car:
 ![Diagrama del ejemplo](https://github.com/Chuchito-Boy/images/blob/main/imagen4.avif)

### Contexto Histórico :book:

El **Problema de Ruta de Vehículo con Restricción de Capacidad** es un clásico en el campo de la optimización combinatoria, especialmente en logística y transporte. Tiene sus raíces en los estudios de distribución de mercancías y planificación de rutas que datan de la década de 1950, pero se formalizó por primera vez en la literatura académica en la década de 1960.

Pertenece a la categoría de problemas NP-hard, lo que significa que no se conoce un algoritmo eficiente que pueda resolver instancias de tamaño arbitrario en tiempo polinómico. Por lo tanto, se utilizan enfoques heurísticos y metaheurísticos para encontrar soluciones aproximadas en un tiempo razonable.

---
### Descripción del Problema :bulb:

Supongamos que la agencia "Envíos Rápidos" tiene un almacén central desde el cual distribuye paquetes. La empresa cuenta con una flota de vehículos con capacidad limitada para realizar las entregas. Dicha agencia tiene que entregar paquetes a cierto número de clientes en la ciudad, donde cada uno tiene una demanda específica que debe ser cubierta.

En este caso, el CVRP se aplicaría de la siguiente manera:

**Datos:** :bar_chart:
- Bodega: Punto de partida y llegada de los vehículos.
- Clientes: Puntos de entrega de paquetes.
- Demandas: Cantidad de paquetes que cada cliente necesita.
- Capacidad de los vehículos: Número máximo de paquetes que pueden transportar.
  
**Objetivo:**  :dart:
Minimizar la distancia total recorrida por los vehículos.

**Restricciones:** :no_entry_sign:
- Los vehículos no pueden exceder su capacidad máxima de carga.
- Cada cliente debe ser visitado exactamente una sola vez.
- Cada cliente debe recibir una cantidad específica de paquetes.
- Cada vehículo debe regresar al almacén central al finalizar su ruta.
  
Dado este contexto, podemos entender que el **CVRP** busca determinar la mejor manera (encontrar rutas) de distribuir mercancías/paquetes desde un almacén central/bodega a una serie de clientes, utilizando una flota de vehículos con capacidad limitada, de tal forma que logre **minimizar la distancia total recorrida de todas las rutas.**

---
### Elementos :hammer_and_wrench:

Para la implementación del CVRP debemos tener en consideración algunos elementos como los listados a continuación:

**1. Nodos:** Representan los puntos de entrega de mercancías (casas/clientes), incluyendo el almacén central/bodega. Estos nodos están numerados desde 1, 2, 3, ... hasta el número de nodos indicados. 

**2. Matriz de Coordenadas:** Cada renglón posee el número del nodo, seguido de sus coordenadas en X,Y. Por ejemplo: (2, 12, 10), indicando que la casa/cliente No.2 esta ubicada en las coordenadas (12,10) del vecindario.
  
**3. Matriz de Demandas:** Cada renglón contiene el número del nodo, seguido de su respectiva demanda. Por ejemplo: (8, 2), indicando que la casa/cliente No.8 requiere de 2 paquetes a entregar.
  
**4. Capacidad:** Los vehículos tienen la **misma** capacidad máxima de carga.

**5. Número de Vehículos.** Representa la cantidad de vehículos disponibles.

**6. Nodo Origen:** Representa el número de nodo que se tomara como la bodega, es decir, el punto de partida. Usualmente, se toma el primer nodo (No.1).

---
### Formulación Matemática :1234:

El modelo matemático de la función del CVRP está definida de la siguiente forma: 

**Función Objetivo:** MINIMIZAR EL COSTO (DISTANCIAS) DE VIAJE DE TODOS LOS VEHÍCULOS
$$\text{Minimizar } \sum_{i \in N} \sum_{j \in N, j \neq i} c_{ij} \cdot x_{ij}$$

**Restricciones:**
- Cada cliente debe ser visitado exactamente una vez:
  $$\sum_{j \in N, j \neq i} x_{ij} = 1, \quad \forall i \in \{2, \ldots, n\}$$
- Cada vehículo debe salir exactamente una vez del almacén central y regresar al mismo:
  $$\sum_{i \in N, i \neq 1} x_{i1} = 1$$
  $$\sum_{j \in N, j \neq 1} x_{1j} = 1$$
- La capacidad de los vehículos es la misma:
  $$\sum_{i \in N} q_i \cdot x_{ij} \leq Q, \quad \forall j \in \{2, \ldots, n\}$$

Donde:
- N = {1,2,…,n}: Conjunto de nodos (Nodo No.1 es la bodega y el resto los clientes).
- $c_{ij}$: Costo de viajar desde el nodo \(i\) al \(j\).
- $q_{i}$: Demanda del cliente \(i\).
- Q : Capacidad de los vehículos.

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

### Ejemplificación

Supongamos que tenemos los siguientes datos:

- N = 6 (Cantidad de nodos:  1 bodega y 5 clientes).
- Cantidad de vehículos = 2 (con este dato, sabemos que serán 2 rutas a encontrar).
- Capacidad = 10 (la misma para todos los vehículos).
- Nodo Origen = 1 (el primer nodo será la bodega).

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

CVRP nos dice que tenemos que construir una ruta para cada vehículo de forma aleatoria permutable, es decir, asignar todos los nodos a las rutas, de tal forma que no se repitan. 

Para ello, seguiremos los siguientes pasos: 

1. Del conjunto `N = { 1, 2, 3, 4, 5, 6}` , el cual contiene todos los nodos enumerados, eliminamos el nodo bodega (nodo 1) quedando así:

`N = { 2, 3, 4, 5, 6}`.

2. Posteriormente, revolvemos el arreglo N para comenzar a distribuir en las rutas.

`N = { 2, 6, 4, 3, 5}`

3. Para la construcción de las rutas, nos apoyaremos de arreglos auxiliares que representaran el recorrido de cada vehículo (en este caso eran 2).

`Ruta 1 = []`

`Ruta 2 = []`

4. Para saber el número de nodos para cada ruta, basta con realizar la división del total de elementos en N sobre la cantidad de vehículos, obteniendo `k` cantidad de elementos para cada ruta; posteriormente, se asignarán los primeros `k` elementos a la primera ruta, los siguientes `k` elementos a la siguiente y así sucesivamente. Quedando las rutas de esta forma:

`Nota:` En caso de que la división no sea exacta, se recomienda redondear al entero menor más cercano. Por lo que para la última ruta, se tomaran todos los nodos faltantes.

`k` = `5/2` = `3`
`Nota:` En este ejemplo se utilizó un redondeo hacia arriba.


`Ruta 1 = [1, 2, 6, 4, 1]`

`Ruta 2 = [1, 3, 5, 1]`


**Nota:** Las rutas comienzan y finalizan en el nodo origen.

Gráficamente, tenemos la primera propuesta de rutas:
 ![Diagrama del ejemplo con rutas](https://github.com/Chuchito-Boy/images/blob/main/imagen2.png)

5. Como siguiente paso, debemos calcular las distancias recorridas en cada ruta con la fórmula de la **distancia euclidiana**.

Ejemplo: Ruta 1,
| Nodo inicial | Nodo Destino | Costo |
|-----------|------|--------|
| 1      | 2   | Costo 1 |
| 2     | 6   | Costo 2  |
| 6    | 4   | Costo 3 |
| 4    | 1   | Costo 4 |

6. Sumamos los costos para obtener el total de la Ruta 1 y de igual forma para la Ruta 2. Finalmente, sumamos ambos para obtener el **costo global**, el cual es el que se desea minimizar.

### Generación de solución vecina

Para generar una solución vecina, basta con seleccionar de forma aleatoria 2 nodos del conjunto N (el último obtenido) e intercambiarlos. En el caso del ejemplo, tenemos que el último conjunto N obtenido es `N = { 2, 6, 4, 3, 5}`. Al seleccionar aleatoriamente 2 elementos: `2`, `6` y realizar el intercambio obtenemos el nuevo conjunto `N = { 6, 2, 4, 3, 5}`

### Metodología

Una vez abordado los conceptos y ejemplos necesarios, resumiremos los pasos a seguir para la resolución de un problema CVRP:

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
   - 2.3 Retornas la nueva lista
3. Obtener solución vecina:
   - 3.1 Seleccionar 2 elementos de N.
   - 3.2 Intercambiarlos.
   - 3.3 Retornar la nueva lista
4. Obtener costo global:
   - 4.1 Obtener costo de cada ruta.
   - 4.2 Sumarlas para obtener el **costo global** a minimizar.

### Instancias:

| Nombre archivo | Dimensión | Vehículos | Capacidad | Bodega | Mejor valor encontrado | Solucion |
|----------------|-----------|-----------|-----------|--------|--------------|----------------|
| instancia.txt  | 16        | 4         | 35        | 1      | 91           | [8, 6, 15, 10, 14, 3, 11, 2, 7, 9, 4, 13, 16, 5, 12] |
| instancia2.txt | 32        | 5         | 100       | 1      | 446          | ---  |
| instancia3.txt | 60        | 9         | 100       | 1      | 812          | ---   |
| instancia4.txt | 80        | 10        | 100       | 1      | 894          | ---   |

A continuación se mostrará la estructura de una instancia en particular; indicando los elementos esenciales para su posterior implementación:

```
TYPE : CVRP
DIMENSION : 16
TRUCKS : 4
CAPACITY : 35
DEPOT_SECTION : 1
NODE_COORD_SECTION
 1 30 40
 2 37 52
 3 49 49
 4 52 64
 5 31 62
 6 52 33
 7 42 41
 8 52 41
 9 57 58
 10 62 42
 11 42 57
 12 27 68
 13 43 67
 14 58 48
 15 58 27
 16 37 69
DEMAND_SECTION
1 0
2 19
3 30
4 16
5 23
6 11
7 31
8 15
9 28
10 8
11 8
12 7
13 14
14 6
15 19
16 11
END
```

### Implementación Python

Lectura de instancias:

```python
def read_data(file_path):
    """
    Parameters
    ----------
    file_path : text
        Path de la instancia.

    Returns trucks, capacity, nodes, depot, data
    -------
    trucks : int
        Numero de vehículos.
    capacity : int
        Capacidad para cada vehículo.
    nodes : list [int]
        Lista de nodos, excluyendo al nodo bodega.
    depot : int
        Nodo bodega.
    data : list [dict]
        Lista de diccionarios con informacion de cada nodo: número, demanda y coordenadas.
    """
    trucks = None
    capacity = None
    nodes = []
    data = []
    depot = None

    with open(file_path, 'r') as file:
        lines = file.readlines()

    node_coords = {}
    node_demands = {}

    for i, line in enumerate(lines):
        parts = line.split()

        keyword = parts[0]
        if keyword == 'TRUCKS':
            trucks = int(parts[2])
        elif keyword == 'CAPACITY':
            capacity = int(parts[2])
        elif keyword == 'DEPOT_SECTION':
            depot = int(parts[2])
        elif keyword == 'DIMENSION':
            num_nodes = int(parts[2])
            nodes = list(range(1, num_nodes + 1))
        elif keyword == 'NODE_COORD_SECTION':
            for j in range(1, num_nodes + 1):
                line_parts = lines[i + j].split()
                node_coords[int(line_parts[0])] = (int(line_parts[1]), int(line_parts[2]))
        elif keyword == 'DEMAND_SECTION':
            for j in range(1, num_nodes + 1):
                line_parts = lines[i + j].split()
                node, demand = int(line_parts[0]), int(line_parts[1])   
                node_demands[node] = demand

    for node in nodes:
        data.append({
            'nodo': node,
            'demanda': node_demands.get(node),
            'coordenadas': node_coords.get(node, None)
        })

    nodes = [node for node in nodes if node != depot]

    return trucks, capacity, nodes, depot, data
```

Suponiendo que se desea leer esta instancia, la carga de datos es la siguiente:

Ejemplo de invocación:

```python
file_path = '.../instancia.txt'
trucks, capacity, nodes, depot, data = read_data(file_path)
print("Número de vehículos:", trucks)
print("Capacidad de los vehículos:", capacity)
print("Nodos:", nodes)
print("Nodo Bodega:", depot)
print("Datos de los nodos:", data)
```

La salida es:

```
Número de vehículos: 4
Capacidad de los vehículos: 35
Nodos: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]
Nodo Bodega: 1
Datos de los nodos: [{'nodo': 1, 'demanda': 0, 'coordenadas': (30, 40)}, {'nodo': 2, 'demanda': 19, 'coordenadas': (37, 52)}, {'nodo': 3, 'demanda': 30, 'coordenadas': (49, 49)}, {'nodo': 4, 'demanda': 16, 'coordenadas': (52, 64)}, {'nodo': 5, 'demanda': 23, 'coordenadas': (31, 62)}, {'nodo': 6, 'demanda': 11, 'coordenadas': (52, 33)}, {'nodo': 7, 'demanda': 31, 'coordenadas': (42, 41)}, {'nodo': 8, 'demanda': 15, 'coordenadas': (52, 41)}, {'nodo': 9, 'demanda': 28, 'coordenadas': (57, 58)}, {'nodo': 10, 'demanda': 8, 'coordenadas': (62, 42)}, {'nodo': 11, 'demanda': 8, 'coordenadas': (42, 57)}, {'nodo': 12, 'demanda': 7, 'coordenadas': (27, 68)}, {'nodo': 13, 'demanda': 14, 'coordenadas': (43, 67)}, {'nodo': 14, 'demanda': 6, 'coordenadas': (58, 48)}, {'nodo': 15, 'demanda': 19, 'coordenadas': (58, 27)}, {'nodo': 16, 'demanda': 11, 'coordenadas': (37, 69)}]
```

#### Creacion de solucion inicial

```python
def create_first_solution(nodes):
    """Función objetivo para CVRP.

    Parameters
    ----------
    nodes : list [int]
        Lista de nodos

    Returns shuffled_nodes
    -------
    shuffled_nodes : list [int]
        Nueva lista de nodos con posiciones aleatorias
    """
    shuffled_nodes = random.sample(nodes, len(nodes))
    return shuffled_nodes

```

Ejemplo de invocación:

```python
shuffled_nodes = create_first_solution(nodes)
print("Solucion Inicial:", nodes)
print("Solucion Aleatoria:", shuffled_nodes)
```

La salida es:

```
Solucion Inicial: [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]
Solucion Aleatoria: [4, 14, 6, 7, 5, 16, 11, 12, 8, 2, 3, 10, 9, 13, 15]
```

#### Creacion de solucion vecina

```python
def create_neighbor_solution(nodes):
    """Función para generar una solucion vecina para CVRP.

    Parameters
    ----------
    nodes : list [int]
        Lista de nodos

    Returns neighbor_nodes
    -------
    neighbor_nodes : list [int]
        Nueva lista vecina de nodos 
    """
    neighbor_nodes = nodes[:]
    idx1, idx2 = random.sample(range(len(nodes)), 2)
    neighbor_nodes[idx1], neighbor_nodes[idx2] = neighbor_nodes[idx2], neighbor_nodes[idx1]
    return neighbor_nodes
```
Ejemplo de invocación:

```python
neighbor_solution = create_neighbor_solution(shuffled_nodes)
print("Solucion Inicial:", shuffled_nodes)
print("Solucion Vecina:", neighbor_solution)
```

La salida es:

```
Solucion Inicial: [6, 8, 3, 5, 16, 15, 10, 11, 14, 13, 12, 2, 4, 9, 7]
Solucion Vecina: [10, 8, 3, 5, 16, 15, 6, 11, 14, 13, 12, 2, 4, 9, 7]
```

#### Funcion objetivo

```python
def objective_CVRP(nodes, data, trucks, capacity, node_origin):
    """Función objetivo para CVRP.

    Parameters
    ----------
    nodes : list [int]
        Lista de nodos.
    data : list [dict]
        Lista de diccionarios con informacion de cada nodo: número, demanda y coordenadas.
    trucks : int
        Numero de vehículos.
    capacity : int
        Capacidad para cada vehículo.
    node_origin : int
        Nodo bodega.

    Returns sum(costs)
    -------
    sum(costs) : float
        Suma de los costos de cada ruta.
    """
    nodes_copy = nodes.copy()
    # Calcular el número de elementos por vehículo
    num_nodes_per_truck = len(nodes_copy) // trucks
    # Calcular el número de elementos restantes para el último vehículo
    remaining_nodes = len(nodes_copy) % trucks

    # Construir las rutas para cada vehículo
    routes = []
    demands = []  # Lista para almacenar las demandas de cada ruta
    costs = []    # Lista para almacenar los costos de cada ruta
    penalties = []  # Lista para almacenar las penalizaciones
    current_node = node_origin
    for i in range(trucks):
        route = []
        # Agregar el nodo de origen al inicio de la ruta
        route.append(node_origin)
        # Agregar los nodos correspondientes al vehículo actual
        route_capacity = 0  # Inicializar la capacidad de la ruta
        route_cost = 0      # Inicializar el costo de la ruta
        for j in range(num_nodes_per_truck):
            next_node = nodes_copy.pop(0)  # Sacar el primer nodo de la lista
            route.append(next_node)
            # Sumar la demanda del nodo a la capacidad de la ruta
            node_demand = next((d['demanda'] for d in data if d['nodo'] == next_node), None)
            if node_demand is not None:
                route_capacity += node_demand
            # Calcular el costo desde el nodo anterior al actual
            if j > 0:
                prev_node = route[-2]
                prev_coords = next((d['coordenadas'] for d in data if d['nodo'] == prev_node), None)
                current_coords = next((d['coordenadas'] for d in data if d['nodo'] == next_node), None)
                if prev_coords is not None and current_coords is not None:
                    # route_cost += euclidean_distance(prev_coords, current_coords)
                    route_cost += math.sqrt((prev_coords[0] - current_coords[0])**2 + (prev_coords[1] - current_coords[1])**2)
        # Si es el último vehículo, agregar los nodos restantes
        if i == trucks - 1:
            for _ in range(remaining_nodes):
                next_node = nodes_copy.pop(0)
                route.append(next_node)
                # Sumar la demanda del nodo a la capacidad de la ruta
                node_demand = next((d['demanda'] for d in data if d['nodo'] == next_node), None)
                if node_demand is not None:
                    route_capacity += node_demand
                # Calcular el costo desde el nodo anterior al actual
                if j > 0:
                    prev_node = route[-2]
                    prev_coords = next((d['coordenadas'] for d in data if d['nodo'] == prev_node), None)
                    current_coords = next((d['coordenadas'] for d in data if d['nodo'] == next_node), None)
                    if prev_coords is not None and current_coords is not None:
                        # route_cost += euclidean_distance(prev_coords, current_coords)
                        route_cost += math.sqrt((prev_coords[0] - current_coords[0])**2 + (prev_coords[1] - current_coords[1])**2)
        
        # Verificar si la capacidad de la ruta supera la capacidad del camión
        if route_capacity > capacity:
            # Calcular la penalización y añadirla a la lista de penalizaciones
            penalty = capacity + (capacity * (route_capacity - capacity))
            penalties.append(penalty)
        
        # Agregar el nodo de origen al final de la ruta
        route.append(node_origin)
        routes.append(route)
        demands.append(route_capacity)
        costs.append(route_cost)

        # Calcular la suma de las penalizaciones
    total_penalty = sum(penalties)
    # print("PENALIZACION:", total_penalty)
    # Sumar las penalizaciones al costo total
    total_cost = sum(costs) + total_penalty
    # print("COSTO SIN PENALIZACION:",total_cost)

    return int(total_cost)
```

Ejemplo de invocacion:

```python
# Descomentar en la funcion objetivo
print("El costo global es: ",sum(costs))
```

```python
# Ejemplo de invotacion
objective_CVRP(neighbor_solution, data, trucks, capacity, depot)
```

La salida es:

```
El costo global es:  214
```

`NOTA:` La función costo ya incluye penalización. Esta se añade cuando alguna ruta excede la capacidad del vehículo. Al calcular las demandas de cada ruta, se verifica que cada una no exceda la capacidad del vehículo. En caso de excederla, se calcula una penalización:

**capacidad_ruta = capacidad_ruta + penalización,**

Donde:

penalización = excedente_de_capacidad + capacidad_vehículo


### TIP: 

Como la función objetivo recibe 5 parámetros pero la metaheurística solo pueden mandar 1, que es el vector solución (el parámetro `nodes` en la función), se puede hacer uso de `functools.partial` para crear una función que reciba solo un parámetro y que los otros dos sean fijos. 

Por ejemplo:
```python
import functools
# Se fijan los parametros (excepto 'nodes')
fn = functools.partial(objective_CVRP, data=data, trucks=trucks, capacity=capacity, node_origin=depot)
initial_solution = create_first_solution(nodes)
miTabu = TabuSearch(...)
miTabu.search(fn)   
```
