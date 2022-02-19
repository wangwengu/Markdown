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

##### 部署 `nginx` 与对接 `acapp`

###### 增加容器运行的端口

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

###### 创建 `Acapp`

+ 第一步：修改相关配置文件

  > 1. [讲义位置](https://www.acwing.com/file_system/file/content/whole/index/content/3257028/)
  > 2. 将 [nginx.conf](https://www.acwing.com/user/myspace/application/conf/nginx_conf/149/) 中的内容写到服务器 `/etc/nginx/nginx.conf`
  > 3. 将 [acapp.key](https://www.acwing.com/user/myspace/application/conf/acapp_key/149/) 中的内容写到服务器 `/etc/nginx/cert/acapp.key`
  > 4. 将 [acapp.pem](https://www.acwing.com/user/myspace/application/conf/acapp_pem/149/) 中的内容写到服务器 `/etc/nginx/cert/acapp.pem`
  > 5. 启动 `nginx` 服务 `sudo /etc/init.d/nginx start`，出现 `OK` 则说明成功启动服务

+ 第二步：修改 `Django` 项目的配置

  > 1. 打开 `acapp/acapp/settings.py`
  >
  >    ```python
  >    # 将域名添加到ALLOWED_HOSTS
  >    ALLOWED_HOSTS = [
  >        # 注意：删除开头的https://
  >        'app149.acapp.acwing.com.cn'
  >    ]
  >    ```
  >
  > 2. 将 `DEBUG = True` 设置为 `DEBUG = False`
  >
  > 3. 将创建的 `app` 下的 `static` 文件，归档（复制）到根目录 `acapp` 下
  >
  >    ```python
  >    # 根目录下出现了static文件
  >    python3 manage.py collectstatic
  >    ```
  
+ 第三步：修改 `uwsgi` 配置文件

  >1. `uwsgi` 相当于在 `nginx` 和 `django` 中间架起了一座桥梁，访问速度更加快速和流畅
  >
  >2. 具体配置如下，例如：将配置文件写入 `acapp/scripts/uwsgi.ini`
  >
  >   ```ini
  >   [uwsgi]
  >   ; IP地址+端口号
  >   socket          = 127.0.0.1:8000
  >   ; Django项目地址
  >   chdir           = /home/django/acapp
  >   ; Django项目中wsgi的地址
  >   wsgi-file       = acapp/wsgi.py
  >   master          = true
  >   ; 开的进程（核）的数量
  >   processes       = 2
  >   ; 每个进程（核）的线程数量
  >   threads         = 5
  >   vacuum          = true
  >   ```
  >
  >3. 启动 `uwsgi` 服务
  >
  >   + `uwsgi --ini scripts/uwsgi.ini`
  >   + <font style="color:red">**注意⚠️：务必关闭 python3 ... runserver 服务**</font>

+ 第四步：填写剩余的信息

  > 1. `css` 地址：`https://app149.acapp.acwing.com.cn/static/css/game.css`
  > 2. `js` 地址：`https://app149.acapp.acwing.com.cn/statis/js/dist/game.js`
  > 3. 主类名：`js` 中暴露出来的类名（加 `export`）

+ 第五步：相关问题出错排查

  + 加载静态文件出错

    + 进入 `nginx` 配置文件

      ```bash
      # 修改nginx配置信息
      # 进入nginx配置文件夹
      cd /etc/nginx
      # 修改nginx配置文件
      sudo vim nginx.conf
      ```

    + 修改 `nginx` 配置文件

      ```nginx
      # 将static文件修改成自己的
      alias /home/django/acapp/static/;
      # 将media文件修改成自己的
      alias /home/django/acapp/media/;
      # 将443端口修改成自己网站地址
      server_name app149.acapp.acwing.com.cn;
      # 将80端口修改成自己网站地址
      server_name app149.acapp.acwing.com.cn;
      ```

+ `acapp bug` 修复

  + `acwing` 中应用的坐标位置不对

    + 产生原因：代码 `e.clientX, e.clientY` 写的是应用页面的相对位置，而画布是网页的绝对位置（最左上角），故需要做一个映射，使用函数 [`getBoundingClientRect()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect)

    + 图示如下：

      ![1642122637189](https://gitee.com/peter95535/image-bed/raw/master/img/1642122637189.jpg)
  
    + 修改 `acapp/game/statis/js/src/playground/player/zbase.js`
  
      ```javascript
      const rect = outer.ctx.canvas.getBoundingClientRect();
      outer.move_to(e.clientX - rect.left, e.clientY - rect.top);
      outer.shoot_fireball(e.clientX - rect.left, e.clientY - rect.top);
      ```

    + 重新打包之后，记得将 `app` 里面的 `static` 加载到根目录下，使用命令 `python3 manage.py collectstatic`

    + 清理下缓存，然后重新加载页面即可

  + 显示菜单界面
  
    + 修改 `acapp/game/static/js/src/zbase.js`
  
      ```javascript
      <!-- 取消此行的注释 -->
      this.menu = new AcGameMenu(this);
      ```
  
    + 修改 `acapp/game/static/js/src/playground/zbase.js`
  
      ```javascript
      class AcGamePlayground {
          constructor(root) {
              this.root = root;
              this.$playground = $(`
                  <div class = "ac_game_playground"></div>
              `);
              <!-- 刚开始隐藏玩家界面 -->
              this.hide();
          }
          show() {
              this.$playground.show();
              <!-- 将代码移动此处，玩家界面出来之后，再计算页面大小 -->
              this.root.$ac_game.append(this.$playground);
              this.width = this.$playground.width();
              this.height = this.$playground.height();
              this.game_map = new GameMap(this);
              this.players = [];
              this.players.push(new Player(this, this.width / 2, this.height / 2, this.height * 0.05, "white", this.height * 0.2, true));
              for (let i = 0; i < 10; i ++ ) {
                  this.players.push(new Player(this, this.width / 2, this.height / 2, this.height * 0.05, this.get_random_color(), this.height * 0.2, false));
              }
          }
      }
      ```
  
    + 修改 `acapp/scripts/compress_game_js.sh`
  
      ```bash
      # 先执行python3 manage.py collectstatic命令
      # 再输入yes
      echo yes | python3 manage.py collectstatic
      ```

  + 自适应页面大小
  
    + 修改 `acapp/game/static/css/game.css`
  
      ```css
      .ac_game_menu_field {
          /* 顶部和左边距修改成百分比% */
          top: 15%;
          left: 20%;
      }
      
      .ac_game_menu_field_item {
          /* 设置字体大小 */
          font-size: 5vh;
      }
      ```
  

##### 创建账号系统

###### 创建数据表

+ 第一步：将 `acapp/acapp/setting.py` 中的 `DEBUG` 改成 `True`，否则，前端只会出现错误代码不会出现具体错误信息

+ 第二步：在根目录下，运行命令 `python3 manage.py createsuperuser` 创建超级用户

+ 第三步：创建数据库表，进入 `acapp/game/models`

  + 创建 `player/player.py` 写入以下内容，函数 [`CASCADE`](https://www.cnblogs.com/nucdy/p/5768784.html) 的用法

    ```python
    # 继承数据库基类
    from django.db import models
    # 继承数据库用户表
    from django.contrib.auth.models import User
    # 每个数据表类都需要继承models.Model类
    class Player(models.Model):
        # 继承基础的用户信息（一对一的映射）
        # CASCADE 级联删除，user 删除的时候与之有关的对象都一并删除
        user = models.OneToOneField(User, on_delete = models.CASCADE)
        # 照片的URL
        photo = models.URLField(max_length = 256, blank = True)
    	# 显示用户的名称
        def __str__(self):
            return str(self.user)
    ```

  + 将数据库表注册到管理员页面，修改 `acapp/game/admin.py`

    ```python
    # 引入Player表
    from game.models.player.player import Player
    
    # Register your models here.
    # 将player表注册到管理员页面
    admin.site.register(Player)
    ```

  + 退到项目根目录下，按照顺序执行如下命令

    ```django
    python3 manage.py makemigrations
    python3 manage.py migrate
    ```

  + 重启 `uwsgi` 服务

  + <font style="color:red">**对数据库每一次改动都需要迁移并且重启服务**</font>

###### 登录注册界面

+ 主要逻辑步骤

  + 用户登录的时候会向服务器发送请求 `getinfo`，服务器会返回相对应的信息；当用户登录成功时，会返回用户的基本信息和头像信息；当用户未登录时，返回未登录信息

  + 附图一

    ![WeChatd3418912a93399114cc7d3435a258078](https://gitee.com/peter95535/image-bed/raw/master/img/WeChatd3418912a93399114cc7d3435a258078.png)

  + 附图二

    ![39750_2f407ec749-a](https://gitee.com/peter95535/image-bed/raw/master/img/39750_2f407ec749-a.png)

+ 判断是 `web` 端还是 `acapp` 端

  + 修改 `acapp/game/static/js/src/zbase.js`

    ```javascript
    export class AcGame {
        <!-- 添加 AcWingOs 变量，用于判断是哪个端口 -->
        constructor(id, AcWingOS) {
            this.AcWingOS = AcWingOS;
        }
    }
    ```

+ 编写 `getinfo` 函数

  + 创建 `acapp/game/views/settings/getinfo.py`

    ```python
    # 引入 JsonResponse 包
    from django.http import JsonResponse
    # 引入 Player 数据库表（类）
    from game.models.player.player import Player
    
    def getinfo_acapp(request):
        # 取得第一个用户值的信息
        player = Player.objects.all()[0]
        return JsonResponse({
            # 返回响应的结果
            'result': "success",
            # 返回用户名
            'username': player.user.username,
            # 返回用户的头像
            'photo': player.photo
        })
    
    def getinfo_web(request):
        player = Player.objects.all()[0]
        return JsonResponse({
            'result': "success",
            'username': player.user.username,
            'photo': player.photo
        })
    
    def getinfo(request):
        # 获取平台的信息
        platform = request.GET.get('platform')
        # 如果是 ACAPP 端
        if platform == 'ACAPP':
            return getinfo_acapp(request)
        elif:
            return getinfo_web(request)
    ```

+ 调用 `getinfo` 函数

  + 修改 `acapp/game/urls/settings/index.py`

    ```python
    from django.urls import path
    # 调用getinfo函数
    from game.views.settings.getinfo import getinfo
    
    urlpatterns = [
        # 添加路径
        path('getinfo/', getinfo, name = 'settings_getinfo')
    ]
    ```

  + 修改 `acapp/game/urls/index.py`

    ```python
    # 导入include关键字
    from django.urls import path, include
    
    urlpatterns = [
        # 添加路径路由
        path('settings/', include('game.urls.settings.index'))
    ]
    ```

+ 解决用户头像问题

  + 创建 `acapp/game/static/js/src/settings/zbase.js` 并写入以下内容
  
    ```javascript
    class Settings {
        constructor(root) {
            this.root = root;
            <!-- 默认平台是 WEB 平台 -->
            this.platform = "WEB";
            if (this.root.AcWingOS) {
                this.platform = "ACAPP";
            }
            this.start();
        }
        start() {
            this.getinfo();
        }
        login() {
            
        }
        getinfo() {
            let outer = this;
            $.ajax({
                <!-- url 地址别写错 -->
                url: "https://app149.acapp.acwing.com.cn/settings/getinfo/",
                type: "GET",
                data: {
                    platform: outer.platform
                },
                success: function(resp) {
                    console.log(resp);
                    <!-- 如果是登录状态，则显示菜单界面 -->
                    if (resp.result == "success") {
                        outer.hide();
                        outer.root.menu.show();
                    }
                    <!-- 如果是未登录状态，则跳转到登录界面 -->
                    else {
                        outer.login();
                    }
                }
            });
        }
        hide() {}
    }
    ```
  
  + 修改 `acapp/game/static/js/src/zbase.js`，并写入以下内容
  
    ```javascript
    this.settings = new Settings(this);
    ```
  
  + 修改 `acapp/game/static/js/src/menu/zbase.js`，添加一下内容
  
    ```javascript
    show() {
    	this.$menu.show();
    }
    ```
  
  + 修改 `acapp/game/views/settings/getinfo.py`
  
    ```python
    def getinfo_web(request):
        user = request.user
        if not user.is_authenticated:
            return JsonResponse({
                'result': "未登录"
            })
        else:
            player = Player.objects.all()[0]
            return JsonResponse({
                'result': "success",
                'username': player.user.username,
                'photo': player.photo
            })
    ```
  
    
  
  + 修改 `acapp/game/static/js/src/menu/zbase.js`
  
    ```javascript
    <!-- 一开始将菜单界面 menu 隐藏 -->
    <!-- 如果是登录状态，再进行显示 -->
    this.hide();
    ```
  
  + 头像渲染问题
  
    + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
      ```javascript
      class Settings {
          constructor(root) {
              <!-- 添加用户的用户名和头像 -->
              this.username = "";
              this.photo = "";
          }
          getinfo() {
                  success: function(resp) {
                      if (resp.result === "success") {
                          <!-- 如果成功登录，获取用户的名称的头像 -->
                          outer.username = resp.username;
                          outer.photo = resp.photo;
                          
                      }
                  }
              });
          }
      }
      ```
  
    + 修改 `acapp/game/static/js/src/playground/player/zbase.js`
  
      ```javascript
      class Player extends AcGameObject {
          constructor(playground, x, y, radius, color, speed, is_me) {
              <!-- 如果是本人，则获取头像和头像地址 -->
              if (this.is_me) {
                  this.img = new Image();
                  this.img.src = this.playground.root.settings.photo;
              }
          }
          render() {
              <!-- 如果是本人，则加载用户头像并且将头像画出来 -->
              if (this.is_me) {
                  this.ctx.save();
                  this.ctx.beginPath();
                  this.ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
                  this.ctx.stroke();
                  this.ctx.clip();
                  this.ctx.drawImage(this.img, this.x - this.radius, this.y - this.radius, this.radius * 2, this.radius * 2); 
                  this.ctx.restore();
              }
              else {
                  this.ctx.beginPath();
                  this.ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
                  this.ctx.fillStyle = this.color;
                  this.ctx.fill();
              }
          }
      }
      ```
  
+ 编写登录注册界面

  + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    class Settings {
        constructor(root) {
            <!-- 创建 settings 界面 -->
            this.$settings = $(`
                <div class ="ac_game_settings"></div>
            `);
            <!-- 将 settigns 界面加入到 ac_game 中 -->
            this.root.$ac_game.append(this.$settings);
        }
    }
    ```
  
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 添加 ac_game_settings 的样式 */
    .ac_game_settings {
        width: 100%;
        height: 100%;
        background-image: url("/static/image/menu/background.gif");
        background-size: 100% 100%;
        user-select: none;
    }
    ```
  
  + 添加登录注册界面 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    class Settings {
        constructor(root) {
            this.$settings = $(`
                <div class="ac_game_settings">
                	<!-- 添加登录界面 -->
                    <div class="ac_game_settings_login"></div>
                    <!-- 添加注册界面 -->
                    <div class="ac_game_settings_register"></div>
                </div>
            `);
            <!-- 获取登录权柄 -->
            this.$login = this.$settings.find(".ac_game_settings_login");
            <!-- 将登录界面隐藏 -->
            this.$login.hide();
            <!-- 获取注册权柄 -->
            this.$register = this.$settings.find(".ac_game_settings_register");
            <!-- 将注册界面隐藏 -->
            this.$register.hide();
        }
        login() {
            <!-- 登录时，先将注册界面隐藏 -->
            this.$register.hide();
            <!-- 再将登录界面打开 -->
            this.$login.show();
        }
        register() {
            <!-- 注册时，先将登录界面隐藏 -->
            this.$login.hide();
            <!-- 再将注册界面打开 -->
            this.$register.show();
        }
    }
    ```
  
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 添加 ac_game_settings_login 的样式 */
    .ac_game_settings_login {
        height: 41vh;
        width: 20vw;
        position: relative;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: rgba(0, 0, 0, 0.7);
    }
    ```
  
  + 编写登录代码，修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    this.$settings = $(`
      	<div class="ac_game_settings">
    		<div class="ac_game_settings_login">
      			<div class="ac_game_settings_title">
    				登录
              	</div>
    		</div>
    	</div>
    `);
    
    ```
  
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 添加 ac_game_settings_login 的样式 */
    .ac_game_settings_login {
        border-radius: 5px;
    }
    
    /* 修改 ac_game_settings_title 的样式 */
    .ac_game_settings_title {
        color: white;
        font-size: 3vh;
        text-align: center;
        padding-top: 2vh;
        margin-bottom: 2vh;
    }
    ```
  
  + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    this.$settings = $(`
    	<div class="ac_game_settings">
    		<!-- 添加用户名信息 -->
    		<div class="ac_game_settings_username">
    			<!-- 用户名和密码等信息样式类似，因此设置类名相同 -->
    			<div class="ac_game_settings_item">
    				<input type="text" placeholder="用户名">
    			</div>
    		</div>
    	</div>
    `);
    ```
    
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 修改 ac_game_settings_username 的样式 */
    .ac_game_settings_username {
        /* 将其变成块状元素 */
        display: block;
        height: 7vh;
    }
    
    /* 修改 ac_game_settings_item 的样式 */
    .ac_game_settings_item {
        width: 100%;
        height: 100%;
    }
    
    /* 修改 ac_game_settings_item 下的 input 的样式 */
    .ac_game_settings_item > input {
        width: 90%;
        line-height: 3vh;
        position: relative;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
    ```
  
  + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    this.$settings = $(`
    	<div class="ac_game_settings">
    		<div class="ac_game_settings_login">
    			<!-- 添加密码信息 -->
    			<div class="ac_game_settings_password">
    				<!-- 用户名和密码等信息样式类似，因此设置类名相同 -->
    				<div class="ac_game_settings_item">
    					<input type="password" placeholder="密码">
    				</div>
    			</div>
    		</div>
    	</div>
    `);
    ```
  
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 修改 ac_game_settings_password 的样式 */
    .ac_game_settings_password {
        display: block;
        height: 7vh;
    }
    ```
  
  + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    this.$settings = $(`
    	<div class="ac_game_settings">
    		<div class="ac_game_settings_login">
    			<div class="ac_game_settings_submit">
    				div class="ac_game_settings_item">
    					<button>登录</button>
    				</div>
    			</div>
    		</div>
    	</div>
    `);
    ```
  
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 修改 ac_game_settings_submit 的样式 */
    .ac_game_settings_submit {
        display: block;
        height: 7vh;
    }
    
    /* 修改 ac_game_settings_item 下的 button 的样式 */
    .ac_game_settings_item > button {
        color: white;
        width: 90%;
        line-height: 3vh;
        position: relative;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: #4CAF50;
        border-radius: 5px;
    }
    ```
  
  + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    this.$settings = $(`
    	<div class="ac_game_settings">
    		<div class="ac_game_settings_login">
    			<div class="ac_game_settings_error_messages">
    				用户名密码错误
    			</div>
    		</div>
    	</div>
    `);
    ```
  
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 修改 ac_game_settings_error_messages 的样式 */
    .ac_game_settings_error_messages {
        color: red;
        font-size: 0.8vh;
        display: inline;
        float: left;
        padding-left: 1vw;
    }
    ```
  
  + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    this.$settings = $(`
    	<div class="ac_game_settings">
    		<div class="ac_game_settings_login">
    			<div class="ac_game_settings_option">
    				注册
    			</div>
    		</div>
    	</div>
    `);
    ```
  
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 修改 ac_game_settings_option 的样式 */
    .ac_game_settings_option {
        color: white;
        font-size: 1.8vh;
        display: inline;
        float: right;
        padding-right: 1vw;
        cursor: pointer;
    }
    ```
  
  + 下载 `logo` 图片到 `acapp/game/static/image/settings/acwing_login.png`
  
    + `wget --output-document=图片别名 图片地址 ` [图片地址](https://cdn.acwing.com/media/article/image/2021/11/18/1_ea3d5e7448-logo64x64_2.png)
    + 记得重启服务
  
  + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    this.$settings = $(`
    	<div class="ac_game_settings">
    		<div class="ac_game_settings_login">
    			<!-- 记得加个换行符 -->
    			<br>
    			<div class="ac_game_settings_acwing">
    				<img width="30px;" src="https://app149.acapp.acwing.com.cn/static/image/settings/acwing_login.png">
    			</div>
    		</div>
    	</div>
    `);
    
    ```
  
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 修改 ac_game_settings_acwing 的样式 */
    .ac_game_settings_acwing {
        display: block;
        height: 7vh;
    }
    
    /* 修改 ac_game_settings_acwing 下的 img 的样式 */
    .ac_game_settings_acwing > img {
        position: relative;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        cursor: pointer;
    }
    ```
  
  + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    this.$settings = $(`
    	<div class="ac_game_settings">
    		<div class="ac_game_settings_login">
    			<div class="ac_game_settings_acwing">
    				<br>
    				<div>
    					AcWing一键登录
    				</div>
    			</div>
    		</div>
    	</div>
    `);
    ```
  
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 修改 ac_game_settings_login 的样式 */
    .ac_game_settings_login {
        /* 修改高度为 43 vh */
        height: 43vh;
    }
    
    /* 修改 ac_game_settings_acwing 下的 img 的样式 */
    .ac_game_settings_acwing > img {
        /* 修改为块状元素 */
        display: block;
    }
    
    /* 修改 ac_game_settings_acwing 下的 div 的样式 */
    .ac_game_settings_acwing > div {
        color: white;
        font-size: 1.5vh;
        text-align: center;
    }
    ```
  
  + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    this.$settings = $(`
    	<div class="ac_game_settings">
    		<div class="ac_game_settings_register">
    			<div class="ac_game_settings_title">
    				注册
    			</div>
    			<div class="ac_game_settings_username">
    				<div class="ac_game_settings_item">
    					<input type="text" placeholder="用户名">
    				</div>
    			</div>
    			<div class="ac_game_settings_password">
    				<div class="ac_game_settings_item">
    					<input type="password" placeholder="密码">
    				</div>
    			</div>
    			<div class="ac_game_settings_password">
    				<div class="ac_game_settings_item">
    					<input type="password" placeholder="确认密码">
    				</div>
    			</div>
    			<div class="ac_game_settings_submit">
    				<div class="ac_game_settings_item">
    					<button>注册</button>
    				</div>
    			</div>
    			<div class="ac_game_settings_error_messages">
    			</div>
    			<div class="ac_game_settings_option">
    				登录
    			</div>
    			<br>
    			<div class="ac_game_settings_acwing">
    				<img width="30px;" src="https://app149.acapp.acwing.com.cn/static/image/settings/acwing_login.png">
    				<br>
    				<div>
    					AcWing一键登录
    				</div>
    			</div>
    	</div>
    `);
    ```
  
  + 修改 `acapp/game/static/css/game.css`
  
    ```css
    /* 修改ac_game_settings_register的样式 */
    .ac_game_settings_register {
        height: 50vh;
        width: 20vw;
        position: relative;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: rgba(0, 0, 0, 0.7);
        border-radius: 5px;
    }
    ```
  
  + 将登录和注册等相关信息抠出来，修改 `acapp/game/static/js/src/settings/zbase.js`
  
    ```javascript
    <!-- 将登录所用的用户名、密码等信息抠出来 -->
    <!-- 注意是this.$login不是this.$settings -->
    this.$login_username = this.$login.find(".ac_game_settings_username input");
    this.$login_password = this.$login.find(".ac_game_settings_password input");
    this.$login_submit = this.$login.find(".ac_game_settings_submit button");
    this.$login_error_message = this.$login.find(".ac_game_settings_error_messages");
    this.$login_register = this.$login.find(".ac_game_settings_option");
    <!-- 将注册所用的用户名、密码等信息抠出来 -->
    <!-- 注意是this.$register不是this.$settings -->
    this.$register_username = this.$register.find(".ac_game_settings_username input");
    this.$register_password = this.$register.find(".ac_game_settings_password_first input");
    this.$register_password_confirm = this.$register.find(".ac_game_settings_password_second input");
    this.$register_submit = this.$register.find(".ac_game_settings_submit button");
    this.$register_error_message = this.$register.find(".ac_game_settings_error_messages");
    this.$register_login = this.$register.find(".ac_game_settings_option");
    hide() {
    	this.$settings.hide();
    }
    ```
  
  + 登录和注册之间的跳转
  
    ```javascript
    start() {
        <!-- 添加监听事件 -->
    	this.add_listening_events();
    }
    add_listening_events() {
        <!-- 添加登录的监听事件 -->
    	this.add_listening_events_login();
        <!-- 添加注册的监听事件 -->
    	this.add_listening_events_register();
    }
    add_listening_events_login() {
    	let outer = this;
        <!-- 绑定监听事件 -->
        <!-- 在登录界面点击注册，跳转到注册界面 -->
    	this.$login_register.click(function() {
    		outer.register();
    	});
    }
    add_listening_events_register() {
    	let outer = this;
        <!-- 绑定监听事件 -->
        <!-- 在注册界面点击登录，跳转到登录界面 -->
    	this.$register_login.click(function() {
    		outer.login();
    	});
    }
    ```
  
  + 登录后台交互
  
    + 创建 `acapp/game/views/settings/login.py`，写入以下内容
  
      ```python
      from django.http import JsonResponse
      from django.contrib.auth import authenticate, login
      
      def signin(request):
          data = request.GET
          username = data.get('username')
          password = data.get('password')
      	# Django存储的是密码的哈希值，因此，使用内置函数
      	# Django比较的是密码的哈希值，不是密码的值本身，先将输入的密码哈希，再与数据库中的哈希值进行比较，查看是否一致
          user = authenticate(username = username, password = password)
          if not user:
              return JsonResponse({
                  'result': "用户名或者密码不正确"
              })
      	# login内置函数将登录的信息存储在浏览器的cookie里面
          login(request, user)
          return JsonResponse({
              'result': "success"
          })
      ```
  
    + 修改 `acapp/game/urls/settings/index.py`
  
      ```python
      # 添加signin函数
      from game.views.settings.login import signin
      
      urlpatterns = [
          # 添加相对应的路由
          path('login/', signin, name = 'settings_login')
      ]
      ```
  
    + 修改 `acapp/game/static/js/src/settings/zbase.js`
  
      ```javascript
      add_listening_events_login() {
          <!-- 给提交按钮绑定监听函数 -->
      	this.$login_submit.click(function() {
      		outer.login_on_remote();
      	});
      }
      login_on_remote() {
      	let outer = this;
          <!-- 获取input里面输入的值 -->
      	let username = this.$login_username.val();
          <!-- 获取input里面输入的值 -->
      	let password = this.$login_password.val();
          <!-- 将错误信息清空 -->
      	this.$login_error_message.empty();
      	$.ajax({
      		url: 'https://app149.acapp.acwing.com.cn/settings/login/',
      		type: 'GET',
      		data: {
      			username: username,
      			password: password
      		},
      		success: function(resp) {
      			console.log(resp);
      			if (resp.result === 'success') {
      				location.reload();
      			}
      			else {
      				outer.$login_error_message.html(resp.result);
      			}
      		}
      	});
      }
      ```
  
  + 登出后台交互
  
    + 修改 `acapp/game/views/settings/logout.py`
  
      ```python
      from django.http import JsonResponse
      # 导入登出函数
      from django.contrib.auth import logout
      
      def signout(request):
          user = request.user
          # 如果用户没有登录，则正常返回
          if not user.is_authenticated:
              return JsonResponse({
                  'result': "success"
              })
          # 如果用户处于登录状态，则退出登录，之后再正常返回
          logout(request)
          return JsonResponse({
              'result': "success"
          })
      ```
    
    + 修改 `acapp/game/urls/settings/index.py`
    
      ```python
      # 导入视图函数
      from game.views.settings.logout import signout
      
      urlpatterns = [
          # 添加路由
          path('logout/', signout, name = 'settings_logout')
      ]
      ```
    
    + 修改 `acapp/game/static/js/src/settings/zbase.js`
    
      ```javascript
      <!-- 添加登出函数 -->
      logout_on_remote() {
          <!-- 如果当前登录的平台是ACAPP，则直接返回即可，因为ACAPP不是退出登录而是关闭界面 -->
      	if (this.platform === 'ACAPP') return false;
      	$.ajax({
      		url: 'https://app149.acapp.acwing.com.cn/settings/logout/',
      		type: 'GET',
      		success: function(resp) {
      			console.log(resp);
      			if (resp.result === 'success') {
                      <!-- 刷新界面 -->
      				location.reload();
      			}
      		}
      	});
      }
      ```
    
    + 绑定登出点击事件，修改 `acapp/game/static/js/src/menu/zbase.js`
    
      ```javascript
      this.$menu = $(`
      	<div class="ac_game_menu">
      		<div class="ac_game_menu_field">
      			<div class="ac_game_menu_field_item ac_game_menu_field_item_settings">
      				退出
      			</div>
      		</div>
      	</div>
      `);
      <!-- 获取设置的权柄 -->
      this.$settings = this.$menu.find('.ac_game_menu_field_item_settings');
      <!-- 点击设置的时候，触发点击事件 -->
      this.$settings.click(function() {
          <!-- 调用logout_on_remote()函数 -->
      	outer.root.settings.logout_on_remote();
      });
      ```
    
  + 注册后台交互
  
    + 修改 `acapp/game/views/settings/register.py`
  
      ```python
      from django.http import JsonResponse
      from django.contrib.auth import login
      from django.contrib.auth.models import User
      from game.models.player.player import Player
      
      def register(request):
          data = request.GET
          # 获取用户名，否则，返回空
          # strip()去除前后空格
          username = data.get('username', '').strip()
          # 获取密码，否则，返回空
          # strip()去除前后空格
          password = data.get('password', '').strip()
          # 获取确认密码，否则，返回空
          # strip()去除前后空格
          password_confirm = data.get('password_confirm', '').strip()
          if not username or not password or not password_confirm:
              return JsonResponse({
                  'result': '用户名或者密码不能为空'
              })
          if password != password_confirm:
              return JsonResponse({
                  'result': '两次密码不一致'
              })
          if User.objects.filter(username = username).exists():
              return JsonResponse({
                  'result': '用户名已存在'
              })
          user = User(username = username)
          user.set_password(password)
          # 将新创建的用户进行保存
          user.save()
          # 创建user的其他属性
          Player.objects.create(
                  user = user,
                  photo = 'https://ss0.baidu.com/-Po3dSag_xI4khGko9WTAnF6hhy/baike/pic/item/4afbfbedab64034f930bfd5eafc379310b551da7.jpg')
          # 将新用户注册到后台页面
          login(request, user)
          return JsonResponse({
              'result': 'success'
          })
      ```
      
    + 修改路由 `acapp/game/urls/settings/index.py`
    
      ```python
      # 导入注册视图
      from game.views.settings.register import register
      
      urlpatterns = [
          # 添加注册路由
          path('register/', register, name = 'settings_register')
      ]
      ```
    
    + 修改前端页面 `acapp/game/static/js/src/settings/zbase.js`
    
      ```javascript
      class Settings {
          add_listening_events_register() {
              <!-- 绑定提交按钮事件 -->
              this.$register_submit.click(function() {
                  outer.register_on_remote();
              });
          }
          register_on_remote() {
              let outer = this;
              let username = this.$register_username.val();
              let password = this.$register_password.val();
              let password_confirm = this.$register_password_confirm.val();
              this.$register_error_message.empty();
              $.ajax({
                  url: 'https://app149.acapp.acwing.com.cn/settings/register/',
                  type: 'GET',
                  data: {
                      username: username,
                      password: password,
                      password_confirm: password_confirm
                  },
                  success: function(resp) {
                      console.log(resp);
                      if (resp.result === 'success') {
                          <!-- 重新加载页面 -->
                          location.reload();
                      }
                      else {
                          <!-- 打印错误信息 -->
                          outer.$register_error_message.html(resp.result);
                      }
                  }
              })
          }
      }
      ```
    
  + 网页端 `AcWing` 一键登录
  
    + 第一步：在 `Django` 中集成 `redis` 也叫内存数据库，存储的是键值对
    
      + 安装 `redis` ，执行命令 `pip3 install django_redis` 进行安装
    
      + 配置 `acapp/acapp/settings.py` 文件，写入内容
    
        ```python
        # 注意粘贴的位置，位于# Database的位置，也就是数据库的位置
        CACHES = { 
            'default': {
                'BACKEND': 'django_redis.cache.RedisCache',
                'LOCATION': 'redis://127.0.0.1:6379/1',
                "OPTIONS": {
                    "CLIENT_CLASS": "django_redis.client.DefaultClient",
                },  
            },  
        }
        USER_AGENTS_CACHE = 'default'
        ```
    
      + 启动 `redis-server` 服务
    
        ```bash
        sudo redis-server /etc/redis/redis.conf
        ```
    
      + 使用 `top` 命令查看是否含有进程 `redis-server`
    
      + 重启服务 `uwsgi`即可
    
      + `redis` 的基本操作
    
        + 执行命令 `python3 manage.py shell`，打开 `django` 后台
    
          ```python
          # 导入缓存包
          from django.core.cache import cache
          # 查询所有的键keys，支持正则表达式
          cache.keys('*')
          # 设置键值对
          # 第一个参数：key
          # 第二个参数：value
          # 第三个参数：过期时间（单位：秒），None表示永远不会过期
          cache.set('yxc', 1, 5)
          # 查询某个键是否存在，存在返回True，否则，返回False
          cache.has_key('wyp')
          # 查询某个键对应的值
          cache.get('wyp')
          # 删除关键字
          cache.delete('wyp')
          ```
    
    + 第二步：`OAuth2` 协议执行过程详解
    
      ![2110029](https://gitee.com/peter95535/image-bed/raw/master/img/2110029.png)
      
      + 注意事项：`token` 令牌只有需要拿用户名和密码的时候才有用，因为，第一次拿完之后会将信息存储在系统的数据库里面，下次，直接查询数据库即可
      
    + 第三步：`OAuth2` 具体逻辑代码，[讲义](https://www.acwing.com/blog/content/12466/)
    
      + 第一步：添加 `player` 的 `openid` 属性
    
        + 修改 `acapp/game/models/player`
    
          ```python
          # 添加openid属性
          # 第一个参数：默认值
          # 第二个参数：最大长度
          # 第三个参数：空
          # 第四个参数：空
          openid = models.CharField(default = '', max_length = 50, blank = True, null = True)
          ```
    
        + 将修改注册到 `Django` 后台，返回到 `acapp/`，执行如下命令
    
          ```bash
          # 准备做迁移，准备工作
          python3 manage.py makemigrations
          # 迁移
          python3 manage.py migrate
          ```
    
        + 务必记得重启服务器
    
      + 第二步：申请授权码 `code`
    
        + 修改 `acapp/game/views/settings/acwing/web/apply_code.py` 并且写入代码
    
          ```python
          from django.http import JsonResponse
          
          # 申请授权码code的函数
          def apply_code(request):
              appid = '149'
          ```
    
        + 修改 `acapp/game/views/settings/acwing/web/receive_code.py` 并且写入代码
    
          ```python
          from django.shortcuts import redirect
          
          def receive_code(request):
              pass
          ```
    
        + 修改 `acapp/game/urls/settings/acwing/index.py`
    
          ```python
          from django.urls import path
          
          urlpatterns = [
          ]
          ```
    
        + 修改 `acapp/game/urls/settings/index.py`
    
          ```python
          # 导入include函数
          from django.urls import path, include
          
          urlpatterns = [
              # 添加web的路由
              path('acwing/', include('game.urls.settings.acwing.index'))
          ]
          ```
    
        + 修改 `acapp/game/urls/settings/acwing/index.py`
    
          ```python
          # 导入申请授权码的函数apply_code
          from game.views.settings.acwing.apply_code import apply_code
          # 导入接收授权码的函数receive_code
          from game.views.settings.acwing.receive_code import receive_code
          
          urlpatterns = [
              # 添加申请授权码的路由
              path('web/apply_code/', apply_code, name = 'settings_acwing_apply_code'),
              # 添加接收授权码的路由
              path('web/receive_code/', receive_code, name = 'settings_acwing_receive_code')
          ]
          ```
    
        + 修改 `acapp/game/views/settings/acwing/web/apply_code.py`
    
          ```python
          from django.http import JsonResponse
          
          def apply_code(request):
              # 返回值
              return JsonResponse({
                  'result': "success"
              })
          ```
    
        + 修改 `acapp/game/views/settings/acwing/web/receive_code.py`
    
          ```python
          from django.shortcuts import redirect
          
          def receive_code(request):
              # 返回重定向的网址
              # 'index'对应【acapp/game/urls/index.py】中的路由的第一项，name=index
              return redirect('index')
          ```
    
        + 修改 `acapp/game/views/settings/getinfo.py`
    
          ```python
          def getinfo_web(request):
              user = request.user
              if not user.is_authenticated:
                  return JsonResponse({
                      'result': "未登录"
                  })
              else:
                  # 返回当前用户的信息，而不是每次返回管理员信息，不能写死
                  player = Player.objects.get(user = user)
                  return JsonResponse({
                      'result': "success",
                      'username': player.user.username,
                      'photo': player.photo
                  })
          ```
    
        + 修改 `acapp/game/views/settings/acwing/web/apply_code.py`
    
          ```python
          # 导入编码库
          from urllib.parse import quote
          # 导入随机数库
          from random import randint
          # 导入redis相关库
          from django.core.cache import cache
          
          # 随机生成8位随机数
          def get_state():
              res = ''
              for i in range(8):
                  res += str(randint(0, 9))
              return res
          
          def apply_code(request):
              # 重定向地址
              redirect_uri = quote('https://app149.acapp.acwing.com.cn/settings/acwing/web/receive_code/')
              # 获取用户信息
              scope = 'userinfo'
              # 获取状态，8位的随机数
              state = get_state()
              # 设置状态的有效期，单位：秒
              cache.set(state, True, 7200)
              # code的申请地址
              apply_code_url = 'https://www.acwing.com/third_party/api/oauth2/web/authorize/'
              return JsonResponse({
                  'result': "success",
                  # 返回拼接的url地址
                  'apply_code_url': apply_code_url + '?appid=%s&redirect_uri=%s&scope=%s&state=%s' % (appid, redirect_uri, scope, state)
              })
          ```
    
        + 修改 `acapp/game/static/js/src/settings/zbase.js`
    
          ```javascript
          class Settings {
              constructor(root) {
                  <!-- 找到AcWing一键登录的按钮 -->
                  this.$acwing_login = this.$settings.find(".ac_game_settings_acwing img")
              }
              add_listening_events() {
                  <!-- 添加外部权柄 -->
                  let outer = this;
                  <!-- 点击AcWing一键登录按钮 -->
                  this.$acwing_login.click(function() {
                      <!-- 触发一键登录函数 -->
                      outer.acwing_login();
                  });
              }
              <!-- 一键登录函数 -->
              acwing_login() {
                  <!-- 返回ajax信息 -->
                  $.ajax({
                      <!-- 请求的url -->
                      url: 'https://app149.acapp.acwing.com.cn/settings/acwing/web/apply_code',
                      type: 'GET',
                      success: function(resp) {
                          console.log(resp);
                          if (resp.result === 'success') {
                              <!-- 如果成功，返回重定向的网址 -->
                              window.location.replace(resp.apply_code_url)
                          }
                      }
                  })
              }
          }
          ```
    
        + 修改 `acapp/game/views/settings/acwing/web/receive_code.py`
        
          ```python
          import requests
          from django.shortcuts import redirect
          from django.core.cache import cache
          from django.contrib.auth.models import User
          from game.models.player.player import Player
          from django.contrib.auth import login
          from random import randint
          
          def receive_code(request):
              # 获取数据
              data = request.GET
              # 获取授权码
              code = data.get('code')
              # 获取状态
              state = data.get('state')
              # 如果当前请求来源于别的地方，则直接忽略
              if not cache.has_key(state):
                  return redirect('index')
              # 将当前状态删掉
              cache.delete(state)
              # 获取access_token的API地址
              apply_access_token_url = 'https://www.acwing.com/third_party/api/oauth2/access_token/'
              # 参数
              params = {
                  'appid': '149',
                  'secret': '023a763448e444ae80815e8dc107fa8f',
                  'code': code
              }
              # 转成json格式
              access_token_res = requests.get(apply_access_token_url, params = params).json()
              # 提取access_token
              access_token = access_token_res['access_token']
              # 提取openid
              openid = access_token_res['openid']
              # 获取用户
              players = Player.objects.filter(openid = openid)
              # 如果用户已经存在
              if players.exists():
                  # 直接登录即可
                  login(request, players[0].user)
                  # 重定向到首页
                  return redirect('index')
              # 获取userinfo的API接口
              get_userinfo_url = 'https://www.acwing.com/third_party/api/meta/identity/getinfo/'
              # 参数
              params = {
                  'access_token': access_token,
                  'openid': openid
              }
              # 转成json格式
              userinfo_res = requests.get(get_userinfo_url, params = params).json()
              # 提取用户名
              username = userinfo_res['username']
              # 提取用户头像
              photo = userinfo_res['photo']
              # 如果用户已经存在【授权的账号的名称和当前网站的用户名重复】，则随机添加某个数，防止冲突
              while User.objects.filter(username = username).exists():
                  username += str(randint(0, 9))
            	# 创建用户的基本信息
              user = User.objects.create(username = username)
              # 创建用户的附加信息
              player = Player.objects.create(user = user, photo = photo, openid = openid)
              # 使用新创建的用户进行登录
              login(request, user)
              return redirect('index')
          ```
          
  

#### end

#### 注意事项

> 1. `js css ` 等代码更新时，浏览器不能及时更新的问题
>    
>    + 方案一
>    
>      + 打开谷歌浏览器 点开 `Network` 选项卡
>    
>      + 禁用缓存即可 ☑️`Disable cache`
>    
>      + <font style="color:red">**千万不要清除浏览器的所有数据**</font>
>    
>    + 方案二
>    
>      + 选择需要刷新的网页
>      + 【<font style = "color: red">**右键**</font>】点击标签栏的刷新按钮
>      + 点击第三项"清空缓存并硬性重新加载"

