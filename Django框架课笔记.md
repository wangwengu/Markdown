####  开发流程

##### 初始化设置

1. 在全局 `urls.py` 里面设置 `game` 的 `url`

   ```python
   from django.urls import path, include
   path('', include('game.urls.index'))
   ```

2. 打开 `game/urls/index.py` 文件（没有就自己创建）

   ```python
   from django.urls import path
   
   urlpatterns = []
   ```

3.  删除本身存在的 `views.py` 文件 创建 `game/views/index.py` 文件（没有就自己创建）

   + [Django模板详细介绍](https://www.cnblogs.com/rexcheny/p/9635764.html)

   ```python
   from django.shortcuts import render
   
   def index(request):
       return render(request, 'multiends/web.html') # render的作用就是加载模板
   ```

4. 创建 `game/templates/multiends/web.html`

   ```html
   <head>
      	<!-- 引入 css 文件-->
       <link rel = "stylesheet" href = "https://cdn.acwing.com/static/jquery-ui-dist/jquery-ui.min.css">
       <!-- 引入 js 文件 -->
       <script src = "https://cdn.acwing.com/static/jquery/js/jquery-3.3.1.min.js"></script>
   </head>
   <!-- 将 body 的外边框设置为 0px 更加美观-->
   <body style = "margin: 0px;">
      	<div id = "ac_game_12345678">你好</div>
   </body>
   ```

5. 在 `game/urls/index.py` 中引入 `game/views/index` 函数

   ```python
   from django.urls import path
   
   urlpatterns = [
       path('', index, name = 'index')
   ]
   ```

6. 创建菜单界面 进入 `game/templates/multiends/web.html`

   ```html
   <!-- 加载 static 文件夹-->
   {% load static %}
   <head>
      	<!-- 引入 css 文件-->
       <link rel = "stylesheet" href = "https://cdn.acwing.com/static/jquery-ui-dist/jquery-ui.min.css">
       <!-- 引入 js 文件 -->
       <script src = "https://cdn.acwing.com/static/jquery/js/jquery-3.3.1.min.js"></script>
   </head>
   <!-- 将 body 的外边框设置为 0px 更加美观-->
   <body style = "margin: 0px;">
      	<div id = "ac_game_12345678"></div>
       <!-- module 模块化编程-->
       <script type = "module">
           import {AcGame} from "{% static 'js/dist/game.js' %}";
           $(document).ready(function() {
               let ac_game = new AcGame("ac_game_12345678");
           });
       </script>
   </body>
   ```

7. 创建 `static/js/src/zbase.js`

   ```javascript
   <!-- 使用 Javescript 面向对象 需要向外界暴露一个窗口 使用 export 关键字-->
   export class AcGame {
       constructor(id) {
          	<!-- id 即为传进来的id 名称-->
           this.id = id;
           <!-- 获取 id 所对应的 div 标签-->
           this.$ac_game = $('#' + id);
           <!-- 创建菜单界面 -->
           this.menu = new AcGameMenu(this);
       }
   }
   ```

8. 编写 `js` 的脚本 创建 `acapp/scripts/compress_game_js.sh`

   ```shell
   #! /bin/bash
   
   JS_PATH=/home/django/acapp/game/static/js
   JS_PATH_DIST=${JS_PATH}/dist # 记得先创建 dist 文件夹
   JS_PATH_SRC=${JS_PATH}/src
   
   find $JS_PATH_SRC -type f -name '*.js' | sort | xargs cat > ${JS_PATH_DIST}/game.js
   ```

##### 界面设置

###### 编写 `AcGameMenu` 界面

+ 创建 `acapp/game/static/js/src/menu/zbase.js` 并写入以下内容

  ```javascript
  class AcGameMenu {
      constructor(root) {
          this.root = root;
          this.$menu = $(`
              <div class="ac_game_menu">
                  <div class="ac_game_menu_field">
                      <div class="ac_game_menu_field_item ac_game_menu_field_item_single_mode">
                          单人模式
                      </div>
                      <!-- 添加换行 -->
                      <br>
                      <div class="ac_game_menu_field_item ac_game_menu_field_item_multi_mode">
                          多人模式
                      </div>
                      <!-- 添加换行 -->
                      <br>
                      <div class="ac_game_menu_field_item ac_game_menu_field_item_settings">
                          设置
                      </div>
                  </div>
              </div>
          `);
          this.root.$ac_game.append(this.$menu);
      }
  }
  ```

+ 下载背景图片 放到 `acapp/game/static/image/menu/`

  + `ubuntu` 下载图片命令 `wget --output-document=图片别名 图片地址 图片存储地址`
  + [图片地址](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Finews.gtimg.com%2Fnewsapp_match%2F0%2F11156556256%2F0.jpg&refer=http%3A%2F%2Finews.gtimg.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg)

+ 创建 `acapp/game/static/css/game.css`

  ```css
  .ac_game_menu {
      width: 100%;
      height: 100%;
      background-image: url("/static/image/menu/background.gif");
      /* 禁止左右上下拉伸 */
      background-size: 100% 100%;
      /* 禁止用户鼠标点选 */
      user-select: none;
  }
  
  .ac_game_menu_field {
      width: 20vw;
      position: relative;
      top: 40vh;
      left: 19vw;
  }
  
  .ac_game_menu_field_item {
      color: white;
      height: 7vh;
      width: 20vw;
      font-size: 6vh;
      /* 设置字体样式为斜体 */
      font-style: italic;
      padding: 2vh;
      text-align: center;
      background-color: rgba(39, 21, 28, 0.6);
      border-radius: 10px;
      /* 设置字体之间的间距 */
      letter-spacing: 0.5vw;
      /* 设置鼠标样式 接触到之后变成指针样式 */
      cursor: pointer;
  }
  
  .ac_game_menu_field_item:hover {
      /* 设置缩放效果 */
      transform: scale(1.2);
      /* 设置秒数 */
      transition: 100ms;
  }
  ```

+ 将 `css` 文件引入到 `web.html` 中

  ```html
  <head>
     	<!-- 引入 css 文件-->
      <link rel = "stylesheet" href = "{% static 'css/game.css' %}">
  </head>
  ```

###### 编写 `playground` 界面

+ 创建 `acapp/game/static/js/src/playground/zbase.js`

  ```javascript
  class AcGamePlayground {
      constructor(root) {
          this.root = root;
          this.$playground = $(`
              <div class = "ac_game_playground">这是玩家页面</div>
          `);
          <!-- 将玩家页面添加到游戏中去 -->
          this.root.$ac_game.append(this.$playground);
      }
      <!-- 添加显示函数 -->
      show() {
          this.$playground.show();
      }
      <!-- 添加隐藏函数 -->
      hide() {
          this.$playground.hide();
      }
  }
  ```

+ 将玩家页面添加到游戏页面中去 进入 `acapp/game/static/js/src/zbase.js`

  ```javascript
  <!-- 隐藏菜单界面 方便调试 -->
  this.menu = new AcGameMenu(this);
  <!-- 添加此行 -->
  this.playground = new AcGamePlayground(this);
  ```

+ 修改菜单界面 进入 `acapp/game/static/js/src/menu/zbase.js`

  ```javascript
  class AcGameMenu {
      constructor(root) {
          <!-- 找到单人模式的权柄 -->
          this.$single_mode = this.$menu.find('.ac_game_menu_field_item_single_mode');
          this.start();
      }
      start() {
          <!-- 添加监听事件 -->
          this.add_listening_events();
      }
      add_listening_events() {
          <!-- 保存主权柄 -->
          let outer = this;
          <!-- 给 single_mode 添加点击事件 -->
          this.$single_mode.click(function() {
              <!-- 隐藏菜单界面 -->
              outer.hide();
              <!-- 显示玩家界面 -->
              outer.root.playground.show();
          });
      }
      hide() {
          <!-- 隐藏函数 -->
          this.$menu.hide();
      }
  }
  ```

+ 给玩家界面添加 `css` 样式  进入 `acapp/game/static/css/game.css`

  ```css
  .ac_game_playground {
      width: 100%;
      height: 100%;
      user-select: none;
  }
  ```

###### 编写游戏引擎 

+ 创建 `acapp/game/static/js/src/playground/ac_game_object/zbase.js`

```javascript
let AC_GAME_OBJECTS = [];

class AcGameObject {
    constructor() {
        <!-- 将所有继承本类的类对象用列表保存 -->
        AC_GAME_OBJECTS.push(this);
        <!-- 判断是否执行过第一帧 -->
        this.has_called_start = false;
        <!-- 时间戳 统计时间 -->
        this.timedelta = 0;
    }
    <!-- 只在第一帧执行 -->
    start() {}
    <!-- 从第一帧之后的每一帧都执行 -->
    update() {}
    <!-- 在被销毁之前执行一次 -->
    on_destory() {}
    <!-- 销毁对象 -->
    destory() {
        this.on_destory();
        for (let i = 0; i < AC_GAME_OBJECTS.length; i ++ ) {
            if (AC_GAME_OBJECTS[i] === this) {
                <!-- 参数1：位置 参数2：长度 -->
                AC_GAME_OBJECTS.splice(i, 1);
                break;
            }
        }
    }
}

let last_timestamp;
let AC_GAME_ANIMATION = function(timestamp) {
    for (let i = 0; i < AC_GAME_OBJECTS.length; i ++ ) {
        let obj = AC_GAME_OBJECTS[i];
        <!-- 如果没有执行第一帧 执行第一帧 -->
        if (!obj.has_called_start) {
            obj.start();
            obj.has_called_start = true;
        }
        else {
            <!-- 统计时间间隔 last_timestamp 不用初始化的原因就是第一帧走不到此处 -->
            obj.timedelta = timestamp - last_timestamp;
            obj.update();
        }
    }
    last_timestamp = timestamp;
    requestAnimationFrame(AC_GAME_ANIMATION);
};
<!-- 绘制动画 通常情况下与屏幕刷新率保持一致 -->
<!-- 一般情况下不准 比如函数里面写了个循环100000次 时间戳就不准了 就不是屏幕刷新率了 -->
requestAnimationFrame(AC_GAME_ANIMATION);
```

###### 编写游戏地图界面

+ 创建 `acapp/game/static/js/src/playground/game_map/zbase.js`

  ```javascript
  class GameMap extends AcGameObject {
      constructor(playground) {
          <!-- 调用父类的方法 -->
          super();
          this.playground = playground;
          this.$canvas = $(`
              <canvas></canvas>
          `);
          <!-- 获取画布的权柄 -->
          this.ctx = this.$canvas[0].getContext('2d');
          this.ctx.canvas.width = this.playground.width;
          this.ctx.canvas.height = this.playground.height;
          this.playground.$playground.append(this.$canvas);
      }
      start() {}
      update() {
          this.render();
      }
      render() {
          <!-- 给画布添加背景-->
          this.ctx.fillStyle = "rgba(0, 0, 0, 0.2)";
          <!-- 画正方形 -->
          this.ctx.fillRect(0, 0, this.ctx.canvas.width, this.ctx.canvas.height);
      }
  }
  ```

+ 将地图界面添加到玩家界面 进入 `acapp/game/static/js/src/playground/zbase.js`

  ```javascript
  class AcGamePlayground {
      constructor(root) {
          this.$playground = $(`
              <div class = "ac_game_playground"></div>
          `);
          <!-- 将玩家界面添加到游戏界面 注意此行务必写在 this.width this.height 之前 -->
          this.root.$ac_game.append(this.$playground);
          this.width = this.$playground.width();
          this.height = this.$playground.height();
          <!-- 创建地图界面 -->
          this.game_map = new GameMap(this);
      }
      show() {
          this.$playground.show();
      }
  }
  ```

###### 创建玩家 

+ 创建 `acapp/game/static/js/src/playground/player/zbase.js`

  ```javascript
  class Player extends AcGameObject {
      constructor(playground, x, y, radius, color) {
          <!-- 继承父类的方法 -->
          super();
          this.playground = playground;
          <!-- 拿到画布的权柄 -->
          this.ctx = this.playground.game_map.ctx;
          this.x = x;
          this.y = y;
          this.radius = radius;
          this.color = color;
      }
      start() {}
      update() {
          this.render();
      }
      render() {
          <!-- 告诉浏览器开始作画 -->
          this.ctx.beginPath();
          this.ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
          this.ctx.fillStyle = this.color;
          this.ctx.fill();
      }
  }
  ```

+ 将玩家添加到地图中 进入 `acapp/game/static/js/src/playground/zbase.js`

  ```javascript
  class AcGamePlayground {
      constructor(root) {
          <!-- 创建玩家列表 -->
          this.players = [];
          this.players.push(new Player(this, this.width / 2, this.height / 2, this.height * 0.05, "white"));
      }
  }
  ```

###### 操作玩家移动

+ 修改 `acapp/game/static/js/src/playground/player/zbase.js` 

  ```javascript
  class Player extends AcGameObject {
      constructor(speed, is_me) {
          <!-- 添加两个属性 玩家移动的速度 是否是玩家本人 -->
          this.speed = speed;
          this.is_me = is_me;
      }
      start() {
          if (this.is_me) {
              this.add_listening_events();
          }
      }
      add_listening_events() {
          let outer = this;
          <!-- 禁用鼠标右键功能 -->
          this.playground.game_map.$canvas.on("contextmenu", function() {
              return false;
          });
          this.playground.game_map.$canvas.mousedown(function(e) {
              <!-- 如果按下的是右键 -->
              if (e.which == 3) {
                  console.log("click me");
              }
          });
      }
  }
  ```

  + 修改 `acapp/game/static/js/src/playground/zbase.js`

    ```javascript
    <!-- 更新 player 的属性 将速度和标志加上-->
    this.players.push(new Player(this, this.width / 2, this.height / 2, this.height * 0.05, "white", this.height * 0.2, true));
    ```

  + 修改 `acapp/game/static/js/src/playground/player/zbase.js`
  
    ```javascript
    class Player extends AcGameObject {
        constructor(playground, x, y, radius, color, speed, is_me) {
            <!-- 小球移动的长度 -->
            this.move_length = 0;
            <!-- 小球移动的最小值 -->
            this.eps = 0.1;
        }
        add_listening_events() {
            this.playground.game_map.$canvas.mousedown(function(e) {
                if (e.which == 3) {
                    <!-- 获取鼠标点击的横、纵坐标位置 -->
                    outer.move_to(e.clientX, e.clientY);
                }
            });
        }
        get_dist(x1, y1, x2, y2) {
            let dx = x1 - x2;
            let dy = y1 - y2;
            return Math.sqrt(dx * dx + dy * dy);
        }
        <!-- 让小球移动到 tx、ty 位置 -->
        move_to(tx, ty) {
            <!-- 计算距离 -->
            this.move_length = this.get_dist(this.x, this.y, tx, ty);
            <!-- 计算角度 -->
            <!-- [atan2 维基百科](https://zh.wikipedia.org/wiki/Atan2) -->
            let angle = Math.atan2(ty - this.y, tx - this.x);
            <!-- 水平方向的分角度 -->
            this.vx = Math.cos(angle);
            <!-- 垂直方向的分角度 -->
            this.vy = Math.sin(angle);
        }
        update() {
            <!-- 如果移动的长度小于最小值 就认为已经到达目标值 -->
            if (this.move_length < this.eps) {
                <!-- 将变量置为 0 -->
                this.move_length = this.vx = this.vy = 0;
            }
            else {
                <!-- 计算移动的长度 注意 取较小值 因为有可能会越界 原因 this.timedelta 随着时间增加值越来越大 -->
                let moved = Math.min(this.move_length, this.speed * this.timedelta / 1000);
                <!-- 水平方向的新位置等于 水平速度 * 移动距离 -->
                this.x += this.vx * moved;
                <!-- 垂直方向的新位置等于 垂直速度 * 移动距离 -->
                this.y += this.vy * moved;
                <!-- 将当前移动的距离剪掉 -->
                this.move_length -= moved;
            }
            this.render();
        }
    }
    ```

###### 让玩家发射炮弹

+ 创建炮弹类 `acapp/game/static/js/src/playground/skill/fireball`

  ```javascript
  class FireBall extends AcGameObject {
      constructor(playground, player, x, y, radius, vx, vy, color, speed, move_length) {
          super();
          this.playground = playground;
          this.player = player;
          <!-- 获取画布的权柄 -->
          this.ctx = this.playground.game_map.ctx;
          this.x = x;
          this.y = y;
          this.radius = radius;
          this.vx = vx;
          this.vy = vy;
          this.color = color;
          this.speed = speed;
          this.move_length = move_length;
          <!-- 获取最小值 -->
          this.eps = 0.1;
      }
      update() {
          if (this.move_length < this.eps) {
              return false;
          }
          let moved = Math.min(this.move_length, this.speed * this.timedelta / 1000);
          this.x += this.vx * moved;
          this.y += this.vy * moved;
          this.move_length -= moved;
          this.render();
      }
      render() {
          this.ctx.beginPath();
          this.ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
          this.ctx.fillStyle = this.color;
          this.ctx.fill();
      }
  }
  ```

+ 修改 `acapp/game/static/js/src/playground/player/zbase.js`

  ```javascript
  class Player extends AcGameObject {
      constructor(playground, x, y, radius, color, speed, is_me) {
          <!-- 添加当前技能 -->
          this.cur_skill = null;
      }
      add_listening_events() {
          let outer = this;
          this.playground.game_map.$canvas.mousedown(function(e) {
              if (e.which == 3) {
                  outer.move_to(e.clientX, e.clientY);
              }
              else if (e.which == 1) { <!-- 如果当前按的键是左键 -->
                  <!-- 如果当前技能是火球技能 -->
                  if (outer.cur_skill === "fireball") {
                      outer.shoot_fireball(e.clientX, e.clientY);
                  }
                  <!-- 发完技能之后 清空技能 -->
                  outer.cur_skill = null;
              }
          });
          $(window).keydown(function(e) {
              <!-- 如果按下Q键 -->
              if (e.which == 81) {
                  outer.cur_skill = "fireball";
                  return true;
              }
          });
      }
      shoot_fireball(tx, ty) {
          let x = this.x;
          let y = this.y;
          let radius = this.playground.height * 0.01;
          let angle = Math.atan2(ty - this.y, tx - this.x);
          let vx = Math.cos(angle);
          let vy = Math.sin(angle);
          let color = "orange";
          let speed = this.playground.height * 0.5;
          let move_length = this.playground.height * 0.8;
          new FireBall(this.playground, this, x, y, radius, vx, vy, color, speed, move_length);
      }
  }
  ```

###### 添加 `AI` 机器人

+ 修改 `acapp/game/static/js/src/playground/player/zbase.js`

  ```javascript
  class Player extends AcGameObject {
      constructor(playground, x, y, radius, color, speed, is_me) {
      start() {
          if (this.is_me) {
              this.add_listening_events();
          }
          else {
              <!-- 如果不是本人 则随机指定一个地方 -->
              let tx = Math.random() * this.playground.width;
              let ty = Math.random() * this.playground.height;
              this.move_to(tx, ty);
          }
      }
      update() {
          if (this.move_length < this.eps) {
              this.move_length = this.vx = this.vy = 0;
              <!-- 如果不是本人 则当停止下来之后 重新在随机选择一个方向 -->
              if (!this.is_me) {
                  let tx = Math.random() * this.playground.width;
                  let ty = Math.random() * this.playground.height;
                  this.move_to(tx, ty);
              }
          }
          else {
              let moved = Math.min(this.move_length, this.speed * this.timedelta / 1000);
              this.x += this.vx * moved;
              this.y += this.vy * moved;
              this.move_length -= moved;
          }
          this.render();
      }
  }
  ```

+ 修改 `acapp/game/static/js/src/playground/zbase.js`

  ```javascript
  for (let i = 0; i < 5; i ++ ) {
      <!-- 注意最后一个参数是 false 表示不是本人 -->
  	this.players.push(new Player(this, this.width / 2, this.height / 2, this.height * 0.05, "red", this.height * 0.2, false));
  }
  ```

###### 让 `AI` 发射炮弹

+ 修改 `acapp/game/static/js/src/playground/player/zbase.js`

  ```javascript
  class Player extends AcGameObject {
      update() {
          <!-- 如果是机器人 发射炮弹的频率设置 < 1 / 180.0 -->
          if (!this.is_me && Math.random() < 1 / 180.0) {
              let player = this.playground.players[Math.floor(Math.random() * this.playground.players.length)];
              let tx = player.x + player.speed * this.vx * this.timedelta / 1000 * 2;
              let ty = player.y + player.speed * this.vy * this.timedelta / 1000 * 2;
              this.shoot_fireball(tx, ty);
          }
      }
  }
  ```
  

###### 让炮弹对敌人形成伤害

+ 修改 `acapp/game/static/js/src/playground/skill/fireball/zbase.js`

  ```javascript
  class FireBall extends AcGameObject {
      constructor(playground, player, x, y, radius, vx, vy, color, speed, move_length, damage) {
          <!-- 火球的伤害值 -->
          this.damage = damage;
      }
      update() {
          for (let i = 0; i < this.playground.players.length; i ++ ) {
              let player = this.playground.players[i];
              if (this.player !== player && this.is_collision(player)) {
                  console.log("attack");
                  this.attack(player);
              }
          }
          this.render();
      }
      get_dist(x1, y1, x2, y2) {
          let dx = x1 - x2;
          let dy = y1 - y2;
          return Math.sqrt(dx * dx + dy * dy);
      }
      <!-- 判断火球是否攻击到敌人 -->
      is_collision(player) {
          let distance = this.get_dist(this.x, this.y, player.x, player.y);
          if (distance < this.radius + player.radius) {
              return true;
          }
          return false;
      }
      attack(player) {
          <!-- 角度 -->
          let angle = Math.atan2(player.y - this.y, player.x - this.x);
          <!-- 朝着某个角度 进行攻击 伤害值为 this.damage -->
          player.is_attacked(angle, this.damage);
          <!-- 火球攻击完敌人之后 火球即消失 -->
          this.destory();
      }
  }
  ```

+ 修改 `acapp/game/static/js/src/playground/player/zbase.js`

  ```javascript
  class Player extends AcGameObject {
      shoot_fireball(tx, ty) {
          <!-- 给火球添加伤害 -->
          new FireBall(this.playground, this, x, y, radius, vx, vy, color, speed, move_length, this.playground.height * 0.01);
      }
      is_attacked(angle, damage) {
          this.radius -= damage;
          if (this.radius < 20) {
              this.destory();
              return false;
          }
      }
      <!-- on_destory 不是 destroy -->
      <!-- destory 函数删除的是 AC_GAME_OBJECTS 列表里面的对象 -->
      <!-- on_destory 函数删除的是 this.playground.players 列表里面的玩家-->
      on_destory() {
          for (let i = 0; i < this.playground.players.length; i ++ ) {
              if (this.playground.players[i] === this) {
                  this.playground.players.splice(i, 1);
                  break;
              }
          }
      }
  }
  ```
  

###### 设置炮弹击中效果

+ 后退效果
  + 修改 `acapp/game/static/js/src/playground/player/zbase.js`
  
    ```javascript
    class Player extends AcGameObject {
        constructor(playground, x, y, radius, color, speed, is_me) {
            <!-- 被击中在 x 轴上的方向 -->
            this.damage_x = 0;
            <!-- 被击中在 y 轴上的方向 -->
            this.damage_y = 0;
            <!-- 被击中之后的速度 -->
            this.damage_speed = 0;
            <!-- 被击中之后的摩擦力 -->
            this.friction = 0.9;
        }
        is_attacked(angle, damage) {
            this.damage_x = Math.cos(angle);
            this.damage_y = Math.sin(angle);
            <!-- 被击中之后 更新下速度 -->
            this.damage_speed = damage * 80;
            <!-- 被击中之后的速度下降为原来的80% -->
            this.speed *= 0.8;
        }
        update() {
            <!-- 如果被击中的速度 > 10 暂停自己原来的速度 -->
            if (this.damage_speed > 10) {
                <!-- 将原来的速度和移动距离设置为 0 -->
                this.vx = this.vy = this.move_length = 0;
                <!-- 更新下当前的位置 -->
                this.x += this.damage_x + this.damage_speed * this.timedelta / 1000;
                this.y += this.damage_y + this.damage_speed * this.timedelta / 1000;
                <!-- 速度✖️摩擦力 速度会越来越慢 -->
                this.damage_speed *= this.friction;
            }
            <!-- 否则 没有变化 -->
            else {
                if (this.move_length < this.eps) {
                    this.move_length = this.vx = this.vy = 0;
                    if (!this.is_me) {
                        let tx = Math.random() * this.playground.width;
                        let ty = Math.random() * this.playground.height;
                        this.move_to(tx, ty);
                    }
                }
                else {
                    let moved = Math.min(this.move_length, this.speed * this.timedelta / 1000);
                    this.x += this.vx * moved;
                    this.y += this.vy * moved;
                    this.move_length -= moved;
                }
            }
            this.render();
        }
    }
    ```
  
+ 烟花效果
  
  + 创建 `acapp/game/static/js/src/playground/particle/zbase.js`
  
    ```javascript
    class Particle extends AcGameObject {
        constructor(playground, x, y, radius, vx, vy, color, speed, move_length) {
            super();
            this.playground = playground;
            <!-- 获取权柄 -->
            this.ctx = this.playground.game_map.ctx;
            this.x = x;
            this.y = y;
            this.radius = radius;
            this.vx = vx;
            this.vy = vy;
            this.color = color;
            this.speed = speed;
            this.move_length = move_length;
            this.friction = 0.9;
            this.eps = 1;
        }
        update() {
            if (this.move_length < this.eps || this.speed < this.eps) {
                this.destory();
                return false;
            }
            let moved = Math.min(this.move_length, this.speed * this.timedelta / 1000);
            this.x += this.vx * moved;
            this.y += this.vy * moved;
            this.speed *= this.friction;
            this.move_length -= moved;
            this.render();
        }
        render() {
            this.ctx.beginPath();
            this.ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
            this.ctx.fillStyle = this.color;
            this.ctx.fill();
        }
    }
    ```
  
  + 修改 `acapp/game/static/js/src/playground/player/zbase.js`
  
    ```javascript
    class Player extends AcGameObject {
        is_attacked(angle, damage) {
            <!-- 释放烟花 -->
            for (let i = 0; i < 15 + Math.random() * 5; i ++ ) {
                let x = this.x;
                let y = this.y;
                let radius = this.radius * Math.random() * 0.3;
                let angle = Math.PI * 2 * Math.random();
                let vx = Math.cos(angle);
                let vy = Math.sin(angle);
                let color = this.color;
                let speed = this.speed * 10;
                let move_length = this.radius * Math.random() * 10;
                new Particle(this.playground, x, y, radius, vx, vy, color, speed, move_length);
            }
        }
    }
    ```

###### 人机相关设置

1. 给人机分配不同的颜色

   + 修改 `acapp/game/static/js/src/playground/zbase.js`

     ```javascript
     class AcGamePlayground {
         constructor(root) {
             for (let i = 0; i < 5; i ++ ) {
                 <!-- 颜色这一属性调用随机颜色函数即可 -->
                 this.players.push(new Player(this, this.width / 2, this.height / 2, this.height * 0.05, this.get_random_color(), this.height * 0.2, false));
             }
         }
         get_random_color() {
             let colors = ["red", "yellow", "blue", "green", "gray"];
             <!-- 返回随机颜色 -->
             return colors[Math.floor(Math.random() * 5)];
         }
     }
     ```

2. 设置开局 `5s` 后再进行攻击

   + 修改 `acapp/game/static/js/src/playground/player/zbase.js`

     ```javascript
     class Player extends AcGameObject {
         constructor(playground, x, y, radius, color, speed, is_me) {
             <!-- 计算时间 -->
             this.spent_time = 0;
         }
         update() {
             <!-- 计时 -->
             this.spent_time += this.timedelta / 1000;
             if (!this.is_me && this.spent_time > 5 && Math.random() < 1 / 180.0) {
                 let player = this.playground.players[Math.floor(Math.random() * this.playground.players.length)];
                 let tx = player.x + player.speed * this.vx * this.timedelta / 1000 * 2;
                 let ty = player.y + player.speed * this.vy * this.timedelta / 1000 * 2;
                 this.shoot_fireball(tx, ty);
             }
         }
     }
     ```


###### 部署 `nginx` 与对接 `acapp`

+ 增加容器运行的端口

  > 1. `https` 是 `443` 端口；`http` 是 `80` 端口
  >
  > 2. 给容器增加端口映射
  >
  >    + 登录容器，关闭所有运行中的任务
  >
  >    + 登录<font style="color:red">**运行容器的服务器**</font>，然后执行
  >
  >      ```dockerfile
  >      # 将容器保存成镜像，将CONTAINER_NAME替换成容器名称
  >      docker commit CONTAINER_NAME django_lesson:1.1
  >      # 停止容器
  >      docker stop CONTAINER_NAME
  >      # 删除容器
  >      docker rm CONTAINER_NAME
  >      # 使用保存的镜像重新创建容器
  >      docker run -p 20000:22 -p 8000:8000 -p 80:80 -p 443:443 --name CONTAINER_NAME -itd django_lesson:1.1
  >      ```
  >
  >    + 去租的服务器中开放 `80` 和 `443` 端口
  >

+ 创建 `Acapp`

  + 第一步：修改相关配置文件

    > 1. [讲义位置](https://www.acwing.com/file_system/file/content/whole/index/content/3257028/)
    > 2. 将 [nginx.conf](https://www.acwing.com/user/myspace/application/conf/nginx_conf/149/) 中的内容写到服务器 `/etc/nginx/nginx.conf`
    > 3. 将 [acapp.key](https://www.acwing.com/user/myspace/application/conf/acapp_key/149/) 中的内容写到服务器 `/etc/nginx/cert/acapp.key`
    > 4. 将 [acapp.pem](https://www.acwing.com/user/myspace/application/conf/acapp_pem/149/) 中的内容写到服务器 `/etc/nginx/cert/acapp.pem`
    > 5. 启动 `nginx` 服务 `sudo /etc/init.d/nginx start`，出现 `OK` 则说明成功启动服务

  + 第二步：修改 `Django` 项目的配置

    >

#### 注意事项

> 1. `js css ` 等代码更新时，浏览器不能及时更新的问题
>    + 打开谷歌浏览器 点开 `Network` 选项卡
>    + 禁用缓存即可 ☑️`Disable cache`
>    + <font style="color:red">**千万不要清除浏览器的所有数据**</font>

