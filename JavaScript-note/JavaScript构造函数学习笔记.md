# JavaScript构造函数(constructor)
##构造函数概念
###什么是构造函数？
构造函数 ，是一种特殊的方法。主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，总与new运算符一起使用在创建对象的语句中。

看下面一段代码和图便清楚

    // 构造函数
    function Foo(y) {
    // 构造函数将会以特定模式创建对象：被创建的对象都会有"y"属性
        this.y = y;
    }
 
    // "Foo.prototype"存放了新建对象的原型引用
    // 所以我们可以将之用于定义继承和共享属性或方法
    // 所以，和上例一样，我们有了如下代码：
    
    Foo.prototype.x = 10;  // 继承属性"x"
 
    // 继承方法"calculate"
    Foo.prototype.calculate = function (z) {
        return this.x + this.y + z;
    };
 
    // 使用foo模式创建 "b" and "c"
    var b = new Foo(20);
    var c = new Foo(30);
 
    // 调用继承的方法
    b.calculate(30); // 60
    c.calculate(40); // 80
    // 让我们看看是否使用了预期的属性
    console.log(
        b.__proto__ === Foo.prototype, // true
        c.__proto__ === Foo.prototype, // true
        
      // "Foo.prototype"自动创建了一个特殊的属性"constructor"
      // 指向Foo的构造函数本身
      // 实例"b"和"c"可以通过授权找到它并用以检测自己的构造函数
 
        b.constructor === Foo, // true
        c.constructor === Foo, // true
        Foo.prototype.constructor === Foo // true
 
        b.calculate === b.__proto__.calculate, // true
        b.__proto__.calculate === Foo.prototype.calculate // true
 
    );

上述代码可表示为如下的关系：
![constructor](images/constructor.png)

