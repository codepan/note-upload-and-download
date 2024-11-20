<!--
 * @项目名称: monitor-resource-web
 * @文件名: filename
 * @版权保护声明: tecent.Co.Ltd
 * @内容描述: file content
 * @创建者: ppennzhou
 * @创建时间: 2021-07-28 11:48:23
 * @修订记录: 
-->
Object URL
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