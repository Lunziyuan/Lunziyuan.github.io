---
layout: post
title: JS Event Loop
---

&ensp;&ensp;&ensp;&ensp;最近看到一道关于Js Event Loop的面试题，如下：

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

&ensp;&ensp;&ensp;&ensp;..to be continue..
