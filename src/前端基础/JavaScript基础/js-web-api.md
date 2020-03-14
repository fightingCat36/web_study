<!--
 * @Author: your name
 * @Date: 2020-03-12 21:25:38
 * @LastEditTime: 2020-03-14 21:56:23
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /web_study/src/前端基础/JavaScript基础/js-web-api.md
 -->
# DOM(Document Object Model) ---- 文档对象模型

# DOM的本质
dom是一棵从html文件解析出的树形结构，特殊的XML，规定了标签

# DOM节点

# DOM操作
1. 获取节点
```javascript
document.getElementById()
document.getElementsByTagName()
document.getElemenetsByClassName()
document.querySelector()
document.querySelectorAll()
```
2. 获取属性
```javascript
const p1 = document.querySelector('.p1')
p1.getAttribute('data-name')
p1.setAttribute('data-id', '007')
p1.style.width = '400px'
p1.nodeType
p1.nodeName
```

3. 新增，删除，插入
```html
<html>
    <body>
        <div class="div">
        </div>
    </body>
</html>
<script>
    const body = document.querySelector('body')
    const div = document.querySelector('div')
    const p = document.createElement('p')
    p.innerText = 'hello world!'
    //  插入
    body.appendChild(p)
    // 移动
    div.appendChild(p)
    // 父节点
    console.log(p.parentNode)
    // 子节点
    console.log(body.childNodes)
    // 过滤得到dom节点
    const nodes = Array.prototype.slice.call(body.childNodes).filter(node => {
        return node.nodeType === 1 ? true : false
    })
    // 删除
    body.removeChild(nodes[node.length - 1])
</script>
```
# DOM操作性能



# DOM相关问题
## 1. DOM是哪种数据结构

## 2. DOM操作常用API

## 3. attribute和property的区别
attribute设置的是标签上可见的属性，property是js设置dom对象属性，标签上不可见（都有可能造成重绘重排）
## 4. 高性能的一次性插入多个dom节点

