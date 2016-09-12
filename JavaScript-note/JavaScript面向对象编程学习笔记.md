#JavaScript面向对象编程
##封装
###一、 生成实例对象的原始模式
假定我们把猫看成一个对象，它有“名字”和”颜色“两个属性。

    var cat={
        name:'',
        color:''
    }

现在，我们需要根据这个原型对象的模式（schema），生成一个实例对象。

        var cat1 = {}; // 创建一个空对象
        cat1.name = "桐桐"; // 按照原型对象的属性赋值
        cat1.color = "黄白色";
        
        var cat2={};
        cat2.name="lulu";
        cat2.color="black";
　　　　
这就是最简单的***封装***了，把两个属性封装在一个对象里面。但是，这样的写法有两个缺点，一是如果多生成几个实例，写起来就非常麻烦；二是实例与原型之间，没有任何联系。

###二、 原始模式的改进
编写一个函数，解决代码重复问题

        function Cat(name,color) {
        
        return {
            name:name,
            color:color
        }
    } 

上面生成了实例对象，就是等于在调用函数。

        var cat1=Cat("桐桐","黄白色"）；
        var cat2=Cat("lulu"."black");
        
这种方法的问题依然是，cat1和cat2之间没有内在的联系，不能反映出它们是同一个原型对象的实例。

###三、构造函数模式
这种模式主要是解决从原型对象生成实例的问题。

所谓"构造函数"，其实就是一个普通函数，但是内部使用了this变量。对构造函数使用new运算符，就能生成实例，并且this变量会绑定在实例对象上。 

cat原型对象

        function Cat(name,color){
            this.name=name;
            this.color=color;
        }
    
    
下面生成实例

        var cat1= new Cat("桐桐","黄白色");
        var cat2= new Cat("lulu"."black");
        alert(cat1.name); //桐桐
        alert(cat1.color); //黄白色

这时cat1和cat2会自动含有一个constructor属性，指向它们的构造函数。

        alert(cat1.constructor == Cat); //true
        
###四、构造函数模式的问题
存在一个浪费内存的问题。

        function Cat(name,color){

            this.name = name;

            this.color = color;

            this.type = "猫科动物";

            this.eat = function(){alert("吃猫粮");};

        } 

还是采用同样的方法，生成实例：

        var cat1 = new Cat("桐桐","黄白色");

        alert(cat1.type); // 猫科动物

        cat1.eat(); // 吃猫粮
        
表面上好像没什么问题，但是实际上这样做，有一个很大的弊端。那就是对于每一个实例对象，`type`属性和`eat()`方法都是一模一样的内容，每一次生成一个实例，都必须为重复的内容，多占用一些内存。这样既不环保，也缺乏效率。

        alert(cat1.eat == cat2.eat); //false
        为什么判断为假，我猜它们两个对象不一样，所以它们的属性自然被判断不同。
        
###五、 原型（Prototype）模式 
Javascript规定，每一个构造函数都有一个prototype属性，指向另一个对象。这个对象的所有属性和方法，都会被构造函数的实例*继承*。

这意味着，我们可以把那些不变的属性和方法，直接定义在prototype对象上。

        function Cat(name,color){

            this.name = name;
            this.color = color;

        }

        Cat.prototype.type = "猫科动物";

        Cat.prototype.eat = function(){alert("吃猫粮")}; 
        
然后，生成实例。

    　　var cat1 = new Cat("桐桐","黄白色");

    　　alert(cat1.type); // 猫科动物

    　　cat1.eat(); // 吃猫粮

这时所有实例的type属性和eat()方法，其实都是同一个内存地址，指向prototype对象，因此就提高了运行效率。

    　　alert(cat1.eat == cat2.eat); //true

###六、 Prototype模式的验证方法 
####isPrototypeOf()
这个方法用来判断，某个proptotype对象和某个实例之间的关系。

    　　alert(Cat.prototype.isPrototypeOf(cat1)); //true
    　　
####hasOwnProperty()
每个实例对象都有一个hasOwnProperty()方法，用来判断某一个属性到底是本地属性，还是继承自prototype对象的属性。

    　　alert(cat1.hasOwnProperty("name")); // true
    　　alert(cat1.hasOwnProperty("type")); // false

####in运算符
in运算符可以用来判断，某个实例是否含有某个属性，不管是不是本地属性。

        alert("name" in cat1); // true
        alert("type" in cat1); // true
        
in运算符还可以用来遍历某个对象的所有属性。

    　　for(var prop in cat1) { alert("cat1["+prop+"]="+cat1[prop]); }
    　　

##继承
###构造函数的继承
对象之间的"继承"有五种方法。

比如，现在有一个"动物"对象的构造函数。


    　　function Animal(){

    　　　　this.species = "动物";

    　　}

还有一个"猫"对象的构造函数。


    　　function Cat(name,color){

    　　　　this.name = name;

    　　　　this.color = color;

    　　}

怎样才能使"猫"继承"动物"呢？
####一、构造函数绑定
第一种方法也是最简单的方法，使用***call或apply方法***，将父对象的构造函数绑定在子对象上，即在子对象构造函数中加一行：

    　　function Cat(name,color){

    　　　　Animal.apply(this, arguments);

    　　　　this.name = name;

    　　　　this.color = color;

    　　}

    　　var cat1 = new Cat("桐桐","黄色");

    　　alert(cat1.species); // 动物

####二、prototype模式
如果"猫"的prototype对象，指向一个Animal的实例，那么所有"猫"的实例，就能继承Animal了。

    　　Cat.prototype = new Animal();

    　　Cat.prototype.constructor = Cat;//这对象constructor默认指向Animal，所以手动纠正这对象constructor值为Cat。

    　　var cat1 = new Cat("桐桐","黄色");

    　　alert(cat1.species); // 动物

这显然会导致继承链的紊乱（cat1明明是用构造函数Cat生成的），因此我们必须手动纠正，将Cat.prototype对象的constructor值改为Cat。这就是第二行的意思。

####三、 直接继承prototype
将Cat的prototype对象，然后指向Animal的prototype对象，这样就完成了继承。

    　　Cat.prototype = Animal.prototype;

    　　Cat.prototype.constructor = Cat;

    　　var cat1 = new Cat("大毛","黄色");

    　　alert(cat1.species); // 动物

与前一种方法相比，这样做的优点是效率比较高（不用执行和建立Animal的实例了），比较省内存。缺点是 Cat.prototype和Animal.prototype现在指向了同一个对象，那么任何对Cat.prototype的修改，都会反映到Animal.prototype。

        Cat.prototype.constructor = Cat;
        
这样做的优点是效率比较高（不用执行和建立Animal的实例了），比较省内存。缺点是 Cat.prototype和Animal.prototype现在指向了同一个对象，那么任何对Cat.prototype的修改，都会影响到Animal.prototype。

####四、利用空对象作为中介

        var F = function(){}; //空对象

        F.prototype = Animal.prototype;

        Cat.prototype = new F();
        Cat.prototype.constructor = Cat;
        
F是空对象，所以几乎不占内存。这时，修改Cat的prototype对象，就不会影响到Animal的prototype对象。

