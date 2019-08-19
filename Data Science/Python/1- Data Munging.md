## Data Munging

Proceso en el cual ya se obtiene los datos, a partir de Scrapping, o bases de datos (RDBMS o NoSQL) y se debe manipular dicha información, ya que seguramente, no está lista para ser utilizada para análisis y/o experimentación.
La finalidad es limpiar esa data, y manipularla para convertirla en algo que se pueda digerir, como Datasets, 

Una vez se tiene definido el dataset, se abre una nueva fase, se debe observar la data, se desarrolla y prueba la hipótesis en un loop recurrente. Luego de todo esto, se crea un modelo. 

El proceso de Data Science puede darse con el siguiente acrónimo
**OSEMN(Observe, Scrub, Explore, Model, iNterpret)**. El munging corresponde a segunda etapa, de Scrub. 

### Lectura de CSV

Para lectura de CSV, podemos usar PANDAS. Esto leerá información y la almacenará en un formato especial de pandas, indexará cada fila, inferirá cada tipo de dato, convertirá data (Si es necesario), parseará datos, los campos vacíos y erróneos.

Para leer un dataset, por ejemplo IRIS:

```python
import pandas as pd
import urllib

url = "http://aima.cs.berkeley.edu/data/iris.csv"
set1 = urllib.request.Request(url)
iris_p = urllib.request.urlopen(set1)
iris = pd.read_csv(iris_p, sep=',', decimal='.', header= None,
              names = ['sepal_length', 'sepal_width', 'petal_length', 'petal_width', 'target'])
```
Para leer un archivo, se debe especificar el caracter separador **sep=**, si hay una cabecera en el dataset **(header)**, y los nombres de las variables, usando **names** y una **list**.  El campo **decimal** tienen valores por defecto, y puede ser obviado. 

El resultado de iris, es nuestro dataframe. 

```python
iris.head() # ver los primeros 5 elementos
iris.tail() #Ultimos 5 elementos	
iris.head(x) #X es el numero elementos que queremos mostrar. iris.head(2) muestra los primeros 2
iris.columns # Ver los nombres de las columnas
iris['target'] #target es el nombre de la columna, muestra todos los elementos de dicha columna
iris[['sepal_length', 'target']] # ver columnas sepal_length y target



```

### Trabajando con DataSets problemáticos

Al tener dataset que tienen datos faltantes, por ejemplo:

```
 ![](https://d3ginfw2u4xn7p.cloudfront.net/d09f25b671fa4be2b71a970da5ec0822/images/Packt-logo-135.png) 

-   My Library
-   [Browse All](https://subscription.packtpub.com/search)
-   [Books & Videos](https://subscription.packtpub.com/books-and-videos)
-   [Learning Paths](https://subscription.packtpub.com/learning-paths)
-   [Projects](https://subscription.packtpub.com/projects)
-   Recent
-   [Store (current)](https://www.packtpub.com/all)

-   [Back](https://subscription.packtpub.com/#)
-   ###### Python Data Science Essentials - Third Edition
    
    ![](https://static.packt-cdn.com/products/9781789537864/cover/smaller) 
    

-   Contents
-   Bookmarks (1)

-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/1)
    
    1: First Steps
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/2)
    
    2: Data Munging
    

-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/2)
    
    Data Munging
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/2/ch02lvl1sec15/the-data-science-process)
    
    The data science process
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/2/ch02lvl1sec16/data-loading-and-preprocessing-with-pandas)
    
    Data loading and preprocessing with pandas
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/2/ch02lvl1sec17/working-with-categorical-and-textual-data)
    
    Working with categorical and textual data
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/2/ch02lvl1sec18/data-processing-with-numpy)
    
    Data processing with NumPy
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/2/ch02lvl1sec19/creating-numpy-arrays)
    
    Creating NumPy arrays
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/2/ch02lvl1sec20/numpy-fast-operation-and-computations)
    
    NumPy fast operation and computations
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/2/ch02lvl1sec21/summary)
    
    Summary
    

-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/3)
    
    3: The Data Pipeline
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/4)
    
    4: Machine Learning
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/5)
    
    5: Visualization, Insights, and Results
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/6)
    
    6: Social Network Analysis
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/7)
    
    7: Deep Learning Beyond the Basics
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/8)
    
    8: Spark for Big Data
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/app01)
    
    Strengthen Your Python Foundations
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/app02)
    
    Other Books You May Enjoy
    
-   [](https://subscription.packtpub.com/book/big_data_and_business_intelligence/9781789537864/backindex)
    
    Index
    

## Data loading and preprocessing with pandas

In the previous chapter, we discussed where to find useful datasets and examined the basic import commands of Python packages. In this section, having kept your toolbox ready, you are about to learn how to structurally load, manipulate, process, and polish data using pandas and NumPy.

### Fast and easy data loading

Let's start with a CSV file and pandas. The pandas library offers the most accessible and complete functionality to load tabular data from a file (or a URL). By default, it will store data in a specialized pandas data structure, index each row, separate variables by custom delimiters, infer the right data type for each column, convert data (if necessary), as well as parse dates, missing values, and erroneous values.

We will start by importing the pandas package and reading our `Iris` dataset:

```markup
In: import pandas as pd
iris_filename = 'datasets-uci-iris.csv'
iris = pd.read_csv(iris_filename, sep=',', decimal='.', header=None,
names= ['sepal_length', 'sepal_width', 
                               'petal_length', 'petal_width',
                               'target'])
```

You can specify the name of the file, the character used as a separator (`sep`), the character used for the decimal placeholder (`decimal`), whether there is a header (`header`), and the variable names (using `names` and a list). The settings of the `sep=','` and `decimal='.'` parameters have default values, and they are redundant in function. For European-style CSV, it is important to point out both since, in many European countries, the separator character and the decimal placeholder are different from the default ones.

If the dataset is not available online, you can follow these steps to download it from the internet:

```markup
In: import urllib
url = "http://aima.cs.berkeley.edu/data/iris.csv"
set1 = urllib.request.Request(url)
iris_p = urllib.request.urlopen(set1)
iris_other = pd.read_csv(iris_p, sep=',', decimal='.',
header=None, names= ['sepal_length', 'sepal_width',
'petal_length', 'petal_width', 
                         'target'])
iris_other.head()
```

The resulting object, named `iris`, is a pandas DataFrame. It's more than a simple Python list or dictionary, and in the sections that follow, we will explore some of its features. To get an idea of its content, you can print the first (or the last) row(s) by using the following commands:

```markup
In: iris.head()
```

The head of the DataFrame will be printed in the output:

![](https://static.packt-cdn.com/products/9781789537864/graphics/f76acb6f-3e46-444c-ad36-8e3d733422e8.png)

```markup
In: iris.tail()
```

The function, if called without arguments, will print five lines. If you want to get back a different number of rows, just call the function using the number of rows you want to see as an argument, as follows:

```markup
In: iris.head(2)
```

The preceding command will print only the first two lines. Now, to get the names of the columns, you can simply use the following method:

```markup
In: iris.columns
Out: Index(['sepal_length', 'sepal_width', 
            'petal_length', 'petal_width',  
            'target'], dtype='object')
```

The resulting object is a very interesting one. It looks like a list, but it is actually a pandas index. As suggested by the object's name, it indexes the columns' names. To extract the target column, for example, you can simply do the following:

```markup
In: y = iris['target']
y

Out:0     Iris-setosa
1     Iris-setosa
2     Iris-setosa
3     Iris-setosa
...
149    Iris-virginica
Name: target, dtype: object
```

The type of the object `y` is a pandas series. Right now, think of it as a one-dimensional array with axis labels, as we will investigate it in depth later on. Now, we just understood that a pandas `Index` class acts like a dictionary index of the table's columns. Note that you can also get a list of columns referring to them by their indexes, as follows:

```markup
In: X = iris[['sepal_length', 'sepal_width']]
X

Out: [150 rows x 2 columns]
```

Here are the four head rows of the `X` dataset:

![](https://static.packt-cdn.com/products/9781789537864/graphics/649b2215-ec09-4f4b-9e7f-124fdc321f5a.png)

And here are the four tail ones:

![](https://static.packt-cdn.com/products/9781789537864/graphics/bce3c9d2-c045-4693-85a5-1ec019d044ce.png)

In this case, the result is a pandas DataFrame. Why such a difference in results when using the same function? In the first case, we asked for a column. Therefore, the output was a 1D vector (that is, a pandas series). In the second example, we asked for multiple columns and we obtained a matrix-like result (and we know that matrices are mapped as pandas DataFrames). A novice reader can simply spot the difference by looking at the heading of the output; if the columns are labeled, then you are dealing with a pandas DataFrame. On the other hand, if the result is a vector and it presents no heading, then that is a pandas series.

So far, we have learned some common steps from the data science process; after you load the dataset, you usually separate the features and target labels.

In a classification problem, target labels are the ordinal numbers or textual strings that indicate the class associated with every set of features.

Then, the following steps require you to get an idea of how large the problem is, and therefore, you need to know the size of the dataset. Typically, for each observation, we count a line, and for each feature, a column.

To obtain the dimensions of the dataset, just use the attribute shape on either a pandas DataFrame or series, as shown in the following example:

```markup
In: print (X.shape)

Out: (150, 2)

In:  print (y.shape)

Out: (150,)
```

The resulting object is a tuple that contains the size of the matrix/array in each dimension. Also, note that a pandas series follow the same format (that is, a tuple with only one element).

### Dealing with problematic data

```markup
Date,Temperature_city_1,Temperature_city_2,Temperature_city_3,Which_destination
20140910,80,32,40,1
20140911,100,50,36,2
20140912,102,55,46,1
20140912,60,20,35,3
20140914,60,,32,3
20140914,,57,42,2
```
```
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAwNjE5ODk3OSwtNjI5MTEyNjU1LDMxOT
MzNTMyNywtMTYxMzY4NjM1XX0=
-->