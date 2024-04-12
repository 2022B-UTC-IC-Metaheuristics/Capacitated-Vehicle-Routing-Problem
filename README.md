# The Capacited Vehicle Routing Problem

By Christian Hernandez

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
* Dimension: Número de nodos y depósitos.
* Capacidad de los vehículos: Es la misma para todos.
* Número de nodo y coordenas en forma matricial: Cada renglón posee el número del nodo y sus coordenadas X y Y.
* Demandas de cada destino: Matriz en cuál cada renglón corresponde al número de nodo y su respectiva demanda.
* Nodo origen: Número de nodo inicial.

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


Nuestra propueta para solución inicial:

```python
def getTotalDemand(self):
        totalDemand = 0
        for element in self.data:
            totalDemand += element[2]
        return totalDemand

    def getDemand(self, route):
        demand = 0
        for node in route:
            demand += self.data[node][2]
        return demand

    def create_first_solution(self):
        totalDemand = self.getTotalDemand()
        totalCars = math.ceil(totalDemand / self.capacity)

        while(1):
            routes = []
            for i in np.arange(totalCars):
                routes.append([])
            demands = []

            lista = list(range(1,(len(self.data))))
            random.shuffle(lista)

            for i in np.arange(len(self.data) - 1):
                index = random.randint(0,(totalCars - 1))
                routes[index].append(lista[i])

            for route in routes:
                demands.append(self.getDemand(route))
            
            isUnderDemand = True
            for demand in demands:
                if(demand > self.capacity):
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
import copy

def create_neighbor_solution(self, actual_solution):    
        while(1):
            neighbor = copy.deepcopy(actual_solution)
            idx = random.randint(0,(len(self.data)-2))

            i = 0
            chargedNode = 0
            chargedRoute = 0
            for route in neighbor:
                for node in route:
                    if(i == idx):
                        chargedNode = node
                    i += 1
                if(chargedNode):
                    route.remove(chargedNode)
                    break
                chargedRoute += 1

            idxRoute = random.randint(0,(len(neighbor)-1))
            idxNode = random.randint(0, len(neighbor[idxRoute]))
            neighbor[idxRoute].insert(idxNode, chargedNode)

            demands = []
            for route in neighbor:
                demands.append(self.getDemand(route))

            isUnderDemand = True
            for demand in demands:
                if(demand > self.capacity):
                    isUnderDemand = False
            
            if(isUnderDemand):
                return neighbor

```
Función objetivo:

```python
def CVRP_function(routes):
    distance = 0
    for i in np.arange(len(routes)-1):
        distance += np.sqrt((routes[i][0] - routes[i+1][0])**2 + (routes[i][1] - routes[i+1][1])**2)
    return distance

def getAllDistances(data, routes):
    allDistances = 0
    for route in routes:
        coordenates = []
        coordenates.append(data[0][1])
        for node in route:
            coordenates.append(data[node][1])
        coordenates.append(data[0][1])
        allDistances += CVRP_function(coordenates)
    return allDistances
```
