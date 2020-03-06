# Comentarios

## Single Line

```java
// comentario single line
```

## Multiple Line Comment

Empiezan por /* y terminan con */ . Los asteriscos en cada línea son opcionales.
```java
/* multi-
* line
*/ 
```

## Javadoc Comment

Empiezan con /** 
```java
/**
* Javadoc Comment
* @author Gonzalo Muñoz
*/
```

# Classes vs Files

## Clases no públicas

Archivo:  Test.java
```java
class Animal {
	String name;
}
```
**Resultado: Funciona**

## Más de una clase en el mismo archivo.

- Al hacer esto, **a lo más una clase debe** ser pública. 
- Si una clase es pública, el nombre del archivo debe ser **igual** al nombre de la clase.
- 
- 
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUzNzY5NTY2MSwxNDUyNjk4NzE3LDE5MD
Y0ODA4MDBdfQ==
-->