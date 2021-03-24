# Frame animation

Frame animation is an animation played by multiple consecutive pictures at a certain time interval. The pictures are usually merged into one picture, and then the position of the picture and the order of playback are described through JSON.

The bottom layer of the frame animation of Eva.js is realized by pixi.js spriteAnimation. Therefore, in Eva.js, you can use the materials exported by the sprite animation tool supported by spriteAnimation, such as [TexturePacker](https://www.codeandweb.com/texturepacker /tutorials/how-to-create-sprite-sheets-and-animations-with-pixijs5).

- [https://eva.js.org/playground/#/spriteAnimation](https://eva.js.org/playground/#/spriteAnimation)

## Install

```bash
npm i @eva/plugin-renderer-spriteAnimation
```

## Usage

```js
import {Game, GameObject, resource, RESOURCE_TYPE} from'@eva/eva.js'
import {RendererSystem} from'@eva/plugin-renderer'
import {SpriteAnimation, SpriteAnimationSystem} from'@eva/plugin-renderer-spriteAnimation'

resource.addResource([
  {
    name:'fruit',
    type: RESOURCE_TYPE.SPRITE_ANIMATION,
    src: {
      image: {
        type:'json',
        url:'https://gw.alicdn.com/bao/uploaded/TB15pMkkrsTMeJjSszhXXcGCFXa-377-1070.png'
      },
      json: {
        type:'json',
        url:'https://gw.alicdn.com/mt/TB1qCvumsyYBuNkSnfoXXcWgVXa.json'
      }
    },
    preload: false
  }
])

const game = new Game({
  systems: [
    new RendererSystem({
      canvas: document.querySelector('#canvas'),
      width: 750,
      height: 1000
    }),
    new SpriteAnimationSystem()
  ]
})

const cut = new GameObject('cut', {
  position: {x: 225, y: 400 },
  size: {width: 300, height: 200 },
  origin: {x: 0, y: 0}
})

const frame = cut.addComponent(
  new SpriteAnimation({
    resource:'fruit',
    speed: 100
  })
)

frame.play()

game.scene.addChild(cut)
```

## Options

### resource `string`

Resource name, which needs to be preloaded by `resource.addResource`.

### speed `number`

The interval time for each picture to be played, in ms.

## Methods

### play()

Play animation, automatically play by default.

### stop()

Stop play.

<br/>
<br/>
<br/>
<br/>
<br/>