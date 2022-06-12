### 无障碍插件 
> 这是一个针对于 有视障、听说障碍、读写障碍、肢体障碍，机体功能衰退的老年人群开发的一个辅助插件使用网站的js插件

**项目演示**

---

<p align="center">

<img src="https://qiutian00blog.oss-cn-shenzhen.aliyuncs.com/uPic/assist-tool.png">

<!-- <video src="https://qiutian00blog.oss-cn-shenzhen.aliyuncs.com/uPic/%E5%B1%8F%E5%B9%95%E5%BD%95%E5%88%B62022-03-22%20%E4%B8%8B%E5%8D%889.14.05.mov" controls="controls" width="100%" autoplay>
your browser does not support the video tag
</video> -->

</p>

---
### 插件接入方式
*  1 - 在页面合适的地方如banner处加入id为 assist-open 的标签
*  2 - 在页面底部 body 之前引入插件；
*  3 - 如果需要特殊处理的地方使用后面的API做对应处理;

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>列子</title>
</head>
<body>
  <div id='assist-open'>无障碍</div>
  <script type="text/javascript" src="dist/assist.js"></script>
</body>
</html>
```

[开发者关注](./DEVELOPER.md "开发者关注")

## API

```js
const assist = new Assist({
   "namespace": "mozi-assist",
   "domain": "",  
   "url": "https://nls-gateway.cn-shanghai.aliyuncs.com/stream/v1/tts",
   "audioOptions": {
     "token": "token",
     "appkey": "appkey"
   }
})

// assist is the instance of the tool
```

- showTag;   用于打开无障碍标识，（点击无障碍后并不在当前页面打开，而是跳转到其他没有调用showTag的页面打开）
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>列子</title>
</head>
<body>
  <a id='assist-open' assist-href='https://mozi.com/'>无障碍</a>
  <script type="text/javascript" src="dist/assist.js"></script>
  <script>
    ~(function(){
      assist.showTag() // 如果当前页面点击 无障碍 按钮后需要跳转到其他页面打开无障碍功能，则需要调用此函数做cookie标记
    })()
  </script>
</body>
</html>
```
- zoomState;    返回页面放大倍数
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>列子</title>
</head>
<body>
  <a id='assist-open' assist-href='https://mozi.com/'>无障碍</a>
  <script type="text/javascript" src="dist/assist.js"></script>
  <script>
    ~(function(){
      assist.message.subscribe('zoomState', state => {
            console.log(`页面放大倍数：${state}`);
      })
    })()
  </script>
</body>
</html>
```
- openState;    返回插件打开状态

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>列子</title>
</head>
<body>
  <a id='assist-open' assist-href='https://mozi.com/'>无障碍</a>
  <script type="text/javascript" src="dist/assist.js"></script>
  <script>
    ~(function(){
      assist.message.subscribe('openState', state => {
        console.log(`是否开启无障碍模式：${state}`);
      })
    })()
  </script>
</body>
</html>
```

- bigTextState;    大字幕开启状态


```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>列子</title>
</head>
<body>
  <a id='assist-open' assist-href='https://mozi.com/'>无障碍</a>
  <script type="text/javascript" src="dist/assist.js"></script>
  <script>
    ~(function(){
      assist.message.subscribe('bigTextState', state => {
        console.log(`是否开大字幕模式：${state}`);
      })
    })()
  </script>
</body>
</html>
```


### 页面标注
> 对于插件无法识别或识别不准的标签需业务自行标注，标注规范如下

1 - 对于img标签，需设置 alt ，如

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>列子</title>
</head>
<body>
  <img alt="列子无障碍图片" src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fcdn.duitang.com%2Fuploads%2Fblog%2F201306%2F25%2F20130625150506_fiJ2r.jpeg&refer=http%3A%2F%2Fcdn.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1627139099&t=524628587af020410785e8ba98157609">
</body>
</html>
```

2 - 对于其他标签，需使用 title 进行标注，如

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>列子</title>
</head>
<body>
  <div title="列子无障碍">
    列子无障碍
  </div>
</body>
</html>
```

3 - 对于非语意化标签，需加入 role来标注其真实属性，如果不标注title，则取标签内容，如

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>列子</title>
</head>
<body>
  <div role="button" title="提交">
    提交
  </div>
</body>
</html>
```

### 隐藏模块
> 对业务中需要隐藏的模块加一个class名  qunar-assist-hide ，插件在打开的时候会自动监测这个class名统一隐藏

### 页面缩放影响
>  随着页面放大，部分非自适应或者绝对定位的组件可能会出现错位问题，需业务开发自行调整

### 大段文本识别
>  对于可能出现大段文本的地方，需要在当前标签加一个名为 qunar-assist-long-text 的class，插件会自动对这个class下的内容进行分割





### 兼容性

- ie10+
- 所有主流浏览器
- 火狐（不支持页面放大功能）
