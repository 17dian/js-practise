
# 44道JS难题!

#### 1. ["1", "2", "3"].map(parseInt)
    答案：[1, NaN, NaN]
    解析：parseInt (val, radix) ：两个参数，val值，radix基数（就是多少进制转换）
         map 能传进回调函数 3参数 (element, index, array)
         parseInt('1', 0);  //0代表10进制
         parseInt('2', 1);  //没有1进制，不合法
         parseInt('3', 2);  //2进制根本不会有3
    巩固：["1", "1", "11","5"].map(parseInt) //[1, NaN, 3, NaN]  
#### 2. [typeof null, null instanceof Object]
    答案：["object", false]
    解析：null代表空对象指针，所以typeof判断成一个对象。可以说JS设计上的一个BUG
         instanceof 实际上判断的是对象上构造函数，null是空当然不可能有构造函数
    巩固：null == undefined //true    null === undefined //flase
#### 3. [ [3,2,1].reduce(Math.pow), [].reduce(Math.pow) ]
    答案：an error
    解析：Math.pow (x , y)  x 的 y 次幂的值
         reduce（fn,total）
         fn (total, currentValue, currentIndex, arr) 
             如果一个函数不传初始值，数组第一个组默认为初始值.
             [3,2,1].reduce(Math.pow)
             Math.pow(3,2) //9
             Math.pow(9,1) //9
         [].reduce(Math.pow)      //空数组会报TypeError
    巩固：[1].reduce(Math.pow)     //只有初始值就不会执行回调函数，直接返回1
         [2].reduce(Math.pow,3)  //传入初始值，执行回调函数，返回9
#### 4. 
     var val = 'smtg';
     console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');

这段代码的执行结果？ 
    
    答案：Something
    解析：字符串连接比三元运算有更高的优先级 
         所以原题等价于 'Value is true' ? 'Somthing' : 'Nonthing' 
         而不是 'Value   is' + (true ? 'Something' : 'Nonthing')
    巩固：0 && null === 0  //0    === 优先级高于 &&
#### 5. 
    var name = 'World!';
    (function () {
    if (typeof name === 'undefined') {
        var name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
    })();

这段代码的执行结果？ 
    
    答案：Goodbye Jack
    解析：（1）typeof时var name 在函数内部之声明未定义
         （2）typeof优先级高于===
    巩固：
        var str = 'World!';   
        (function (name) {
        if (typeof name === 'undefined') {
            var name = 'Jack';
            console.log('Goodbye ' + name);
        } else {
            console.log('Hello ' + name);
        }
        })(str);
        答案：Hello World 因为name已经变成函数内局部变量
#### 6.               
    var END = Math.pow(2, 53);
    var START = END - 100;
    var count = 0;
    for (var i = START; i <= END; i++) {
        count++;
    }
    console.log(count);

这段代码的执行结果？ 
    
    答案：other ,不是101
    解析：js中可以表示的最大整数不是2的53次方，而是1.7976931348623157e+308。2的53次方不是js能表示的最大整数而应该是能正确计算且不失精度的最大整数，
    巩固：
    var END = 1234567635;
    var START = END - 1024;
    var c = count = 0;
    for (var i = START; i <= END; i++) {
        c = count++;
    }
    console.log(count);   //1025
    console.log(c);       //1024
#### 7.         
    var ary = [0,1,2];
    ary[10] = 10;
    ary.filter(function(x) { return x === undefined;}); 
这段代码的执行结果？ 

    答案：[]
    解析：filter() 不会对空数组进行检测。会跳过那些空元素
    巩固：
          var ary = [0,1,2,undefined,undefined,undefined,null];
          ary.filter(function(x) { return x === undefined;});
          // [undefined, undefined, undefined] 
#### 8.                 
    var two   = 0.2
    var one   = 0.1
    var eight = 0.8
    var six   = 0.6
    [two - one == one, eight - six == two]
这段代码的执行结果？ 

    答案：[true, false]
    解析：IEEE 754标准中的浮点数并不能精确地表达小数
    巩固：var two   = 0.2;
         var one   = 0.1;
         var eight = 0.8;
         var six   = 0.6;
         ( eight - six ).toFixed(4) == two 
         //true
#### 9.  
    function showCase(value) {
        switch(value) {
        case 'A':
            console.log('Case A');
            break;
        case 'B':
            console.log('Case B');
            break;
        case undefined:
            console.log('undefined');
            break;
        default:
            console.log('Do not know!');
        }
    }
    showCase(new String('A'));
这段代码的执行结果？ 

    答案：Do not know!
    解析：new String(x)是个对象
    巩固：下一题
#### 10.
    function showCase2(value) {
        switch(value) {
        case 'A':
            console.log('Case A');
            break;
        case 'B':
            console.log('Case B');
            break;
        case undefined:
            console.log('undefined');
            break;
        default:
            console.log('Do not know!');
        }
    }
    showCase2(String('A'));
这段代码的执行结果？ 

    答案：Case A
    解析：String('A')就是返回一个字符串
#### 11.
    function isOdd(num) {
        return num % 2 == 1;
    }
    function isEven(num) {
        return num % 2 == 0;
    }
    function isSane(num) {
        return isEven(num) || isOdd(num);
    }
    var values = [7, 4, '13', -9, Infinity];
    values.map(isSane);
这段代码的执行结果？ 

    答案：[true, true, true, false, false]
    解析：%如果不是数值会调用Number（）去转化
         '13' % 2       // 1
          Infinity % 2  //NaN  Infinity 是无穷大
          -9 % 2        // -1
    巩固： 9 % -2        // 1   余数的正负号随第一个操作数
#### 12.
    parseInt(3, 8)
    parseInt(3, 2)
    parseInt(3, 0)
这段代码的执行结果？ 

    答案：3  NaN  3
    解析：2进制不可能有3
#### 13.
    Array.isArray( Array.prototype )
这段代码的执行结果？ 

    答案：true
    解析：Array.prototype是一个数组
#### 14.
    var a = [0];
    if ([0]) {
      console.log(a == true);
    } else {
      console.log("wut");
    }
这段代码的执行结果？ 

    答案：false
    解析：[0]的boolean值是true
    巩固：a[0] 的boolean是 false
#### 15.[]==[]
    答案：false
    解析：两个引用类型， ==比较的是引用地址
    巩固：[]== ![] 
         (1)! 的优先级高于== ，右边运算结果等于 false
         (2)一个引用类型和一个值去比较 把引用类型转化成值类型，左边0
         (3)所以 0 == false  答案是true
#### 16.        
    '5' + 3
    '5' - 3
  这段代码的执行结果？  

    答案：53  2
    解析：加号有拼接功能，减号就是逻辑运算
#### 17.  1 + - + + + - + 1      
    答案：2
    解析：+-又代表正负号， 负负得正。

#### 18. 
    var ary = Array(3);
    ary[0]=2
    ary.map(function(elem) { return '1'; });
这段代码的执行结果？ 

    答案：["1", empty × 2]
    解析：如过没有值，map会跳过不会执行回调函数
#### 19.
    function sidEffecting(ary) {
      ary[0] = ary[2];
    }
    function bar(a,b,c) {
      c = 10
      sidEffecting(arguments);
      return a + b + c;
    }
    bar(1,1,1) 
这段代码的执行结果？ 

    答案：21, 
    解析：

#### 20.
    var a = 111111111111111110000,
    b = 1111;
    a + b;
这段代码的执行结果？  

    答案：11111111111111111000
    解析：在JavaScript中number类型在JavaScript中以64位（8byte）来存储。这64位中有符号位1位、指数位11位、实数位52位。2的53次方时，是最大值。其值为：9007199254740992（0x20000000000000）。超过这个值的话，运算的结果就会不对.
#### 21.
    var x = [].reverse;
    x();
 这段代码的执行结果？ 
    答案：window
    解析：但在chrome上执行报错，没太懂
#### 22.Number.MIN_VALUE > 0
    答案：true
    解析：MIN_VALUE 属性是 JavaScript 中可表示的最小的数（接近 0 ，但不是负数）。它的近似值为 5 x 10-324。
#### 23.[1 < 2 < 3, 3 < 2 < 1]
    答案：[true,true]
    解析： 1 < 2    =>  true;
          true < 3 =>  1 < 3 => true;
          
          3 < 2     => false;
          false < 1 => 0 < 1 => true;
#### 24.2 == [[[2]]]
    答案：true
    解析：值和引用类型去比较,把引用类型转话成值类型
         [[[2]]]）//2

