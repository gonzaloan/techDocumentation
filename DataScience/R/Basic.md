## Basico

- **str()**: muestra la estructura del dataset  
- **summary()**: muestra el resumen del dataset  
- **transform()**: permite modificar los valores de un dataset  
- **as.logical(dataset$variable)**: te permite cambiar el tipo de dato de un dataset a booleano
- **sum(vector)**: Suma todos los valores dentro de un vector.


#### Matrices

- Se crean con el *matrix*
- **colnames({matriz}) <- {datos}**: Agrega nombre de las columnas
- **rownames({matriz}) <- {datos}**: Agrega nombre de las filas.
- **colSums**: Suma de las columnas
- **rbind(matriz1, matriz2):** Añade una nueva matriz dentro de la matriz 1.
- **matriz[n:m]**: Ver datos a partir de fila n, hasta la columna m. 
```
dias <- c("Lunes", "Martes", "Miércoles", "Jueves", "Viernes")
tiempo_platzi <- c(25,5,10,15,10)
tiempo_lecturas <- c(30,10,5,10,15)

tiempo_matrix <- matrix(c(tiempo_platzi, tiempo_lecturas),
                        nrow = 2, byrow = TRUE)

tiempo <- c("Tiempo 1", "Tiempo 2")

colnames(tiempo_matrix) <- dias
rownames(tiempo_matrix) <- tiempo
tiempo_matrix
#Ver suma de cada columna
colSums(tiempo_matrix)

```



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzNDA0NTU5OV19
-->