### Upload 的 大小问题 ###
使用css属性zoom 类似于transform的scale 可以直接等比例缩小upload，而不用去upload的所有嵌套的div去一个个缩小宽高

### Upload 的 customRequest 使用fetch的用法 ###
```
customRequest={(data) => {
  const { action, file, onSuccess } = data
  // onSuccess(new Response(), file)
  fetch(action, {
    method: 'get',
    credentials: 'include',
  }).then((response) => {
    const contentType = response.headers.get('Content-Type')
    console.log(response, contentType)
    onSuccess(response, file) // 这个回掉需要真正的请求成功的response才能是的upload预览出新的加载图片
  })
}}

// 写成这样也能直接显示出file的预览，猜测问题的关键是要有个异步调用success
customRequest={(data) => {
  const { action, file, onSuccess } = data
  new Promise((resolve) => {
    const response = new Response()
    resolve(response) 
  }).then((response) => {
    onSuccess({}, file)
  })
}
```
