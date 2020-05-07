---


---

<h1 id="use-java-operators">Use Java Operators</h1>
<ul>
<li>Los operadores de asignación  ( =, +=, -=, *=, /=, %=, &amp;=, ^=, |=, &lt;&lt;=, &gt;&gt;=, &gt;&gt;&gt;=) son evaluados de <strong>derecha a izquierda</strong></li>
</ul>
<pre><code>int i1 = 1, i2 = 2;
int i = i2 = i1;
sout(i) //Va a ser uno. Todos toman el valor de i1 
</code></pre>
<ul>
<li>Los operadores, ++, --, !, +, -, *, /, +, -, &gt;&gt;, &lt;&lt;, &gt;&gt;&gt;, &lt;, &gt;, &lt;=,&gt;=, ==, !=, tienen mayor imprtancia y son evaluados antes que los &amp;, ^, | &amp;&amp;, ||, ?:, y los operadores de asignación</li>
</ul>
<pre><code>int i = 4 &lt;&lt; 4/2; // la división es primero evaluada.
//Se calcula como 4 times 2 for the power of 2

//OTRO EJEMPLO

12 &gt;&gt; 1 //Sería 12 / 2 = 6
12 &gt;&gt; 2 // Sería 12/2 = 6/2 = 3
</code></pre>
<h1 id="switch-statement">Switch Statement</h1>
<ul>
<li>Switch funciona con byte, short, char, int, en forma primitiva y Wrapper, además de enum, y String.</li>
</ul>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

