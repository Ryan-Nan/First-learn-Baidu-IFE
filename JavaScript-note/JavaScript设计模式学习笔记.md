#JavaScript设计模式学习笔记
##JavaScript设计模式都有哪些?
###单体(singleton)模式
单体是一个用来划分命名空间并将一批相关的属性和方法组织在一起的对象，如果他可以被实例化，那么他只能被实例化一次。

单体模式是javascript里面最基本但也是最有用的模式之一。

特点：

1. 可以来划分命名空间，从而清除全局变量所带来的危险。

2. 利用分支技术来封装浏览器之间的差异。

3. 可以把代码组织的更为一体，便于阅读和维护。

使用单体的方法就是用一个命名空间包含自己的所有代码的全局对象，示例：

    var functionGroup = {  
     name:'Darren',  
     method1:function(){  
     //code  
     },  
     init:function(){  
     //code  
     }  
     } 

或者

    var functionGroup  = new function myGroup(){  
     this.name = 'Darren';  
     this.getName = function(){  
     return this.name  
     }  
     this.method1 = function(){}  
     ...  
     } 


    //单体模式
    var circle = (function(){
    //pravite member!
        var r = 5;
        var pi = 3.1416;//后面用分号
        return{//public member
            getArea:function(){
                return r*r*pi;//访问私有成员不要加this
            },//后面用逗号
            //如果想改变r和pi的值，只能通过设置一个公有的函数来实现
            init:function(setR){
                r = setR;
            }
        }
    })()
    window.onload = function(){
        circle.r = 0;//无法访问私有成员,相当于又为circle创建了一个共有成员r
        alert(circle.getArea());
        circle.init(0);//通过公有的工具函数便可以访问了。
        alert(circle.getArea());
    };

    
###工厂(factory)模式
工厂(Factory)模式：是由一个方法来决定到底要创建哪个类的实例, 而这些实例经常都拥有相同的接口. 这种模式主要用在所实例化的类型在编译期并不能确定， 而是在执行期决定的情况。 说的通俗点，就像公司茶水间的饮料机，要咖啡还是牛奶取决于你按哪个按钮。

工厂就是把成员对象的创建工作转交给一个外部对象，好处在于消除对象之间的耦合(何为耦合?就是相互影响)。通过使用工厂方法而不是new关键字及具体类，可以把所有实例化的代码都集中在一个位置，有助于创建模块化的代码，这才是工厂模式的目的和优势。

非常有名的实例-XHR工厂：

    var XMLHttpFactory = function(){};//这是一个简单工厂模式  
     XMLHttpFactory.createXMLHttp = function(){  
     var XMLHttp = null;  
     if (window.XMLHttpRequest){  
     XMLHttp = new XMLHttpRequest()  
     }else if (window.ActiveXObject){  
     XMLHttp = new ActiveXObject("Microsoft.XMLHTTP")  
     }  
     return XMLHttp;  
     }  
     //XMLHttpFactory.createXMLHttp()这个方法根据当前环境的具体情况返回一个XHR对象。  
     var AjaxHander = function(){  
     var XMLHttp = XMLHttpFactory.createXMLHttp();  
     ...  
     } 
     
