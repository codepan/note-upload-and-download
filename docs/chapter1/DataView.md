<!--
 * @项目名称: monitor-resource-web
 * @文件名: filename
 * @版权保护声明: tecent.Co.Ltd
 * @内容描述: file content
 * @创建者: ppennzhou
 * @创建时间: 2021-07-28 11:24:20
 * @修订记录: 
-->
DataView对象
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