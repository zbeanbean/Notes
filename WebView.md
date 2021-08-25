### webview issues ##

在react-native中使用webview postmessage的时候h5没有监听到,[stackoverflow回答](https://stackoverflow.com/questions/41160221/react-native-webview-postmessage-does-not-work)
```
window.removeEventListener('message', handleMessage)
document.removeEventListener("message", handleMessage)
```
