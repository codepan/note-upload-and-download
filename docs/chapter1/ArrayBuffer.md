ArrayBuffer对象用来表示通用的、固定长度的原始二进制数据缓冲区

* 它是一个字节数组，通常在其他语言中称为byte array
* 你不能直接操作 ArrayBuffer 的内容，而是要通过 `TypedArray`（类型数组对象）或 `DataView` 对象来操作，它们会将缓冲区中的数据表示为特定的格式，并通过这些格式来读写缓冲区的内容。

```js
// 创建一个长度为8个字节的buffer
const buffer = new ArrayBuffer(8)
console.log(buffer.byteLength) // 64, 64位=8*8
```