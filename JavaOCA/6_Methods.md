---


---

<h1 id="methods">Methods</h1>
<h2 id="fases-del-overloading">Fases del Overloading</h2>
<ul>
<li>Realiza overloading sin permitir boxing o unboxing, ni el uso de varargs</li>
<li>Si lo anterior no funciona, permite boxing y unboxing, pero sin usar varargs</li>
<li>Si lo anterior no funciona permite boxing, unboxing y varargs</li>
</ul>
<p>** Convertir un valor primitivo en un Wrapper Class, es AUTOBOXING**<br>
<strong>Convertir un valor de Wrapper Class en primitivo se llama UNBOXING</strong></p>
<h2 id="static-keyword">Static Keyword</h2>
<pre><code>Data data1 = new Data();
Data data2 = new Data();
data1.instanceValue = 1;
data1.staticValue = 1;
System.out.println(data2.instanceValue); // Prints: 0
System.out.println(data.staticValue); //Print 1
//class declaration
public class Data {
	int instanceValue =0;
	static int staticValue = 0;
}
</code></pre>
<ul>
<li>Un método static puede acceder sólo a atributos static.</li>
<li>Un atributo de clase static, puede ser accedido por métodos static o no static</li>
</ul>
<h2 id="access-modifiers">Access Modifiers</h2>
<ul>
<li>
<p>Hay niveles:<br>
– Top Level: public, package-private<br>
– Member level: public, protected, package, private.</p>
</li>
<li>
<p>Permisos:<br>
– private: Sólo accedidos desde su propio tipo.<br>
– default-package-private:  Visible sólo dentro de su mismo package.<br>
– protected: Sólo accedido dentro de su mismo package, y denttro de subtypes de su tipo en otros packages.<br>
– public: anywhere.</p>
</li>
</ul>
<h2 id="encapsulation-principles-in-a-class">Encapsulation Principles in a Class</h2>
<ul>
<li>Es sólo tener atributos private y que puedan ser accedidos desde métodos públicos.</li>
</ul>
<h2 id="pasar-primitivos-vs-object-references">Pasar primitivos vs Object References</h2>
<ul>
<li>Al pasarle un primitivo a un método, el valor es copiado desde el argumento al parámetro. Son independientes uno del otro.</li>
<li>Al pasar un objeto, se copia la dirección del objeto, por lo que el argumento y el parámetro apuntan al mismo objeto y cualquier cambio en el parámetro será reflejado en el argumento.</li>
<li>Reasignaciones no son válidas, no cambian nada, ejemplo:</li>
</ul>
<pre><code>void swapData(Data data1, Data data2){
	Data temp = data1;
	data1 = data2;
	data2 = temp;
}

//Class
class Data {
	int value;
	Data(int value){
	this.value = value;}
}


// RESULTADOS
Data data1 = new Data(-1), data2 = new Data(1);
swapData(data1, data2);
System.out.println(data1.value + "" + data2.value); //DA COMO RESULTADO -1 1
</code></pre>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

