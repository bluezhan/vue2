
# Vue.js快速介绍-超级马里奥像素艺术

> 原文出处：[Quick Introduction to Vue.js — Super Mario Pixel Art](https://medium.com/nepfin-engineering/quick-introduction-to-vue-js-super-mario-pixel-art-5ad01440663)

::代码我已经归纳在github上：[vue2-pixel-art](https://github.com/itemsets/vue2-pixel-art)::

![](https://github.com/itemsets/vue2-pixel-art/raw/master/image/1.jpeg)

最近有人问我为什么选择使用Vue.js来实现我们公司的第一款产品。哈哈哈，并不是每个人都有机会去探索Vue.js的；使用在这里，我先可以通过写一个简简单单的Vue.js例子来快速介绍它，将让大家对Vue.js有着很好认识和了解，希望这些能给大家有所帮助。

![](https://github.com/itemsets/vue2-pixel-art/raw/master/image/2.gif)

绘制图形可能不是Vue.js最受欢迎的用例，甚至很多市场上的Demo都很少有关绘制图形的；但在这篇文章中，我想用绘制图形来举一个例子，我想其他人已经在github上发现这个非常乐趣并好玩的动sai -- 超级马里奥像素艺术（灵感来自Github[Data-Pixels](https://github.com/gmattie/Data-Pixels)），它绘制许多像素，当点击其中一个像素时，周边相似的都会随之而改变。
哦，我们这里不是使用canvas来说实现的，是使用了div。

在这里我使用了Vue.js来改写，用Vue.js的方法来绘制和更新颜色，感觉超级棒棒的。

### 构建两个Vue组件

在开始编写代码之前，我就构建了两个Vue组件来实现这个图形：
- pixel.vue
  pixel.vue是一个组件（这里放着每个小小像素单位）；
  的具有`color`（RGB值）和`size`（像素大小）的两个数据，当它被点击并触发事件，就是通知其父组件并也将会触发一个事件。
- canvas.vue
  是一个基于具有每个像素的颜色的二维数组初始化像素分量的容器。

![](https://github.com/itemsets/vue2-pixel-art/raw/master/image/3.png)

### pixel.vue

```html
<template>
    <div class="l-pixel" :style="pixelStyle" @click="onPixelClick">
    </div>
</template>

<script>
    export default {
        props: { // could be "props: ['color', 'size']" if there no need for "type" and "required".
            color: {
                // RGB color - i.e. "255, 255, 255"
                // applied in "pixelStyle()" computed data
                type: String,
                required: true,
            },
            size: {
                type: String,
                required: true
            }
        },
        computed: {
            pixelStyle() { // style binding in template
                return {
                    'background-color': `rgb(${this.color})`, // this.color is "color" in "props"
                    'width': this.size,
                    'height': this.size
                }
            }
        },
        methods: {
            onPixelClick() {  // click event handler
                this.$emit('pixel-click', this.color)
            }
        }
    }
</script>

<style scoped>
    .l-pixel {
        display: inline-block;
    }
</style>

```

`.vue` 文件可以包含`template`模板、JavaScript和CSS样式块，因此组件的所有必需代码都可以存在于单个文件中。

在script标签中，color(background color)和size(pixel size)是组件初始化时传递的必需属性（props）。

如果没有必要指定type，并required不在props有要求，那它可以简化为props: ['color', 'size']。属性的值应用于pixelStyle()计算的属性，该属性绑定到div.style。

如果color属性值更改，它将通过计算属性传播到模板，并且div.l-pixel将更新背景。v-bind:（完整语法）或 :（简写）用于绑定模板中的属性或数据。传播是：

```
color change in canvas.vue >>> "color" in "props" in pixel.vue >>> "pixelStyle()" in "computed" >>> style attribute of "div.l-pixel" in "<template>" 
```

v-on:（完整语法）或@（简写）用于绑定事件处理程序，并且click方法中div.l-pixel绑定的click事件onPixelClick。

该方法将pixel-click使用其颜色信息发布事件，画布组件将侦听该颜色信息。

### main.js

```js
import Vue from 'vue';
import NCanvas from 'canvas';

new Vue({
    components: {NCanvas},
    el: '#main-canvas',
    template: '<n-canvas :pixel-data="pixelData" :colors="colors" background="rgb(229, 230, 232)"></n-canvas>',
    data() {
        return {
            pixelData: null,
            colors: null
        }
    },
    created() {
        const C = "C";        //Hat & Shirt
        const B = "B";     //Brown Hair & Boots
        const S = "S";  //Skin Tone
        const O = "O";      //Blue Overalls
        const Y = "Y";    //Yellow Buckles
        const W = "W";  //White Gloves
        const _ = "_";
        this.pixelData = [
            [_, _, _, _, _, _, _, _, _, _, _, _, _, _],
            [_, _, _, _, C, C, C, C, C, _, _, _, _, _],
            [_, _, _, C, C, C, C, C, C, C, C, C, _, _],
            [_, _, _, B, B, B, S, S, B, S, _, _, _, _],
            [_, _, B, S, B, S, S, S, B, S, S, S, _, _],
            [_, _, B, S, B, B, S, S, S, B, S, S, B, _],
            [_, _, B, B, S, S, S, S, B, B, B, B, _, _],
            [_, _, _, _, S, S, S, S, S, S, S, _, _, _],
            [_, _, _, C, C, O, C, C, C, C, _, _, _, _],
            [_, _, C, C, C, O, C, C, O, C, C, C, _, _],
            [_, C, C, C, C, O, O, O, O, C, C, C, C, _],
            [_, W, W, C, O, Y, O, O, Y, O, C, W, W, _],
            [_, W, W, W, O, O, O, O, O, O, W, W, W, _],
            [_, W, W, O, O, O, O, O, O, O, O, W, W, _],
            [_, _, _, O, O, O, _, _, O, O, O, _, _, _],
            [_, _, B, B, B, _, _, _, _, B, B, B, _, _],
            [_, B, B, B, B, _, _, _, _, B, B, B, B, _]];

        this.colors = {
            [C]: "255, 0, 0",
            [B]: "100, 50, 0",
            [S]: "255, 200, 150",
            [O]: "0, 0, 255",
            [Y]: "255, 255, 0",
            [W]: "255, 255, 255",
            [_]: "229, 230, 232"};
    }
});
```

像素初始化的代码写在canvas.vue组件里，在这里，我们可以看到为canvas.vue属性提供的数据。this.pixelData包含任意颜色的像素名称，this.colors是包含RGB值的颜色字典（JavaScript对象）。（可以通过创建class Color，包含颜色名称和RGB信息来修改）。

### canvas.vue

```html
<template>
    <div class="l-canvas-container" :style="{background: background}">
        <div v-for="(row, rowIndex) in pixelData" :key="rowIndex" :style="{height: pixelSize}">
            <n-pixel v-for="(col, colIndex) in row" :key="colIndex" :color="colors[col]"
                     :size="pixelSize" @pixel-click="onPixelClick">
            </n-pixel>
        </div>
    </div>
</template>

<script>

    import NPixel from 'pixel';

    export default {
        components: {NPixel},
        props: {
            pixelData: {type: Array, required: true},
            colors: {type: Object, required: true},
            pixelSize: {type: String, default: '20px'},
            background: {type: String, default: 'white'}
        },
        methods: {
            onPixelClick(color) {
                let newColor = this.getRandomColor();
                let colors = this.colors;
                for(let c in colors) {
                    if (colors[c] === color) {
                        colors[c] = newColor;
                    }
                }
            },
            getRandomColor() {
                return this.getRandom() + ', ' + this.getRandom() + ', ' + this.getRandom();
            },
            getRandom() {
                return Math.floor(Math.random() * 256);
            }
        }
    }
</script>

<style scoped>
    .l-canvas-container {
        background: rgb(229, 230, 232);
        width: 280px;
        margin: 0 auto;
        margin-top: 50px;
        height: 340px;
    }
</style>
```

从main.js中使用了v-for来渲染pixelData。

```html
<div v-for="(row, rowIndex) in pixelData" :key="rowIndex" :style="{height: pixelSize}">
    <n-pixel v-for="(col, colIndex) in row" :key="colIndex" :color="colors[col]"
             :size="pixelSize" @pixel-click="onPixelClick">
    </n-pixel>
</div>
```

pixel注册的组件component: { NPixel }，等同于components: {'n-pixel': NPixel} ，并且前缀n-用于防止组件名称冲突，以防万一pixel已经注册为全局组件。

在n-pixel中的v-for，col是颜色名称（即，“C”，“_”），其是来自main.js和  :color="colors[col]"发送一个RGB值（即“255，255，255”），以一个pixel组件。colIndex是当前项目（0，1，2，...）的索引。它被传递给key属性，Vue.js需要一个唯一的值v-for 。

@pixel-click="onPixelClick"pixel-click从pixel组件侦听事件并onPixelClick更改字典中的颜色（JavaScript对象，this.colors 而不是遍历整个二维数组this.pixelData ，并通过反应性，像素的背景颜色更新！

![](https://github.com/itemsets/vue2-pixel-art/raw/master/image/4.png)

### 后续

如果您想进一步探索Vue.js，我建议您继续阅读[他们的文章](https://vuejs.org/v2/guide/)，甚至可以去看官方api并且自己动手写更多东西。还有一个很不错的免费系列视频 - []()学习拉拉斯卡特的[ Learn Vue 2: Step By Step（Vue 2：一步一步）](https://laracasts.com/series/learn-vue-2-step-by-step)。

在我看来，使用Vue.js来编写一些小应用程序来更好地了解库或框架，这样对你很有帮助的。您可以找到一个有趣的Demo或将您以前的代码的一部分转换为新的库。在你提炼了自己的代码之后，您会发现不同库或框架转换并非是件容易的事情，尤其对比了它们的优点和缺点通常不是一下子就能理清楚的。

加油！！！！