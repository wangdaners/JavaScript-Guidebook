## History对象的方法

### history.go()

使用 `history.go()` 方法可以在用户的历史记录中任意跳转，可以向后也可以向前。这个方法接受一个参数，表示向后或向前跳转的页面数的一个整数值。负数表示向后跳转（类似于单击浏览器的“后退”按钮），正数表示向前跳转（类似于单击浏览器的“前进”按钮）。

```javascript
// 后退一页
history.go(-1);

// 前进一页
history.go(1);

// 前进两页
history.go(2);
```

也可以给 `go()` 方法传递一个字符串参数，此时浏览器会跳转到历史记录中包含该字符串的第一个位置——可能后退，也可能前进，具体要看哪个位置最近。如果历史记录中不包含该字符串，那么这个方法什么也不做。

```js
// 跳转到最近的 github.com 页面
history.go("github.com");
```

当`history.go()` 方法无参数时，相当于 `history.go(0)`，可以刷新当前页面

```javascript
// 刷新当前页面
history.go();

// 刷新当前页面
history.go(0);
```

### history.back()

`history.back()` 方法用于模仿浏览器的后退按钮，相当于 `history.go(-1)`

```js
// 下面两种写法效果一致
history.back();
history.go(-1);
```

### history.forward()

`forward()` 方法用于模仿浏览器的前进按钮，相当于 `history.go(1)`

```javascript
// 后退一页
history.back()
// 前进一页
history.forward()
```

如果移动的位置超出了访问历史的边界，以上三个方法并不报错，而是静默失败 。

使用历史记录时，页面通常从浏览器缓存之中加载，而不是重新要求服务器发送新的网页。

### history.pushState()

向当前浏览记录栈中添加一条新的历史记录，添加后页面不会重新加载。

```js
history.pushState(state, title, url)
```

- state：**用于存储该 URL 对应的状态对象**。该对象可通过 `history.state` 或 `popstate` 事件回调中的 event 对象获取。如果不需要这个对象，此处可以填 null。
- title：**新页面的标题**，但是所有浏览器目前都忽略这个值，因此这里可以填 null。
- url：**URL 地址**，不允许跨域。这个参数可选，如果它没有被特别标注，会被设置为文档的当前URL。

`history.pushState` 函数向浏览器的历史堆栈压入一个url为设定值的记录，并改变历史堆栈的当前指针至栈顶。

调用 `pushState()` 方法将新生成一条历史记录，方便用浏览器的“后退”和“前进”来导航（“后退”可是相当常用的按钮）。另外，从URL的同源策略可以看出，HTML5 history API 的出发点是很明确的，就是让无跳转的单站点也可以将它的各个状态保存为浏览器的多条历史记录。当通过历史记录重新加载站点时，站点可以直接加载到对应的状态。

### history.replaceState()

它和 `history.pushState()` 方法基本相同，区别只有一点，**`history.replaceState()` 不会新生成历史记录，而是将当前历史记录替换掉，常用于落地页**。

```js
history.replaceState( state, title, url);
```

### window.onpopstate

push 的对立就是 pop，可以猜到这个事件是在浏览器取出历史记录并加载时触发的。但实际上，它的条件是比较苛刻的，几乎只有**点击浏览器的“前进”、“后退”这些导航按钮，或者是由 JavaScript 调用的 `history.back()` 等导航方法**，且**切换前后的两条历史记录都属于同一个网页文档**，才会触发本事件，因为这些操作有一个共性，即修改了历史堆栈的当前指针。

上面的“同一个网页文档”请理解为 JavaScript 环境的 `document` 是同一个，而不是指基础 URL（去掉各类参数的）相同。也就是说，只要有重新加载发生（无论是跳转到一个新站点还是继续在本站点），JavaScript 全局环境发生了变化，`popstate` 事件都不会触发。

`popstate` 事件是设计出来和前面的2个方法搭配使用的。一般只有在通过前面2个方法设置了同一站点的多条历史记录，并在其之间导航（前进或后退）时，才会触发这个事件。同时，前面2个方法所设置的状态对象（第1个参数），也会在这个时候通过事件的 `event.state` 返还回来。

此外请注意，`history.pushState()` 及 `history.replaceState()` 本身调用时是不触发 `popstate` 事件的。pop和push毕竟不一样！

```js
window.onpopstate = function(event) {
  alert("location: " 
    + document.location 
    + ", state: " 
    + JSON.stringify(event.state));
};
```

### 浏览器兼容性

| Feature                | Chrome | Firefox（Gecko） | Internet Explorer | Opera | Safari |
| ---------------------- | ------ | ---------------- | ----------------- | ----- | ------ |
| replaceState.pushState | 5      | 4.0（2.0）       | 10                | 11.50 | 5.0    |
| history.state          | 18     | 4.0（2.0）       | 10                | 11.50 | 6.0    |

---

资料参考（深入了解）：

- [「前端」History API与浏览器历史堆栈管理](https://github.com/ShowJoy-com/showjoy-blog/issues/2)