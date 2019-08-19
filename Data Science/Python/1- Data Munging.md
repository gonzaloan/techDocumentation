## Data Munging

Proceso en el cual ya se obtiene los datos, a partir de Scrapping, o bases de datos (RDBMS o NoSQL) y se debe manipular dicha información, ya que seguramente, no está lista para ser utilizada para análisis y/o experimentación.
La finalidad es limpiar esa data, y manipularla para convertirla en algo que se pueda digerir, como Datasets, 

Una vez se tiene definido el dataset, se abre una nueva fase, se debe observar la data, se desarrolla y prueba la hipótesis en un loop recurrente. Luego de todo esto, se crea un modelo. 

El proceso de Data Science puede darse con el siguiente acrónimo
**OSEMN(Observe, Scrub, Explore, Model, iNterpret)**. El munging corresponde a segunda etapa, de Scrub. 

### Lectura de CSV

Para lectura de CSV, podemos usar PANDAS. Esto leerá información y la almacenará en un formato especial de pandas, indexará cada fila, inferirá cada tipo de dato, convertirá data (Si es necesario), parseará datos, los campos vacíos y erróneos.

Para leer un dataset, por ejemplo IRIS:

Para leer un archivo, se debe especificar el caracter separador **sep=**, si hay una cabecera en el dataset **(header)**, y los nombres de las variables, usando **names** y una **list**.  El campo **decimal** tienen valores por defecto, y puede ser obviado. 

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzE5MzM1MzI3LC0xNjEzNjg2MzVdfQ==
-->