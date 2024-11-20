<!--
 * @项目名称: monitor-resource-web
 * @文件名: filename
 * @版权保护声明: tecent.Co.Ltd
 * @内容描述: file content
 * @创建者: ppennzhou
 * @创建时间: 2021-07-28 11:16:53
 * @修订记录: 
-->
TypedArray对象描述了一个底层的二进制数据缓冲区(binary data buffer)的一个类数组视图(view)

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