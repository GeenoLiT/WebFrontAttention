# Array

- [Array](#array)
  - [数组注意事项](#数组注意事项)
  - [数组中的常用方法](#数组中的常用方法)
      - [实现数组增删改的操作](#实现数组增删改的操作)
        - [增加](#增加)
          - [push=>向数组末尾](#push向数组末尾)
          - [unshift=>向数组开始](#unshift向数组开始)
        - [删除](#删除)
          - [shift=>删除数组中的第一项](#shift删除数组中的第一项)
          - [pop=>删除数组中的最后一项](#pop删除数组中的最后一项)
        - [综合函数](#综合函数)
          - [splice=>实现数组的增加、删除、修改](#splice实现数组的增加删除修改)
      - [实现数组的查询和拼接](#实现数组的查询和拼接)
        - [查询](#查询)
          - [slice](#slice)



### 数组注意事项

* 数组索引位置处（索引可以跨顺序），没有数据的话，默认为：**undefined**
* **splice**函数--增删改的操作

### 数组中的常用方法

* 作用和含义
* 实参
* 返回值
* 原数组是否发生改变

#### 实现数组增删改的操作

> 会修改原来的数组值

##### 增加

###### push=>向数组末尾

```javascript
/*
 * push : 向数组末尾增加内容
 * @params
 *   多个任意类型
 * @return
 *   新增后数组的长度 
 */
let ary = [10, 20];
let res = ary.push(30, 'AA');
// 基于原生JS操作键值对的方法，也可以向末尾追加一项新的内容
ary[ary.length] = 40;
console.log(res, ary); 
```

###### unshift=>向数组开始

```javascript
/*
 * unshift : 向数组开始位置增加内容
 * @params
 *   多个任意类型
 * @return
 *   新增后数组的长度 
 */
let ary = [10, 20];
let res = ary.unshift(30, 'AA');
console.log(res, ary); //=>4  [30,'AA',10,20]
// 基于原生ES6展开运算符，把原有的ARY克隆一份，在新的数组中创建第一项，其余的内容使用原始ARY中的信息即可，也算实现了向开始追加的效果
ary = [100, ...ary];
console.log(ary); //=>[100,30,'AA',10,20] 

```

##### 删除

###### shift=>删除数组中的第一项

```javascript
/*
 * shift : 删除数组中的第一项
 * @params
 * @return
 *   删除的那一项 
 */
let ary = [10, 20, 30, 40];
let res = ary.shift();
console.log(res, ary); //=>10  [20, 30, 40]

// 基于原生JS中的DELETE，把数组当做普通的对象，确实可以删除掉某一项内容，但是不会影响数组本身的结构特点（length长度不会跟着修改）,真实项目中杜绝这样的删除使用
delete ary[0];
console.log(ary); //=>{1:30,2:40,length:3} 
```

###### pop=>删除数组中的最后一项

```javascript
/*
 * pop : 删除数组中的最后一项
 * @params
 * @return
 *   删除的那一项 
 */
let ary = [10, 20, 30, 40];
let res = ary.pop();
console.log(res, ary); //=>40  [10,20,30]

// 基于原生JS让数组数组长度干掉一位，默认干掉的就是最后一项
ary.length--; //=>ary.length = ary.length - 1;
console.log(ary); 
```

##### 综合函数

###### splice=>实现数组的增加、删除、修改

- **删除**-索引开始位置包括自身

```javascript
/*
 * splice : 实现数组的增加、删除、修改
 * @params
 * 	 n,m 都是数字  从索引n（包括n）开始删除m个元素（m不写，是删除到末尾）
 * @return
 *   把删除的部分用新数组存储起来返回 
 */
//删除指定位置，数量多少个
let ary = [10, 20, 30, 40, 50, 60, 70, 80, 90];
let res = ary.splice(2, 4);
console.log(res, ary); //=>[30, 40, 50, 60]  [10, 20, 70, 80, 90] */

// 基于这种方法可以清空一个数组，把原始数组中的内容以新数组存储起来（有点类似数组的克隆：把原来数组克隆一份一模一样的给新数组）
 res = ary.splice(0);
console.log(res, ary);//=>[10, 20, 70, 80, 90] [] 

// 删除最后一项和第一项
ary.splice(ary.length - 1);
ary.splice(0, 1);
console.log(ary); 

```

- **增加-修改**

```javascript
/*
 * splice : 实现数组的增加、修改
 * @params
 * 	 n,m,x  从索引n开始删除m个元素，用x占用删除的部分
 *   n,0,x  从索引n开始，一个都不删，把x放到索引n的前面
 * @return
 *   把删除的部分用新数组存储起来返回 
 */
let ary = [10, 20, 30, 40, 50];
let res = ary.splice(1, 2, '珠峰培训', '哈哈哈');
console.log(res, ary); //=> [20,30] [10,'珠峰培训','哈哈哈', 40, 50]

// 实现增加
ary.splice(3, 0, '呵呵呵');
console.log(ary); //=>[10, "珠峰培训", "哈哈哈", "呵呵呵", 40, 50]

// 向数组末尾追加
ary.splice(ary.length, 0, 'AAA');

// 向数组开始追加
ary.splice(0, 0, 'BBB'); 

```

#### 实现数组的查询和拼接

> 不会改变原有数组数据

##### 查询

###### slice

```javascript
/*
 * slice : 实现数组的查询
 * @params
 * 	 n,m 都是数字 从索引n开始，找到索引为m的地方（不包含m这一项）
 * @return
 *   把找到的内容以一个新数组的形式返回 
 */
let ary = [10, 20, 30, 40, 50];
let res = ary.slice(1, 3);
console.log(res); //=>[20,30]

// m不写是找到末尾
res = ary.slice(1);
console.log(res); //=>[20, 30, 40, 50]

// 数组的克隆，参数0不写也可以
res = ary.slice(0);
console.log(res); //=>[10, 20, 30, 40, 50]

// 思考：1.如果n/m为负数会咋地，如果n>m了会咋地，如果是小数会咋地，如果是非有效数字会咋地，如果m或者n的值比最大索引都会咋地？ 2.这种克隆方式叫做浅克隆，可以回去先看看深度克隆如何处理! 
```

