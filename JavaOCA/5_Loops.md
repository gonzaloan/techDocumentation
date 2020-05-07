---


---

<h1 id="loops">Loops</h1>
<h2 id="while-loops">While Loops</h2>
<p>La expresión evaluada debe ser un BOOLEAN.</p>
<h2 id="for-loop">For Loop</h2>
<ul>
<li>En un for loop antiguo, se puede inicializar la variable a iterar fuera de la operación y es válido</li>
</ul>
<pre><code>int i = 0;
for(i=1; i&lt; 10 &lt; i++){
	//Esto es válido
}
</code></pre>
<ul>
<li>En un enhanced for loop, esto es inválido, la variable debe inicializarse en el operador:</li>
</ul>
<pre><code>int element;
for(element : intArray) {
//Compilation Error
	System.out.println(element);
}
</code></pre>
<h2 id="do-while-loops">Do While Loops</h2>
<ul>
<li>La inicialización de la variable que será condición dentro del while, debe ser declarada antes, si se declara e inicializa dentro del do, no será visible para el while.</li>
</ul>
<pre><code>do {
	int i=0;
	System.out.println(i);
	i++;
}while(i&lt;10);
//Compilation error
</code></pre>
<h2 id="breaks--contiene">Breaks / Contiene</h2>
<ul>
<li>Un break o continue con label, sólo puede darse si su uso está dentro del loop que lo utiliza, el siguiente ejemplo es un error de compilación:</li>
</ul>
<pre><code>int i = 0, j=0;
while(i&lt;2){
	testing: while(j&lt;2){
		j++;
	}
	if (i+j == 2) break testing;
	i++
}
</code></pre>
<p>El ejemplo anterior, como el <strong>break testing</strong> estaba fuera de donde se usa el label, da un Compilation Error.</p>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

