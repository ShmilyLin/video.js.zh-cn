# 皮肤

---
## 默认皮肤

当您包含Video.js CSS文件（`video-js.min.css`）时，将使用默认的Video.js外观。 这意味着自定义Video.js播放器的外观是利用CSS的级联方式来覆盖样式的。

---
## 其他`<style>`元素

除了 Video.js CSS 文件外，还有一些样式由JavaScript自动生成，并作为`<style>`元素包含在`<head>`中。

* `"vjs-styles-defaults"`元素为页面上的所有 Video.js 播放器设置默认尺寸。
* 为页面上的每个播放器创建一个`"vjs-styles-dimensions"`元素，并用于调整其大小。这种样式以这种方式处理，使你可以使用自定义CSS覆盖它，而无需依赖脚本或`!important`的修改内联样式。

### 禁用其他`<style>`元素

在某些情况下，特别是使用可能管理`<head>`元素的框架的Web应用程序（例如React，Ember，Angular等），这些`<style>`元素是不可获得。在包含 Video.js 之前，可以通过设置`window.VIDEOJS_NO_DYNAMIC_STYLE = true`来抑制它们。

*这会禁用所有基于CSS来设置的播放器大小。播放器在默认情况下将没有`height`或`width`！* 即使是尺寸属性，例如`width="600" height="300"`也会被忽略。当使用这个全局设置的时候，你需要以一种对他们的网站或网络应用有效果的方式来设置他们自己的尺寸。

#### `Player#width()`对`Player#height()`的影响

当设置`VIDEOJS_NO_DYNAMIC_STYLE`时，`Player#width()`和`Player#height()`将应用直接设置到`<video>`元素（或当前技术使用的任何元素）的任何宽度和高度。

---
## 图标

Video.js 附带了许多通过图标字体内置到皮肤中的图标。

你可以通过将[`sandbox/icons.html.example`](https://github.com/videojs/video.js/blob/master/sandbox/icons.html.example)重命名为`sandbox/icons.html`，使用`npm run build`构建 Video.js ，然后在你选择的浏览器中打开`sandbox/icons.html`来查看默认外观中可用的所有图标。

---
## 创建一个皮肤

建议通过覆盖默认皮肤提供的样式来创建皮肤。这样，你不需要从头开始。

### 给播放器添加一个自定义Class

在播放器中为你的皮肤创建钩子的最方便的方法是向它添加一个class。你可以通过向最初的`<video>`元素添加一个class来实现：

```html
<video class="vjs-matrix video-js">...</video>
```

或者通过JavaScript：

```js
var player = videojs('my-player');

player.addClass('vjs-matrix');
```

> **注意：** `vjs-`前缀是在 Video.js 播放器中引用的所有class的约定。

### 自定义样式

用自定义样式覆盖默认样式的第一步是确定哪些选择器和属性需要覆盖。作为一个例子，假设我们不喜欢控件的默认颜色（白色），我们希望将它们更改为明亮的绿色（例如`#00ff00`）。

为此，我们将使用浏览器的开发人员工具来检查播放器并找出我们需要使用哪些选择器来调整这些样式——然后我们将添加自定义的`.vjs-matrix`选择器以确保我们的最终选择器足够具体的覆盖默认外观。

在这种情况下，我们需要以下内容：

```css
/* 改变播放器的所有文字和图标的颜色。 */
.vjs-matrix.video-js {
  color: #00ff00;
}

/* 改变大的播放按钮的边框。*/
.vjs-matrix .vjs-big-play-button {
  border-color: #00ff00;
}

/* 改变各种"bars"的颜色。*/
.vjs-matrix .vjs-volume-level,
.vjs-matrix .vjs-play-progress,
.vjs-matrix .vjs-slider-bar {
  background: #00ff00;
}
```

最后，我们可以将其保存为`videojs-matrix.css`文件并将其在 Video.js CSS 之后引用：

```html
<link rel="stylesheet" type="text/css" href="path/to/video-js.min.css">
<link rel="stylesheet" type="text/css" href="path/to/videojs-matrix.css">
```

如果你创建了一个你特别引以为傲的皮肤，你可以通过在[Skins wiki page](https://github.com/videojs/video.js/wiki/Skins)上添加一个链接来分享它。创建可分享皮肤的一种方法是[在CodePen上 forking 这个例子](http://codepen.io/heff/pen/EarCt)。
