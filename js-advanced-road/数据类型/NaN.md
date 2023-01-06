## NaN 和 Number.NaN 

**特点**
1. typeof 返回都是数字
2. 都与自身不想等, 除了使用 Object.is() 
3. 不能被删除


```
typeof NaN // 'number'
typeof Number.NaN // 'number'

Object.is(NaN, Number.NaN) // true
Object.is(NaN, NaN) // true

Object.getOwnPropertyDescriptor(window, 'NaN')
{
  configurable: false,
  enumerable: false,
  value: NaN,
  writable: false
}


```

加餐：
### 判断两个值是否相等的方式
- ==
- ===
- Object.is()

三者区别：
object.is 这种相等性判断逻辑和传统的 == 运算不同，== 运算符会对它两边的操作数做隐式类型转换（如果它们类型不同），然后才进行相等性比较，（所以才会有类似 “” == false 为 true 的现象），但 Object.is不会做这种类型转换。这与
===运算符也不一样。===运算符（和==运算符）将数字值-0和+0视为相等，并认为 Number.NaN 不等于 NaN。


### isNaN

isNaN: 检查 toNumber 返回值，如果是 NaN，就返回 true，否则返回 false

```
isNaN(10n)  // TypeError: Cannot convert a BigInt value to a number

var a = Symbol(1)
isNaN(a) // TypeError: Cannot convert a Symbol value to a number

```


### Number.isNaN
意思： 判断一个值是否是数字，并且值等于 NaN

实现思路
```
function isNaNVal(val) {
    return Object.is(val, NaN);
}

function isNaNVal(val) {
    return val !== val;
}

function isNaNVal(val) {
    return typeof val === 'number' && isNaN(val)
}

// 综合垫片
if (!("isNaN" in Number)) {
    Number.isNaN = function(val) {
        return typeof val === 'number' && isNaN(val)
    }
}

```

### 透过陷阱看本质

例子
```
var arr = [NaN]
arr.indexOf(NaN) // -1
arr.includes(NaN)  // true
```

includes: 调用内部的 Number::sameValueZero 
indexOf:  调用内部的 Number::equal

说明：
Number::sameValueZero(x, y)  => If x is NaN and y is NaN return true
Number::equal(x, y) => if x is NaN,return false; if y is NaN, return false

