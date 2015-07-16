<html lang="ch">
<head>
	<meta charset="UTF-8">
	<title>js作用域学习笔记</title>
</head>
<body>
	<h1>js的作用域</h1>
	<p>在js中变量的作用域有<b>全局作用域</b>和<b>局部作用域</b>两种</p>
	<h2>1.全局作用域</h2>
	<h3>(1)最外层函数及最外层定义的变量拥有全局作用域</h3>
	<pre>
var apple = "apple";
function alertBanana(){
   var banana = "banana";
   function alertBanana2(){
      alert("banana");
   }
   alertBanana2();
}
alert(apple);//apple
alert(banana);//错误
alertBanana();//banana
alertBanana2();//错误
</pre>
	<h3>(2)所有未定义直接赋值的变量拥有全局作用域</h3>
	<pre>
apple = "apple";
function alertBanana(){
   var banana = "banana";
   orange = "orange";
}
alert(apple);//apple
alert(banana);//错误
alert(orange);//orange
	</pre>
		<h3>(3)所有window对象的属性和方法拥有全局作用域</h3>
	<pre>
alert(typeof alert);//function
alert(closed);//false
alert(document);//[object HTMLDocument]
	</pre>
	<h2>2.局部作用域</h2>
	<p>和全局作用域相反，局部作用域一般只在固定的代码片段内可访问到，最常见的例如函数内部，所以在一些地方也会看到有人把这种作用域称为函数作用域.</p>
	<pre>
function alertBanana(){
   var banana = "banana";
}
alert(banana);//错误
alert(body);//错误
alery(document.body);//[object HTMLBodyElement]
	</pre>
	<h1>js作用域链</h1>	
	<p>“在JS中，一切皆是对象”</p>
	<p>这就意味着函数也是对象，它与其他对象一样，拥有可通过代码访问的属性和一系列仅供JavsScript引擎访问的<b>内部属性</b>。其中有一个内部属性[[Scope]]，由ECMA-262标准第三版定义，该内部属性包含了函数被创建的作用域中对象的集合，<b>这个集合被称为函数的作用域链</b>，它决定了哪些数据能被函数访问。</p>
	<p>当一个函数创建后，它的作用域链会被<b>创建此函数的作用域</b>中可访问的数据对象填充。例如定义下面这样一个函数：</p>
	<pre>
var fruit = "apple";
var vegetable = "potato";
function alertFruit(fruit1){
   alert(fruit);
   alert(fruit1);
   var fruit = "banana";
   alert(fruit);
   alert(vegetable);
   alert(meat);
}
	</pre>
	<p>(1)函数“alertFruit”拥有全局作用域，在其被创建时，它的作用域链中会填入一个全局对象，该全局对象包含了所有的全局变量。</p>
	<pre>
[[scope]] = {
	global object//eg:window(object),this(window),fruit(object),alertFruit(function)
}
	</pre>
	<p>(2)然后执行该函数：</p>
	<pre>
alertFruit("orange");
	</pre>
	<p>执行此函数是会创建一个称为“运行期上下文”的内部对象，该对象的作用域链初始化为当前运行函数的[[Scope]]所包含的对象。这些对象的值 <b>按照它们出现在函数中的顺序</b>被复制到运行期上下文的作用域链中。它们共同组成了一个新的对象，叫“活动对象(activation object)”，该对象包含了函数的所有局部变量、命名参数、参数集合以及this，然后此对象会被推入作用域链的<b>前端</b>。
	</p>
	<pre>
[[scope]] = {
	this: window,
	arguments: ["orange"],
	fruit1: "orange",
	fruit: undefined,
	...
},{
	global object
}
	</pre>
	<p>(3)在函数执行过程中，每遇到一个变量，都会经历一次标识符解析过程以决定从哪里获取和存储数据。该过程从作用域链头部，也就是从活动对象开始搜索，查找同名的标识符，如果找到了就使用这个标识符对应的变量，如果没找到继续搜索作用域链中的下一个对象，如果搜索完所有对象都未找到，则认为该标识符未定义。函数执行过程中，每个标识符都要经历这样的搜索过程。<p>
	<p>所以该函数的运行结果为：</p>
	<pre>
undefined
orange
banana
potato
错误
	</pre>
<h1>参考资料</h1>
<h5>1.<a href="http://www.cnblogs.com/lhb25/archive/2011/09/06/javascript-scope-chain.html">理解JavaScript作用域和作用域链</a></h5>
<h5>2.<a href="http://www.laruence.com/2009/05/28/863.html">鸟哥：JavaScript作用域原理</a></h5>
</body>

</html>