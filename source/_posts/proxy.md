---
title: ES6中proxy拦截对象
date: 2018-06-04 11:40:40
tags:
---

![image](http://www.wailian.work/images/2018/06/04/timg.th.jpg)

> Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

<!-- more -->

#### 问题抛出


**定义一个对象，指定对象sex属性不能重写，抛出错误**


- [x] 来看看ES3的写法
    
```
//es3拦截方法
function ProxyEs3() {
    var obj = {
        name:'小明',
        sex:"男"
    };
    this.get = function (key) {
        if(key in obj){
            return obj[key]
        }else{
            throw new ReferenceError('混蛋，没有这个属性,重写！！')
        }
    };
    this.set = function (key,value) {
        if(key==='sex'){
            throw new ReferenceError('该属性只能读，不能重写')
        }else{
            obj[key] = value
        }
    }
}
var es3P1 = new ProxyEs3();
es3P1.set("sex",'女');
console.log(es3P1.get('sex'));
```


- [x] 在试试ES5的写法
    
    
```
//es5拦截方法
var obj = {
    name:'小明'
};
Object.defineProperty(obj,"sex",{
    value:'女',
    writable:false
});
obj.sex = '李四'; //未报错
console.log(obj.sex); //女

```

ES5方法通过定义对象的属性来解决属性只读的效果，但是对不能读写的属性赋值时，控制台不报错.对遍历，对其他操作对象的方法未布置属性

- [x] 来看看ES6终极写法
    
```
//es6拦截方法
const obj = {
    name:'小明',
    sex:'女'
};
const proxy = new Proxy(obj, {
    get(target, prop) {
        if(prop in target){
            return target[prop]
        }else{
            throw new TypeError('没有这个属性');
        }
    },
    set(target,prop,value){
        if (prop === 'sex') {
            throw new TypeError('sex属性只能读');
        }
        obj[prop] = value;
    }
});

proxy.sex = '张三'; //报错  TypeError: sex属性只能读
```

Proxy 支持的拦截操作一览，一共 13 种,列举常用的 


#####  1.get(target, propKey, receiver)：
拦截对象属性的读取
##### 2.set(target, propKey, value, receiver):
拦截对象属性的设置
##### 3.has(target, propKey)：
拦截propKey in proxy的操作，返回一个布尔值
##### 4.deleteProperty(target, propKey)
拦截delete proxy[propKey]的操作，返回一个布尔值。
##### 5.ownKeys(target)：
Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)、for...in循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而Object.keys()的返回结果仅包括目标对象自身的可遍历属性。

##### 6.defineProperty(target, propKey, propDesc)：
拦截Object.defineProperty(proxy, propKey, propDesc）、Object.defineProperties(proxy, propDescs)，返回一个布尔值
##### 7.getPrototypeOf(target)
拦截Object.getPrototypeOf(proxy)，返回一个对象。
。
##### 8.setPrototypeOf(target, proto)：
拦截Object.setPrototypeOf(proxy, proto)，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
##### 9.apply(target, object, args)：
拦截 Proxy 实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。
##### 10.construct(target, args)：
拦截 Proxy 实例作为构造函数调用的操作，比如new proxy(...args)。

### 实例：Web 服务的客户端
Proxy 对象可以拦截目标对象的任意属性，这使得它很合适用来写 Web 服务的客户端。

```
const service = createWebService('http://example.com/data');

service.employees().then(json => {
  const employees = JSON.parse(json);
  // ···
});
```
上面代码新建了一个 Web 服务的接口，这个接口返回各种数据。Proxy 可以拦截这个对象的任意属性，所以不用为每一种数据写一个适配方法，只要写一个 Proxy 拦截就可以了。


```
function createWebService(baseUrl) {
  return new Proxy({}, {
    get(target, propKey, receiver) {
      return () => httpGet(baseUrl+'/' + propKey);
    }
  });
}
```
