---
category: JavaScript
path: '/JavaScript'
title: 'JavaScript变量作用域'
type: '变量作用域'

layout: nil
---

 
js变量作用域可分为："全局变量"和"局部变量"  

"全局变量"：申明在函数之外的变量  

"局部变量"：申明在函数体中的变量，并且只能在当前函数体内访问，如：function(){var a = 0;}  

注：在申明变量是凡是没有var关键字，而直接赋值的变量均为全局变量  

```   function test() {  
        a = 30; 
        var b = 20;
     }
   test();
   console.log("a="+a); //这里很明显，a为全局变量 a=30
   console.log("b="+b);
   //b为局部变量，故在函数test外调用是，提示未定义 Uncaught ReferenceError:b is not defined  ```  

```   var a = 1;
   function test() {  
        console.log("a="+a); //这里a为undefined
/*函数中声明的变量在整个函数中都有定义。如果函数内部有定义变量，即使在定义之前输出但会先执行
后面定义语句，然后判断输出结果，所以说声明的变量在整个函数中都是起作用的。*/
        var a = 2;
     }
   test();//a=undefined```

```   var a; 
   function fun() { 
	a = "global";
      } console.log(a);//输出undefined ```

```   var a; 
   function fun() { 
	a = "global"; 
      } 
	fun();
	console.log(a);//输出 global```

对于上面这两个小例子，它们唯一的区别就是一个执行了fun函数，一个没有执行；

结果就是

var a;console.log(a);//由于a只定义，但没初始化，故输出undefined 

而function fun(){...}中对a进行初始化，初始化操作实在fun函数作用域内，如果不执行fun()那么初始化操作也不会执行

函数域优先于全局域，故局部变量a会覆盖掉全局变量a

```   var a=1;
   function main(){
        var a=2;//局部变量
        console.log(a);//2
    } 
   main();//2
   console.log(a);//1```

javascript没有块级作用域  

```   function test(){
	for(var i = 0 ; i < 3 ; i++){
            //i=0,1,2,最后执行到i=3时退出循环
	}
      console.log(i);//3
   }
   test();//3
相当于
  function test(){
  var i;
  for(i = 0; i < 3; i++){
      //i=0,1,2,最后执行到i=3时退出循环
   }
   console.log(i);//3
}
  test();//3```
