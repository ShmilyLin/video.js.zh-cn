# 设置

## 获取Video.js

Video.js 通过 CDN 和 npm 提供。

Video.js 不仅可以使用 HTML 标签`<script>`和`<link>`，还可以使用所有主流捆绑器/打包器/构建器，如 Browserify，Node，WebPack 等。

有关详细信息，请参阅[开始](http://videojs.com/getting-started/)文档。

---
## 创建一个播放器

> **注意：** Video.js 适用于`<video>`和`<audio>`元素，但为了简单起见，我们将仅引用`<video>`元素。

一旦[你的页面上加载了](http://videojs.com/getting-started/) Video.js，你就可以创建一个播放器了！

Video.js 的核心优势在于它装饰了一个[标准的`<video>`元素](http://www.w3.org/TR/html5/embedded-content-0.html#the-video-element)并模拟了它的相关[事件和 API](https://www.w3.org/2010/05/video/mediaevents.html)，同时提供了一个可定制的基于 DOM 的 UI。

Video.js 支持`<video>`元素的所有属性（如`controls`，`preload`等），但它也支持[它自己的选项](http://docs.videojs.com/tutorial-setup.html#options)。有两种方法可以创建 Video.js 播放器并传递选项，但它们都以具有属性`class="video-js"`的标准`<video>`元素开头：

```html
<video class="video-js">
  <source src="//vjs.zencdn.net/v/oceans.mp4" type="video/mp4">
  <source src="//vjs.zencdn.net/v/oceans.webm" type="video/webm">
</video>
```

要深入了解所有各种嵌入选项，请查看[嵌入页面]()，然后按照该页面的其余部分进行操作。

### 自动设置

默认情况下，当您的网页完成加载时，Video.js 将扫描具有`data-setup`属性的媒体元素。`data-setup`属性用于将选项传递给 Video.js。一个最简单的例子看起来像这样：

```html
<video class="video-js" data-setup='{}'>
  <source src="//vjs.zencdn.net/v/oceans.mp4" type="video/mp4">
  <source src="//vjs.zencdn.net/v/oceans.webm" type="video/webm">
</video>
```

> **注意：** 你在`data-setup`中必须使用单引号，因为它将使用 JSON。

### 手动配置

在现代网页上，当页面加载完成时，`<video>`元素通常不存在。 在这些情况下，自动设置是不可能的，但通过[`videojs`函数]()可以手动设置。

调用此函数的一种方法是提供一个匹配`<video>`元素`id`属性的字符串：

```html
<video id="my-player" class="video-js">
  <source src="//vjs.zencdn.net/v/oceans.mp4" type="video/mp4">
  <source src="//vjs.zencdn.net/v/oceans.webm" type="video/webm">
</video>
```

```javascript
videojs('my-player');
```

但是，使用`id`属性并不一定总行得通; 所以，`videojs`函数也可以接受一个 DOM 元素：

```html
<video class="video-js">
  <source src="//vjs.zencdn.net/v/oceans.mp4" type="video/mp4">
  <source src="//vjs.zencdn.net/v/oceans.webm" type="video/webm">
</video>
```

```js
videojs(document.querySelector('.video-js'));
```

### 获取已存在的播放器引用

一旦播放器被创建，Video.js 会在内部跟踪它们。 有几种方法可以获得对已有播放器的引用。

**使用 `videojs`**

使用已有播放器的元素ID调用`videojs()`将返回该播放器，并且不会创建另一个播放器。

如果没有播放器与参数匹配，它将尝试创建一个新的播放器。

**使用 `videojs.getPlayer()`**

有时，你想获得一个没有调用`videojs()`的潜在播放器引用。 这可以通过用对应的元素的ID或元素本身的字符串来调用`videojs.getPlayer()`。

**使用 `videojs.getPlayers()` 或者 `videojs.players`**

`videojs.players`属性可以获得所有已知的播放器。该方法——`videojs.getPlayers()`只是返回相同的对象。

播放器使用它的 ID 相作为 key 存储在此对象上。

> **注意：** 如果创建的播放器是一个没有ID的元素，则将被分配一个自动生成的ID。

---
## 选项

> **注意：** 本指南仅介绍如何在播放器设置过程中传递选项。有关所有可用选项的完整参考资料，请参阅[选项指南]()。

有三种方法可以将选项传递给 Video.js。 由于 Video.js 修饰了 HTML5 的`<video>`元素，因此许多可用选项也可用作标准`<video>`标签属性：

```html
<video controls autoplay preload="auto" ...>
```

或者，你可以使用`data-setup`属性将选项作为 JSON 传入。这也是你对非标准的`<video>`元素如何设置选项：

```html
<video data-setup='{"controls": true, "autoplay": false, "preload": "auto"}'...>
```

> **注意：** 你必须对`data-setup`的值使用单引号，因为它将包含必须使用双引号的JSON字符串。

最后，如果你没有使用`data-setup`属性来触发播放器设置，你可以将一个播放器选项对象作为第二个参数传递给`videojs`函数：

```js
videojs('my-player', {
  controls: true,
  autoplay: false,
  preload: 'auto'
});
```

> **注意：** 不要同时使用`data-setup`和选项对象。

### 全局默认值

所有播放器的默认选项都可以在`videojs.options`中找到，并可以直接更改。例如，要为之后所有的播放器设置`{autoplay：true}`：

```js
videojs.options.autoplay = true;
```

### 关于`<video>`标签属性的注意事项

许多属性都是所谓的[布尔属性](https://www.w3.org/TR/2011/WD-html5-20110525/common-microsyntaxes.html#boolean-attributes)。这意味着它们可以打开或关闭。在这种情况下，这个属性应该没有值（或者应该有它的名字作为它的值）——这个属性存在就意味着值为`true`，没有这个属性就意味着值为`false`。

*这些不正确：*

```html
<video controls="true" ...>
<video loop="true" ...>
<video controls="false" ...>
```

这些是正确的：

```html
<video controls ...>
<video loop="loop" ...>
<video ...>
```

---
## 播放器准备就绪

由于 Video.js 技术有可能异步加载，安装后立即与播放器进行交互并不总是安全的。出于这个原因，Video.js 播放器有一个“准备就绪”的概念，以前使用过 jQuery 的人都会很熟悉。

基本上，可以为 Video.js 播放器定义任何数量的就绪回调。有三种方法可以传递这些回调。在每个例子中，我们将向播放器添加一个相同的类：

将一个回调函数作为第三个参数传递给`videojs()`函数：

```js
// 给options参数传入 `null`。
videojs('my-player', null, function() {
  this.addClass('my-example');
});
```

将回调传递给播放器的`ready()`方法：

```js
var player = videojs('my-player');

player.ready(function() {
  this.addClass('my-example');
});
```

监听播放器的“ready”事件：

```js
var player = videojs('my-player');

player.on('ready', function() {
  this.addClass('my-example');
});
```

在上面的每种情况，回调都是异步调用的。

上述方法之间的一个重要区别是，在播放器准备好之前，必须先用`on()`给`ready`添加监听。使用`player.ready()`时，如果播放器已经准备就绪，则立即调用该函数。

---
## 播放器工作流程进阶

有关更深层次的播放器工作流程的讨论，请参阅[播放器工作流程指南](./player-workflows.md)。
