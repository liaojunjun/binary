FileReader 是一个对象，其唯一目的是从 Blob（因此也从 File）对象中读取数据

```js
let reader = new FileReader(); // 没有参数
```

- readAsArrayBuffer(blob) —— 将数据读取为二进制格式的 ArrayBuffer。
- readAsText(blob, [encoding]) —— 将数据读取为给定编码（默认为 utf-8 编码）的文本字符串。
- readAsDataURL(blob) —— 读取二进制数据，并将其编码为 base64 的 data url。
- abort() —— 取消操作。

读取过程中，有以下事件：

- loadstart —— 开始加载。
- progress —— 在读取过程中出现。
- load —— 读取完成，没有 error。
- abort —— 调用了 abort()。
- error —— 出现 error。
- loadend —— 读取完成，无论成功还是失败。

读取完成后，我们可以通过以下方式访问读取结果：

- reader.result 是结果（如果成功）
- reader.error 是 error（如果失败）。

在很多情况下，我们不必读取文件内容。就像我们处理 blob 一样，我们可以使用 URL.createObjectURL(file) 创建一个短的 url，并将其赋给 a 或 img。这样，文件便可以下载文件或者将其呈现为图像，作为 canvas 等的一部分
