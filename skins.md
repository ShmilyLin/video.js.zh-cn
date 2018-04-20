# 皮肤

---
## 默认皮肤

当您包含Video.js CSS文件（`video-js.min.css`）时，将使用默认的Video.js外观。 这意味着自定义Video.js播放器的外观是利用CSS的级联方式来覆盖样式的。

---
## 其他`<style>`元素

除了Video.js CSS文件外，还有一些样式由JavaScript自动生成，并作为<style>元素包含在<head>中。

“vjs-styles-defaults”元素为页面上的所有Video.js播放器设置默认尺寸。
为页面上的每个玩家创建“vjs-styles-dimensions”元素，并用于调整其大小。 这种样式以这种方式处理，使您可以使用自定义CSS覆盖它，而无需依赖脚本或！重要的是克服内联样式。

In addition to the Video.js CSS file, there are some styles generated automatically by JavaScript and included in the <head> as <style> elements.

The "vjs-styles-defaults" element sets default dimensions for all Video.js players on the page.
A "vjs-styles-dimensions" element is created for each player on the page and is used to adjust its size. This styling is handled in this manner to allow you to override it with custom CSS without relying on scripting or !important to overcome inline styles.