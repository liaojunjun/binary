#### 构造函数

![blob](https://zh.javascript.info/article/blob/blob.svg)

```js
new Blob(blobParts, options);
```

- blobParts 是 Blob/BufferSource/String 类型的值的数组

- options { type:MIME 类型，endings 换行：transparent | native }

```js
// 从类型化数组（typed array）和字符串创建 Blob
let hello = new Uint8Array([72, 101, 108, 108, 111]); // 二进制格式的 "hello"

let blob = new Blob([hello, " ", "world"], { type: "text/plain" });
```

slice 方法来提取 Blob 片段

```js
blob.slice([byteStart], [byteEnd], [contentType]);
```

#### Blob 用作 URL

```js
let link = document.createElement("a");
link.download = "hello.txt";

let blob = new Blob(["Hello, world!"], { type: "text/plain" });

link.href = URL.createObjectURL(blob);

link.click();

URL.revokeObjectURL(link.href); // 内部映射中移除引用,释放内存
```

> 浏览器内部为每个通过 URL.createObjectURL 生成的 URL 存储了一个 URL → Blob 映射。因此，此类 URL 很短，但可以访问 Blob

#### Blob 转换为 base64

```js
let link = document.createElement("a");
link.download = "hello.txt";

let blob = new Blob(["Hello, world!"], { type: "text/plain" });

let reader = new FileReader();
reader.readAsDataURL(blob); // 将 Blob 转换为 base64 并调用 onload

reader.onload = function () {
  link.href = reader.result; // data url
  link.click();
};
```

| URL.createObjectURL(blob)                                              | Blob 转换为 data url                                                     |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| 如果介意内存，我们需要撤销（revoke）它们直接访问 Blob，无需“编码/解码” | 无需撤销（revoke）任何操作。对大的 Blob 进行编码时，性能和内存会有损耗。 |

#### Image 转换为 blob

```js
canvas.toBlob(function (blob) {
  // blob 创建完成，下载它
  let link = document.createElement("a");
  link.download = "example.png";

  link.href = URL.createObjectURL(blob);
  link.click();

  // 删除内部 blob 引用，这样浏览器可以从内存中将其清除
  URL.revokeObjectURL(link.href);
}, "image/png");
```

#### Blob 转换为 ArrayBuffer

```js
// 从 blob 获取 arrayBuffer
let fileReader = new FileReader();

fileReader.readAsArrayBuffer(blob);

fileReader.onload = function (event) {
  let arrayBuffer = fileReader.result;
};
```
