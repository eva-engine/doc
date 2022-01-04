
# MatterJS

Eva.js 基于 [Matterjs](https://brm.io/matter-js) 的物理引擎，目前只接入了小部分内容，如需更多内容，可持续接入。

## Install

### 使用 NPM
```bash
npm install @eva/plugin-matterjs
```

### 在浏览器中
```html
<script src="https://unpkg.com/@eva/plugin-matterjs@1.2.x/dist/EVA.plugin.renderer.matterjs.min.js"></script>
```

### 使用

```js
import {Physics} from '@eva/plugin-matterjs';
// 1.安装物理引擎后引入
import {PhysicsSystem, Physics, PhysicsType} from '@eva/plugin-matterjs';

// 2.在Eva.js中注册插件
const game = new Game({
  autoStart: true,
  frameRate: 60,
  systems: [
    new RendererSystem({
      transparent: true,
      canvas: canvasNode,
      backgroundColor: 0xfee79d,
      width: 750,
      height: 1624,
      resolution: 2,
    }),
    new GraphicsSystem(),
    new PhysicsSystem({
      resolution: 2, // 保持RendererSystem的resolution一致
      // isTest: true, // 是否开启调试模式
      // element: document.getElementById('game-container'), // 调试模式下canvas节点的挂载点
      world: {
        gravity: {
          y: 2, // 重力
        },
      },
      mouse: {
        open: true,
      },
    }),
    new TextSystem(),
    new ImgSystem(),
    new EventSystem(),
  ],
});

// 3.新建游戏实体对象
const box = new GameObject('box', {
  size: {
    width: 75,
    height: 75,
  },
  position: {
    x: 75 + Math.random() * 300,
    y: 75,
  },
  // origin 会映射为物理系统的质心，二维环境下即物理的几何中心，此处必须固定为 {x:0.5,y:0.5}
  origin: {
    x: 0.5,
    y: 0.5,
  },
});
// 4.给游戏对象添加Componet
const physics = box.addComponent(
  new Physics({
    type: PhysicsType.RECTANGLE,
    bodyOptions: {
        isStatic: false, // 物体是否静止，静止状态下任何作用力作用于物体都不会产生效果
        restitution: 0.8,
        frictionAir: 0.1,
        friction: 0,
        frictionStatic: 0,
        force: {
          x: 0,
          y: 0,
        },
    },
    stopRotation: true, // 默认false，通常不用设置
  }),
);
//目前支持的碰撞事件 collisionStart  collisionActive  collisionEnd
//刚体事件 tick,beforeUpdate,afterUpdate,beforeRender,afterRender,afterTick 通常使用beforeUpdate和afterUpdate即可
physics.on('collisionStart', (body, gameObject1, gameObject2) => {});

console.log("physics",physics);
```

## 参数
### type `PhysicsType`
碰撞包围盒的形状 
- `RECTANGLE` 矩形
- `CIRCLE` 圆形
- `POLYGON` 等边多边形

### sides
当type为 `POLYGON` 的时候，设置包围盒的边数

### radius
当 type 为 `POLYGON` 和 `CIRCLE` 时的半径

### bodyOptions
相关属性，可参考 [Matterjs官网](https://brm.io/matter-js/docs/classes/Body.html#properties)


<br />
<br />
<br />
<br />
<br />