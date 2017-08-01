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

Array.of()和Array构造函数之间的区别在于处理整数参数：Array.of(7)创建一个具有单个元素7的数组，而Array(7)创建一个包含7个undefined元素的数组。

```js

Array.of(7);       // [7]
Array.of(1,2,3);   // [1,2,3]

Array(7);          // [,,,,,,]
Array(1,2,3);      // [1,2,3]

```

### array.copyWithin() 

语法：arr.copyWithin(目标索引,[源开始索引],[结束源索引])

返回值：改变的新数组

copyWithin()方法潜伏之数组的一部分到统一数组中的另一个位置，并返回它，而不修改其大小

```js

[1,2,3,4,5].copyWithin(-2);
// [1,2,3,1,2]

[1,2,3,4,5].copyWithin(0,3);
// [4,5,3,4,5]

[1,2,3,4,5].copyWithin(0,3,4);
// [4,2,3,4,5]

```

### array.entries()

entries()方法返回一个新1的Array Iterator对象，该对象包含数组中每个索引的键/值对。

```js

var arr = ['a','b','c'];
var iterator = arr.entries();
// undefined

console.log(iterator);
console.log(iterator.next().value);
// [0,'a']
console.log(iterator.next().value);
// [1,'b']
console.log(iterator.next().value);
// [2, 'c']

```

### array.every()

every()方法测试数组的所有元素是否都能通过了指定函数的测试

语法：`arr.every(callback[, thisArg])`

```js

function isBigEnough(element, index, array) {
	return (element >= 10);
}
var passed = [12, 5, 8, 130, 44].every(isBigEnough);
// pass is false
passed = [12, 54, 18, 130, 44].every(isBigEnough);
// passed is true

```

### array.fill()

fill()方法用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。

```js

// 语法
arr.fill(value);
arr.fill(value, start);
arr.fill(value, start, end);

var number = [1,2,3];
number.fill(1);

// results in [1,1,1]

```

### array.filter()

filter()方法创建一个新数组，其包含通过所提供函数实现的测试的所有元素。

```js

function isBigEnough(value) {
	return value >= 10;
}

var filtered = [12,5,8,130,44].filter(isBigEnough);
// filtered is [12, 130, 44]

const isBigEnough = value => value >= 10;

let [...spraed] = [12,5,8,130,44];

let filtered = spraed.filter(isBigEnough);
// filtered is [12,130,44]

```

### array.find()

find()方法返回数组中满足提供的测试函数的第一个元素的值，否则返回undefined

```js

function isBigEnough(element) {
	return element >= 15;
}

[12,5,8,130,44].find(isBigEnough); // 130

```

### array.findIndex()

findIndex()方法返回数组中满足提供测试函数的第一个元素的索引。否则返回-1

```js

function isBigEnough(element){
	return element >= 15;
}

[12,5,8,130,44].findIndex(isBigEnough); // 3

```

### array.forEach()

forEach()方法对数组的每个元素执行一次提供的函数

```js

let a = ['a','b','c'];
a.forEach(function(element){
	console.log(element);
});
// a
// b
// c

```

### array.includes()

includes()方法用来判断一个数组是够半酣一个指定的值，如果是，酌情返回true或false

```js

// 语法
arr.includes(searchElement)
arr.includes(searchElement, fromIndex)

let a = [1,2,3];
a.includes(2);
// true

a.includes(4);
// false

```

### array.indexOf()

indexOf()方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在则返回-1.

```js

// 语法
arr.indexOf(searchElement)
arr.indexOf(searchElement[, fromIndex = 0])

let a = [2, 9, 7, 8, 9]; 
a.indexOf(2); // 0 
a.indexOf(6); // -1
a.indexOf(7); // 2
a.indexOf(8); // 3
a.indexOf(9); // 1

if (a.indexOf(3) === -1) {
  // element doesn't exist in array
}

```

### array.keys()

keys()方法返回一个新的Array迭代器

```js

let arr = ["a", "b", "c"];

let iterator = arr.keys();
// undefined

console.log(iterator);
// Array Iterator {}

console.log(iterator.next()); 
// Object {value: 0, done: false}

console.log(iterator.next()); 
// Object {value: 1, done: false}

console.log(iterator.next()); 
// Object {value: 2, done: false}

console.log(iterator.next()); 
// Object {value: undefined, done: true}

```

### array.lastIndexOf()

lastIndexOf()方法返回指定元素（也即有效的javascript值或变量）在数组中的最后一个的索引，如果不存在则返回-1.从数组的后面向前查找，从fromIndex处开始。

语法：arr.lastIndexOf(searchElement[, fromIndex = arr.length - 1])

参数：searchElement---被查找的元素；formIndex---从此位置开始逆向查找。默认为数组的长度减一，即真个数组都被查找。如果该值大于或等于数组的长度，则整个数组会被查找。如果为负值，将其视为从数组末尾向前偏移。即使该值为负值，数组仍然会被从后向前查找。如果该值为负时，其绝对值大于数组长度，则返回-1，即数组不会被查找。

**lastIndexOf使用严格相等（即 ===）比较searchElement和数组中的元素**

```js

var array = [2,5,9,2];
var index = array.lastIndexOf(2);
// index is 3

index = array.lastIndexOf(7);
// index is 7

index = array.lastIndexOf(2,3);
// index is 3

index = array.lastIndexOf(2,2);
// index is 0

index = array.lastIndexOf(2,-2);
// index is 0

index = array.lastIndexOf(2,-1);
// index is 3

```

### array.map()

语法：

```

let array = arr.map(function callback(surrentValue, index, array) {
	// return element for new_array
}[, thisArg])

```

参数：

> callback---生成新数组的函数，使用三个参数：
> currentValue（callback第一个参数）: 数组中正在处理的当前元素
> index（callback第二个参数）:数组中正在处理的当前元素的索引
> array（callback的第三个参数，map方法被调用的数组）
> thisArg---执行callback函数时使用的this值

map()方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

```js

// 使用三个参数

const numbers = [1, 2, 3, 4, 5];

let arr = numbers.map((currentValue, index, array) => {
    console.log(`currentValue = `, currentValue);
    console.log(`index = `, index);
    console.log(`array= `, array);
    return currentValue * 2;
}, numbers);

console.log(`arr `, arr);



let numbers = [1, 5, 10, 15];
let doubles = numbers.map((x) => {
   return x * 2;
});

// doubles is now [2, 10, 20, 30]
// numbers is still [1, 5, 10, 15]


let numbers = [1, 4, 9];
let roots = numbers.map(Math.sqrt);

// roots is now [1, 2, 3]
// numbers is still [1, 4, 9]

```

### array.reduce()

语法：

```

array.reduce(function(accumulator, currentValue, currentIndex, array), initialValue)

```

参数：

> callback:执行数组中每个值的函数，包含四个参数
> --- accumulator 上一次调用回调返回的值，或者是提供的初始值（initialValue）
> --- currentValue 数组中正在处理的元素
> --- currentIndex 数据中正在处理的元素的索引，如果提供了initialValue ，从0开始；否则从1开始
> --- array调用reduce的数组
> initialValue：可选项，其值用于第一调用callback的第一个参数。如果没有设置初始值，则将数组中的第一个元素作为初始值。空数组调用reduce时没有设置初始值将会报错。

返回值：函数累计处理的结果。

reduce()方法对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。

```js

var total = [0,1,2,3].reduce(function(sum,value){
	return sum + value;
});
// total is 6

var flattened = [[0,1],[2,3],[4,5]].reduce(function(a,b){
	return a.concat(b);
},[]);
// flattened is [0,1,2,3,4,5]

```

### array.reduceRight()

reduceRight()方法接受一个函数作为累加器和数组的每个值（从右到左）将其减少为单个值。

语法：`arr.reduceRight(callback[, initialValue])`

```js

left flattened = [
	[0,1],
	[2,3],
	[4,5]
].reduceRight((a,b) => {
	return a.concat(b);
},[]);

// flattened is [4,5,2,3,0,1]

```

### array.some()

some()方法测试数组中的某些元素是否通过由提供的函数实现的测试。

语法：`arr.some(callback[,thisArg])`

```js

const isBiggerThan10 = (element, index, array) => {
	return element > 10;
}

[2,5,8,1,4].some(isBiggerThan10);
// false

[12, 5, 8, 1, 4].some(isBiggerThan10); 
// true

```

### array.toLocaleString()

toLocaleString()返回一个字符串表示数组中的元素。数组中的元素将使用个字的toLocaleString方法转成字符串，这些字符串将使用一个特定语言环境的字符串（例如一个逗号“，”）隔开

语法：`arr.toLocaleString()`

```js

var number = 1337;
var date = new Date();
var myArr = [number, date, 'foo'];
var str = myArr.toLocaleString();
console.log(str);
// 输出 "1,337,2017/7/25 下午8:36:53,foo" 
// 假定运行在中文（zh-CN）环境，北京时区

```

### array.toSource()

**该特性是非标准的，请尽量不要在生产环境中使用它！**

语法：`array.toSource()`

返回一个字符串，代表该数组的源代码

```js

// 谷歌不支持
var alpha = new Array('a','b','c');
alpha.toSource(); // 返回['a','b','c']

```

### array.toString()

toString()返回一个字符串，表示指定的数组及其元素

```js

var monthNames = ['Jan', 'Feb', 'Mar', 'Apr'];
var myVar = monthNames.toString(); // assigns "Jan,Feb,Mar,Apr" to myVar.

```

### array.values()

values()方法返回一个新的ArrayIterator对象，该对象包含数组每个索引值。

**Chrome未实现**

使用 for...of 循环进行迭代

```js

let arr = ['w', 'y', 'k', 'o', 'p'];
let eArr = arr.values();
// 您的浏览器必须支持 for..of 循环
// 以及 let —— 将变量作用域限定在 for 循环中
for (let letter of eArr) {
  console.log(letter);
}

``` 

另一种迭代方式

```js

let arr = ['w', 'y', 'k', 'o', 'p'];
let eArr = arr.values();
console.log(eArr.next().value); // w
console.log(eArr.next().value); // y
console.log(eArr.next().value); // k
console.log(eArr.next().value); // o
console.log(eArr.next().value); // p

```

## Function

**属性**

### function.length

length 属性值明函数的形参个数

length 是函数对象的一个属性值，指该函数有多少个必须要传入的参数，那些已定义了默认值的参数不算在内，比如function(xx = 0)的length是0.与之对比的是，<font color="#387894">arguments.length</font>是函数被调用时实际传参的个数。

```js

console.log(Function.length); // 1
console.log(function()        {}.length);  // 0
console.log(function(a)       {}.length);  // 1
console.log(function(a, b)    {}.length);  // 2
console.log(function(...args) {}.length);  // 0, rest parameter is not counted

```


**方法**

### Function.prototype.apply()

语法： `fun.apply(thisArg, [argsArray])`


参数：

> thisArg 在fun函数运行时指定的this值。需要注意的是，指定的this值并不一定是该函数执行时真正的this值，如果这个函数处于<font color="#A52A2A">非严格模式</font>下，则指定为null或undefined时会自动指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的this会指向该原始值的自动包装对象。

> argsArray 一个数组或者类数组对象，其中的数组元素将作为单独的参数传给fun函数。如果该参数的值为null或undefined，则表示不需要传入任何参数。从ECMAScript5开始使用类数组对象。

apply方法调用函数function，传递一个将被绑定到this上的对象和一个可选的参数数组。apply方法被调用在apply调用模式（apply invocation pattern）中。

```js

Function.method('bind', function (that) {
	// 返回一个函数，调用这个函数就像它是那个对象的方法一样。
	var method = this,
		slice  = Array.prototype.slice,
		args   = slice.apply(arguments, [1]);
	return function () {
		return method.apply(that, args.concat(slice,apply(arguments, [0])));
	} 
});

var x = function () {
	return this.value
}.bind({value: 666});
alert(x()); // 666

```

**使用apply和内置函数**

聪明的apply用法允许你在某些本来需要写成遍历数组的任务中使用内建的函数。在下面的例子中我们使用Math.max/Math.min来找出数组中的最大/最小值。

```js

// min/max number is an array
var numbers = [5, 6, 2, 3, 7];

// using Math.min/Math.max spply
var max = Mtah.max.apply(null, numbers); // This about equal to Math.max(numbers[0],...) or Math.max(5, ,6, ...)
var min = Math.min.apply(null, numbers);

// vs. simple loop based algorithm
max = -Infinity, min = +Infinity;

for(var i = 0; i< numbers.length){
	if(numbers[i] > max){
		max = numbers[i];
	}
	
	if(numbers[i] < min){
		min = numbers[i];
	}
};

```

### Function.prototype.bind()

bind()方法创建一个新的函数，当被调用时，将其this关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定参数序列。

**语法** `fun.bind(thisArg[, arg1[, arg2[, ...]]])`

**参数**
> thisArg 当绑定函数被调用时，该参数作为原函数运行时的this指向。当使用<font color="#387894">new 操作符</font>调用绑定函数时，该参数无效。

> arg1， arg2... 当绑定函数被调用时，这些参数将至于实参之前传递给被绑定的方法。

创建绑定函数：

bind()最简单的用法是创建一个函数，使这个函数不论怎么调用都有同样的this值。Javascript新手经常犯的一个错误是将方法从对象中拿出来，然后在调用，希望方法中的this是原来的对象。（比如在回调中传入这个方法）如果不做特殊处理的话，一般会丢失原来的对象。从原来的函数和原来的对象创建一个绑定函数，则能很漂亮地解决这个问题：

```js

this.x = 9;
var module = {
	x: 81,
	getX: function() { return this.x; }
};

module.getX() // 返回 81
var retrieveX = module.getX;
retrieveX(); // 返回9， 这种强狂下，"this"指向全局作用域

// 创建一个新函数，将"this"绑定到module对象
// 新手可能会被全局的x变量和module里的属性x所迷惑
var boundGetX = retrieveX.bind(module);
boundGetX(); // 返回81

```

### Function.prototype.call() 

call()方法调用一个函数，其具有一个指定的this值和分别地提供的参数

**注意：该方法的作用和<font color="#387894">apply()</font>方法类似，只有一个区别，就是call()方法接受的是若干参数的列表，而apply()方法接受的是一个包含多个参数的数组。**

语法：`fun.call(thisArg[, arg1[, arg2[, ...]]])`

参数：

> thisArg 在fun函数运行时指定的this值。需要注意的是指定的this值并不一定是该函数执行时真正的this值，如果这个函数处于非严格模式下，则指定为null和undefined的this值会自动指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的this会指向该原始值的自动包装对象。

> arg1, arg2, ... 执行的参数列表

使用call方法调用匿名函数

在下例中的for循环内，我们创建了一个匿名函数，通过调用该函数call方法，将每个数组元素作为指定的this值执行了那个匿名函数。这个匿名函数的主要目的是给每个数组元素对象添加一个print方法，这个print方法可以打印出各元素在数组中的正确索引号。当然，这里不是必须得让数组元素作为this值传入那个匿名函数（普通参数就可以了），目的是为了掩饰call用法

```js

var animals = [
	{species: 'Lion', name: 'King'},
	{species: 'Whale', name: 'Fail'}
];

for(var i = 0; i < animals.length; i += 1){
	(function (i) {
		this.print = function () {
			console.log('#' + i + ' ' + this.species + ': ' + this.name);
		}
		this.print();
	}).call(animals[i], i);
}

```

使用call方法调用函数并且制定上下文的this

在下面的例子中，当调用greet方法的时候，该方法的this值会绑定到i对象

```js

function greet() {
	var reply = [this.person, 'Is An Awesome', this.role].join(' ');
	console.log(reply);
};

var i = {
	person: 'Douglas Crockford', role: 'Javascript Developer'
};

greet.call(i); // Douglas Crockford Is An Awesome Javascript Developer

```

### Function.prototype.isGenerator()

<font color="#FF4040">非标准：该特性是非标准的，请尽量不要再生产环境使用它</font>

作用： 判断一个函数是否是一个[生成器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators#Generators.3a_a_better_way_to_build_Iterators)

语法：`fun.isGenerator()`

```js

function f () {};
function* g() {
	yield 42;
};

console.log("f.isGenerator() = " + f.isGenerator()); // f.isGenerator() = false
console.log("g.isGenerator() = " + g.isGenerator()); // g.isGenerator() = true

```

### Function.prototype.toSource()

<font color="#FF4040">非标准：该特性是非标准的，请尽量不要再生产环境使用它</font>

作用：返回函数的源代码的字符串表示

语法： `function.toSource()/Function.toSource()`

> toSource方法返回下面的值：
> 
> 对于内置的Function对象，toSource返回下面的字符串

```

function Function() {
    [native code]
}

```

> 对于自定义函数来说，toSource返回能定义该函数的Javascript源码

### Function.prototype.toString()

作用：toString()方法返回一个表示当前函数源代码的字符串

语法：`function.toString()`

## Number

### number.toExponential(fractionDigits)

toExponential方法把这个number转换成一个指数形式的字符串。

可选参数：fractionDigits控制其小数点后的数字位数。它的值必须在0至20之间。

```js

document.writeln(Math.PI.toExponential(0));
document.writeln(Math.PI.toExponential(2));
document.writeln(Math.PI.toExponential(7));
document.writeln(Math.PI.toExponential(16));
document.writeln(Math.PI.toExponential());

// 结果
3e+0
3.14e+0
3.1415926535897930e+0
3.141592653589793e+0

```

### number.toPrecision(precision)

toPrecision() 方法把这个number转换成一个十进制形式的字符串。可选参数precision控制有效数字的位数。它的值必须在0至21之间：

```js

document.writeln(Math.PI.,toString(2));
document.writeln(Math.PI.,toString(8));
document.writeln(Math.PI.,toString(16));
document.writeln(Math.PI.,toString());

// 结果
11.00100100011111101101010100010001000010110100011
301103755242102643
30243f6a8885a3
30141592653589793

```

### number.toString(radix)

toString()方法把这个number转换成一个字符串。

可选参数radix控制基数。它的值必须在2和36之间。默认的radix是以10为基数的。radix参数最常用的是整数，但是它可以用任意的数字。

在最普通的情况下，number.toString()可以更简单地写为String(number):

```js

document.writeln(Math.PI.toString(2));
document.writeln(Math.PI.toString(8));
document.writeln(Math.PI.toString(16));
document.writeln(Math.PI.toString());

// 结果
11.00100100011111101101010100010001000010110100011
301103755242102643
30243f6a8885a3
30141592653589793

```

### Number.isFinite()

isFinite()方法用来检测传入的参数是否是一个有穷数（finite number）

语法：`Number.isFinite(value)`

参数： vlaue 要检测有穷性的值

返回值： 一个布尔值表示给定值是否是一个有穷数

```js

Number.isFinite(Inifinity); // false
NUmber.isFinite(NaN) // false
Number.isFinite(-Inifinity); // false

Number.isFinite(0); // true
Number.isFinite(2e64); // true

Number.isFinite('0'); // false, 全局函数 isFinite('0')会返回true

```

### Number.isInteger()

isInteger()方法用来判断给定的参数是否为整数

语法：`Number.isInteger(value)`

参数： value要判断此参数是否为整数

返回值： 判定给定值是否是整数的布尔值

```js

Number.isInteger(0);            // true
Number.isInteger(1);            // true
Number.isInteger(-10000);       // true
Number.isInteger(0.1);          // false
Number.isInteger(Math.PI);      // false
Number.isInteger(Infinity);     // false
Number.isInteger(-Infinity);    // false
Number.isInteger("10");         // false
Number.isInteger(true);         // false
Number.isInteger(false);        // false
Number.isInteger([1]);          // false

```

### Number.isNaN()

isNaN() 方法确定传递的值是否为NaN和其他类型的Number。它是原始的全局isNaN的更强大的版本

语法： `Number.isNaN(value)`

参数： value要检测是否是NaN的值

返回值：一个布尔值，表示给定的值是否是NaN

```js

Number.isNaN(NaN);        // true
Number.isNaN(Number.NaN); // true
Number.isNaN(0 / 0)       // true

// 下面这几个如果使用全局的 isNaN() 时，会返回 true。
Number.isNaN("NaN");      // false，字符串 "NaN" 不会被隐式转换成数字 NaN。
Number.isNaN(undefined);  // false
Number.isNaN({});         // false
Number.isNaN("blabla");   // false

// 下面的都返回 false
Number.isNaN(true);
Number.isNaN(null);
Number.isNaN(37);
Number.isNaN("37");
Number.isNaN("37.37");
Number.isNaN("");
Number.isNaN(" ");

```

### Number.isSafeInteger()

isSafeInteger()方法用来判断传入的参数是否是一个“安全整数（safe integer）”。一个安全整数是符合下面条件的整数：

* can be exactly represented as an IEEE-754 double precision number, and
* whose IEEE-754 representation cannot be the result of rounding any other integer to fit the IEEE-754 representation.

比如，2^53 - 1是一个安全整数，它能被精确表示，在任何IEEE-754舍入模式下，没有其他整数舍入结果为该整数。作为对比，2^53就不是一个安全整数，他能够使用IEEE-754表示，但2^53 + 1不能使用IEEE-754直接表示，在就近舍入和向零舍入中，会被舍入为2^53

安全整数范围为-(2^53 - 1)到(2^53 - 1)之间的整数，包含-(2^53 - 1)到(2^53 - 1)

### Number.parseFloat()

Nnumber.parseFloat() 方法可以把一个字符串解析成浮点数。该方法与全局的parseFloat()函数相同，并且处于ECMAScript6规范中（用于全局变量的模块化）。

语法：`Number.parseFloat(string)`

参数： string 被解析的字符串

### Number.parseInt()

Number.parseInt()方法可以根据给定的进制数把一个字符串解析成整数

语法： `Number.parseInt(string[, radix])`

> 参数： 
> 
> string 被解析的值。如果不是一个字符串，则将其转换成字符串。字符串的开头的空符将会被忽略。
> 
> radix 一个整数值，指定转换中采用的基数。总是指定该参数可以保证结果可预测。当忽略该参数时，不同的实现环境可能产生不同的结果。

该方法和全局的parseInt()函数是同一个函数

```js

Number.parseInt === parseInt; // true

```

### Number.prototype.toExponential()

toExponential()方法以指数表示法返回该数值字符串表示形式。

语法： `numObj.toExponential(fractionDigits)`

参数： fractionDigits 可选。一个整数，用来指定小数点后几位数字。默认情况下尽可能多的位数显示数字。

```js

var numObj = 77.1234;

alert("numObj.toExponential() is " + numObj.toExponential()); //输出 7.71234e+1

alert("numObj.toExponential(4) is " + numObj.toExponential(4)); //输出 7.7123e+1

alert("numObj.toExponential(2) is " + numObj.toExponential(2)); //输出 7.71e+1

alert("77.1234.toExponential() is " + 77.1234.toExponential()); //输出 7.71234e+1

alert("77 .toExponential() is " + 77 .toExponential()); //输出 7.7e+1

```

### Number.prototype.toFixed(digits)

toFixed()方法使用定点表示法来格式化一个数 四舍五入

语法： `numObj.toFixed(digits)`

参数： digits 小数点后数字的个数；介于0到20（包括）之间，实现环境可能不支持更大范围。如果忽略该参数，则默认为0.

```js

var numObj = 12345.6789;

numObj.toFixed();         // 返回 "12346"：进行四舍五入，不包括小数部分
numObj.toFixed(1);        // 返回 "12345.7"：进行四舍五入

numObj.toFixed(6);        // 返回 "12345.678900"：用0填充

(1.23e+20).toFixed(2);    // 返回 "123000000000000000000.00"

(1.23e-10).toFixed(2);    // 返回 "0.00"

2.34.toFixed(1);          // 返回 "2.3"

-2.34.toFixed(1);         // 返回 -2.3 （由于操作符优先级，负数不会返回字符串）

(-2.34).toFixed(1);       // 返回 "-2.3" （若用括号提高优先级，则返回字符串）

```

### Number.prototype.toPrecision()

toPrecision() 方法以指定的精度返回该数值对象的字符串表示。

语法： `numObj.toPrecision(precision)`

参数： precision 可选。一个用来指定有效数个数的整数。

```js

var numObj = 5.123456;
console.log("numObj.toPrecision()  is " + numObj.toPrecision());  //输出 5.123456
console.log("numObj.toPrecision(5) is " + numObj.toPrecision(5)); //输出 5.1235
console.log("numObj.toPrecision(2) is " + numObj.toPrecision(2)); //输出 5.1
console.log("numObj.toPrecision(1) is " + numObj.toPrecision(1)); //输出 5

// 注意：在某些情况下会以指数表示法返回
console.log((1234.5).toPrecision(2)); // "1.2e+3"

```

### Number.prototype.toString()

toString() 方法返回指定 Number 对象的字符串表示形式。

语法： `numObj.toString([radix])`

参数： radix 指定要用于数字到字符串的转换的基数(从2到36)。如果未指定 radix 参数，则默认值为 10。

```js

var count = 10;

console.log(count.toString());    // 输出 '10'
console.log((17).toString());     // 输出 '17'
console.log((17.2).toString());   // 输出 '17.2'

var x = 6;

console.log(x.toString(2));       // 输出 '110'
console.log((254).toString(16));  // 输出 'fe'

console.log((-10).toString(2));   // 输出 '-1010'
console.log((-0xff).toString(2)); // 输出 '-11111111'

```

### Number.prototype.valueOf()

valueOf() 方法返回一个被 Number 对象包装的原始值。

语法： `numObj.valueOf()`

```js

var numObj = new Number(10);
console.log(typeof numObj); // object

var num = numObj.valueOf();
console.log(num);           // 10
console.log(typeof num);    // number

```

## String

### String.length

length 属性表示一个字符串的长度，空字符串长度为0

```js

var x = "Mozilla";
var empty = "";

console.log("Mozilla is " + x.length + " code units long");
// "Mozilla is 7 code units long" 

console.log("The empty string is has a length of " + empty.length);
// "The empty string is has a length of 0" 

```

### String.prototype.chatAt()

charAt() 方法从一个字符串中返回指定的字符

语法: `str.chatAt(index)`

参数： index 一个介于0和字符串长度减1之间的整数，吐过没有提供索引，chatAt()将使用0。

输出字符串中不同位置的字符

```js

var anyString = "Brave new world";

console.log("The character at index 0   is '" + anyString.charAt(0)   + "'");
console.log("The character at index 1   is '" + anyString.charAt(1)   + "'");
console.log("The character at index 2   is '" + anyString.charAt(2)   + "'");
console.log("The character at index 3   is '" + anyString.charAt(3)   + "'");
console.log("The character at index 4   is '" + anyString.charAt(4)   + "'");
console.log("The character at index 999 is '" + anyString.charAt(999) + "'");

// The character at index 0 is 'B'
// The character at index 1 is 'r'
// The character at index 2 is 'a'
// The character at index 3 is 'v'
// The character at index 4 is 'e'
// The character at index 999 is ''

```

### String.prototype.chatCodeAt()

charCodeAt() 方法返回0到65535之间的整数，表示给定索引处的utf-16代码单元（在 Unicode 编码单元表示一个单一的 UTF-16 编码单元的情况下，UTF-16 编码单元匹配 Unicode 编码单元。但在——例如 Unicode 编码单元 > 0x10000 的这种——不能被一个 UTF-16 编码单元单独表示的情况下，只能匹配 Unicode 代理对的第一个编码单元）如果你想要整个代码点的值，使用codePointAt()

语法： `str.charCodeAt(index)`

参数： index 一个大于等于0，小于等于字符串长度的整数。如果不是一个数值，则默认为0.

返回值： 返回值是一个表示给定索引处字符的UTF-16代码单元值的数字；如果索引超出范围，则返回NaN

```js

"ABC".charCodeAt(0) // returns 65

```

### String.prototype.concat()

concat() 方法讲一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回

**强烈建议使用副值操作符（+, +=）代替caocat方法。**参看[性能测试](https://jsperf.com/concat-vs-plus-vs-join)

语法： `str.concat(string2, string3[, ..., stringN])`

参数： string2...stringN 和元字符串连接的多个字符串

```js

var hello = "Hello, ";
console.log(hello.concat("Kevin", " have a nice day.")); /* Hello, Kevin have a nice day. */

```

### String.prototype.indexOf()

indexOf()方法返回调用String对象中第一次出现指定值的左尹，开始在fromIndex进行搜索

语法： `str.indexOf(searchValue[, fromIndex])`

参数： searchValue 一个字符串表示被查找的值， fromIndex 可选，表示调用该方法的字符串中开始查找的位置。可以是任意的整数。默认为0.如果fromIndex < 0 则查找整个字符串。如果fromIndex >= str.length，则该方法返回-1，除非被查找的字符串是一个空字符串，此时返回str.length。

```js

"Blue Whale".indexOf("Blue");     // returns  0
"Blue Whale".indexOf("Blute");    // returns -1
"Blue Whale".indexOf("Whale", 0); // returns  5
"Blue Whale".indexOf("Whale", 5); // returns  5
"Blue Whale".indexOf("", 9);      // returns  9
"Blue Whale".indexOf("", 10);     // returns 10
"Blue Whale".indexOf("", 11);     // returns 10

```

### String.prototype.lastIndexOf(searchValue[, fromIndex])

语法： `str.lastIndexOf(searchValue[, fromIndex])`

参数： 
> searchValue 一个字符串，被查找的值
> fromIndex 从调用该方法字符串的此位置处开始查找。可以使任意整数。默认值为str.length。如果为负值，则被看做0.如果fromIndex > str.length，则fromIndex被看做str.length。

lastIndexOf() 方法和indexOf方法类似，只不过它是从该字符串的末尾开始查找而不是从开头

```js

"canal".lastIndexOf("a")   // returns 3
"canal".lastIndexOf("a",2) // returns 1
"canal".lastIndexOf("a",0) // returns -1
"canal".lastIndexOf("x")   // returns -1

"Blue Whale, Killer Whale".lastIndexOf("blue"); // returns -1

``

### String.prototype.localeCompare()




















































