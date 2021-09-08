# ES6Learning


# ECMA Script6  学习记录

## 继承

### 1.原型链继承

```js
function Parent() {
    this.name = 'Michael'
}
Parent.prototype.getName = function() {
    console.log(this.name)
}
function Child() {
    
}
Child.prototype = new Parent()
var child1 = new Child()
child1.getName() // 'Michael'
```

缺陷：

- 引用类型的属性被所有实例共享

  ```js
  function Parent() {
  	this.names = ['Michael', 'Milk+']
  }
  function Child() {
  	
  }
  Child.prototype = new Parent()
  var child1 = new Child()
  child1.names.push('Lily')
  console.log(child1.names) // ['Michael', 'Milk+', 'Lily']
  var child2 = new Child()
  console.log(child2.names) // ['Michael', 'Milk+', 'Lily']
  ```

- 在创建Child实例时不能向Parent传参

### 2.借用构造函数（经典继承）

```js
function Parent() {
	this.names = ['Michael', 'Milk+']
}
function Child() {
	Parent.call(this)
}
var child1 = new Child()
child1.names.push('Lily')
console.log(child1.names) // ['Michael', 'Milk+', 'Lily']
var child2 = new Child()
console.log(child2.names) // ['Michael', 'Milk+']
```

优点：

- 避免了引用类型的属性被所有实例共享

- 可以在Child中向Parent传参

  ```js
  function Parent (name) {
      this.name = name
  }
  function Child (name) {
      Parent.call(this, name)
  }
  var child1 = new Child('kevin')
  console.log(child1.name);// kevin
  var child2 = new Child('daisy')
  console.log(child2.name) // daisy
  ```

缺点：

- 方法都在构造函数中定义，每次创建实例都会创建一遍方法

### 3.组合继承

```js
function Parent() {
    this.name = name
    this.colors = ['black', 'pink']
}
Parent.prototype.getName = function() {
    console.log(this.name)
}
function Child(name, age) {
    Parent.call(this, name)
    this.age = age
}
Child.prototype = new Parent()
Child.prototype.constructor = Child
var child1 = new Child('Lily', 18)
child1.colors.push('blue')
console.log(child1.name) // Lily
console.log(child1.age) // 18
console.log(child1.colors) // ['black', 'pink', 'blue']
var child2 = new Child('Mike', 22)
console.log(child2.name) // Mike
console.log(child2.age) // 22
console.log(child2.colors) // ['black', 'pink']
```

优点：

- 融合了原型链继承和经典继承的优点，是JavaScript中最常用的继承方式

缺点：

- 调用了两次父构造函数

### 4.原型式继承

```js
function createObj(o) {
	function F() {}
	F.prototype = o
	return new F()
}
```

缺点：

- 和原型链继承一样，包含引用类型的属性值始终都会共享相应的值

  ```js
  var person = {
  	name: 'Mike',
  	friends: ['Lily', 'Daisy']
  }
  var person1 = createObj(person)
  var person2 = createObj(person)
  person1.name = 'person1'
  console.log(person2.name) // Mike
  person1.friends.push('Cole')
  console.log(person2.friends) // ['Lily', 'Daisy', 'Cole']
  ```

  此处修改person1.name的值，可以看到person2.name并未改变，并非因为二者有独立的name值，而是因为是给person1添加了name值，而非修改原型上的name值

### 5.寄生式继承

创建一个仅用于封装继承过程的函数，该函数在内部以某种形式来做增强对象，最后返回对象

```js
function createObj(o) {
	var clone = Object.create(o)
	clone.sayName = function() {
		console.log('hi')
	}
	return clone
}
```

缺点：

- 和经典继承一样，每次创建对象都会创建一次方法

### 6.寄生组合式继承

**引用类型最理想的继承范式**

