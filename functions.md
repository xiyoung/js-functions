> bind

```javascript
Function.prototype.mybind = function (context) {
	if(typeof this !== 'function') {
		throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
	}
	const self = this; // 这里的this指向绑定的函数
	const args = Array.prototype.slice.call(arguments, 1); // 获取绑定函数时的参数
	const bFunc = function () {
        // 如果把绑定后的函数当作构造函数，这里的this指向构造出来的新对象
		return self.apply(this instanceof bFunc ? this : context, args.concat([...arguments]));
	}
    // 使绑定后的函数的原型指向被绑定的函数的原型 之所以不用bFunc.prototype = self.prototype, 防止改变绑定后的函数的prototype影响到被绑定的prototype
	function Bson () {

	}
	Bson.prototype = self.prototype;
	bFunc.prototype = new Bson();
	return bFunc;
}
```

> apply

```javascript
Function.prototype.myApply = function (context) {
  var context = context || window
  context.fn = this

  var result
  // 需要判断是否存储第二个参数
  // 如果存在，就将第二个参数展开
  if (arguments[1]) {
    result = context.fn(...arguments[1])
  } else {
    result = context.fn()
  }

  delete context.fn
  return result
}
```

> call

```javascript
Function.prototype.myCall = function (context) {
  var context = context || window
  context.fn = this
  var args = [...arguments].slice(1)
  var result = context.fn(...args)
  // 删除 fn
  delete context.fn
  return result
}
```

> instanceof

```javascript
function instanceof(left, right) {
	const prototype = right.prototype;
	left = left.__proto__;
	while(true) {
		if(left === null) {
			return false
		} else if(left === prototype){
			return true
		}
		left = left.__proto__
	}
}
```

