# ArrayBuffer
* ArrayBuffer对象用来表示通用的、固定长度的原始二进制数据缓冲区
* 它是一个字节数组，通常在其他语言中称为 `byte array`
* 你不能直接操作 ArrayBuffer 的内容，而是要通过 `TypedArray`（类型数组对象）或 `DataView` 对象来操作，它们会将缓冲区中的数据表示为特定的格式，并通过这些格式来读写缓冲区的内容。

```js
// 创建一个长度为8个字节的buffer
const buffer = new ArrayBuffer(8)
console.log(buffer.byteLength) // 64, 8个字节*8个位=64位
```
# TypedArray
* TypedArray对象描述了一个底层的二进制数据缓冲区(binary data buffer)的一个类数组视图(view)

> 但它本身不可以被实例化，甚至无法访问，你可以把它理解为接口,它有很多的实现

类型|单个元素值的范围|大小(bytes)|描述
---|---|---|---
Int8Array|-128 to 127|1|8位二进制有符号整数
Uint8Array|0 to 255|1|8位无符号整数
Int16Array|-32768 to 32767|2|16位二进制有符号整数
Uint16Array|0 to 65535|2|16位无符号整数

```js
const buffer = new ArrayBuffer(8)
console.log(buffer.byteLength) // 8
const int8Array = new Int8Array(buffer)
console.log(int8Array.length) // 8，数组的长度，1个字节表示一个整数
const int16Array = new Int16Array(buffer)
console.log(int16Array.length) // 4，数组的长度，2个字节表示一个整数
```
# DataView对象
* DataView视图是一个可以从二进制ArrayBuffer对象中读写多种数值类型的底层接口
* `setInt8()` 从DataView起始位置以byte为计数的指定偏移量(byteOffset)处储存一个8-bit数(一个字节)
* `getInt8()` 从DataView起始位置以byte为计数的指定偏移量(byteOffset)处获取一个8-bit数(一个字节)

```js
new DataView(buffer [, byteOffset [, byteLength]])
```

```js
const buffer = new ArrayBuffer(8)
console.log(buffer.byteLength) // 8个字节

const view1 = new DataView(buffer)

view1.setInt8(0, 1)
console.log(view1.getInt8(0)) // 1

view1.setInt8(1, 2)
console.log(view1.getInt8(1)) // 2
console.log(view1.getInt16(0)) // 258
```
# Blob
* Blob对象表示一个不可变、原始数据的类文件（类似文件）对象。Blob 表示的不一定是JavaScript原生格式的数据。
* File 接口基于Blob，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。
* Blob() 构造函数返回一个新的 Blob 对象。 blob的内容由参数数组中给出的值的串联组成。
* FileReader 对象允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 File 或 Blob 对象指定要读取的文件或数据。
  * readAsText()：读取文本文件（可以使用txt打开的文件），返回文本字符串，默认编码是UTF-8
  * readAsDataURL()：读取文件获取一段以data开头的字符串，这段字符串的本质就是DataURL，DataURL是一种将文件嵌入到文档的方案。DataURL是将资源转换为base64编码的字符串形式,并且将这些内容直接储存在url中
* 构造函数 var aBlob = new Blob( array, options );
  * array 是一个由ArrayBuffer, ArrayBufferView, Blob, DOMString 等对象构成的 Array ，或者其他类似对象的混合体，它将会被放进 Blob。DOMStrings会被编码为UTF-8。
  * options 是一个可选的BlobPropertyBag字典
    * type 默认值为 "",它代表了将会被放入到blob中的数组内容的MIME类型

```js
const user = { name: 'codepan' }
const userString = JSON.stringify(user)
console.log('userString', userString)

const blob = new Blob([userString], { type: 'application/json' })
console.log('blob.size', blob.size)

// 同样一个blob可以被读成各种类型
const blobTypes = {
  arrayBuffer: 'arrayBuffer',
  dataURL: 'dataURL',
  text: 'text'
}

function readBlob(blob, type) {
  return new Promise(resolve => {
    const reader = new FileReader()
    reader.onload = event => resolve(event.target.result)

    switch (type) {
      case blobTypes.arrayBuffer:
        reader.readAsArrayBuffer(blob)
        break
      case blobTypes.dataURL:
        reader.readAsDataURL(blob)
        break
      case blobTypes.text:
        reader.readAsText(blob,'UTF-8')
        break
      default:
        break
    }
  });
}
readBlob(blob, blobTypes.arrayBuffer).then(buffer => console.log('buffer', buffer))
readBlob(blob, blobTypes.dataURL).then(base64String => console.log("base64String", base64String))
readBlob(blob, blobTypes.text).then(text => console.log('text', text))
```
# Object URL
* 可以使用浏览器新的 API URL 对象通过方法生成一个地址来表示 Blob 数据
* 格式为 blob:<origin>/<uuid>
* URL.createObjectURL 静态方法会创建一个 DOMString，其中包含一个表示参数中给出的对象的URL。这个 URL 的生命周期和创建它的窗口中的 document 绑定。这个新的URL 对象表示指定的 File 对象或 Blob 对象。
* revokeObjectURL 静态方法用来释放一个之前已经存在的、通过调用 URL.createObjectURL() 创建的 URL 对象

```js
function download () {
  const user = { name: 'codepan' }
  const userString = JSON.stringify(user)
  const blob = new Blob([userString], { type: 'application/json' })

  const objectURL = URL.createObjectURL(blob)
  const a = document.createElement('a')
  a.download = 'hello.json'
  a.rel = 'noopener'
  a.href = objectURL
  a.click()
  URL.revokeObjectURL(objectURL)
}
```