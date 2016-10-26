---
title: es6中一些比较有用特性
date: 2016-10-26 11:30:00
tags:
---

#### 1. 函数方法中可以设置参数默认值

```
function(a = 1,b = 2)
{
    return a + b;
}
```

#### 2. 字符串模版对象
```
var p1 = '123';
var p2 = '345';
var str = `this is ${p1} ${p2}`;
```

#### 3. 解构对象
```
var data = {
    a : 1,
    b : 2
}

var {a,b} = data;
console.log(a);
console.log(b);
```

#### 4. 箭头函数
```
var t = a => console.log(a);
t(1);
```

#### 5. 类（es6中类的声明方式非常的oo）
```
//定义
class baseModel{
    constructor(data){
        this.data = data;
    }
    getData(){
        console.log(this.data);
    }
}

//继承
class childModel extends baseModel{
    constructor(data){
        super(data);
    }
}

//使用
var b = new baseModel(1);

var c = new childModel();
```

#### 6. 导出导入
```
//导出属性
export var a = 1;

//导出方法
export {getData,getData11};
function getData()
{
}

function getData1()
{
}

//导出类
export default class baseModel{
}

//导入方法，属性
import {a} from 'xxx';
import {getData} from 'xxx';

//导入类
import baseModel from 'xxx';
```


