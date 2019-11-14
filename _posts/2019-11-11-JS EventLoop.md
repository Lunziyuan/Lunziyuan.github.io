---
layout: post
title: JS EventLoop
---

&ensp;&ensp;&ensp;&ensp;最近看到一道关于Js执行机制的面试题，如下：

{% highlight js %}
// 请说出下面代码的执行顺序
async function async1() {
  console.log(1);
  const result = await async2();
  console.log(3);
}
async function async2() {
  console.log(2);
}
Promise.resolve().then(()=>{
  console.log(4);
});
setTimeout(()=>{
  console.log(5);
});
async1();
console.log(6);
{% endhighlight %}

&ensp;&ensp;&ensp;&ensp;众所周知，js是一门单线程的语言。那么单线程要怎么实现支持异步而不阻塞进程呢？主要就是靠js的事件循环机制。js的编译器在编译一段代码的时候，自上而下，根据事件的执行顺序，将其加入到“执行栈”，然后从头开始执行。如果是异步事件，并不会一直等待它的结果返回，而是继续执行栈中的下一个事件，等到异步事件的结果返回的时候，将异步事件的回调放入“事件队列”中。如果“执行栈”中的事情都执行完成后，js会把“事件队列”中的第一个事件（也就是异步的回掉）放入“执行栈”，然后执行其内容。如此往复，就是js的事件循环机制。

![eventLoop](/assets/img/eventLoop.jpg){:height="400px" width="400px"}

&ensp;&ensp;&ensp;&ensp;图中的stack表示我们所说的执行栈，web apis则是代表一些异步事件，而callback queue即事件队列。

&ensp;&ensp;&ensp;&ensp;另外值得注意的一点，异步事件根据其类型，被分为“微任务”和“宏任务”，其回调的执行先后顺序不同。“微任务”的回调先于“宏任务”的回调。只有当“微任务”的回调全部执行完毕之后，才会执行“宏任务”的回调。

以下事件属于宏任务：
* setInterval()
* setTimeout()

以下事件属于微任务：
* new Promise()
* new MutaionObserver()

&ensp;&ensp;&ensp;&ensp;再来看上面的问题，console.log(3)是async1的回调。
先是同步事件按顺序执行，输出： 1 、2 、6。
再是“微任务”，输出： 4 、3。
最后再是“宏任务”，输出：5。
答案是：1、2、6、4、3、5。



