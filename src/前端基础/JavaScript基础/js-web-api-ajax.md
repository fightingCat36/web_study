<!--
 * @Author: your name
 * @Date: 2020-03-21 12:09:47
 * @LastEditTime: 2020-03-21 13:41:02
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /web_study/src/前端基础/JavaScript基础/ajax.md
 -->
# XHR(XMLHttpRequest)
```javascript
function ajax (config) {
    const { url, method = 'GET', data = null, isAsync = true } = config
    const p = new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest()
        xhr.onreadystatechange = () => {
            if (xhr.readystate === 4) {
                if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
                    resolve(JSON.parse(xhr.responseText))
                } else {
                    reject`request was unsuccessful: ${xhr.status}`)
                }
            }
        }
        xhr.open(method, url, isAsync)
        xhr.send(data)
    })
    return p
}
```

# 跨域（同源策略）
ajax请求，浏览器要求当前网页和server必须同源（安全），协议，域名，端口都相同

加载图片，css,js无视同源策略

所有的跨域都必须经过server端的运行，否则是浏览器端的漏洞
## JSONP
script标签可以跨域
服务端可以拼接任意数据返回
script可以获得跨域数据，只要服务端允许返回
# CORS(服务端支持)
服务端设置response header: Access-Control-Allow-Origin, Access-Control-Allow-Headers, Access-Control-Allow-Methods,
// 接收跨域的cookie
Access-Control-Allow-Credentials : 'true'