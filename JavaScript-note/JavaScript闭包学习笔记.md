#学习JavaScript闭包 

难点: 高； 重点：重中之重。
##闭包（closure）的概念

闭包就是能够读取其他函数内部的变量的函数。

本质上，闭包就是将外部函数和内部函数连接起来的一座桥梁。

应用闭包两种情况：第一，函数作为返回值；第二，函数作为参数传递。

##闭包的作用
它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。

还有网友给出了ppt，下面是这样的

闭包的作用

  1.setTimeout/setInterval
  
  2.回调函数（callback）
  
  3.事件句柄（event handle）

看下面的代码：

        function fn(){
            var a=10;
            add=function(){console.log(a+=1;)};
            function fn1(){
                console.log(a);
            }
            return fn1;
        }
        
        var result=fn();
        result(); //10
        add();
        result(); //11
        
在这段代码中，result实际上就是fn1闭包函数，它一共运行了两次，第一次的值是10，第二次的值是11；这证明了fn函数里的局部变量a一直保存在内存中，并没有fn函数调用后自动被销毁。

为什么？因为fn是fn1的父函数，fn1被赋值给了一个全局变量，这导致fn1始终在内存中。fn1的存在依赖于fn，因此fn也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。

另一个值得注意的地方，就是"add=function(){a+=1}"这一行，首先在add前面没有使用var关键字，因此add是一个全局变量，而不是局部变量。其次，add的值是一个匿名函数（anonymous   function），而这个匿名函数本身也是一个闭包，所以add相当于是一个setter，可以在函数外部对函数内部的局部变量进行操作。
