# ooomap-editor
this is a 3D indoor and outdoor map editing tools that helps cartographers quickly create and publish 3D maps.

ooomap-editor-demo 主要用来编写ooomap editor使用的示例，包含地图的创建、SDK加载地图及发布地图等，也可参考https://www.ooomap.com/main/omeditor.html?tag=editor-use&i=0&j=0

# 一、地图编辑器介绍

随着5G、工业互联网及物联网的发展，我们的身边随处可见三维场景。比如：三维景区园区导览、三维机房监控、VR、AR、三维实景看房等，ooomap editor 的出现解决了制图人员效率低下，3D制图材料资源共享困难及项目中需要使用3D地图，却没有相关的技术储备，投入大量精力去研发3D地图渲染引擎，使得项目的成本和周期无法控制的问题。

# 二、地图编辑器的优势

1.快速生成3D地图

3DMax制图人员可以使用工具导出模型，然后使用导入到ooomap editor中，使用ooomap editor在线编辑地图发布等

2.所见即所得

使用ooomap editor 发布3D场景，发布后的场景可实时查看，也可通过官网的示例查看运行后的效果

3.自定义3D场景动画效果

可以通过编写脚本控制3D场景中的动画效果(Unity 3D中也有)

4.地图功能完善

可实现2D/3D 地图编辑、标注、路径规划、定位算法等

5.快速上手

SDK API 简单，可快速上手实现各种3D效果

6.插件系统

可以将常用的功能封装成插件，如：界面控件、定位功能等，不需要重复开发，提高开发效率

7.离线地图

将max中做好的地图导入到ooomap editor，然后生成可本地化的.omap文件，使用SDK加载.omap文件

# 三、脚本系统

园圈地图编辑器中设计了功能强大, 开发简单灵活的脚本系统, 相比较传统的使用SDK的开发方式, 使用脚本系统可以使开发更具条理性, 并且易于复用和分享

## 脚本系统特点

使用 es6 语法

一个脚本文件中对应着一个同名的 class

全局变量:

地图对象: map

场景对象: scene

内置的事件方法:

constructor: es6 类的构造方法, 在这里面, 可以创建一些在脚本运行过程中, 需要用于的变量, 注意: 此时 this.node 为空

start: 当脚本第一次挂载上结点上时运行, 一般用于初始化

update(delta): 地图渲染时每一帧的回调方法, delta为与上一帧的时间间隔(毫秒数)

picked: 结点被点击时的回调方法

doubleClicked: 结点被双击时的回调方法

longPressed: 结点被长按时的回调方法

inView: 当结点对象进入视图范围时的回调方法

outView: 当结点对象离开视图范围时的回调方法

![image](https://github.com/tangyajun/ooomap-editor-demo/blob/master/images/script.png)

## 脚本示例代码-浮动效果

这段脚本的效果是, 可以让绑定的结点做上下浮动的效果

```
/** 
* ooomap 地图脚本 
* 全局对象: map, scene 
* 访问脚本绑定的 OMNode 对象: this.node
*/ 
class UpAndDown extends om.OMScript { 

    constructor() { 
        super(); 

        // 计数 count
        this.cnt = Math.random() * 3;

    }

    // 在地图的每帧运行 
    update(delta) { 
        this.cnt += delta * 3;
        this.node.entity.position.z = Math.sin(this.cnt) * 5 + 10;
    }

}
```

#效果演示:

![image](https://github.com/tangyajun/ooomap-editor-demo/blob/master/images/updown.gif)

## 脚本示例代码-全局脚本

脚本对像一般是挂载到某一个结点上的, 如果想对地图中整体的一类结点进行一些操作, 可以使用全局脚本

全局脚本特点:

代码一般放到 start 方法里

脚本挂载在 地图场景(scene) 上

```
/** 
* ooomap 地图脚本 
* 全局对象: map, scene 
* 访问脚本绑定的 OMNode 对象: this.node
*/ 
class Init extends om.OMScript { 

    constructor() { 
        super();  
    }

    // 当脚本添加到结点上时运行 
    start() { 

        // 为 map 对象添加事件处理方法
        map.on('picked', res => {

            // 如果是空点击
            if (!res.node) {
                return;
            }

            // 如果点击的是体块对象
            if (res.node.type === 'OMBlock') {

                res.node.flashHeight();

            } else if (res.node.type === 'SpriteMarkerNode') {

                // 如果点击的是 POI标注对象
                res.node.flash();

            }

        })

    }
}
```

## 效果演示

![image](https://github.com/tangyajun/ooomap-editor-demo/blob/master/images/globalScript.gif)

## 脚本系统视频教程

[![asciicast](https://github.com/tangyajun/ooomap-editor-demo/blob/master/images/script_video.png)](https://www.ooomap.com/main/assets/videos/script.mp4)


# 四、应用案例

## 1. 手绘图案例

![image](https://github.com/tangyajun/ooomap-editor-demo/blob/master/images/20200322103044.png)

## 2. 室内地图

![image](https://github.com/tangyajun/ooomap-editor-demo/blob/master/images/20200322103152.png)

## 3. 室内外一体化

![image](https://github.com/tangyajun/ooomap-editor-demo/blob/master/images/20200322103246.png)

# 五、编辑器操作视频

[![asciicast](https://github.com/tangyajun/ooomap-editor-demo/blob/master/images/20200322105136.png)](https://www.ooomap.com/main/assets/videos/overview.mp4)
