### javascript中var、let、const声明的区别
```
	//全局声明
	var a = 1;
	let b = 1;
	const c=1;

	function foo() {};

	class Foo {};

	{
	   //块级声明
	   var ba=1;
	   let bb=1;
	   const bc=1;

	   class BFoo {};
	   function bfoo() {}
	}
```
1. LexicallyDeclaredNames（词法声明名称列表）：« bb,bc,bfoo,BFoo »
2. LexicallyScopedDeclarations（词法作用域声明列表）：« let bb=1,const bc=1,function bfoo(){},class BFoo{} »
3. VarDeclaredNames（var声明名称列表）：« ba »
4. VarScopedDeclarations（var作用域声明列表）：« ba=1 »
5. TopLevelLexicallyDeclaredNames（顶级词法声明名称列表）：« b,c,Foo »
6. TopLevelLexicallyScopedDeclarations（顶级词法作用域声明列表）：« let b=1,const c=1,class Foo{} »
7. TopLevelVarDeclaredNames（顶级var声明名称列表）：« a,ba,bfoo »
8. TopLevelVarScopedDeclarations（顶级var作用域声明列表）：« a=1,ba=1,function foo(){}»
注：« »结构是ECMAScript中的一个规范类型，表示一个List，具体你可以认为它是一个类数组（当然实际肯定不是，只是方便理解）


function声明在顶级作用域（TopLevel）中被视为var声明，而不在顶级作用域也就是Block或catch块中被认为是词法声明，这就导致了一些有趣的事情。
Block只有前四个列表，函数（function）和脚本（script）只有后四个列表（其实函数和脚本也只有前四个，不过前四个列表的值取的是后四个列表的值）。Block虽然有自己的作用域但是它和函数有着本质上的区别。函数和脚本你可以看成是相互独立的而Block是属于function和script的一部分。具体就是Block中的var声明同时也被认为是顶级声明，不管你嵌了多少层块在里面都不会变,因为Block没有顶级作用域。

#### 我们再来看看Block中的声明与function和script中有何不同：

1.LexicallyDeclaredNames中如果包含任何重复项，则语法错误。
2.LexicallyDeclaredNames中出现的任何元素在VarDeclaredNames声明中出现，语法错误。

```
{
  var foo=1;
  function foo(){}        
	//Syntax Error，var和function不能声明同一个标识符，脚本和函数中是不存在这个问题的。
}
```
```
{
  function a(){};  
  function a(){};      //it's ok,no syntax Error
}
//-----------------------
'use strict';
{
  function a(){};  
  function a(){};      //error, syntax Error redeclaration a; 
}
```
规范文档的附录中找到原因：为了实现网页浏览器的兼容性，允许在非严格模式下的Block中的function可以重复声明


参考：https://segmentfault.com/a/1190000012221834






