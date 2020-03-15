# 深入javascript异步与promise，async-await原理

## 一. JavaScript引擎和运行时
在开始深入```JavaScript```异步原理之前，首先先了解下什么是```JavaScript```引擎(```engine```)以及运行时(```runtime```)。

### 1.1 引擎

```Javascript```引擎，比如说，```Javascript```代码如果运行在```Chrome```浏览器，这个时候的的```Javascript```引擎就是```v8```引擎，如果是运行在其它浏览器，比如```safari```(```Javascript```引擎是```nitro```),```IE```(```Javascript```引擎是```chakra```),```Mozilla```(```Javascript```引擎是```spidermonkey```或者```Rhino```),```node```端(```Javascript```引擎是```v8```),都有着不同的```JavaScript```引擎。

```JavaScript```引擎的作用就是把我们写的```JavaScript```代码转换为机器码，来让计算机最终执行我们的逻辑。```ECMA```规定来```JavaScript```的语法和```API```，但是语法和```API```的具体实现，是交由不同浏览器自己的引擎来完成的，也就是说，同样一段```JavaScript```代码，在不同浏览器，由于用来实现的引擎不同，实现的方式也是有一定区别的。不同浏览器只是需要保证，对于同样的```JavaScript```语法或者```API```,表现出来的结果应该是一样的。

```v8```引擎和其它现代浏览器的```JavaScript```引擎一样，并不使用解释器（```interpreter```），而是在执行代码的时候，使用一个```JIT（Just In Time）```编译器（```compiler```），将```JavaScript```代码转换为机器码，这样能够获得更快的运行速度。v8引擎和其它现代浏览器```JavaScript```引擎的在编译```JavaScript```代码成为机器码的过程中，一个主要区别在于，v8引擎不会生成字节码```（bytecode）```和中间代码```（intermediate code）```

### 1.2 运行时
运行时，通俗来讲，就是```JavaScript```引擎在运行的时侯所处的执行环境。不同的运行时为```JavaScript```提供了很多不同的特性。比如，浏览器环境中，```JavaScript```运行时，提供了像是```dom```事件操作等特性,这些特性，```JavaScript```引擎（比如v8）是不具备的

## 二. JavaScript单线程
首先，我们回顾下基本概念：线程（```thread```）是操作系统能够进行运算调度的最小单位。它被包含在进程之中，是进程中的实际运作单位。

所谓单线程，我们可以很通俗的理解为，JavaScript程序，在同一时间只能做一件事情，无法并行的进行多件事情。

需要明确一点：```JavaScript```是单线程的，指的是```JavaScript```代码在执行的时候，是处在单线程中执行，但是对于```JavaScript```的运行时，不是处在单线程的。```JavaScript```运行时内部是存在线程池的，其由运行时自身控制。

## 三. 事件循环（```Event-loop```）
因为```JavaScript```程序，在执行时是运行在单线程上的，就会有一个问题，当程序运行的时候，如果遇到一个非常耗时间的流程，程序便会暂停执行这段流程，导致处于一种卡死的状态，这显然是不合理的。所以，```JavaScript```引入了异步这种机制，来解决单线程所带来的问题。

要想搞清楚JavaScript的异步机制，就需要弄明白JavaScript的事件循环（```Event-loop```）机制。

先看一段代码
```javascript
console.log(1)
function test (num) {
    console.log(2)
}
test(num)
console.log(3)
```
上面这段代码输出结果显而易见，因为是完全同步的过程，结果是
```javascript
1
2
3
```
我们把上述代码改造一下

```javascript
console.log(1)
function test (num) {
    setTimeout(() => {
        console.log(2)
    })
}
test(num)
console.log(3)
```
这个时候，执行test函数时，由于setTimeout定时器的使用（异步），似的这部分内部被推入一个任务队列，JavaScript单线程首先跳过test的执行，先执行完后面同步代码，等到同步代码都执行结束后，在从异步事件队列中取出之前放进队列的test任务，执行里面的逻辑，所以最后输出的结果顺序为
```javascript
1
3
2
```
