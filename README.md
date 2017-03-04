# Immutable用法笔记
## 重要的API
### fromJS()
fromJS() 是最最最常用的将原生JS数据转换为ImmutableJS数据的转换方法。使用方式类似于 JSON.parse()，接收两个参数：json 数据和 reviver函数。在不传递reviver函数的情况下，默认将原生JS的Array转为List，Object转为Map.

```
const a1 = Immutable.fromJS({a: {b: [10, 20, 30]}, c: 40});

Immutable.Map({key: "value"}).toJS()); // {key: "value"}
```
### toJS()
转化为普通的JavaScript类型数据
类似JSON.stringify();

### Map
Map 数据类型，对应原生 Object 数组。最最常用的 数据结构之一

```
const a =Immutable.Map().set("QQ", '232323');
a.get("QQ");

const b = Immutable.Map([["key", "value"], ["key", "value2"], ["key1", "value1"]]);
console.log(b.toJS());
//会去掉重复的key值，{key: "value2", key1: "value1"}
```

### List
List 数据类型，对应原生 Array 数组。

```
console.log(Immutable.List().toJS());
//[]
console.log(Immutable.List([1,2,4,{a:"www"}]).toJS());
// [ 1, 2,  4, {a: "www"}]
```
### List.of()
创建新的list
```
console.log(Immutable。List.of({x:1}, 2, [3], 4).toJS()); // [{x:1}, 2, [3], 4]
```

### Map.of()
根据键值对，创建新的map对象
```
Immutable.Map.of(
  'key', 'value',
  'numerical value', 3,
   0, 'numerical key'
).toJS();
// { '0': 'numerical key', key: 'value', 'numerical value': 3 }
```

### 判断
判断是否是一个Map , 对原生Object不生效

```
console.log(Immutable.Map.isMap({})); // false
console.log(Immuable.Map.isMap(Map({}))); // true
```
判断是否是一个List , 对原生Array不生效

```
console.log(Immuable.List.isList([])); // false
console.log(Immuable.List.isList(List([]))); // true
```

### 获取大小

#### size
```

// map
console.log(Immuable.Map({key: "value2", key1: "value1"}).size);// 2
console.log(Immuable.Map.of({x:1}, 2, [3], 4).size);// 2
// list
console.log(Immuable.List([1,2,3,4]).size);// 4
console.log(Immuable.List.of(1, 2, 3, 4).size);// 4


```
#### count()


```
console.log(Immutable.fromJS({key: "value2", key1: "value1"}).count());// 2
```
可以传入函数进行判断

```
console.log(Immutable.fromJS({key: 1, key1: 23}).count((value, key, obj) => {
    return value > 3;
}));// 1 value大于3的有1个
console.log(Immutable.fromJS([1, 2, 5, 6]).count());// 4
```
#### countBy()

```
//map
console.log(Immutable.fromJS({key:console.log(Immutable.fromJS({key: 1, key1: 23,key2:2}).countBy((value, key, obj) => {
    return value > 3;
}).toJS());// {false: 2, true: 1}

// list
console.log(Immutable.fromJS([1, 2, 5, 6]).countBy((value, index, array) => {
    return value > 3;
}).toJS());// {false: 2, true: 2}
```

### 添加元素
#### Set


```
//map
const a1 = Immutable.Map({a: {a1: 1}, b: 2, c: 3, d: 4});
console.log(a1.set('a', 1).toJS()); // {a: 1, b: 2, c: 3, d: 4}
console.log(a1.set('e', 5).toJS());  // {a: {a1: 1}, b: 2, c: 3, d: 4,e:5}
```

```
/ List
// 将 index 位置的元素替换为 value
const arr1 = Immutable.List([1, 2, 3]);
console.log(arr1.set(2, 0).toJS()); // [1, 2, 0]  
console.log($arr1.set(4, 0).toJS());
```
#### SetIn()


```
console.log(Immutable.fromJS([1, 2, 3, {a: 4, b: 5}]).setIn(['3', 'a'], 1000).toJS());
//[1, 2, 3, {a: 4444, b: 5}]
```

#### List 添加元素


```
// 向 index 位置插入 value
console.log(Immutable.fromJS([1, 2, 3]).insert(1, 1.5).toJS()); // [ 1, 1.5, 2, 3 ]
```

#### pop、push、shift、unshift
这四种操作方法，和原生Array的四种方法使用方式一致.但唯一区别就是返回新的List，并且不改变原来的数组本身，而原生则是会改变元素本身。



```
const a = Immutable.List([1, 2, 3, 4]);
console.log(a.pop().toJS(), a.toJS());
// [1, 2, 3] [1, 2, 3, 4]
```

#### delete(key)

```
// delete(index: number)
//或者key值
console.log(Immutable.fromJS([1, 2, 3]).delete(1).toJS())

console.log(Immutable.fromJS({a: {a1: 34}, b: 2, c: 3, d: 444}).delete('c').toJS())
```

#### clear

```
console.log(Immutable.fromJS([1, 2, 3]).clear().toJS())
//[]
console.log(Immutable.fromJS({a: {a1: 34}, b: 2, c: 3, d: 444}).clear().toJS());
// {}

```

#### update

```
const originalMap = Immutable.Map({
  key: 'value'
});

const newMap = originalMap.update('key', value => {
  return value + value;
});
newMap.toJS(); // { key: 'valuevalue' }

const originalMap = Immutable.Map({
  key: 'value'
});

let newMap = originalMap.update('noKey', 'no value', value => {
  return value + value;
});
newMap.toJS(); // { key: 'value', noKey: 'no valueno value' }
```

```
const $obj1 = Immutable.fromJS({a: 1, b: 2, c: 3, d: 444});
console.log($obj1.update('e',"else", (value)=> {
    return value+"e";
}).toJS(),
```
#### get getIn

```
const a = Immutable.fromJS([1111111, 22222, {a: 333333}]);
console.log(a.get(0));
// 1111111   只有数组可以用 number 类型 的key
```


```
const a = Immutable.fromJS({a: {a1: 1111}, b: 2, c: 3, d: 444});
console.log(a.getIn(['a', 'a1'])); // 1111
```
#### find() 返回 value

```
console.log(Immutable.fromJS([1, 2, 56, {a: {b: 111}}]).find((value, index, array) => {
    return index === 3;
}).toJS());// {a: {b: 111}}

// Map
console.log(Immutable.fromJS({a: {a1: 222}, b: 2, c: 3, d: 444}).find((value, key, obj) => {
    return key === "b";
}));// 3
```
#### findKey()、findLastKey() 返回 key



```
// List
Immutable.fromJS([1, 2, 3, {a: {b: 111}}]).findKey((value, index, array) => {
    return index === 3;
});// 3

// Map
Immutable.fromJS({a: {a1: 222}, b: 2, c: 3, d: 444}).findKey((value, key, obj) => {
    return value === 3;
});// c
```
#### findEntry()、findLastEntry()
返回 key:value。


```
Immutable.fromJS([1, 2, 3, {a: {b: 111}}]).findEntry((value, index, array) => {
    return index === 3;
});// [3, Map]

// Map
Immutable.fromJS({a: {a1: 222}, b: 2, c: 3, d: 444}).findEntry((value, key, obj) => {
    return Immutable.is(value, Immutable.fromJS({a1: 222}));
});// ["a", Map]
```
#### indexOf()

```
Immutable.fromJS([1, 2, 3, {a: {b: 111}}]).indexOf(Immutable.fromJS({a: {b: 111}})); // 3
```

#### findIndex()

```
Immutable.fromJS([1, 2, 3, {a: {b: 111}}]).findIndex((value, index, array) => {
    return value/3 === 1;
}); // 2
```

#### maxBy()


```
Immutable.fromJS([{a: 2}, {a: 1}, {a: 2301}, {a: 222}]).maxBy((value, index, array) => {
    return value.get('a');
}).toJS();// {a: 2301}

Immutable.fromJS([{a: 2}, {a: 1}, {a: 2301}, {a: 222}]).maxBy((value, index, array) => {
    return value.get('a');
}, (valueA, valueB) => {
    return valueA > valueB;
}).toJS();// {a: 2301}

```

#### slice()


```
Immutable.fromJS([1, 2, 3]).slice(0).toJS();// [1, 2, 3]
```
#### map()


```
Immutable.fromJS([1, 2, 3, 4, 5]).map((value, index, array)=>{
    return value * 2;
}).toJS(); // [2, 4, 6, 8, 10]
```
#### filter()
```
Immutable.fromJS([1, 2, 3, 4, 5]).filter((value, index, array)=>{
    return value % 2 === 0;
}).toJS(); // [2, 4]
```
####  every()
```

Immutable.fromJS([1, 2, 3, 4, 5]).every((value, index, array)=>{
    return value > 2;
}); // false
```

#### some()

```
Immutable.fromJS([1, 2, 3, 4, 5]).some((value, index, array)=>{
    return value > 2;
}); // true
```
#### mapKeys()

```
Immutable.fromJS({a: 5, b: 2, c: 3, d: 444}).mapKeys((key)=>{
    return key + 'hhh';
}).toJS();
// {ahhh: 5, bhhh: 2, chhh: 3, dhhh: 444}
```
#### mapEntries()


```
Immutable.fromJS({a: 5, b: 2, c: 3, d: 444}).mapEntries(([key, value])=>{
    return [key + 'aaa', value+'hhhh'];
}).toJS();//   {aaaa: "5hhhh", baaa: "2hhhh", caaa: "3hhhh", daaa: "444hhhh"}


```


#### merge

```
const a =Immutable.fromJS([1, 2, 3, 4, {a: {b: 55, c: 66}}]);
const b =Immutable.fromJS([1, 2, 3, 4, {a: {b: 333, d: 67}}]);
a.merge(b)
//[1, 2, 3, 6, {b: 333, d: 67}]
深merge
a.mergeDeep(b)
//[1, 2, 3, 6, {b: 333, c: 66, d: 67}]

```
#### jonin()

```
Immutable.fromJS([1, 2, 3, {a: 123, b: 321}]).join();

//1,2,3,Map { "a": 123, "b": 321 }
```
#### has() hasIn()
检查是否有某个key


```
Immutable.fromJS([1, 2, 3, {a: 123, b: 321}]).has('0'); // trueconsole.log(Immutable.fromJS([1, 2, 3, {a: 123, b: 321}]).hasIn([3, 'b']); // true
```

#### includes()

```
Immutable.fromJS([6, 5, 4, 3, 2, 1, 7]).includes('7')
//false
Immutable.fromJS([6, 5, 4, 3, 2, 1, '7']).contains('7')
//true

Immutable.fromJS({b: 2, a: {a1: 222, a3: 456}, c: 3, d: 333}).includes('89')
//false
```
#### isSubset()


```
Immutable.fromJS([6, 5, 1, [6, 5, 4]]).isSubset(Immutable.fromJS([[6, 5, 4], 6, 5, 4, 3, 2, 1, '89']));
// true
```
#### reverse()

```
Immutable.fromJS([1, 2, 3, 4, 5, 6]).reverse().toJS();
// [6, 5, 4, 3, 2, 1]
```

#### sort()和sortBy()



#### flatten()

```
Immutable.fromJS([1, 2, 3, 4, [1, 11, 111, 12344], {a: 1234, b: {bb: [777, 888]}}, 5, 6]).flatten().toJS()
// [1, 2, 3, 4, 1, 11, 111, 12344, 1234, 777, 888, 5, 6]
```

#### groupBy()


```
Immutable.fromJS([{v: 0}, {v: 1}, {v: 1}, {v: 0}, {v: 1}])
  .groupBy(x => x.get('v'))
  // Map {0: [{v: 0},{v: 0}], 1: [{v: 1},{v: 1},{v: 1}]}
```

#### flip()

```
Immutable.fromJS({b: 'b1', a: 'a1', c: 'c1', d: 'd1'}).flip().toJS();
// {b1: "b", a1: "a", c1: "c", d1: "d"}
```
#### concat()

```
 const a = Immutable.fromJS([1, 2, 3, 4, 5, 6]);
const b = Immutable.fromJS([111, 222, 333, 444, 555, 666]);
a.concat(b).toJS();
////[1, 2, 3, 4, 5, 6, 111, 222, 333, 444, 555, 666]
```
