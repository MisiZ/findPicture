                                懂你找图 微信小程序项目

### 技术

用到了 Javascript , vue ,微信小程序 ， uni-app 等技术

uni-ui     uni-api    moment.js   手势封装

### uni-app

网址: https://uniapp.dcloud.io

是一个使用Vue.js语法来开发所有前端应用的框架也称之为全端开发框架

### uni-ui 

网址:  https://uniapp.dcloud.io/

```
npm install @dcloudio/uni-ui
```

使用文档:   https://www.npmjs.com/package/@dcloudio/uni-ui

分段器组件说明: https://ext.dcloud.net.cn/plugin?id=54

### moment.js

JavaScript日期处理类库

官网:  http://momentjs.cn/

下载:  npm install moment  / 并引入

中文格式:

import moment from "moment"

moment.locale("zh-cn");

使用:this.monthes.MM = moment(时间戳).format("MM")

###  项目文档:

https://www.jianshu.com/p/3dec2cc2e30b 学习笔记



接口文档 https://www.showdoc.cc/414855720281749?page_id=3678621017219602

首页模块推荐页面的数据携带参数:
http://157.122.54.189:9088/image/v3/homepage/vertical?limit=10&order=hot&skip=2





### 手脚架搭建项目

 全局安装   npm install -g @vue/cli

创建项目   vue create -p dcloudio/uni-preset-vue my-project

启动项目 （微信小程序）npm run dev:mp-weixin

微信开发者工具my-project\dist\dev\mp-weixin目录下导入项目

安装sass依赖：npm i node-sass sass-loader  属性style：lang="scss"



### 项目知识点:

uni-app对request请求进行了promise封装,而原生微信小程序没有

网络请求:uni.request({url:"https://www.weixin.com"}).then(res=>{consloe.log(res)})

图片比例:  mode="widthFix/aspectFit/aspectFill"跟随宽/高度保留/保持宽高和比例

循环注意 v-for=""  :key=""



传递带有模板字符 ${id}  的参数时,url要添加``

模板字符串传递参数: :url="`/pages/album/index/?id=${item.id}`"

触底事件,可直接使用 onReachBottom

 object-fit="fill"  video标签属性实现填充父元素

video标签的muted属性实现声音的开关

button 的 open-type设置为share实现转发



 if(Object.keys(this.album).length===0)



图片详情在多个页面适用  可以封装超链接组件,

需要:缓存图片数组和图片索引 用于滑动

标签包裹组件,item和index传递给子组件



**components**声明组件注意

:list="item"   给值注意""

新建页面记得手动改json



缓存数据  getApp().globalData.list = item

获取数据  getApp().globalData

下载图片:

uni. downloadFile({url: 文件地址}) 下载远程文件到小程序的内存中

uni. saveImageToPhotosAlbum({filePath: 临时文件})  将小程序中的内存临时文件下载到本地上

wx.saveVideoToPhotosAlbum   将小程序中的内存视频文件下载到本地上



数据没有获取到却正常显示,需在获取到数据的后面将值赋为变量

request网络请求的url用到模板字符时需要删除双引号



正确 res.data.res.comment.forEach(v=>v.btime=moment(v.atime*1000).fromNow()); 


scroll-view组件有display:flex属性不生效的话标签上加enable-flex

scroll-view要包裹滚动部分,高度减去不滚动部分即可

  //watch监听发生变化

  watch: {

   urlObj() {

​    this.getList();

   }

  },



