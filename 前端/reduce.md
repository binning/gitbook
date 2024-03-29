# 浅谈JS中 reduce( ) 的用法用法
### 一、语法
```js
arr.reduce(function(prev,cur,index,arr){  
  ...  
  }, init);
```

其中，  
**arr** 表示原数组；  
**prev** 表示上一次调用回调时的返回值，或者初始值 init;  
**cur** 表示当前正在处理的数组元素；  
**index** 表示当前正在处理的数组元素的索引，若提供 init值，则索引为0，否则索引为1；  
**init** 表示初始值。

### 二、实例

现提供一个数组：  
```js
var arr = [2,3,6,8,3,2,6]
```

实现以下需求的方式有很多，其中就包含使用reduce()的求解方式，也算是实现起来比较简洁的一种吧

#### 1.求数组项之和

```js
var sum = arr.reduce(function (prev, cur) {  
    return prev + cur;  
},0);
```

由于传入了初始值0，所以开始时prev的值为0，cur的值为数组第一项3，相加之后返回值为3作为下一轮回调的prev值，然后再继续与下一个数组项相加，以此类推，直至完成所有数组项的和并返回。

#### 2.求数组项最大

```js
var max = arr.reduce(function (prev, cur) {  
    return Math.max(prev,cur);  
});
```

由于未传入初始值，所以开始时prev的值为数组第一项3，cur的值为数组第二项9，取两值最大值后继续进入下一轮回调。

### 3.数组去重

```js
var newArr = arr.reduce(function (prev, cur) {  
    prev.indexOf(cur) === -1 && prev.push(cur);  
    return prev;  
},[]);
```

实现的基本原理如下：

>① 初始化一个空数组  
② 将需要去重处理的数组中的第1项在初始化数组中查找，如果找不到（空数组中肯定找不到），就将该项添加到初始化数组中  
③ 将需要去重处理的数组中的第2项在初始化数组中查找，如果找不到，就将该项继续添加到初始化数组中  
④ ……  
⑤ 将需要去重处理的数组中的第n项在初始化数组中查找，如果找不到，就将该项继续添加到初始化数组中  
⑥ 将这个初始化数组返回

### 三、其他相关方法

#### 1.reduceRight( )

该方法用法与reduce()其实是相同的，只是遍历的顺序相反，它是从数组的最后一项开始，向前遍历到第一项

#### 2.forEach()、map()、every()、some()和filter()

请自行搜索forEach()、map()、every()、some()和filter()的用法

### 重点总结：
>reduce() 是数组的归并方法，与forEach()、map()、filter()等迭代方法一样都会对数组每一项进行遍历，但是reduce() 可同时将前面数组项遍历产生的结果与当前遍历项进行运算，这一点是其他迭代方法无法企及的










