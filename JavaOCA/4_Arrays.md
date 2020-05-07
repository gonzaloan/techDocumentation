---


---

<h1 id="arrays">Arrays</h1>
<h3 id="declaración">Declaración</h3>
<p>int [] intArray;<br>
String [] stringArr;</p>
<ul>
<li>También puede ir los corchetes después del nombre, aunque no es considerado buena práctica:</li>
</ul>
<p>int intArray[]<br>
String stringArray []</p>
<h3 id="instanciación">Instanciación</h3>
<p>intArray = new int[5];</p>
<p>También puede usarse array initializer</p>
<p>int[] intArray = {1,2,3,4,5};<br>
intArray = new int[]{1,2,3,4,5}</p>
<p><strong>Importante, si bien los primitivos se pueden castear, los arrays no, por lo tanto algo como long[] myArr = new int[]{1,2}; es un error de compilación</strong></p>
<h1 id="multi-dimensional-arrays">Multi Dimensional Arrays</h1>
<h2 id="declaración-1">Declaración</h2>
<p>int[][] intArray;<br>
String[][][] stringArray;<br>
MyObject[][][][] objectArray;</p>
<h2 id="instanciación-1">Instanciación</h2>
<p>intArray = new int[2][3];</p>
<p><strong>Cuando se instancia, es necesario que el primer corchete venga con datos, y los demás no son obligatorios así: intArray = new int[2][][] es válido, mientras que intArray = new int[][][] y intArray = new int [][][4] no lo son</strong></p>
<h2 id="inicialización">Inicialización</h2>
<p>Es asignar valores a us elementos en todos los niveles</p>
<pre><code>int[][] intArray = new int[2][3];
for (int i = 0; i&lt; 2; i++) {
	for(int j = 0; j&lt;3; j++) {
		intArray[i][j] = i +j;
	}
}
</code></pre>
<p>Usando array initializer:</p>
<pre><code>int [][] intString = {{1,2,3},{4,5}};
int [][] intArray = new int[][]{{1,2,3},{4,5}};
</code></pre>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

