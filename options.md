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

允许将默认URL重写为vtt.js，它可以异步加载到`WebVTT`的 polyfill support。

该选项将用于 Video.js 的“novtt”版本（即`video.novtt.js`）。 其他版本中，vtt.js 与 Video.js 捆绑在一起。

---
## 组件选项

Video.js 播放器是一个组件。像所有组件一样，你可以定义它包含的子项，它们显示的顺序以及传递给它们的选项。

这是一个快速参考; 因此，有关 Video.js 中组件的更多详细信息，请查看[组件指南]()。

### `children`

> 类型：`Array|Object`

如果是一个`Array`——这是该选项的默认值——这是用来确定哪些子组件（通过组件名称）以及它们在播放器（或其他组件）上创建的顺序：

```js
// 以下代码创建一个只有bigPlayButton和controlBar子组件的播放器。
videojs('my-player', {
  children: [
    'bigPlayButton',
    'controlBar'
  ]
});
```

`children`选项也可以作为一个`Object`传递。在这种情况下，它被用来为任何/所有子项提供`options`，包括使用`false`来禁用它们：

```js
// 该播放器的唯一孩子将是controlBar。显然，这不是禁用孙子的理想方法！
videojs('my-player', {
  children: {
    controlBar: {
      fullscreenToggle: false
    }
  }
});
```

### `${componentName}`

> 类型：`Object`

可以通过组件名称的骆驼命名方式（例如用`ControlBar`表示`ControlBar`）为组件提供自定义选项。这些可以嵌套在子代关系的表示中。例如，要禁用全屏控制：

```js
videojs('my-player', {
  controlBar: {
    fullscreenToggle: false
  }
});
```

---
## 技术选项

### `${techName}`

> 类型：`Object`

Video.js 播放技术（即“techs”）可以作为`videojs`函数的选项的一部分自定义选项传递过去。它们应该使用技术名称的小写形式（例如`"flash"`或`"html5"`）传递。

### `flash`

#### `swf`

指定`Flash`技术的 Video.js SWF文件所在的位置：

```js
videojs('my-player', {
  flash: {
    swf: '//path/to/videojs.swf'
  }
});
```

然而，更改全局默认值通常更合适一些：

```js
videojs.options.flash.swf = '//path/to/videojs.swf'
```

### `html5`

#### `nativeControlsForTouch`

> 类型：`boolean`

只有`Html5`技术支持，此选项可以设置为`true`以强制使用触摸设备的原生控件。

#### `nativeAudioTracks`

> 类型：boolean

可以设置为`false`以禁用本地音频轨道支持。最常用于[`videojs-contrib-hls`](https://github.com/videojs/videojs-contrib-hls)。

#### `nativeTextTracks`

> 类型：`boolean`

可以设置为`false`来强制使用仿原生文本轨迹而不是原生支持。 `nativeCaptions`选项也存在，但只是`nativeTextTracks`的别名。

#### `nativeVideoTracks`

> 类型：`boolean`

可以设置为`false`以禁用原生视频轨道支持。最常用于[`videojs-contrib-hls`](https://github.com/videojs/videojs-contrib-hls)。