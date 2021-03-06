# html5新特性总结

**一、语义标签**

html5语义标签，可以使开发者更方便清晰构建页面的布局

| 标签      | 描述                               |
| --------- | ---------------------------------- |
| <header>  | 定义了文档的头部区域               |
| <footer>  | 定义了文档的尾部区域               |
| <nav>     | 定义文档的导航                     |
| <section> | 定义文档中的节                     |
| <article> | 定义文章                           |
| <aside>   | 定义页面以外的内容                 |
| <details> | 定义用户可以看到或者隐藏的额外细节 |
| <summary> | 标签包含details元素的标题          |
| <dialog>  | 定义对话框                         |
| <figure>  | 定义自包含内容，如图表             |
| <main>    | 定义文档主内容                     |
| <mark>    | 定义文档的主内容                   |
| <time>    | 定义日期/时间                      |

![img](https://img2018.cnblogs.com/blog/1239961/201905/1239961-20190527090526985-576135815.png)

**二、增强型表单**

html5修改一些新的input输入特性，改善更好的输入控制和验证

| 输入类型       | 描述                     |
| -------------- | ------------------------ |
| color          | 主要用于选取颜色         |
| date           | 选取日期                 |
| datetime       | 选取日期(UTC时间)        |
| datetime-local | 选取日期（无时区）       |
| month          | 选择一个月份             |
| week           | 选择周和年               |
| time           | 选择一个时间             |
| email          | 包含e-mail地址的输入域   |
| number         | 数值的输入域             |
| url            | url地址的输入域          |
| tel            | 定义输入电话号码和字段   |
| search         | 用于搜索域               |
| range          | 一个范围内数字值的输入域 |

html5新增了五个表单元素

| <datalist> | 用户会在他们输入数据时看到域定义选项的下拉列表 |
| ---------- | ---------------------------------------------- |
| <progress> | 进度条，展示连接/下载进度                      |
| <meter>    | 刻度值，用于某些计量，例如温度、重量等         |
| <keygen>   | 提供一种验证用户的可靠方法生成一个公钥和私钥   |
| <output>   | 用于不同类型的输出比如尖酸或脚本输出           |

html5新增表单属性

| 属性         | 描述                                  |
| ------------ | ------------------------------------- |
| placehoder   | 输入框默认提示文字                    |
| required     | 要求输入的内容是否可为空              |
| pattern      | 描述一个正则表达式验证输入的值        |
| min/max      | 设置元素最小/最大值                   |
| step         | 为输入域规定合法的数字间隔            |
| height/wdith | 用于image类型<input>标签图像高度/宽度 |
| autofocus    | 规定在页面加载时，域自动获得焦点      |
| multiple     | 规定<input>元素中可选择多个值         |

**三、音频和视频**

html5提供了音频和视频文件的标准，既使用<audio>元素。

音频：<audio src=" "></audio>

```
<audio controls>    //controls属性提供添加播放、暂停和音量控件。
  <source src="horse.ogg" type="audio/ogg">
  <source src="horse.mp3" type="audio/mpeg">
您的浏览器不支持 audio 元素。        //浏览器不支持时显示文字
</audio>
```

视频：<video src=" "></video>

```
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
您的浏览器不支持Video标签。
</video>
```

**四、Canvas绘图**

**五、SVG绘图**

**六、地理定位**

使用getCurrentPosition()方法来获取用户的位置。以实现“LBS服务”

```
<script>
var x=document.getElementById("demo");
function getLocation()
  {
  if (navigator.geolocation)
    {
    navigator.geolocation.getCurrentPosition(showPosition);
    }
  else{x.innerHTML="Geolocation is not supported by this browser.";}
  }
function showPosition(position)
  {
  x.innerHTML="Latitude: " + position.coords.latitude +
  "<br />Longitude: " + position.coords.longitude;
  }
</script>
```

**七、拖放API**

 拖放是一种常见的特性，即捉取对象以后拖到另一个位置。

在html5中，拖放是标准的一部分，任何元素都能够拖放。

```
<div draggable="true"></div>
```

当元素拖动时，我们可以检查其拖动的数据。

```
<div draggable="true" ondragstart="drag(event)"></div>
<script>
function drap(ev){
    console.log(ev);
}
</script>
```

| 拖动生命周期 | 属性名      | 描述                                           |
| ------------ | ----------- | ---------------------------------------------- |
| 拖动开始     | ondragstart | 在拖动操作开始时执行脚本                       |
| 拖动过程中   | ondrag      | 只要脚本在被拖动就运行脚本                     |
| 拖动过程中   | ondragenter | 当元素被拖动到一个合法的放置目标时，执行脚本   |
| 拖动过程中   | ondragover  | 只要元素正在合法的放置目标上拖动时，就执行脚本 |
| 拖动过程中   | ondragleave | 当元素离开合法的放置目标时                     |
| 拖动结束     | ondrop      | 将被拖动元素放在目标元素内时运行脚本           |
| 拖动结束     | ondragend   | 在拖动操作结束时运行脚本                       |