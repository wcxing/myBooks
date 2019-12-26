

#### 二、
1. 对象、数组是引用类型
2. 参数在方法内，相当于一个局部变量(参数传递是值复制)

```js
// 1. 
var a = [1, 2, 3];
function fn() {
    a.[3] = 4;
    a = [10];
}
fn();
console.log(a);

// 2.
var a = [1, 2, 3];
function fn(b) {
    b.[3] = 4;
    b = [10];
}
fn(a);
console.log(a);
// [1, 2, 3, 4]
// 参数传递是值复制，若参数类型为引用类型则复制指针，fn(a) 中的参数复制了一个与a相同的指针
// b.[3] = 4 ，这一步中的 b实际是一个指针， 指向了与a相同的内存地址（等同于操作 a[3] = 4）
// b = [10] 这一步改变了b的指针指向，指向了其他内存地址
// 经历过以上两步，a 指针指向的内存已变为 [1, 2, 3, 4], 而 b 是一个局部变量，指向的内存地址值为 [10]

// 3.
var a = 123;
function fn(a) {
    var a;
    console.log(a);
} 
fn(a);
// 打印结果： 123

// 预编译过程：
// NO1. 创建VO（variable object）开辟一个存储空间
// NO2. 找形参和变量声明，将其作为VO属性名，值为undefined。其实VO里还有   arguments: {}和this: window
    VO: {
        arguments: {},
        this: window || gloable,
        a: undefined
    }
// NO3. 将实参传给形参
    VO: {
        a: 123
    }
// NO4. 在函数体里找函数声明，值为函数体

// 对于全局，在预编译时先产生global object，GO。
    GO: {
            // 没有arguments
            this: window,
            window: {},
            ...
        }

```

**js 数组并不是数据结构意义上的数组，为什么？**
1. 数据结构意义上的数组是连续相等内存变量，大小、类型；
2. 真的数组是不可以扩容的；js 数组可以随便添加元素。

```js
var a = {n:1};
var b = a;
// a.x 注意  . 号运算优先级最高， 连等 从右往左运算； 
a.x = a = {n:2};

console.log('a.x', a.x); // undefined
console.log('b.x', b.x); // {n: 2}
```