### WebScrapping en R

Se utiliza package **RVEST**

- **html_name()** : Obtiene los atributos html
- **html_text()**: Extrae el texto html
- **html_attr()**: Regresa los atributos específicos html
- **html_attrs()** Obtiene atributos html de un conjunto de elementos
- **html_table()**: Convierte una tabla html en una estrucuta de datos en R


```R
url <- "https://www.imdb.com/title/tt2140553/"
webpage<-read_html(url)
class(webpage)

#buscamos el seleector
selector<- ".ratingValue > strong:nth-child(1) > span:nth-child(1)"

#creamos el nodo que estamos buscando
node <-html_node(webpage, selector)

#Imprimimos nodo
node

#Transformamos el nodo en texto
node_text<-html_text(node)
#Resultado
node_text

```

Para las tablas: 

```R
#Buscar una tabla
selector_table<-".cast_list"
node_table<- html_node(webpage, selector_table)
node_text<-html_text(node_table)
#este resultaod no es visiblemente bueno
node_text

#Usamos html_table 
node_text<-html_table(node_table)
node_text
```



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzMDMxNjYxMF19
-->