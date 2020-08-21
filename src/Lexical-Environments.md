### Lexical Environments 名词解释


#### Realms (领域)

1. 在执行ECMAScript代码之前，所有ECMAScript代码都必须与一个领域相关联。从概念上讲，一个领域由一组内部对象，一个ECMAScript全局环境，在该全局环境作用域内加载的所有ECMAScript代码以及其他相关的状态和资源组成
2. Realms.[[Intrinsics]] %Object%（Object构造器），%ObjectPrototype%（％Object％的原型数据属性的初始值），相似的有%Array%（Array构造器），%ArrayPrototype%、%String%、%StringPrototype%、%Function%、%FunctionPrototype%等等的内部方法，可以说全局对象上的属性和方法的值基本都是从[[Intrinsics]]来的（不包括宿主环境提供的属性和方法如：console、location等）。想查看所有的内部方法请查看官方文档内部方法列表。
3. Realms.[[GlobalObject]]和[[GlobalEnv]]一目了然，在浏览器中[[GlobalObject]]就是值window了，node中[[GlobalObject]]就是值global。[[HostDefined]] 值宿主环境提供的附加信息
4. Realms.[[TemplateMap]]

#### Execution Contexts (执行上下文)

1. LexicalEnvironment 标识在此执行上下文中用于解析有代码所做的标识符引用的词法环境
2. VariableEnvironment 标识在此执行上下文中的词法环境，它的环境记录保存了由VariableStatements创建的绑定
3. 当创建执行上下文时，它的LexicalEnvironment和VariableEnvironment组件最初具有相同的值 LexicalEnvironment在运行中可能会改变 

编译器在评估Block做了如下操作：
1. 让oldEnv成为正在运行的执行上下文（running execution context）的LexicalEnvironment
2. 让blockEnv成为一个新的声明性环境，它的外部词法环境引用指向oldEnv
3. 对block中的声明进行实例化
4. 把正在运行的执行上下文（running execution context）的LexicalEnvironment设为blockEnv
5. 让blockValue成为执行block中的代码的结果
6. 把正在运行的执行上下文（running execution context）的LexicalEnvironment设为oldEnv
7. 返回blockValue