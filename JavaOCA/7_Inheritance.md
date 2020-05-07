---


---

<h1 id="inheritance">Inheritance</h1>
<ul>
<li>Clases no pueden extender más de una superclase</li>
<li>Si hay declaraciones así:</li>
</ul>
<pre><code>package com;
public class Super {
	String value;
}

//other
package com.whizlabs;
public class Me extends Super {

}

//other
package com;
public class Sub extends Me {
}
 
</code></pre>
<ul>
<li>En este caso Sub no hereda el valor de Super. Porque hay una jerarquía que seguir, a pesar de que estén en el mismo package.</li>
</ul>
<h2 id="polymorphism">Polymorphism</h2>
<pre><code>//Reference type     //Obnject Type
Number number = new Integer(0);
number = new Float(0);
</code></pre>
<ul>
<li>Para hacer override, el tipo de visibilidad debe ser el mismo o mayor que la subclase. No puede ser que la clase padre sea <em>public</em> y la que extienda <em>default</em>.</li>
<li>Para override, también debe ser el mismo tipo de retorno.</li>
</ul>
<h2 id="type-casting">Type Casting</h2>
<ul>
<li>UpCasting: Se puede hacer implícitamente</li>
</ul>
<pre><code>String myString = "";
Object myObject = myString;
</code></pre>
<ul>
<li>DownCasting: Necesita el casteo explícito</li>
</ul>
<pre><code>Object myObject = //blabla
String myString = (String) myObject; 
</code></pre>
<ul>
<li>Examples:</li>
</ul>
<pre><code>Double myDouble = new Double(0);
Floaqt myFloat = (Float) myDouble;
System.out.println(myFloat);
</code></pre>
<ul>
<li>Esto da un error de compilación, ya que Float o no es subclase ni superclase de Double. El casting no está permitido.</li>
</ul>
<h2 id="using-this-or-super">Using This or Super</h2>
<ul>
<li>Cuando se usa un this en un constructor, debe ser la primera instrucción, sino ocurre un compilation error:</li>
</ul>
<pre><code>public class Test {
	public int number;
	public Test(){
		this.number++;
		this(0); //Compilation error
	}
}


</code></pre>
<ul>
<li>Si un constructor no llama a ningún otro constructor en la primera línea, el compilador automáticamente inserta un “super()” en la primera línea:</li>
</ul>
<pre><code>public class Super {
	public int number = 0;
	public Super() {
		this.number++;
	}
}

public class Sub extends Super {
	public Sub() {
		//AQUI INSERTA super();
		this.number = this.number *2 ;
	}
}

</code></pre>
<h1 id="using-abstract-classes-and-interfaces">Using abstract classes and interfaces</h1>
<ul>
<li>
<p>Similitudes:<br>
– Ambas no pueden ser instanciadas.<br>
– Ambas pueden contener un mix de métodos declarados con o sin implementación</p>
</li>
<li>
<p>Diferencias:<br>
– Interfaces: Los campos deben ser public, static y final. En las clases Abstractas pueden ser non-public, non-static y non-final.<br>
– Interfaces: Métodos deben ser públicos. En las clases abstractas pueden tener cualquier modificador.<br>
– Interfaces: Una clase puede implementar muchas interfaces, en las clases abstractas sólo se puede extender a una.</p>
</li>
</ul>
<h2 id="casos-de-uso-de--clases-abstractas">Casos de Uso de 	Clases Abstractas</h2>
<ul>
<li>Se quiere compartir código entre algunas clases relacionadas.</li>
<li>Se espera que las clases que extiendan la clase abstracta tengan métodos o campos en común, o requieran modificadores de acceso distintos a public.</li>
<li>Se quieren declarar campos que no sean estáticos o non-final, permitiendo definir métodos que puedan acceder y modificar el estado del objeto al cual pertenecen.</li>
</ul>
<h2 id="casos-de-uso-de-interfaces">Casos de Uso de Interfaces</h2>
<ul>
<li>Se espera que clases no relacionadas implementen la interfaz.</li>
<li>Se quiere especificar el comportamiento de un DataType particular, pero sin importar sobre quién implementa el comportamiento</li>
</ul>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

