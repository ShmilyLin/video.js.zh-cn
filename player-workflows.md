# 播放器工作流程

本文档概述了在高级播放器工作流程中使用 Video.js 的许多注意事项。请务必先阅读[设置指南](./setup.md)！

---
## 访问已经在页面上创建的播放器

创建实例后，可以通过两种方式全局访问：

* 通过调用`videojs('example_video_id')`；
* 直接通过`videojs.players.example_video_id`使用；

---
## 移除播放器

不管怎么说，Web应用程序已经变得很普遍。并且不是所有的东西都是一个静态的，加载一次完成的网页了！这意味着开发人员需要能够管理视频播放器的整个生命周期——从创建到销毁。Video.js 通过`dispose()`方法支持移除播放器。

### [`dispose()`]()

此方法适用于所有 Video.js 播放器和组件。它是从DOM和内存中移除 Video.js 播放器唯一支持的方法。例如，用以下代码设置播放器，然后在媒体播放完成时将其丢弃：

```js
var player = videojs('my-player');

player.on('ended', function() {
  this.dispose();
});
```

调用`dispose()`会产生一些影响：

* 触发播放器上的`dispose`事件，允许集成你需要运行的任何自定义清理任务。
* 从播放器中删除所有事件监听器。
* 删除播放器的DOM元素。

此外，这些操作将通过递归应用于*所有*播放器的子组件。

> **注意：** 不要通过标准的DOM删除方法删除播放器：这会将监听和其他对象留在内存中，你可能无法清理！

### 未处理播放器的状况

看到如下错误：

```js
TypeError: this.el_.vjs_getProperty is not a function
```

或者：

```js
TypeError: Cannot read property 'vdata1234567890' of null
```

建议在不使用`dispose()`的情况下从DOM中删除播放器或组件。它通常意味着试图触发事件或调用其上的方法。

---
## 显示和隐藏播放器

不建议你尝试切换 Video.js 播放器的可见性或显示。这样做在Flash技术方面可能会造成问题。相反，播放器应该根据需要创建和[dispose]()。

这与使用情况相关，例如在 modal/overlay 中显示播放器。与其将隐藏的 Video.js 播放器放在DOM元素中，不如在 modal 打开时创建播放器，并在 modal 关闭时对其进行 dispose。

在涉及内存/资源使用情况（例如移动设备）时，这尤其重要。

根据使用的库/框架，实现可能如下所示：

```js
modal.on('show', function() {
  var videoEl = modal.findEl('video');
  modal.player = videojs(videoEl);
});

modal.on('hide', function() {
  modal.player.dispose();
});
```

---
## 改变播放器的音量

播放器的音量可以通过播放器上的`volume`功能进行更改。`volume`功能接受0-1的数字。不带参数调用它将返回当前音量。

例如：

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  // 获取
  var howLoudIsIt = myPlayer.volume();
  // 设置
  myPlayer.volume(0.5); // 设置音量为一半
});
```

音量也可以使用`muted`功能静音（实际上并未改变音量值）。在没有参数的情况下调用它将返回播放器上静音的当前状态。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  // 获取，应该是false
  console.log(myPlayer.muted());
  // 设置为true
  myPlayer.muted(true);
  // 获取到的应该是true
  console.log(myPlayer.muted());
});
```

---
## 让播放器全屏

要检查播放器当前是否全屏，请像这样调用播放器上的`isFullscreen`功能。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  // 获取，应该是false
  console.log(myPlayer.isFullscreen());

  // 设置，告诉播放器进入全屏
  myPlayer.isFullscreen(true);

  // 获取，应该是true
  console.log(myPlayer.isFullscreen());
});
```

想要播放器进入全屏请调用`requestFullscreen`。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  myPlayer.requestFullscreen();
});
```

想要退出全屏请调用`exitFullscreen`。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  myPlayer.requestFullscreen();
  myPlayer.exitFullscreen();
});
```

---
## 使用播放信息功能

`play`可以使一个具有信号源的播放器开始播放。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  myPlayer.play();
});
```

`pause`可以使一个正在播放的播放器暂停播放。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  myPlayer.play();
  myPlayer.pause();
});
```

`paused`可以被用来确定一个播放器是否正在暂停。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');

myPlayer.ready(function() {
  // true
  console.log(myPlayer.paused());
  // false
  console.log(!myPlayer.paused());

  myPlayer.play();
  // false
  console.log(myPlayer.paused());
  // true
  console.log(!myPlayer.paused());

  myPlayer.pause();
  // true
  console.log(myPlayer.paused());
  // false
  console.log(!myPlayer.paused());
});
```

`currentTime`将为你提供当前正在播放的currentTime（以秒为单位）。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  // 将当前时间设置为视频中的2分钟
  myPlayer.currentTime(120);

  // 获得当前时间，应该是120秒
  var whereYouAt = myPlayer.currentTime();
});
```

`duration`会给你正在播放的视频的总持时长。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  var lengthOfVideo = myPlayer.duration();
});
```

`remainingTime`会给你视频中剩余的秒数。

```js
var myPlayer = videojs('some-player-id');
myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
   myPlayer.currentTime(10);

   // 应该比duration少10秒
   console.log(myPlayer.remainingTime());
});
```

`buffered`将为你提供一个timeRange对象，表示当前可以继续播放的时间范围。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  var bufferedTimeRange = myPlayer.buffered();

  // 已经缓冲了不同时间范围的数量。
  // 一般情况是1
  var numberOfRanges = bufferedTimeRange.length,

  // 第一个范围开始时的时间（秒）。
  // 通常是0
  var firstRangeStart = bufferedTimeRange.start(0),

  // 第一个范围结束时的时间（秒）
  var firstRangeEnd = bufferedTimeRange.end(0),

  // 第一时间范围内的长度（以秒为单位）
  var firstRangeLength = firstRangeEnd - firstRangeStart;
});
```

`bufferedPercent`会为你提供当前视频缓冲的百分比。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
  // 例如0.11又名11％
  var howMuchIsDownloaded = myPlayer.bufferedPercent();
});
```

---
## 处理播放器上的来源或海报

通过API将视频源传递给播放器。（这也可以使用选项来完成）

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
```

**源对象（或元素）：** 包含有关源文件信息的javascript对象。如果你希望播放器使用类型信息确定它是否可以支持该文件，请使用此方法。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src({type: "video/mp4", src: "http://www.example.com/path/to/video.mp4"});
```

**源对象数组**：为了提供多个版本的视频源，以便可以跨浏览器使用HTML5播放，你可以使用源对象数组。Video.js 将检测哪个版本被支持并加载该文件。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src([
  {type: "video/mp4", src: "http://www.example.com/path/to/video.mp4"},
  {type: "video/webm", src: "http://www.example.com/path/to/video.webm"},
  {type: "video/ogg", src: "http://www.example.com/path/to/video.ogv"}
]);
```

通过API更改或设置海报。（这也可以通过选项来完成）

```js
var myPlayer = videojs('example_video_1');

// 设置
myPlayer.poster('http://example.com/myImage.jpg');

// 获取
console.log(myPlayer.poster());
// 'http://example.com/myImage.jpg'
```

---
## 在播放器上访问Tech

播放器的技术只能通过将`{IWillNotUseThisInPlugins: true}`放入播放器的`tech()`函数中来实现。

```js
var myPlayer = videojs('some-player-id');

myPlayer.src('http://www.example.com/path/to/video.mp4');
myPlayer.ready(function() {
   // 如果不传入参数，tech()将会报错
   var tech = myPlayer.tech({IWillNotUseThisInPlugins: true});
});
```

---
## 和其他框架一起使用 Video.js

敬请期待...

### jQuery

### React

请参阅[ReactJS集成示例]()

### Ember

### Angular