
## Data Engineering

### Ambiente
Se puede crear ambientes usando anaconda. 
Para crear un nuevo ambiente (como venv) usamos:
```
conda create --name {nombre_ambiente} {librerias}
```
Ejemplo:
```
conda create --name gonzodata beautifulsoup4 numpy matplotlib yaml
```

Una vez creamos el ambiente, podemos 'acceder' a él o activarlo, usando el siguiente comando por consola:

```
conda activate gonzodata
```

Salir del ambiente:
```
conda deactivate
```

Eliminar un ambiente:
```
conda remove --name {nombre_env}
```
co
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjU0Mzk0MTkwXX0=
-->