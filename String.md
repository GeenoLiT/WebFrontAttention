# String

## 注意事项

### charAt和str[index]获取到的字符区别

> charAt : 获取不存在的索引返回=> '空字符串'
>
> str[uindex]: 无数值的索引返回=> undefined

### substr 和substring截取字符串

> substr(n,m)：从索引n开始**截取m个字符**，m不写截取到末尾（后面方法也是）
>
> substring(n,m)：从索引n开始找到**索引为m处(不含m)**
>
> slice(n,m)：和substring一样，都是找到索引为m处，但是slice可以**支持负数作为索引**，其余两个方法是不可以的

### includes查询是否包含数据

```javascript
/*
 * str.includes(searchString[, position])
 * @params
 *  searchString:查询的字符串
    position: 可选，从当前字符串的哪个索引位置开始搜寻子字符串，默认值为0。
 * @return
 *   如果当前字符串包含被搜寻的字符串，就返回 true；否则返回 false。
 */

/*
* 这是一个简单的polyfill,es6定义的
*/
if (!String.prototype.includes) {
  String.prototype.includes = function(search, start) {
    'use strict';
    if (typeof start !== 'number') {
      start = 0;
    }

    if (start + search.length > this.length) {
      return false;
    } else {
      return this.indexOf(search, start) !== -1;
    }
  };
}
```

### split-拆分字符串为数组

> 也支持：正则表达式的分隔表达式操作
> [mdn-split](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

## 常用方法

### split

> 拆分字符串，返回一个数组
> [mdn-split](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

```javascript
/*
 * str.split([separator[, limit]])
 * @params
 * separator:分隔字符
 * limit: 分隔之后截取的数量[数组的个数]
 * @return
 *   返回一个新的数组对象
 */

/*
字符倒序排列
如果字符串包含图形素集群，即使使用Unicode感知的拆分(use for example esrever instead)，
也不能工作。
const str = 'résumé';
const strReverse = str.split(/(?:)/u).reverse().join('');
// => "́emuśer"
*/
let str = 'abcdefg';
let newstr = str.split('').reverse().toString();
console.log(newstr);

/*
* 拆分一个字符串为数组
*/
let str = 'abcdefg';
let newstr = str.split('');
//return 就是一个数组了 (7) ["a", "b", "c", "d", "e", "f", "g"]
```

### toLowercase()/toUppercase()

> 大小写的全局转换

