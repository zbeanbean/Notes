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
```
