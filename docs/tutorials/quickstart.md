# Quick start

## Demo project

Demo project created based on webpack: [https://github.com/eva-engine/start-demo](https://github.com/eva-engine/start-demo)

Demo project created based on webpack with cdn: [https://github.com/eva-engine/start-demo-with-cdn](https://github.com/eva-engine/start-demo-width-cdn)

Demo project build for multi platform(Web/Wechat minigame): [https://github.com/eva-engine/eva-multi-platform-demo](https://github.com/eva-engine/eva-multi-platform-demo)

## Install

### With NPM
```bash
npm install @eva/eva.js
```

### In Browser
```html
<script src="https://unpkg.com/@eva/eva.js@1.2.x/dist/EVA.min.js"></script>
```
在浏览器中以CDN的形式引入Eva.js，所有的对象将会绑定在 window.EVA 上，未来所用到的插件都会绑定在window.EVA.plugin上。详情参考：基于CDN开发

## Create a canvas

Eva.js relies on canvas in HTML for drawing. If the width and height in the design draft are fixed (for example, 750px\*1000px) and occupy the full screen, we can set the CSS width of the canvas to 100% and the height to auto.

```html
<style>
  #canvas {
    width: 100%;
    height: auto;
  }
</style>
<canvas id="canvas"></canvas>
```

## Add resource

Before creating the game, we need to add resource files to the resource manager, here we add two image resources. Of course, you can add keel animation and spine animation resources. For more information, please see [Resource Management](resourceManagement).

```js
import {resource, RESOURCE_TYPE} from '@eva/eva.js'

resource.addResource([
  {
    name:'image1',
    type: RESOURCE_TYPE.IMAGE,
    src: {
      image: {
        type:'png',
        url:'https://gw.alicdn.com/tfs/TB1DNzoOvb2gK0jSZK9XXaEgFXa-658-1152.webp'
      }
    },
    preload: true
  },
  {
    name:'image2',
    type: RESOURCE_TYPE.IMAGE,
    src: {
      image: {
        type:'png',
        url:'https://gw.alicdn.com/tfs/TB15Upxqk9l0K4jSZFKXXXFjpXa-750-1624.jpg'
      }
    },
    preload: true
  }
])
```

## Create a game

The Eva.js kernel is a very lightweight runtime, and other functions are implemented through plug-ins. If you want to achieve the most basic rendering capabilities of the game, you need to install the rendering plug-in `@eva/plugin-renderer`.

- With NPM
```bash
npm i @eva/plugin-renderer
```

- In Browser
```html
<!-- import PixiJS -->
<script src="//unpkg.com/pixi.js@4.8.9/dist/pixi.min.js"></script>
<!-- import RendererAdapter -->
<script src="//unpkg.com/@eva/renderer-adapter@1.2.x/dist/EVA.rendererAdapter.min.js"></script>
<script src="https://unpkg.com/@eva/plugin-renderer@1.2.x/dist/EVA.plugin.renderer.min.js"></script>
```


```js
import {Game} from '@eva/eva.js'
import {RendererSystem} from '@eva/plugin-renderer'

// Create a rendering system
const rendererSystem = new RendererSystem({
  canvas: document.querySelector('#canvas'), // Optional, automatically generated canvas hanging on game.canvas
  width: 750,
  height: 1000,
  transparent: false,
  resolution: window.devicePixelRatio / 2, // Optional, if it is 2 times the image design, it can be divided by 2
  enableScroll: true, // Enable page scrolling
  renderType: 0 // 0: automatic judgment, 1: WebGL, 2: Canvas, it is recommended to use Canvas below android6.1 ios9, business judgment is required.
})

// Create GameObject
const game = new Game({
  frameRate: 60, // Optional, game frame rate, default 60
  autoStart: true, // optional, start automatically
  systems: [rendererSystem]
})
```

Of course, this only allows Eva.js to have basic rendering capabilities, but no elements have been displayed on the canvas. Next, we will add gameObject, which will be displayed on the canvas.

## Add GameObject

After creating the game, we need to add a [GameObject](gameObject) to the game, and add [component](customComponent) to the GameObject. The GameObject is the most basic operable unit in the game, and the component gives the GameObject various abilities. For example, the Img component allows a gameObject to display a picture.

```bash
npm i @eva/plugin-renderer @eva/plugin-renderer-img
```


```js
import { GameObject } from '@eva/eva.js'
import {Img, ImgSystem} from '@eva/plugin-renderer-img' // Introduce the components and systems needed to render pictures

game.addSystem(new ImgSystem()) // Add the ability to render pictures to the game

const gameObject = new GameObject('gameObj1', {
  size: {
    width: 658,
    height: 1152
  }
})

gameObject.addComponent(
  new Img({
    resource:'image1'
  })
)

game.scene.addChild(gameObject) // Put the GameObject into the scene so that the picture can be displayed on the canvas
```

## Component Management

### Get components

Method 1: Keep components when creating

```js
const img = gameObject.addComponent(
  new Img({
    resource:'image1'
  })
)
// or
const img = new Img({ resource:'image1' })
gameObject.addComponent(img)
```

Method 2: Obtain from the GameObject after creation

```js
const img = gameObject.getComponent(Img)
// or
const img = gameObject.getComponent('Img')
```

### Modify component properties

```js
img.resource ='image2' // Switch the resource name, make sure the resource has been added to the resource manager
```

## Are you ready?

Just introduced the simplest demo of Eva.js, let’s look at some 2D interactive games [Common Ability](resourceManagement).

