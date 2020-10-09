```js
// 分配一个16字节的连续的内存空间，并用0预填充
let buffer = new ArrayBuffer(16);
alert(buffer.byteLength);
```

ArrayBuffer 与 Array 没有任何共同之处

- 它的长度是固定的，无法增加或减少它的长度
- 它正好占用那么多空间
- 要访问单个字节，需要另一个“试图”对象，而不是 buffer\[index\]

**要操作 ArrayBuffer，我们需要使用“视图”对象**

视图对象并不存储任何东西，只是一副“眼睛”，透过它来解释存储 ArrayBuffer 中的字节

- TypedArray

  - Uint8Array Uint16Array Uint32Array 用于 8 位、16 位和 32 位无符号整数
  - Uint8ClampedArray 用于 8 位整数，在赋值时便“固定”其值
  - Int8Array Int16Array，Int32Array —— 用于有符号整数（可以为负数）
  - Float32Array Float64Array —— 用于 32 位和 64 位的有符号浮点数

- DataView 使用方法来指定格式的视图

![arrayBuffer](https://zh.javascript.info/article/arraybuffer-binary-arrays/arraybuffer-views.svg)

##### TypedArray

视图的超类`TypedArray`，参数共有 5 种变体

```js
new TypedArray(buffer, [byteOffset], [length]); // 截取ArrayBuffer一部分
new TypedArray(object); // 参数是数组或类数组对象，则会创建一个相同长度的类型化数组，并复制内容
new TypedArray(typedArray); // 创建一个相同长度的类型化数组并复制内容，可能会类型转换
new TypedArray(length); // 创建指定长度的类型化数组，最后的字节长度是 `length * BYTES_PER_ELEMENT`
new TypedArray(); // 创建0长度的类型化数组
```

除了以一种提供 ArrayBuffer, 之外的都将自动创建 ArrayBuffer,要访问 ArrayBuffer 可以使用 `.buffer`,`.byteLength`,因此可以一个视图转另外一个视图。

```js
let arr8 = new Uint8Array([0, 1, 2, 3, 4]);
let arr16 = new Uint16Array(arr8.buffer);
```

##### 越界行为

256 放入 Uint8Array ,结果是 0；257 放入 Uint8Array,结果是 1.

![8bit-integer-256](https://zh.javascript.info/article/arraybuffer-binary-arrays/8bit-integer-256.svg)
![8bit-integer-257](https://zh.javascript.info/article/arraybuffer-binary-arrays/8bit-integer-257.svg)

```js
let arr8 = new Uint8Array(16);
let num = 256;
console.log(num.toString(2));

arr8[0] = 256;
arr8[1] = 257;

console.log(arr8[0]);
console.log(arr8[1]);
```

##### TypedArray 方法

1. 遍历（iterate）
2. map
3. slice
4. reduce
5. set
6. subarray

##### DataView 用于从同一 buffer 提取不同格式数字

```js
new DataView(buffer, [byteOffset], [byteLength]);
```

```js
// 4 个字节的二进制数组，每个都是最大值 255
let buffer = new Uint8Array([255, 255, 255, 255]).buffer;

let dataView = new DataView(buffer);

// 在偏移量为 0 处获取 8 位数字
alert(dataView.getUint8(0)); // 255

// 现在在偏移量为 0 处获取 16 位数字，它由 2 个字节组成，一起解析为 65535
alert(dataView.getUint16(0)); // 65535（最大的 16 位无符号整数）

// 在偏移量为 0 处获取 32 位数字
alert(dataView.getUint32(0)); // 4294967295（最大的 32 位无符号整数）

dataView.setUint32(0, 0); // 将 4 个字节的数字设为 0，即将所有字节都设为 0
```

##### 汇总

![arraybuffer-view-buffersource](https://zh.javascript.info/article/arraybuffer-binary-arrays/arraybuffer-view-buffersource.svg)

```js
// 将二维数组中数据转为一个Uint8Array
function concat(arrays) {
  const length = arrays.reduce((acc, cur) => (acc += cur.length), 0);
  const arr8 = new Uint8Array(length);
  let offset = 0;
  for (let array of arrays) {
    arr8.set(array, offset);
    offset += array.length;
  }
  return arr8;
}
```
