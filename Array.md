# 关于数组身上的方法和属性总结

* concat() 用于连接两个或多个数组，不会改变原有数组
    > arrayObject.concat(arrayX,arrayX,......,arrayX)

    > arrayX必须传入,该参数可以是具体的值，也可以是数组对象。可以是任意多个。

    > 返回一个新的数组。该数组是通过把所有 arrayX 参数添加到 arrayObject 中生成的。如果要进行 concat() 操作的参数是数组，那么添加的是数组中的元素，而不是数组。

    ``` javascript
    var a = [1,2,3];
    a.concat(1,2);
    //a = [1,2,3,1,2]在尾部添加

    a.concat([4,5]);
    //a = [1,2,3,4,5]在尾部添加
    ```

* constructor属性返回对创建此对象的数组函数的引用

    ``` javascript
    object.constructor
    var a = [1,2,3];
    console.log(a.constructor == Array);
    //true

    var test=new Array();

    if (test.constructor==Array){
        document.write("This is an Array");
    }
    if (test.constructor==Boolean){
        document.write("This is a Boolean");
    }
    if (test.constructor==Date){
        document.write("This is a Date");
    }
    if (test.constructor==String){
        document.write("This is a String");
    }
    ```
* copyWithin()方法
    > 在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组。

    > arrayObject.copyWithin(target,start,end);

    > target必传，从该位置开始替换数据

    > start可选，从该位置开始读取数据，默认为 0 。如果为负值，表示倒数。

    > end可选，到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。

    ``` javascript
    //  将 3 号位复制到 0 号位
    [1, 2, 3, 4, 5].copyWithin(0, 3, 4)
    // [4, 2, 3, 4, 5]
    // -2 相当于 3 号位， -1 相当于 4 号位
    [1, 2, 3, 4, 5].copyWithin(0, -2, -1)
    // [4, 2, 3, 4, 5]
    //  将 3 号位复制到 0 号位
    [].copyWithin.call({length: 5, 3: 1}, 0, 3)
    // {0: 1, 3: 1, length: 5}
    //  将 2 号位到数组结束，复制到 0 号位
    var i32a = new Int32Array([1, 2, 3, 4, 5]);
    i32a.copyWithin(0, 2);
    // Int32Array [3, 4, 5, 4, 5]
    //  对于没有部署 TypedArray 的 copyWithin 方法的平台
    //  需要采用下面的写法
    [].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4);
    // Int32Array [4, 2, 3, 4, 5]
    ```
* entries()方法，试验性方法，后期补充

* every()方法
    > 用于检测数组所有元素是否都符合指定条件（通过函数提供）。

    > 用指定函数检测数组中的所有元素：

    > 如果数组中检测到有一个元素不满足，则整个表达式返回 false ，且剩余的元素不会再进行检测。

    > 如果所有元素都满足条件，则返回 true。

    **every() 不会对空数组进行检测。**

    **every() 不会改变原始数组。**

    ``` javascript
    array.every(function(currentValue,index,arr), thisValue)

    //参数function(currentValue, index,arr)，必选，
        //currentValue必选，当前元素的值
        //index可选，当期元素的索引值
        //arr，可选，当期元素属于的数组对象
    //thisValue可选，对象作为该执行回调时使用，传递给函数，用作 "this" 的值。如果省略了 thisValue ，"this" 的值为 "undefined"
    ```
* fill()方法给数组填充一个具体的值，返回一个修改过数组，会改变原数组

    > arr.fill(value,start,end);

    > value 必选，不选会填充undefined

    > start 可选，开始fill的下标，默认为0

    > end 可选，结束fill的下标，默认为this.length

    ``` javascript
    [1, 2, 3].fill();                //[undefined, undefined, undefined]
    [1, 2, 3].fill(4);               // [4, 4, 4]
    [1, 2, 3].fill(4, 1);            // [1, 4, 4]
    [1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
    [1, 2, 3].fill(4, 1, 1);         // [1, 2, 3]
    [1, 2, 3].fill(4, -3, -2);       // [4, 2, 3]
    [1, 2, 3].fill(4, NaN, NaN);     // [1, 2, 3]
    Array(3).fill(4);                // [4, 4, 4]
    [].fill.call({ length: 3 }, 4);  // {0: 4, 1: 4, 2: 4, length: 3}
    ```
* filter方法，创建一个满足条件的新数组

    **不会对空数组进行检测**

    **不会改变原数组**

    **返回数组，包含了符合条件的所有元素。如果没有符合条件的元素则返回空数组**

    > array.filter(function(currentValue,index,arr), thisValue)

    > function(currentValue, index,arr),必选，函数，过滤条件

        > currentValue必选，当前元素的值

        > index可选，当期元素的索引值

        > arr可选，当期元素属于的数组对象

    > thisValue 把数组当做参数传进去，this指向数组，如果不传this为undefined

    ``` javascript
    //var newArray = arr.filter(callback[, thisArg]);

    function isBigEnough(value) {
      return value >= 10;
    }

    var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
    // filtered is [12, 130, 44]
    ```
* find()方法，寻找数组中满足条件的第一个值，如果没有，返回undefined

    > arr.find(callback(element,index,array),thisArg])，参数同上

    ``` javascript
    function isBigEnough(element) {
      return element >= 15;
    }

    [12, 5, 8, 130, 44].find(isBigEnough); // 130
    ```
* findIndex方法
