**js中函数可以作为参数传递**

- 匿名函数：直接使用的函数，只使用一次。
```javascript
function say (value) {
    alert(value);
}
// 注意看下面,直接写say方法的方法名与下面的匿名函数可以认为是一个东西
// 这样再看上面两段代码是不是对函数可以作为参数传递就更加清晰了
say;

function (value) {
    alert(value);
}

// 这里的say或者匿名函数就可以被称为回调函数
```


# 回调函数
回调函数，或简称回调（call），是指通过函数参数传递到其它代码的，某一块可执行代码的引用。这一设计允许了底层代码调用在高层定义的子程序。

==回调函数就是一个函数，它是在我们启动一个异步任务的时候就告诉它：等你完成了这个任务之后要干什么。这样一来主线程几乎不用关心异步任务的状态了，他自己会善始善终。==

假设一个场景：

老师给学生布置了作业，学生收到作业后开始写作业，写完之后通知老师查看，老师查看之后就可以回家。
回调的概念，在这里面就体现的淋漓尽致，在这里面有两个角色，一个是老师，一个是学生。老师有两个动作，第一个是布置作业，第二个是查看作业。而学生有一个动作是做作业， 那么问题来了，老师并不知道学生何时才能做完作业，所以比较优雅的解决办法是等学生的通知，也就是学生做完之后告诉老师就可以。这就是典型的回调理念。

那么在编程中，该如何体现？ 从上面的分析中，可以得出来回调模式是双方互通的，老师给学生布置作业，学生做完通知老师查看作业。 关于回调，这里面还分同步回调和异步回调两种模式：
- 同步模式：
	如果老师在放学后，给学生布置作业，然后一直等待学生完成后，才能回家，那么这种方法就是同步模式。
- 异步模式：
	如果老师在放学后，给学生布置作业，这个时候老师并不想等待学生完成，而是直接就回家了，但告诉学生，如果完成之后发[短信](https://cloud.tencent.com/product/sms?from=10680)通知自己查看。这种方式就是异步的回调模式。



## 若回调函数需要传参，有两种方法
1. 将回调函数的参数作为与回调函数同等级的参数进行传递
![[Pasted image 20220419121025.png]]
2.  回调函数的参数在调用回调函数内部创建
![[Pasted image 20220419121045.png]]


# 箭头函数
箭头函数表达式更适用于那些本来需要匿名函数的地方，并且它不能用作构造函数。

## 基础语法
```TypeScript
(param1, param2, …, paramN) => { statements }
(param1, param2, …, paramN) => expression
//相当于：(param1, param2, …, paramN) =>{ return expression; }

// 当只有一个参数时，**圆括号**是可选的：
(singleParam) => { statements }
singleParam => { statements }

// 没有参数的函数应该写成一对圆括号。
() => { statements }
```

## 高级语法
```typescript
//加括号的函数体返回对象字面量表达式：
params => ({foo: bar})

//支持剩余参数和默认参数
(param1, param2, ...rest) => { statements }
(param1 = defaultValue1, param2, …, paramN = defaultValueN) => {
statements }

//同样支持参数列表解构
let f = ([a, b] = [1, 2], {x: c} = {x: a + b}) => a + b + c;
f();  // 6

```


```typescript
import type { ProFormInstance } from '@ant-design/pro-form';
const formRef = useRef<ProFormInstance>();
const onFill = () => {

    formRef?.current?.setFieldsValue({

      name: '张三',

      company: '蚂蚁金服',

    });

  };
<Button htmlType="button" onClick={onFill} key="edit">

```