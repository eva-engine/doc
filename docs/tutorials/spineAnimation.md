# Spine skeletal animation

Spine is a 2D animation software tailored specifically for software and game developers. Animators, original artists, and engineers work together to give life to your game. Using Spine skeletal animation can achieve some richer effects, which can better reduce memory usage compared to frame animation. Spine is a paid software, please use it after purchase, Spine official website: [http://zh.esotericsoftware.com/](http://zh.esotericsoftware.com/)

-[https://eva.js.org/playground/#/spine](https://eva.js.org/playground/#/spine)

## Install

```bash
npm i @eva/plugin-renderer-spine
```

## Usage

```js
import {Game, GameObject, resource, RESOURCE_TYPE} from'@eva/eva.js'
import {RendererSystem} from'@eva/plugin-renderer'
import {Spine, SpineSystem} from'@eva/plugin-renderer-spine'

resource.addResource([
  {
    name:'anim',
    type:'SPINE',
    src: {
      ske: {
        type:'json',
        url:'https://pages.tmall.com/wow/eva/b5fdf74313d5ff2609ab82f6b6fd83e6.json'
      },
      atlas: {
        type:'atlas',
        url:'https://pages.tmall.com/wow/eva/b8597f298a5d6fe47095d43ef03210d4.atlas'
      },
      image: {
        type:'png',
        url:'https://gw.alicdn.com/tfs/TB1YHC8Vxz1gK0jSZSgXXavwpXa-711-711.png'
      }
    }
  }
])

const game = new Game({
  systems: [
    new RendererSystem({
      canvas: document.querySelector('#canvas'),
      width: 750,
      height: 1000
    }),
    new SpineSystem()
  ],
  autoStart: true,
  frameRate: 60
})

game.scene.transform.size = {
  width: 750,
  height: 1000
}
const gameObject = new GameObject('spine', {
  anchor: {
    x: 0.5,
    y: 0.5
  },
  scale: {
    x: 0.5,
    y: 0.5
  }
})

const spine = new Spine({ resource:'anim', animationName:'idle' })

gameObject.addComponent(spine)
spine.on('complete', e => {
  console.log('Animation playback ended', e.name)
})

spine.play('idle')
game.scene.addChild(gameObject)
```

## Options

### resource `string`

Resource Name

### animationName `string`

Animation name

## Methods

### play(name?: string, loop?: boolean)

Play animation

-name action name
-loop Whether to play in a loop

### stop()

Stop play

<br/>
<br/>
<br/>
<br/>
<br/>