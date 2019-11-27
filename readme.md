# canvas刮刮卡游戏开发

先看效果:

![video](C:\Users\91583\Desktop\video.gif)








## 一.基础知识-画布元素的使用

### 1 绘制线条



- #### 思路

  1. 在页面中添加画布元素
  2. 获取画布元素的上下文环境对象
  3. 使用上下文环境对象绘制图形，保存在内存中
  4. 绘制一个线条

  ```javascript
  //设置画布的起始点
  context.moveTo(x,y);
  //从当前位置绘制直线到x,y坐标
  context.lineTo(x,y);
  ```
  案例和效果:

  ![1574838663776](C:\Users\91583\AppData\Roaming\Typora\typora-user-images\1574838663776.png)



### 2. 绘制不同线条颜色的三角形

- #### 绘制三角形

  绘制一个简单的三角形

- #### 绘制的颜色与线条宽度

  ```javascript
  //设置笔触图形的颜色
  context.strokeStyle=color
  -------------------------
  //设置线条的宽度，以像素为单位
  context.lineWidth=number
  ```

- #### 重置路径

- ```javascript
  //开始一条路径，或重置当前的路径,并切断和上一个图形的路径联系
  context.beginPath()
  ```

- #### 重置路径的优势

  可以实现不同大小和颜色图形的单独绘制。

- #### 案例和效果:

代码:
```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <canvas id="cvs" style="border: solid 1px #ccc"></canvas>

    <script type="text/javascript">
        //获取画布元素
        var cvs=document.getElementById("cvs");
        //获取工具集
        var cxt=cvs.getContext("2d");
        //设置绘制图形的线条宽度
        cxt.lineWidth=5;

         
        //设置绘制图形的颜色
        cxt.strokeStyle='red';
        //定位一个起始点
        cxt.moveTo(50,50);
        //绘制第二个点
        cxt.lineTo(150,50)
        //绘制线条
        cxt.stroke()
        //重置当前路径
        cxt.beginPath();

        //设置绘制图形的颜色
        cxt.strokeStyle='blue';
        //设置绘制第二条线起始点
        cxt.moveTo(150,50);
        //绘制第二个点
        cxt.lineTo(80,100)
        //绘制线条
        cxt.stroke()
        //重置当前路径
        cxt.beginPath();

        //设置绘制图形的颜色
        cxt.strokeStyle='green';
        //设置绘制第二条线起始点
        cxt.moveTo(80,100);
        //返回原始点
        cxt.lineTo(50,50)
        //绘制线条
        cxt.stroke()

    </script>
</body>
</html>
```
效果:



![1574838839267](C:\Users\91583\AppData\Roaming\Typora\typora-user-images\1574838839267.png)






## 二.基础知识-其他绘制API

### 1. 文字绘制API

- #### 指定位置

  ```javascript
  //在指定位置和宽度内绘制文字
  context.fillText(text,x,y,maxWidth); //最大宽度
  ```

- #### 设置字体名称和样式

  ```javascript
  //设置字体名称和形状
  context.font='字体属性' //bold 32px sans-serif
  ```

- #### 设置字体对齐位置

  ```javascript
  //设置文本内容水平对齐方式
  context.textAlign='水平方位值' //center|left|right
  //设置文本内容垂直对齐方式
  context.textBaseline='垂直方位值' //top|middle|bottom
  ```

- #### 绘制内容另存为图片

  ```javascript
  //当前绘制内容存为图片
  context.toDataURL(type, encoderOptions);//image/png图片格式,0到1区间图片质量
  ```

- #### 案例和效果:

- 普通文字

![1574838894223](C:\Users\91583\AppData\Roaming\Typora\typora-user-images\1574838894223.png)

带有对齐方式和另存为图片

![1574839011294](C:\Users\91583\AppData\Roaming\Typora\typora-user-images\1574839011294.png)





### 2. 绘制矩形和圆形及图片

- 绘制矩形

  ```javascript
  //绘制矩形的路径
  context.rect(x,y,width,height);
  //绘制无填充的矩形
  context.strokeRect(x,y,width,height);
  //绘制填充的矩形
  context.fillRect(x,y,width,height);
  //清空指定矩形内像素
  context.clearRect(x,y,width,height);
  ```
  案例和效果:

  ![1574839411125](C:\Users\91583\AppData\Roaming\Typora\typora-user-images\1574839411125.png)

- 绘制有弧度的圆形

  ```javascript
  //在指定位置绘制一个圆形
  context.arc(x,y,r,sAngle,eAngle,clockwise);
  ```


- 绘制不同大小的图片

  ```javascript
  //在画布上绘制固定坐标的图像
  context.drawImage(img,x,y);
  //在画布上绘制不仅固定坐标，且规定图像的宽度和高度图像
  context.drawImage(img,x,y,width,height);
  //在画布上剪切图像，并在画布上绘制被剪切的部分
  context.drawImage(img,sx,sy,swidth,sheight,x,y,width,height);
  
  ```
  案例和效果:

  ![1574839493469](C:\Users\91583\AppData\Roaming\Typora\typora-user-images\1574839493469.png)


## 三.开始刮刮卡游戏开发:  实现刮刮卡效果

思路步骤:

​    1.将随机的中奖信息绘制到画布中;

​    2.使用灰色的矩形覆盖中奖信息;

​    3.使用鼠标滑擦区域实现刮刮效果;

完整代码:


```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <canvas style="border:solid 1px #ccc" id="cas"></canvas>
</body>

</html>
<script type="text/javascript">
    /* 思路步骤:
    1.将随机的中奖信息绘制到画布中;
    2.使用灰色的矩形覆盖中奖信息;
    3.使用鼠标滑擦区域实现刮刮效果;
     */
    var cas = document.getElementById("cas"); //获取画布元素
    var cxt = cas.getContext('2d') //获取画布工具集
    console.log(cxt);


    //步骤1开始
    var arr = ["特等奖", "一等奖", "二等奖", "三等奖", "谢谢参与"]
    // 随机生成文字
    var randomText = arr[Math.floor(Math.random() * arr.length)];
    // console.log(randomText);
    cxt.font = "bold 30px 黑体";
    cxt.textAlign = "center";
    cxt.textBaseline = 'middle';
    cxt.fillStyle = "red"
    cxt.fillText(randomText, cas.width / 2, cas.height / 2);
    // 将绘制的文字画布生成图片,做为画布的背景图
    var bgm = cas.toDataURL('image/png', 1); //记得用cas
    cas.style.background = "url(" + bgm + ")";
    //步骤1完成

    //步骤2开始
    cxt.fillStyle = "#ddd";
    cxt.fillRect(0, 0, cas.width, cas.height);
    //步骤2完成

    //步骤3开始
    var flag = false
    cas.addEventListener("mousedown", () => {
        flag = true;
        cxt.globalCompositeOperation="destination-out";//很重要,设置这个属性,实现绘画的区域与背景透明
    })
    cas.addEventListener("mousemove", (e) => {
        if (flag) {
            var x = e.clientX;
            var y = e.clientY;
            cxt.fillStyle = "red";
            cxt.fillRect(x, y, 30, 30)
        }
    })
    cas.addEventListener("mouseup", () => {
        console.log("onmouseup");
        flag = false;

    })
    //步骤3完成
</script>
```
### 







### 