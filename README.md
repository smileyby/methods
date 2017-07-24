Methods
=======

## Array

### array.concat(item...)

**item**：需要与原数组合并的数组或非数组值

**返回值**：新的Array实例

concat方法返回一个新数组，它包含array的浅复制（shallow copy）并将1个或多个参数item附加在其后。如果参数item是一个数组，那么它的每一个元素会被分别添加。

concat()方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

```js

var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var d = a.concat(b, true);
// c 是 ['a' , 'b', 'c', 'x', 'y', 'z', true]

```

### array.join(separator)

**separtor**：指定一个字符串来分隔数组的每个元素

**返回值**：一个所有数组元素连接的字符串。如果arr.length为0，则返回空字符串

join 方法把一个array构造成一个字符串。它将array中的每个元素构造成一个字符串并用一个sperator为分隔符把他们连接在一起。默认的seprator是','。为了实现无间隔的连接，我们可以使用空字符串作为seprator。

如果你把大量的片段拼装成一个字符串，把这些片段放到一个数组中并用join方法连接他们通常比用+元素连接运算符连接这些片段要快。

**join()方法，不会改变数组！**

```js

var a = ['a', 'b', 'c'];
a.push('d');
var c = a.join('');
// c 是 'abcd'

```

### array.pop()

**返回值**：从数组中删除的元素，undefined如果数组为空。

**此方法更改数组的长度**

pop和push方法使数组array像堆栈（stack）一样工作。pop方法移除array中的最后一个元素并返回该元素。如果array是空的，他会返回undefined。

```js

var a = ['a', 'b', 'c'];
var c = a.pop();
// a 是 ['a', 'b'] & c 是 'c'

// pop可以像这样实现：
Array.method('pop', function(){
	return this.splice(this.length - 1, 1)[0];
});

```

### array.push(item...)

**返回值**：当调用该方法时，新的length属性将被返回

push方法将一个或多个参数item附加到一个数组的尾部。不像concat方法那样，它会修改该数组array，如果参数item是一个数组，他会将参数数组作为单个元素整个添加到数组中。它返回这个数组array的新长度。

```js

var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.push(b, true);
// a 是 ['a', 'b', 'c', ['x', 'y', 'z'], true]
// c 是 5;

// push可以像这样实现：
Array.method('push', function(){
	this.splice.apply(
		this,
		[this.length, 0].concat(Array.prototype.slice.apply(arguments))
	);
	return this.length;
});

// 还可以用来合并两个数组
var arr1 = [1, 2, 3];
var arr2 = ['x', 'y', 'z'];
// 将第二个数组融合进第一个数组
// 相当于 arr1.push('x', 'y', 'z')
Array.prototype.push(arr1, arr2);
console.log(arr1);
// [1, 2, 3, 'x', 'y', 'z']

```

### array.reverse()

**返回值**：返回当前的array

reverse方法翻转array中的元素顺序。第一个数组元素成为最后一个数组元素，最后一个数组元素成为第一个。

```js

var a = ['a', 'b', 'c'];
var b = a.reverse();
// a 和 b 都是 ['c', 'b', 'a']

```

### array.shift()

shift方法移除数组array中的第一个元素并返回该元素。如果这个数组array是空的，它会返回undefined。**shift通常比pop慢得多**：

```js

var a = ['a', 'b', 'c'];
var c = a.shift(); // a 是 ['b', 'c'] & c 是 'a'

```

shift可以这样实现：

```js

Array.method('shift', function(){
	return this.splice(0, 1)[0];
});

```

### array.slice(start, end)

**返回值**：一个含有提取元素的**新数组**

start 下标应小于 end，否则截取返回值为空数组 

slice方法对array中的一段做浅复制。第一个被复制的元素array[start]。它将一直复制到array[end]为止。end参数可选，并且默认值是该数组的长度array.length。如果像个参数的任何一个是负数，array.length将和它们相加来试图使他们称为非负数。如果start大于等于array.length，得到的结果将是一个新的空数组。**千万别把slice和splice混淆了**。

```js

var a = ['a', 'b', 'c'];
var b = a.slice(0, 1);  // b 是 ['a']
var c = a.slice(1);     // c 是 ['b', 'c']
var d = a.slice(1, 2);  // d 是 ['b']
var e = a.slice(2,1);   // e 是 []
var f = a.slice(-2,-1) // f 是 ['b']

```

### array.sort(comparefn)

[冒泡排序法](http://www.cnblogs.com/kkun/archive/2011/11/23/2260280.html)

**注意：**[对于sort具体实现方法，不同浏览器是存在差异的](https://stackoverflow.com/questions/234683/javascript-array-sort-implementation)


sort方法对array中的内容进行适当的排序。它不能正确地给出一组数字排序：

```js

var n = [4, 8, 15, 16, 23, 42];
n.sort();
// n 是 [15, 16, 23, 4, 42, 8]

n.sort(function(a,b){
	return a-b; // 升序
	// return b-a; 降序
})；

```

javascript的默认比较函数假定所有要排序的元素都是字符串。它尚未足够只能到在比较这些元素之前先检测它们的类型，所有当它比较这些数字的时候会将它们转化为字符串，导致得到一个令人吃惊的错误结果。

幸运的是，你可以使用自己的比较函数来替换默认的比较函数。你的比较函数应该接受两个参数，并且如果这两个参数相等则返回0，如果第一个参数应该排列在前面，则返回一个负数，如果第二个参数应该排列在前面，则返回一个正数。

```js

n.sort(function (a, b) {
	return a - b;
});
// n 是 [4, 8, 15, 16, 23, 42]

```

上面这个函数将给数字排序，但它不能给字符串排序。如果我们想要给任何简单值数组排序，则必须要做更多的工作：

```js

var m = ['aa', 'bb', 'a', 4, 8, 15, 16, 23, 42];
m.sort(function(a, b){
	if(a === b){
		return 0;
	}
	if(typeof a === typeof b){
		return a < b ? -1 : 1; 
	}
	return typeof a < typeof b ? -1 : 1;
});
// m 是 [4, 8, 15, 16, 23, 42, 'a', 'aa', 'bb']

```
如果大小写不重要，你的比较函数应该在比较运算符之前将他们转化为小写。

如果有一个更智能的比较函数，我们也可以给对象排序。为了在一般情况下让这个事情更容易，我们将编写一个构造比较函数的函数：

```js

var by = function (name) {
	return function (o, p) {
		if (typeof o === 'object' && typeof p === 'object' && o && p){
			a = o[name];
			b = p[name];
			if (a === b){
				return 0;
			}
			if (typeof a === typeof b){
				return a < b ? -1 : 1;
			}
		} else {
			throw {
				name: 'Error',
				message: 'Expected an object when sorting by ' + name
			};		
		}
	};
};

var s = [
	{first: 'Joe', last: 'Besser'},
	{first: 'Moe', last: 'Howard'},
	{first: 'Joe', last: 'Derita'},
	{first: 'Shemp', last: 'Howard'},
	{first: 'Larry', last: 'Fine'},
	{first: 'Curly', last: 'Howard'}
];
s.sort(by('first'));
// s 是 [
	{first: 'Curly', last: 'Howard'},
	{first: 'Joe', last: 'Derita'},
	{first: 'Joe', last: 'Besser'},
	{first: 'Larry', last: 'Fine'},
	{first: 'Moe', last: 'Howard'},
	{first: 'Shemp', last: 'Howard'}
]

```

sort 方法是不稳定的，所以下面的调用：

```

s.sort(by('first')).sort(by('last'));

```

不能保证产生正常的序列。如果你想基于多个键值进行排序，你需要再次做更多的工作我们可以修改by函数，让其可以接受第二个参数，当主要的键值产生一个匹配的时候，另一个compare方法将被调用以决出高下。

```js

var by = function (name, minor) {
	return function (o, p) {
		var a, b;
		if (o && p && typeof o === 'object' && typeof p === 'object'){
			a = o[name];
			b = p[name];
			if(a === b){
				return typeof minor === 'function' ? minor(o, p) : 0;
			}
			if(typeof a === typeof b){
				return a < b ? -1 : 1;
			}
			return typeof a < typeof b ? -1 : 1;
		} else {
			throw {
				name: 'Error',
				message: 'Expected am object when sorting by ' +name;
			};
		}
	};
};
s.sort(by('last',by('first')));
// s 是 [
	{first: 'Joe', last: 'Besser'},
	{first: 'Joe', last: 'Derita'},
	{first: 'Larry', last: 'Fine'},
	{first: 'Curly', last: 'Howard'},
	{first: 'Moe', last: 'Howard'},
	{first: 'Shemp', last: 'Howard'}
]

```

### array.splice(start, deleteCount, item...)

splice方法从array中移除1个或多个元素，并用新的item替换他们。参数start是从数组array中移除元素的开始位置。参数deleteCount是要移除的元素个数。如果有额外的参数，那些item都将插入到所以出元素的位置上。它返回一个包含被移除元素的数组。

splice最主要的用处是从一个数组中删除元素。**千万不要把splice和slice混淆了**

```js

var a = ['a', 'b', 'c'];
var r = a.splice(1, 1, 'ache', 'bug');
// a 是 ['a', 'ache', 'bug', 'c']
// r 是 ['b']

```

## array.unshift(item...)

unshift方法像push方法一样用于将元素添加到数组中，但它是把item插入array的开始部分而不是结尾部分。它返回array的新的长度值：

```js

var a = ['a', 'b', 'c'];
var r = a.unshift('?', '@');
// a 是 ['?','@','a','b','c']
// r 是 5

```

### Array.of()

Array.of()方法创建一个具有可变数量参数的新数组示例，而不是考虑数组的数量和类型

Array.of()和Array构造函数之间的区别在于处理整数参数：Array.of(7)创建一个具有











