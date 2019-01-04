# fed-upload-demos

> knowledge module of js uploading

## FileReader & FormData & axios

### file-input属性

* accept
指定选择文件类型的范围。默认为所有文件类型
图片为 accept="image/*"
文件类型取值见MDN

* capture
当文件类型为图片或视频且在移动端时，此属性才有意义。
capture = 'user' 调用前置摄像头
capture = 'environment' 调用后置摄像头
不设置则为用户自己选择

* multiple
一个 Boolean 值，如果存在，则表示用户可以选择多个文件
files
返回一个 FileList，列出每个所选文件对象。除非 multiple 指定了属性，否则此列表只有一个成员。主要用于 JS 操作。

```javascript
// dom
<input type="file" multiple id="imgLocal" accept="image/*" />

// js
let inp = document.querySelector('#imgLocal')
inp.onchange = function(e) {
  let fileList = document.querySelector('#imgLocal').files
  console.log(fileList)
}

```

### FileReader对象

#### FileReader.readyState

表示 FileReader 状态的数字，取值如下
0：EMPTY/还没有加载任何数据
1：LOADING/数据正在被加载
2：DONE/已完成全部的读取请求

#### 事件&方法

* FileReader.onload

> 该事件在读取操作完成时触发

* FileReader.readAsDataURL()

> 开始读取指定的 Blob 中的内容。一旦完成，result 属性中将包含一个 data: URL 格式的字符串以表示所读取文件的内容

```javascript
// 上文的fileList
let file = fileList[0]
const fileReader = new FileReader()
fileReader.readAsDataURL(file) //读取图片
fileReader.addEventListener('load', function() {
  // 读取完成
  let res = fileReader.result
  // res是base64格式的图片
})
```

### FormData对象

* append方法

```javascript
// 继续使用上文的file
const formDate = new FormData()
formDate.append('userPicture', file, '1.jpg')
```

### 使用axios上传

```javascript
let config = {
  headers: {
    'Content-Type': 'multipart/form-data'
  }
}
axios
  .post('serverUrl', formDate, config)
  .then(res => {
    console.log(res)
  })
  .catch(err => {
    console.log(err)
  })
```

## 参考资料

[图片方案详解](https://juejin.im/post/5c2dd1855188257c30462962)