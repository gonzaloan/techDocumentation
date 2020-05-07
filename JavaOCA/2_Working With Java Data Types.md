---


---

<h1 id="declarar-e-inicializar-variables">Declarar e inicializar variables</h1>
<h2 id="class-level-variables">Class-Level Variables</h2>
<ul>
<li>Son inicializadas y declaradas al mismo tiempo. Si declaras una variable sin inicializarla, el compilador seteará la variable al valor defecto de su tipo.<br>
– <strong>byte, short, int, long</strong>: 0<br>
– <strong>float, double</strong>: 0.0<br>
– <strong>char</strong>: ‘\u0000’<br>
– <strong>boolean</strong>: false<br>
– <strong>Object</strong>: null</li>
</ul>
<h2 id="local-variables">Local Variables</h2>
<ul>
<li>
<p>Usadas para almacenar valores temporales, deben ser declarados y <strong>explícitamente</strong> inicializados antes de ser usados.</p>
</li>
<li>
<p>Debe ser inicializada en cualquier lugar luego de ser declarada, y antes del cierre del método contenedor.</p>
</li>
</ul>
<h1 id="primitive-data-type-casting">Primitive Data Type Casting</h1>
<ul>
<li>
<p>Widening casting: Implementado implícitamente<br>
<strong>byte -&gt; short -&gt; int -&gt; long -&gt; float -&gt; double</strong></p>
</li>
<li>
<p>Narrowing casting: Implementado explícitamente.<br>
<strong>double -&gt; float -&gt; long -&gt; int -&gt; short -&gt; byte</strong></p>
</li>
<li>
<p>Casting hacia y desde <em>char</em><br>
– Widening casting: desde <em>char</em> hacia <em>int, long, float o double</em><br>
– Narrowing casting: desde <em>char</em> hacia <em>byte o short</em>; desde <em>short, int, long, float o double</em> a <em>char</em>.<br>
– Widening and Narrowing Casting: <em>byte</em> a <em>char</em></p>
</li>
</ul>
<p><strong>Nota</strong>: Los casting entre primitivos, nunca resulta en un RuntimeException</p>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

