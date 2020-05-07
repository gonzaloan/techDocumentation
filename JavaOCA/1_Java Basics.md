---


---

<h1 id="alcance-de-variables">Alcance de Variables</h1>
<h2 id="class-level-scope">Class-Level Scope</h2>
<ul>
<li>Las variables Class-Level, conocidas como campos, son declaradas dentro de una clase y afuera de cualquier método.</li>
<li>Estas variables son accedidas desde cualquier lugar dentro de la clase, y quizás desde afuera si se proporciona un modificador.</li>
</ul>
<h2 id="method-level-scope">Method Level Scope</h2>
<ul>
<li>Variables dentro de un método</li>
</ul>
<h2 id="block-level-scope">Block Level Scope</h2>
<ul>
<li>Definido entre llaves {}</li>
<li>Las variables definidas en llaves, sólo son visibles dentro de esas llaves.</li>
</ul>
<h1 id="estructura-de-una-clase-java">Estructura de una clase Java</h1>
<h2 id="declaración-de-clase">Declaración de Clase</h2>
<ul>
<li>Modificadores, como public o private.</li>
<li><strong>El keyword class</strong></li>
<li><strong>El nombre de la clase, con la letra inicial en mayúscula por convención</strong></li>
<li>El nombre de la superclase, si existe, precedida de la palabra <em>extends</em> (debe ir antes de las interfaces)</li>
<li>Una lista de interfaces separadas por comas implementadas por la clase, si existen, precedidas de la palabra <em>implements</em>.</li>
<li><strong>El cuerpo de la clase, alrededor de un par de llaves {}</strong></li>
</ul>
<h2 id="fields">Fields</h2>
<ul>
<li>Tiene cero o más modificadores, como public o private.</li>
<li><strong>El tipo del campo</strong></li>
<li><strong>El nombre del campo</strong></li>
</ul>
<h2 id="methods">Methods</h2>
<ul>
<li>Tienen modificadores como public o private.</li>
<li><strong>El tipo de retorno</strong> si el método retorna un valor, o void.</li>
<li><strong>El nombre del método</strong></li>
<li><strong>Una lista de parámetros delimitados por coma</strong>, precedidos de sus tipos de datos, entre paréntesis. Si no hay parámetros, se usan paréntesis vacíos.</li>
<li>Una lista de excepciones.</li>
<li><strong>El cuerpo del método</strong>, cerrado entre llaves {}</li>
</ul>
<h2 id="constructor">Constructor</h2>
<ul>
<li>
<p>Usados para crear objetos desde el blueprint de la clase.</p>
</li>
<li>
<p>Las declaraciones de constructor se ven como declaraciones de método, pero usan el nombre de la clase y no tienen tipo de retorno.<br>
– Modificadores<br>
– <strong>El nombre de la clase</strong><br>
– <strong>Un listado de parámetros separados por coma</strong>.<br>
– Una lista de exceptions<br>
– <strong>El cuerpo del constructor</strong>.</p>
</li>
<li>
<p>Si no se coloca modificador en el constructor, Java coloca implícitamente el mismo modificador usado para la declaración de la clase.</p>
</li>
</ul>
<h1 id="main-method">Main Method</h1>
<p>Para que una clase sea ejecutable:</p>
<ul>
<li>Se requiere una clase con el modificador public.</li>
<li>Un método de la forma:</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">static</span> <span class="token keyword">void</span> <span class="token function">main</span><span class="token punctuation">(</span>String <span class="token punctuation">[</span><span class="token punctuation">]</span> args<span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token punctuation">}</span>
<span class="token comment">//o</span>
<span class="token keyword">public</span> <span class="token keyword">static</span> <span class="token keyword">void</span> <span class="token function">main</span><span class="token punctuation">(</span>String<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>args<span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>Nombrar al archivo contenedor del código como el nombre de la clase pública, <strong>sino hay error de compilación</strong>.</li>
<li>Compilar el archivo de fuentes en una clase java usando el compilador.</li>
<li>Ejecutar la aplicación usando el Java launcher tool.</li>
</ul>
<h1 id="import-java-packages">Import Java Packages</h1>
<ul>
<li>Sin el statement <em>import</em>, los tipos de otros paquetes se deben referir con sus nombres completos: java.util.List, java.util.Map</li>
<li>Deben ser puestos al principio del archivo, justo después del statement del package, si existe.</li>
<li>Todos los types de un package pueden ser importados usando (asterisco): <em>import java.util.</em>*</li>
<li>java.lang es automáticamente importado.</li>
</ul>
<h1 id="componentes-de-java">Componentes de Java</h1>
<h2 id="buzzwords">Buzzwords</h2>
<p>Simple, Object Oriented, Distributed, Interpreted, Robust, Secure, Architecture Neutral, Platform Independent, High Performance, Multithreaded, Dynamic.</p>
<h2 id="conceptos-poo">Conceptos POO</h2>
<p>Herencia, Polimorfismo, Abstracción, Encapsulación</p>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

