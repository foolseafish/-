1. 简要说明js原型链？
原型链存储对象的属性和方法，并且可以被原型继承。new 构造函数创建出的对象可以引用构造函数原型链上的属性和方法，且生成对象的隐式原型指向构造函数的原型对象。对象的隐式原型都会指向Object.prototype，并最终指向null。构造函数的隐式原型最终都指向Function.prototype。
2. 简要说明js不同情况下this指针指向？
区别于语言类型，js还有一种内部的规范类型，规定this指针的是Renference类型。在执行函数表达式处理this指针时，会获取成员表达式匹配，并基于以下判断：
1.isRef(ref)
    1.是，getBase(ref) && IsPropertyReference(ref) (getBase(ref) 为Object, String, Number, Boolean)
        1.是，this = getBase(ref)
        2.否，this = ImplicitThisValue(ref)
    2.否，this = undefined

var value = 1;
var foo = {
  value: 2,
  bar: function () {
    return this.value;
  }
}
function bar () {console.log(this)}
//示例1
console.log(foo.bar()); // 2
//示例2
console.log((foo.bar)()); // 2
//示例3
console.log((foo.bar = foo.bar)()); // 1 赋值表达式返回getValue(ref)。成员表达式为非reference类型，this为undefined
//示例4
console.log((false || foo.bar)()); // 1 逻辑运算返回同上
//示例5
console.log((foo.bar, foo.bar)()); // 1 逗号运算返回同上
//示例6
console.log(bar()); // 1 getBase()为EnvironmentRecord，IsPropertyReference(ref)为false, this 等于 ImplicitThisValue()等于undefined。非严格模式this指向window
3.简要说明js函数作用域和作用域链？
作用域： js函数是静态作用域，js在执行可执行代码前会提升函数的创建，初始化和赋值。并将当前的活动对象作为函数的作用域赋值给函数。
作用域链： 函数在执行时会先创建执行上下文，执行上下文会提取Arguments对象，并创建AO,提升函数的创建初始化和赋值，提升变量的创建，初始化（var）。
4.简要说明js闭包？出一道闭包题考一下自己。
闭包 = 函数 + 其能引用的自由变量，理论上所有函数都是闭包，实践上指的是函数内部引用了自由变量且尽管创建函数的上下文已经销毁但是函数仍然存在。

5.函数的执行上下文？
6.bind,call和apply实现？
Function.prototype.call = function(context){
    context = context === undefined || context === null ? window: Object(context);
    context.fn = this;
    var args = [];
    for(let i = 1; i < arguments.length; i++) {
        args.push('arguments[' + i + ']');
    }
    var result = eval('context.fn('+args+')');
    delete context.fn;
    return result;
}

Function.prototype.apply = function(context, arr){
    context = context || window;
    context = Object(context);
    context.fn = this;
    var args = [];
    for(let i = 0; arr && i < arr.length; i++) {
        args.push('arr[' + i + ']');
    }
    var result = eval('context.fn('+args+')');
    delete context.fn;
    return result;
}

Function.prototype.bind = function(context){
    var bindArgs = Array.prototype.slice.call(arguments, 1);
    var _this =this;
    var bindFunc = function(){
        var args = Array.prototype.slice.call(arguments);
        return _this.apply(context instanceof bindFunc? this: context, bindArgs.concat(args));
    }
    bindFunc.prototype = Object.create(this.prototype);
    return bindFunc
}

7.new实现？
1.创建新对象2.隐式原型3.构造函数this变更执行4.返回值判断(函数和对象其本身，其他新对象)

functionNew = function(Ctor){
    var args = Array.prototype.slice.call(arguments, 1);
    var obj = {};
    obj.__proto__ = Ctor.prototype;
    var result = Ctor.apply(obj, args);

    return (typeof result === 'object' && Object.prototype.toString.call(result) !== '[object Null]') //对象及其衍生对象
    || typeof result === 'function'// (函数及其衍生函数)
    ? result: obj;
}
8.继承实现方式和优缺点？
    1.原型继承
        优点：原型链属性和方法共享
        缺点：构造函数传参和子类变更父类数据不容易
    2.构造函数继承
        跟上面反一下
    3.组合继承
        优点：原型链属性和方法共享， 构造函数传参等方便
        缺点：构造函数执行两遍
    4.寄生组合继承
        优点： 原型链属性和方法共享， 构造函数传参等方便，构造函数执行一遍
        过程： 原型链使用寄生原型继承，构造函数执行使用构造函数继承。
9. let & const?
let,const 会在预处理阶段提升进行创建，初始化发生在申明处语句。在for循环内部使用let或const会在内部新建一个新的词法环境（作用域链）和环境记录（作用域）。并初始化变量对象并赋值为for循环内申明定义的变量。
10. generator和co实现？
11. async实现？
5.什么是函数式编程？
    1. 输出只依赖输入，易维护易测试
    2. 要求数据不可突变



