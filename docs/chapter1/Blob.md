* Blob对象表示一个不可变、原始数据的类文件（类似文件）对象。Blob 表示的不一定是JavaScript原生格式的数据。File 接口基于Blob，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。
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