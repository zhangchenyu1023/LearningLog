https://blog.csdn.net/qq_29808089/article/details/108997621?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.no_search_link&spm=1001.2101.3001.4242

# OpenLayers/leaflet/Mapbox/AGS等GIS前端总结概述

![img](https://csdnimg.cn/release/blogv2/dist/pc/img/reprint.png)

[一碗老面i](https://blog.csdn.net/qq_29808089) 2020-10-10 15:15:55 ![img](https://csdnimg.cn/release/blogv2/dist/pc/img/articleReadEyes.png) 1007 ![img](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollect.png) 收藏 7

分类专栏： [GIS相关资料](https://blog.csdn.net/qq_29808089/category_6168806.html) 文章标签： [gis](https://www.csdn.net/tags/NtzaQgysMTQ1NjYtYmxvZwO0O0OO0O0O.html)

版权

[![img](https://img-blog.csdnimg.cn/20190927151117521.png?x-oss-process=image/resize,m_fixed,h_224,w_224)GIS相关资料](https://blog.csdn.net/qq_29808089/category_6168806.html)专栏收录该内容

9 篇文章0 订阅

订阅专栏

有关GIS前端的一些总结概述。

# 一、轻量级的leaflet，轻在哪里？

leaflet/openlayers轻量级，那么轻在哪里（相比ArcGIS js，甚至Openlayers）？

1. **查询分析**：本身版本不支持属性查询、空间查询；（但可以其他GIS厂家已经开发出插件来支持）[Leaflet 关于查询的总结](https://blog.csdn.net/fengyekafei/article/details/79885102)
2. **编辑功能**：leaflet不支持编辑
3. **服务类型**：leaflet只支持OGC标准的WMS\WMTS服务，暂未看到其他服务；AGS全面支持且支持AGS独有的各项服务。
4. **数据对象**：对AGS的要素类等内置数据对象支持，例FeauteSet、Geometry、Symbol
5. **可视化组件多**：更丰富的视图组件，如图例、编辑控件等
6. **动态修改渲染方式**：AGS可以分级渲染等动态的修改样式，而leaflet的图层是否只能固定渲染方式？
7. AGS基于Dojo框架，一个比较大的框架，考虑了兼容多浏览器一致性。
8. leaflet默认只支持WGS 1984 Web Mercator (Auxiliary Sphere)坐标系的服务。
9. leaflet没有三维相关，AGS有，Openlayers包含了cesium

这里有个leaflet与openlayer的对比（可以类比与AGS）http://ivansanchez.github.io/leaflet-vs-openlayers-slides/#/

![img](https://img-blog.csdnimg.cn/20191226090532955.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d5Zjg2,size_16,color_FFFFFF,t_70)

# 二、MapBox.js与Leaflet.js

原leaflet的团队似乎现在在为Mapbox工作。

leaflet没有OpenLayer那么复杂，专注性能和可用性，简单的API，精巧，移动支持。

MapBox.js与Leaflet.js主要的不同点：

1、Mapbox.js是Leaflet的一个插件，使用方式是通过结合leaflet使用。

2、mapbox-gl.js 则是使用WebGL技术独立渲染前端库，不需要结合其它渲染引擎（比如Leaflet、OpenLayer）来使用。

3、使用mapbox-gl.js的浏览器必须支持WebGL渲染，在老旧的浏览器中是不支持mapbox-gl.js的。而mapbox.js则没有此限制。

[参考链接](https://www.zhihu.com/question/264500905/answer/281963848)

# 三、OpenLayers概述

## 3.1OpenLayers核心职责

  [OpenLayers](https://link.jianshu.com/?t=http%3A%2F%2Fopenlayers.org%2F)最新大版本是openlayers4，它是一个基于h5的GIS前端库，地图渲染方式为Canvas和WebGL，常用Canvas展示二维地图，支持WebGL渲染显示出将来的OpenLayers有支持三维方向的可能。OpenLayers作为一个地图前端库主要负责GIS数据的**展示与交互**。
  OpenLayers仅仅是开源GIS框架中的**前端部分**，并不等于是GIS系统，所以很多非GIS专业的前端使用OpenLayers常常会出现很多误区，如社区中每隔几天就有人问的问题：

1. 请问怎样用ol加载几百万点啊？我现在加载了感觉很卡。
2. 请问ol能实现路径分析吗？ol能实现缓冲区分析吗？

提问的人忽略了ol的核心职责是**展示与交互**，实际项目中也不可能有将几千万几百万数据推到前端展示和交互的，一般这种都是后端渲染图刷到前端展示，或者使用矢量切片抽希数据到前端展示，正如普通的web开发中的表单需要分页查询和分页展示是一个道理。至于分析一般是服务端或者空间数据库负责分析，分析结果提交前端展示。业务常常是复杂的，但是每个工具的职责是清晰的，请将复杂的业务交由正确的工具去完成！

## 3.2 OpenLayers的定位

  GIS前端渲染库除了OpenLayers还有LeafLet和ESRI公司的ArcGIS API，同样能支持地图的前端库还有百度api，高德api，谷歌api等，还有Echarts，D3.js等，初学者常常不能理解他们之间的关系。常常听人说，路径分析我就用高德API不就可以了吗？展示数据我用下Echarts不可以吗？仍然是一句话，选择什么样的工具，完全是依据实际业务需求而定的。当前和地图相关的库大概分类如下：

- 在线地图lbs服务：这类库的代表是百度api，高德api，谷歌api，主要特点是：公网环境，开发者需要申请key，key的地图请求服务有次数限制。地图数据和服务都是百度高德提供的，开发者常常是将业务有限的点（几个点，几十个点，几百个点等）定到地图上定个位置。开发中使用它们主要是如招聘网站上公司位置的一个定位，互联网应用中的lbs服务，如各种快递，外卖等app中附近的餐馆影院等。在企业和政府应用中，业务非常复杂，在线地图服务提供的数据不是我们要的，提供的服务不能满足我们的应用，所以实际上基本不会在企业开发中使用。LBS！=GIS。
- 数据可视化库：Echarts，D3.js主要作用是web端实现数据可视化的，提供丰富的图表等展示和交互，由于地图的使用越来越普及，所以不可避免的他们也会支持数据在地图上的展示。但主要定位仍然是数据可视化，在开发中，常常指定某个div，用来展示和交互下数据，属于页面的一小部分业务。而一般的综合指挥调度系统的地图是一个应用，加载非常多的图层，可以随时通过地图向地图单元发送指挥命令。page！=application。
- GIS地图库:ol,LeafLet,arcgis api等都属于企业级地图应用开发库，彼此之间大同小异。稍微的差异是arcgis api需要arcserver提供服务，离开了server基本没任何优势。leaflet主要优势还是在开发的第三方控件比较多，但是兼容性比较差。且以“体积小，对移动端友好”为著称，在ol2的年代的确如此，但个人认为API的结构不如ol好，且ol3之后版本支持自定义打包，也支持移动端应用，ol4版本实现es6的import语法，实现按需加载，足以胜任开发大型GIS应用的要求。

综述：OpenLayers是GIS地图库，定位于开发GIS应用，而非地图页面，用于复杂的展示和交互用户数据。





**openlayers和leaflet：**现在看，是前端地图开源库的唯二选择，两个都是将切片或者空间数据在浏览器中可视化，并提供与之交互的能力。**d3或者echart是数据可视化**，**但是主要是普通的可视化，例如图表，不是地图的可视化，虽然可以做，但并不方便。**这两个类别现在基本都可以使用canvas渲染，因此可以把两个渲染结果叠加，可以得到百度迁徙图之类的效果，但要注意统一坐标系。

**openlayers和leaflet有些许区别**，leaflet专注小而美，只提供基本的地图调用和交互，超出的基本依赖插件，插件很多，也很全；openlayers把所有的功能做到了一起，自成一体，但保留了扩展其类的功能，全部引入太过臃肿，所以最新的openlayers提供了vue组件式开发，可以只引入需要的部分。所以，这两个各有特点。**如果只需要显示地图和简单的交互，使用leaflet合适，复杂的可以尝试使用openlayers。**(不要被他们在github中的star数量欺骗，它们两个实力旗鼓相当)

综上，webgis开发，可以放心的使用openlayers或leaflet作为前端库，结合d3或echart可以做出更炫的效果，但不是必须。







npm config set registry https://registry.npm.taobao.org（恢复：npm config set registry https://registry.npmjs.org）

npm install -g cnpm --registry=https://registry.npm.taobao.org

npm install -g @vue/cli

npm install -g webpack

npm install -g webpack-cli

vue --version

vue create yeb

npm run serve



npm install element-plus --save

```vue
// 引入Element Plus
import ElementPlus from 'element-plus';
import 'element-plus/dist/index.css'
```

cnpm install ol

cnpm install -g parcel-bundler





![image-20210916131844996](https://gitee.com/zhangchenyu1023/Pictures/raw/master/img/20210916131854.png)
