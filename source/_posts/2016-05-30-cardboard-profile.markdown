---
layout: post
title: "Cardboard一些物理参数解释"
date: 2016-05-30 19:28:35 +0800
comments: true
categories: 
---
当我们定制自己的VR盒子时可以借助google的VR视图参数在线生成工具去生成调试参数，地址https://vr.google.com/cardboard/viewerprofilegenerator/。
我们可以在手机上用chrome打开提示的短网址，<!-- more -->会有一个参数实时效果预览页面，之后将手机放入VR盒子里面调整参数观察变化（当然，一切都是要翻墙的）。

{% img /images/cardboard_profile/1.png %} 

研究Cardboard源码时有些参数是必须弄清楚的，否则我们根本不明白屏幕距离，透镜规格怎么影响成像质量。

Cardboard里面相关参数定义在脚本CardboardProfile里面，DayGream定义在脚本GvrProfile里面。

我们讨论的这些物理参数会决定手机能否在屏幕上正确渲染VR视图。所以使用前你得确保你的手机相关的参数，VR盒子参数和应用里面的是匹配的。

### Screen to lens distance（屏幕到镜片的距离）
你可以用游标卡尺测量手机屏幕到镜片的距离，脚本里面的单位是米，google参数生产网页单位是毫米。

{% img /images/cardboard_profile/2.png %}
 
如果你的设备可以调节镜片间的距离来适应不同的瞳间距，那么你可以测量镜片到屏幕的平均距离来作为屏幕到镜片的距离。

### Inter-lens distance（镜片间的距离）
你可以用游标卡尺测量VR盒子两个镜片的中心点距离，脚本里面的单位是米，google参数生产网页单位是毫米。

{% img /images/cardboard_profile/3.png %} 

如果你的设备可以调节镜片间的距离来适应不同的瞳间距，那么你可以测量镜片间的平均距离来作为需要的数值。

测量完镜片间的距离后你需要保证你能看清屏幕中间的红点标记。这里有些标准来帮助你检测镜片间的距离是否准确：

- 正确的镜片间距离：当观察VR盒子里面屏幕中间的红点标记是清晰的且形状分明的。眼睛能轻松聚焦到红点上，不会感到不舒服。

{% img /images/cardboard_profile/4.png %} 


- 镜头间距离应该和用游标卡尺测量的红点标记间距离相等。

{% img /images/cardboard_profile/5.png %} 

- 错误的镜片间距离：当观察VR盒子里面屏幕中间的红点标记时，红点是模糊的，或者有重影。或者可能会看到两个红点。

{% img /images/cardboard_profile/6.png %} 

- 如果出现上面的情况就需要重新测量。

Screen vertical alignment(屏幕垂直方向的对齐方式)
屏幕和VR盒子的对齐方式有三种：顶部对齐，底部对齐和中心对齐。

不同对齐方式的详细解释如下：

- 底部对齐：手机完全插入VR盒子时对齐VR盒子的底部，或者是视图与手机底部对齐。

{% img /images/cardboard_profile/7.png %} 

          很多VR盒子，比如Google Cardboard就是这种对齐方式。

- 顶部对齐：手机完全插入VR盒子时对齐VR盒子的顶部，或者视图在手机的顶部。

{% img /images/cardboard_profile/8.png %} 

- 中心对齐：你透过镜头的视图中心点在屏幕的中心。

如果你选择了底部对齐或顶部对齐选项，你需要测量对应的底部或顶部的托盘到镜片中心的距离。
你可以通过验证红点在屏幕上的位置来判断托盘到镜片中心的距离是否准确。
下么有些标准来帮助你判断托盘到镜片中心的距离是否准确：

- 正确的托盘到镜片中心的距离：我们看到的VR盒子里面的红点的位置是垂直居中的（可以双眼观察，然后依次闭上左眼或右眼观察）。

{% img /images/cardboard_profile/9.png %} 

- 不正确的托盘到镜片中心的距离：我们看到的VR盒子里面的红点的位置不是垂直居中的。

{% img /images/cardboard_profile/10.png %} 

          如果这种情况需要重新测量。

Distortion coefficients(失真系数)
Unity和Android平台的Google Cardboard SDKs可以自动纠正径向失真。用于VR盒子的镜片都有这种失真现象：
{% img /images/cardboard_profile/11.png %} 
这种径向失真可以用布朗的失真模型进行修正：
{% img /images/cardboard_profile/12.png %} 
{% img /images/cardboard_profile/13.png %} 是扭曲的图像坐标，{% img /images/cardboard_profile/14.png %} 是没有扭曲的图像坐标点，{% img /images/cardboard_profile/15.png %} 是是真的中心点，{% img /images/cardboard_profile/16.png %} 里面{% img /images/cardboard_profile/17.png %} 是第n个失真系数。Google cardboard用K1和K2两个失真系数来大致建立失真模型。

你可以将手机插入VR盒子里面，调整失真系数直到场景看起来正常，这样可以确认失真系数数值是否正确。
如果失真系数正确，直线在VR场景里面应该仍然是直线，直角在VR场景里面仍然是直角。不管是镜片中心还是镜片边缘都应该是这样的。






