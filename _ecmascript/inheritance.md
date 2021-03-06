---
title: 继承
---

## 原型链

```javascript
function SuperType() {
    this.property = true;
}

SuperType.prototype.getSuperValue = function() {
    return this.property;
};

function SubType() {
    this.subproperty = false;
}

SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function() {
    return this.subproperty;
};

var instance = new SubType();
alert(instance.getSuperValue());
```

缺点：如果引用类型继承自 `SuperType`，那么该引用类型绝对是共享的了。

## 组合继承（原型链 + 构造函数）

```javascript
function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function() {
    alert(this.name);
};

function SubType(name, age) {
    SuperType.call(this, name);
    this.age = age;
}

SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;
SubType.prototype.SayAge = function() {
    alert(this.age);
};

var instance = new SubType("Mike", 29);
```


## 原型式继承

```javascript
function object(o) {
    function F(){}
    F.prototype = o;
    return new F();
}
var person = {
    name: "a",
    friends: ["b", "c", "d"]
}

var person1 = object(person);
person1.friends = ["c"]
```

person1 的原型对象是 person。

注意：操作 `person1.friends[1] = 'aa'` 相当于 `person1.__proto__.friends[1] = 'aa'`，所以 `person.friends[1] === 'aa'`。

ES5 中的 `Object.create()` 与 `object()` 相似。

## 寄生组合式继承

```javascript
function inheritPrototype(subType, superType) {
    subType.prototype = Object.create(superType.prototype);
    subType.prototype.constructor = subType;
}

function SuperType(name) {
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function() {
    alert(this.name);
};

function SubType(name, age) {
    SuperType.call(this, name);
    this.age = age;
}

inheritPrototype(SubType, SuperType);

SubType.prototype.sayAge = function() {
    alert(this.age);
};
```

优点：

1. 避免调用两次 SuperType
1. 能正常使用 `instanceof` 和 `isPrototypeOf()`

## Class Extends

[Class](ecmascript/class/#extends)
