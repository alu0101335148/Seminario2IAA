# Redes Bayesianas en Juegos (parte 2)

## Autores

Airam Rafael Luque Leon (alu0101335148@ull.edu.es)

Lucas Hernández Abreu (alu0101335148@ull.edu.es)

## Descripción de la práctica

En esta práctica se quiere implementar el cálculo del próximo estado del bot mediante la programación. Para esto vamos a utilizar las librerrias de SMILE, que nos brindan la posibilidad de, dada una red (en este caso la desarrollada para la primera parte de este seminario) y un conjunto de evidencias, que hacen referencia al estado anterior(`St`), poder calcular las probabilidades de cada una de las posibles acciones a tomar en el estado `St+1`.

## Explicación del código

En esta sección vamos a explicar como hemos implementado el código (El lenguaje usado ha sido *`python`*).

Este código consta de dos funciones principales `askInitialValues` y la función `main`

### Input del usuario

Gracias la siguiente función pedimos al usuario que nos introduzca los valores de cada uno de los estados *`St`*.


*Codigo de la función `askInitialValues`*
```python
def askInitialValues():
	evidences = []
	print("Introduce el valor de St(Ataca|Recoge_arma|Recoge_energia|Explorar|Huir|Detectar_enemigo):")
	evidences.append(input())
	print("Introduce el valor de H(Alta|Baja):")
	evidences.append(input())
	print("Introduce el valor de HN(Si|No):")
	evidences.append(input())
	print("Introduce el valor de NE(Si|No):")
	evidences.append(input())
	print("Introduce el valor de OW(Armado|Desarmado):")
	evidences.append(input())
	print("Introduce el valor de PH(Si|No):")
	evidences.append(input())
	print("Introduce el valor de PW(Si|No):")
	evidences.append(input())
	print("Introduce el valor de W(Armado|Desarmado):")
	evidences.append(input())
	return evidences
```

Como podemos ver, al usuario se le pregunta por los valores de St y se almacenan en una lista (`evidences`).

### Función main

Una vez que tengamos los valores del estado previo podemos calcular cuál es la probabilidad del siguiente estado (*`St+1`*).

Para calcular la probabilidad de St+1 utilizaremos la función que se describe a continuación:

```python
def main():
	# Cargamos la red y la librería de la red
	net = pysmile.Network()
	net.read_file("P3_IAA.xdsl")

	# Fijamos las evidencias iniciales
	evidences = askInitialValues()

	try:
		net.set_evidence("St", evidences[0])
	except:
		print("Error 1 initial value not valid")
		return -1

	try:
		net.set_evidence("H", evidences[1])
	except:
		print("Error 2 initial value not valid")
		return -1

	try:
		net.set_evidence("HN", evidences[2])
	except:
		print("Error 3 initial value not valid")
		return -1

	try:
		net.set_evidence("NE", evidences[3])
	except:
		print("Error 4 initial value not valid")
		return -1

	try:
		net.set_evidence("OW", evidences[4])
	except:
		print("Error 5 initial value not valid")
		return -1

	try:
		net.set_evidence("PH", evidences[5])
	except:
		print("Error 6 initial value not valid")
		return -1

	try:
		net.set_evidence("PW", evidences[6])
	except:
		print("Error 7 initial value not valid")
		return -1

	try:
		net.set_evidence("W", evidences[7])
	except:
		print("Error 8 initial value not valid")
		return -1

	# Actualizamos la red
	net.update_beliefs() 

	# Calculamos los valores de St+1
	beliefs = net.get_node_value("St_1")

	# Mostrar por pantalla la tabla St+1
	for i in range(0, len(beliefs)):
		print(net.get_outcome_id('St_1', i) + "=" + str(round(float(beliefs[i] * 100))) + "%")

	return 0
```

En primer lugar utilizamos la librería *SMILE* para cargar los datos del fichero de GeNIe.

Una vez obtenida la red (`net`) procedemos a llamar a la functión que preguntará al usuario por las evidencias del estado previo.

Una vez obtenido el *array* de evidencias, podemos asignar a cada uno de los nodos de la red la evidencia dada por el usuario. Estas evidencias, si no se corresponden con lo que se espera, lanzarán una excepción para advertir al usuario de los errores cometidos.

Una vez con la red completamente asignada podemos ver los resultados de la ejecución.

*Se describe en el siguiente apartado*


## Ejemplos de ejecución

Ahora vamos a ver algunos ejemplos de la ejecución del programa, y posteriormente contrastaremos las soluciones con las que hemos obtenido mediante la herramienta GeNIe, en la que originalmente describimos la red.

### Ejemplo 1

#### Programa:

```
Introduce el valor de St(Ataca|Recoge_arma|Recoge_energia|Explorar|Huir|Detectar_enemigo):
Recoge_arma
Introduce el valor de H(Alta|Baja):
Alta
Introduce el valor de HN(Si|No):
Si
Introduce el valor de NE(Si|No):
No
Introduce el valor de OW(Armado|Desarmado):
Armado
Introduce el valor de PH(Si|No):
Si
Introduce el valor de PW(Si|No):
Si
Introduce el valor de W(Armado|Desarmado):
Armado
Ataca=0%
Recoge_arma=0%
Recoge_energia=0%
Explorar=0%
Huir=0%
Detectar_enemigo=100%
```

#### GeNIe:

<img src="./img/ejemplo1.jpg" alt="Ejemplo1">

### Ejemplo 2
```
Introduce el valor de St(Ataca|Recoge_arma|Recoge_energia|Explorar|Huir|Detectar_enemigo):
Detectar_enemigo
Introduce el valor de H(Alta|Baja):
Alta
Introduce el valor de HN(Si|No):
No
Introduce el valor de NE(Si|No):
No
Introduce el valor de OW(Armado|Desarmado):
Desarmado
Introduce el valor de PH(Si|No):
Si
Introduce el valor de PW(Si|No):
Si
Introduce el valor de W(Armado|Desarmado):
Armado
Ataca=0%
Recoge_arma=0%
Recoge_energia=0%
Explorar=0%
Huir=0%
Detectar_enemigo=100%
```

#### GeNIe:

<img src="./img/ejemplo2.jpg" alt="Ejemplo2">

## Porcentaje de participación

Airam Rafael Luque León (50%)

Lucas Hernández Abreu (50%)