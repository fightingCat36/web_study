<!--
 * @Author: your name
 * @Date: 2020-03-17 22:18:38
 * @LastEditTime: 2020-03-17 22:32:50
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /web_study/src/前端基础/JavaScript基础/js-web-api-events.md
 -->
# 事件绑定
```javascript
dom.addEventListener()
```
# 事件冒泡
```javascript
event.stopPropagation()
```
# 事件代理

#  问题
1. 编写一个通用的事件监听函数
```javascript
function bindEvent (ele, type, fn) {
    ele.addEventListener(type, fn)
}
```
2. 事件冒泡流程
3. 无限下拉图片列表，如何监听每个图片的点击

