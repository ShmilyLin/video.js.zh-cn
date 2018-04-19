# Video.js 选项参考

> **注意：** 本文仅作为可用选项的参考。要了解将选项传递给 Video.js，请参阅[设置指南](./setup.md)。

---
## 标准`<video>`元素选项

这些选项中的每一个都可以作为[标准的`<video>`元素属性](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video#Attributes)使用; 因此，可以按照[设置指南](./setup.md)中概述的所有三种方式来定义它们。通常情况下，默认值没有列出，因为这是留给浏览器供应商的。

### `autoplay`

> 类型：`boolean`

如果属性为 true / present，则在播放器准备就绪时就开始播放。

> **注意：** 从iOS 10开始，Apple在Safari中提供自动播放支持。有关详细信息，请参阅“[New `<video>` Policies for iOS](https://webkit.org/blog/6784/new-video-policies-for-ios/)”

### `controls`

> 类型：`boolean`

确定播放器是否拥有用户可以与之交互的控件。如果没有控件，开始视频播放的唯一方法是使用`autoplay`属性或通过播放器API。

### `height`

> 类型：`string|number`

设置视频播放器的显示高度，单位是像素。

### `loop`

> 类型：`boolean`

视频一结束就重新开始。

### `muted`

> 类型：`boolean`

默认情况下将会静音任何音频。

### `poster`

> 类型：`string`

视频开始播放前显示的图片的网址。这通常是视频的框架或自定义标题屏幕。只要用户点击“播放”，图像就会消失。

### `preload`

> 类型：`string`

建议浏览器在加载`<video>`元素后立即开始下载视频数据。支持的值是：

* **`auto`**
  
  立即开始加载视频（如果浏览器支持）。一些移动设备不会预加载视频，以保护用户的带宽/数据使用。这就是为什么这个值被称为“auto”，而不是像“true”那样更确定的原因。

  *这往往是最常见和推荐的价值，因为它允许浏览器选择最佳行为。*

* **`metadata`**

  只加载视频的元数据，其中包括视频的时长和尺寸等信息。有时，元数据将通过下载几帧视频来加载。

* **`none`**

  不要预加载任何数据。浏览器将等待用户点击“播放”开始下载。

### `src`

> 类型：`string`

要嵌入的视频源的源URL。

### `width`

> 类型：`string|number`

设置视频播放器的显示宽度，单位是像素。

---
## Video.js 特有选项

除非另有说明，否则每个选项默认都是`undefined`。

### `aspectRatio`

> 类型：

### ``

> 类型：`string`

将播放器置于[`fluid`](#fluid)模式，并在计算播放器的动态大小时使用该值。该值应表示比率——用冒号分隔的两个数字（例如`"16:9"`或`"4:3"`）。

### `children`

> 类型：`Array|Object`

该选项从[`Component`基类](#)继承。

### `fluid`

> 类型：`boolean`

如果为`true`，则 Video.js 播放器将具有流体大小。换句话说，它将扩展以适应其容器。

另外，如果`<video>`元素具有`"vjs-fluid"`，则此选项会自动设置为`true`。

### `inactivityTimeout`

> 类型：`number`

Video.js 表示用户正在通过class`"vjs-user-active"`和`"vjs-user-inactive"`以及`"useractive"`事件与玩家进行交互。

`inactivityTimeout`确定在声明用户不活动之前需要多少毫秒的非活动时间。 值为`0`表示不存在`inactivityTimeout`，并且用户永远不会被视为不活动。

### `language`

> 类型：`string`，默认值：浏览器默认或`'en'`

一种与播放器可用语言相匹配的[语言代码](http://www.iana.org/assignments/language-subtag-registry/language-subtag-registry)。这为播放器设置了初始语言，但它总是可以改变的。

了解更多关于[Video.js中的语言]()的信息。

> **注意：** 一般来说，这个选项是不需要的，将自定义语言传递给`videojs.addLanguage()`会更好，这样它们在所有播放器中都可用！

### `nativeControlsForTouch`

> 类型：`boolean`

为[关联技术选项]()明确设置默认值。

### `notSupportedMessage`

> 类型：`string`

设置后将覆盖 Video.js 无法播放媒体源时显示的默认消息。

### `playbackRates`

> 类型：`Array`

一个元素数量必须大于0的数组，其中1表示常规速度（100％），0.5表示半速（50％），2表示双倍速度（200％）等。如果指定，Video.js 将显示一个控件（class是`vjs-playback-rate`），允许用户从一系列选项中选择播放速度。选项按照从下到上的指定顺序显示。

例如：

```js
videojs('my-player', {
  playbackRates: [0.5, 1, 1.5, 2]
});
```

### `plugins`

> 类型：`Object`

这支持在播放器初始化时使用自定义选项自动初始化插件——而不是要求你手动初始化它们。

```js
videojs('my-player', {
  plugins: {
    foo: {bar: true},
    boo: {baz: false}
  }
});
```

以上大致相当于：

```js
var player = videojs('my-player');

player.foo({bar: true});
player.boo({baz: false});
```

由于`plugins`选项是一个对象，所以不能保证初始化的顺序！

有关Video.js插件的更多信息，请参阅[插件指南]()。

### `sources`

> 类型：`Array`

反映本地`<video>`元素具有的一系列子元素`<source>`元素的对象数组。 这应该是一个具有`src`和`type`属性的对象数组。例如：

```js
videojs('my-player', {
  sources: [{
    src: '//path/to/video.mp4',
    type: 'video/mp4'
  }, {
    src: '//path/to/video.webm',
    type: 'video/webm'
  }]
});
```

使用`<source>`元素将具有相同的效果：

```html
<video ...>
  <source src="//path/to/video.mp4" type="video/mp4">
  <source src="//path/to/video.webm" type="video/webm">
</video>
```

### `techCanOverridePoster`

> 类型：`boolean`

可以覆盖播放器的海报并融入播放器海报的生命周期。这在使用多种技术时很有用，每次播放新源时都必须设置自己的海报。

### `techOrder`

> 类型：`Array`，默认值：`['html5']`

定义 Video.js 技术优先的顺序。默认情况下，意味着Html5技术是首选。其他注册的技术将按照他们注册的顺序在此技术之后添加。

### `vtt.js`

> 类型：`string`

Allows overriding the default URL to vtt.js, which may be loaded asynchronously to polyfill support for WebVTT.

This option will be used in the "novtt" build of Video.js (i.e. video.novtt.js). Otherwise, vtt.js is bundled with Video.js.
