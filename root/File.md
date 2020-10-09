File 对象继承自 Blob，并扩展了与文件系统相关的功能
有两种方法获取它。

##### 构造器

```js
new File(fileParts, fileName, [options]);
```

- fileParts Blob/BufferSource/String 类型值的数组
- fileName 文件名字符串
- options {lastModified : 最后一次修改的时间戳}

##### input[type='file']获取

```js
function showFile(input) {
  let file = input.files[0];

  alert(`File name: ${file.name}`); // 例如 my.png
  alert(`Last modified: ${file.lastModified}`); // 例如 1552830408824
}
```
