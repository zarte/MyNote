
1、 在android中经常看到设置的颜色为八位的十六进制的颜色值，例如：

 1
 2
 3
 public   static   final   class   color {
      public   static   final   int   lightblue= 0x7f040000 ;
 }


或者在Java中 tx.setTextColor( 0xffff 00 f );

说明：

0xffff00ff 是int类型的数据，分组一下 0x | ff | ff00ff ， 0x 表示颜色整数的标记， ff 表示透明度， f 00 f 表示色值，注意： 0x 后面 ffff00ff 必须是8位的颜色表示。

颜色和不透明度 (alpha) 值以十六进制表示法表示。任何一种颜色的值范围都是  0 到  255 （ 00 到  ff ）。

对于 alpha， 00 表示完全透明， ff 表示完全不透明。

表达式顺序是“ aabbggrr ”，其中“ aa =alpha”（ 00 到 ff ）；“ bb =blue”（ 00 到 ff ）；“ gg =green”（ 00 到 ff )；“ rr =red”（ 00 到 ff ）。

 

2、Android中设置文本颜色的四种方法：

一、利用系统自带的颜色类

tx.setTextColor( android.graphics.Color.RED );

二、数字颜色表示

tx.setTextColor( 0xffff 00 f );

三、自定义颜色

在工程目录values文件夹下新建一个color.xml，内容如下：

<?
xml version="1.0" encoding="utf-8"
?>
  

<
resources
>
    

<
drawable 
name
="dkgray"
>
#80808FF0
</
drawable
>
    

<
drawable 
name
="yello"
>
#F8F8FF00
</
drawable
>
    

<
drawable 
name
="white"
>
#FFFFFF
</
drawable
>
    

<
drawable 
name
="darkgray"
>
#938192
</
drawable
>
    

<
drawable 
name
="lightgreen"
>
#7cd12e
</
drawable
>
    

<
drawable 
name
="black"
>
#ff000000
</
drawable
>
    

<
drawable 
name
="blue"
>
#ff0000ff
</
drawable
>
    

<
drawable 
name
="cyan"
>
#ff00ffff
</
drawable
>
    

<
drawable 
name
="gray"
>
#ff888888
</
drawable
>
    

<
drawable 
name
="green"
>
#ff00ff00
</
drawable
>
    

<
drawable 
name
="ltgray"
>
#ffcccccc
</
drawable
>
    

<
drawable 
name
="magenta"
>
#ffff00ff
</
drawable
>
    

<
drawable 
name
="red"
>
#ffff0000
</
drawable
>
    

<
drawable 
name
="transparent"
>
#00000000
</
drawable
>
    

<
drawable 
name
="yellow"
>
#ffffff00
</
drawable
>
  

</
resources
>
 


 

根据个人需要，颜色可以自行添加。

在Java中设置：

tx.setTextColor( tx.getResources().getColor(R. drawable .red) );

color.xml中也可用color标签

<color  name = "red" > #ffff0000 </color>     

java中设置相应改为：

tx.setTextColor( tx.getResources().getColor(R. color .red) );

四、直接在xml的TextView中设置

android:textColor = "#F8F8FF00"  或


android:textColor = "#F8FF00"

 

3.附Android中146种颜色对应的xml色值：


<?
xml version="1.0" encoding="utf-8"
?>


<
resources
>

    
<
color 
name
="white"
>
#FFFFFF
</
color
>
 
<!--
白色 
-->

    
<
color 
name
="ivory"
>
#FFFFF0
</
color
>
 
<!--
象牙色 
-->

    
<
color 
name
="lightyellow"
>
#FFFFE0
</
color
>
 
<!--
亮黄色 
-->

    
<
color 
name
="yellow"
>
#FFFF00
</
color
>
 
<!--
黄色 
-->

    
<
color 
name
="snow"
>
#FFFAFA
</
color
>
 
<!--
雪白色 
-->

    
<
color 
name
="floralwhite"
>
#FFFAF0
</
color
>
 
<!--
花白色 
-->

    
<
color 
name
="lemonchiffon"
>
#FFFACD
</
color
>
 
<!--
柠檬绸色 
-->

    
<
color 
name
="cornsilk"
>
#FFF8DC
</
color
>
 
<!--
米绸色 
-->

    
<
color 
name
="seashell"
>
#FFF5EE
</
color
>
 
<!--
海贝色 
-->

    
<
color 
name
="lavenderblush"
>
#FFF0F5
</
color
>
 
<!--
淡紫红 
-->

    
<
color 
name
="papayawhip"
>
#FFEFD5
</
color
>
 
<!--
番木色 
-->

    
<
color 
name
="blanchedalmond"
>
#FFEBCD
</
color
>
 
<!--
白杏色 
-->

    
<
color 
name
="mistyrose"
>
#FFE4E1
</
color
>
 
<!--
浅玫瑰色 
-->

    
<
color 
name
="bisque"
>
#FFE4C4
</
color
>
 
<!--
桔黄色 
-->

    
<
color 
name
="moccasin"
>
#FFE4B5
</
color
>
 
<!--
鹿皮色 
-->

    
<
color 
name
="navajowhite"
>
#FFDEAD
</
color
>
 
<!--
纳瓦白 
-->

    
<
color 
name
="peachpuff"
>
#FFDAB9
</
color
>
 
<!--
桃色 
-->

    
<
color 
name
="gold"
>
#FFD700
</
color
>
 
<!--
金色 
-->

    
<
color 
name
="pink"
>
#FFC0CB
</
color
>
 
<!--
粉红色 
-->

    
<
color 
name
="lightpink"
>
#FFB6C1
</
color
>
 
<!--
亮粉红色 
-->

    
<
color 
name
="orange"
>
#FFA500
</
color
>
 
<!--
橙色 
-->

    
<
color 
name
="lightsalmon"
>
#FFA07A
</
color
>
 
<!--
亮肉色 
-->

    
<
color 
name
="darkorange"
>
#FF8C00
</
color
>
 
<!--
暗桔黄色 
-->

    
<
color 
name
="coral"
>
#FF7F50
</
color
>
 
<!--
珊瑚色 
-->

    
<
color 
name
="hotpink"
>
#FF69B4
</
color
>
 
<!--
热粉红色 
-->

    
<
color 
name
="tomato"
>
#FF6347
</
color
>
 
<!--
西红柿色 
-->

    
<
color 
name
="orangered"
>
#FF4500
</
color
>
 
<!--
红橙色 
-->

    
<
color 
name
="deeppink"
>
#FF1493
</
color
>
 
<!--
深粉红色 
-->

    
<
color 
name
="fuchsia"
>
#FF00FF
</
color
>
 
<!--
紫红色 
-->

    
<
color 
name
="magenta"
>
#FF00FF
</
color
>
 
<!--
红紫色 
-->

    
<
color 
name
="red"
>
#FF0000
</
color
>
 
<!--
红色 
-->

    
<
color 
name
="oldlace"
>
#FDF5E6
</
color
>
 
<!--
老花色 
-->

    
<
color 
name
="lightgoldenrodyellow"
>
#FAFAD2
</
color
>
 
<!--
亮金黄色 
-->

    
<
color 
name
="linen"
>
#FAF0E6
</
color
>
 
<!--
亚麻色 
-->

    
<
color 
name
="antiquewhite"
>
#FAEBD7
</
color
>
 
<!--
古董白 
-->

    
<
color 
name
="salmon"
>
#FA8072
</
color
>
 
<!--
鲜肉色 
-->

    
<
color 
name
="ghostwhite"
>
#F8F8FF
</
color
>
 
<!--
幽灵白 
-->

    
<
color 
name
="mintcream"
>
#F5FFFA
</
color
>
 
<!--
薄荷色 
-->

    
<
color 
name
="whitesmoke"
>
#F5F5F5
</
color
>
 
<!--
烟白色 
-->

    
<
color 
name
="beige"
>
#F5F5DC
</
color
>
 
<!--
米色 
-->

    
<
color 
name
="wheat"
>
#F5DEB3
</
color
>
 
<!--
浅黄色 
-->

    
<
color 
name
="sandybrown"
>
#F4A460
</
color
>
 
<!--
沙褐色 
-->

    
<
color 
name
="azure"
>
#F0FFFF
</
color
>
 
<!--
天蓝色 
-->

    
<
color 
name
="honeydew"
>
#F0FFF0
</
color
>
 
<!--
蜜色 
-->

    
<
color 
name
="aliceblue"
>
#F0F8FF
</
color
>
 
<!--
艾利斯兰 
-->

    
<
color 
name
="khaki"
>
#F0E68C
</
color
>
 
<!--
黄褐色 
-->

    
<
color 
name
="lightcoral"
>
#F08080
</
color
>
 
<!--
亮珊瑚色 
-->

    
<
color 
name
="palegoldenrod"
>
#EEE8AA
</
color
>
 
<!--
苍麒麟色 
-->

    
<
color 
name
="violet"
>
#EE82EE
</
color
>
 
<!--
紫罗兰色 
-->

    
<
color 
name
="darksalmon"
>
#E9967A
</
color
>
 
<!--
暗肉色 
-->

    
<
color 
name
="lavender"
>
#E6E6FA
</
color
>
 
<!--
淡紫色 
-->

    
<
color 
name
="lightcyan"
>
#E0FFFF
</
color
>
 
<!--
亮青色 
-->

    
<
color 
name
="burlywood"
>
#DEB887
</
color
>
 
<!--
实木色 
-->

    
<
color 
name
="plum"
>
#DDA0DD
</
color
>
 
<!--
洋李色 
-->

    
<
color 
name
="gainsboro"
>
#DCDCDC
</
color
>
 
<!--
淡灰色 
-->

    
<
color 
name
="crimson"
>
#DC143C
</
color
>
 
<!--
暗深红色 
-->

    
<
color 
name
="palevioletred"
>
#DB7093
</
color
>
 
<!--
苍紫罗兰色 
-->

    
<
color 
name
="goldenrod"
>
#DAA520
</
color
>
 
<!--
金麒麟色 
-->

    
<
color 
name
="orchid"
>
#DA70D6
</
color
>
 
<!--
淡紫色 
-->

    
<
color 
name
="thistle"
>
#D8BFD8
</
color
>
 
<!--
蓟色 
-->

    
<
color 
name
="lightgray"
>
#D3D3D3
</
color
>
 
<!--
亮灰色 
-->

    
<
color 
name
="lightgrey"
>
#D3D3D3
</
color
>
 
<!--
亮灰色 
-->

    
<
color 
name
="tan"
>
#D2B48C
</
color
>
 
<!--
茶色 
-->

    
<
color 
name
="chocolate"
>
#D2691E
</
color
>
 
<!--
巧可力色 
-->

    
<
color 
name
="peru"
>
#CD853F
</
color
>
 
<!--
秘鲁色 
-->

    
<
color 
name
="indianred"
>
#CD5C5C
</
color
>
 
<!--
印第安红 
-->

    
<
color 
name
="mediumvioletred"
>
#C71585
</
color
>
 
<!--
中紫罗兰色 
-->

    
<
color 
name
="silver"
>
#C0C0C0
</
color
>
 
<!--
银色 
-->

    
<
color 
name
="darkkhaki"
>
#BDB76B
</
color
>
 
<!--
暗黄褐色
-->

    
<
color 
name
="rosybrown"
>
#BC8F8F
</
color
>
 
<!--
褐玫瑰红 
-->

    
<
color 
name
="mediumorchid"
>
#BA55D3
</
color
>
 
<!--
中粉紫色 
-->

    
<
color 
name
="darkgoldenrod"
>
#B8860B
</
color
>
 
<!--
暗金黄色 
-->

    
<
color 
name
="firebrick"
>
#B22222
</
color
>
 
<!--
火砖色 
-->

    
<
color 
name
="powderblue"
>
#B0E0E6
</
color
>
 
<!--
粉蓝色 
-->

    
<
color 
name
="lightsteelblue"
>
#B0C4DE
</
color
>
 
<!--
亮钢兰色
-->

    
<
color 
name
="paleturquoise"
>
#AFEEEE
</
color
>
 
<!--
苍宝石绿 
-->

    
<
color 
name
="greenyellow"
>
#ADFF2F
</
color
>
 
<!--
黄绿色 
-->

    
<
color 
name
="lightblue"
>
#ADD8E6
</
color
>
 
<!--
亮蓝色 
-->

    
<
color 
name
="darkgray"
>
#A9A9A9
</
color
>
 
<!--
暗灰色 
-->

    
<
color 
name
="darkgrey"
>
#A9A9A9
</
color
>
 
<!--
暗灰色 
-->

    
<
color 
name
="brown"
>
#A52A2A
</
color
>
 
<!--
褐色 
-->

    
<
color 
name
="sienna"
>
#A0522D
</
color
>
 
<!--
赭色 
-->

    
<
color 
name
="darkorchid"
>
#9932CC
</
color
>
 
<!--
暗紫色 
-->

    
<
color 
name
="palegreen"
>
#98FB98
</
color
>
 
<!--
苍绿色 
-->

    
<
color 
name
="darkviolet"
>
#9400D3
</
color
>
 
<!--
暗紫罗兰色 
-->

    
<
color 
name
="mediumpurple"
>
#9370DB
</
color
>
 
<!--
中紫色 
-->

    
<
color 
name
="lightgreen"
>
#90EE90
</
color
>
 
<!--
亮绿色 
-->

    
<
color 
name
="darkseagreen"
>
#8FBC8F
</
color
>
 
<!--
暗海兰色 
-->

    
<
color 
name
="saddlebrown"
>
#8B4513
</
color
>
 
<!--
重褐色 
-->

    
<
color 
name
="darkmagenta"
>
#8B008B
</
color
>
 
<!--
暗洋红 
-->

    
<
color 
name
="darkred"
>
#8B0000
</
color
>
 
<!--
暗红色 
-->

    
<
color 
name
="blueviolet"
>
#8A2BE2
</
color
>
 
<!--
紫罗兰蓝色 
-->

    
<
color 
name
="lightskyblue"
>
#87CEFA
</
color
>
 
<!--
亮天蓝色 
-->

    
<
color 
name
="skyblue"
>
#87CEEB
</
color
>
 
<!--
天蓝色 
-->

    
<
color 
name
="gray"
>
#808080
</
color
>
 
<!--
灰色 
-->

    
<
color 
name
="grey"
>
#808080
</
color
>
 
<!--
灰色 
-->

    
<
color 
name
="olive"
>
#808000
</
color
>
 
<!--
橄榄色 
-->

    
<
color 
name
="purple"
>
#800080
</
color
>
 
<!--
紫色 
-->

    
<
color 
name
="maroon"
>
#800000
</
color
>
 
<!--
粟色 
-->

    
<
color 
name
="aquamarine"
>
#7FFFD4
</
color
>
 
<!--
碧绿色 
-->

    
<
color 
name
="chartreuse"
>
#7FFF00
</
color
>
 
<!--
黄绿色 
-->

    
<
color 
name
="lawngreen"
>
#7CFC00
</
color
>
 
<!--
草绿色 
-->

    
<
color 
name
="mediumslateblue"
>
#7B68EE
</
color
>
 
<!--
中暗蓝色 
-->

    
<
color 
name
="lightslategray"
>
#778899
</
color
>
 
<!--
亮蓝灰 
-->

    
<
color 
name
="lightslategrey"
>
#778899
</
color
>
 
<!--
亮蓝灰 
-->

    
<
color 
name
="slategray"
>
#708090
</
color
>
 
<!--
灰石色 
-->

    
<
color 
name
="slategrey"
>
#708090
</
color
>
 
<!--
灰石色 
-->

    
<
color 
name
="olivedrab"
>
#6B8E23
</
color
>
 
<!--
深绿褐色 
-->

    
<
color 
name
="slateblue"
>
#6A5ACD
</
color
>
 
<!--
石蓝色 
-->

    
<
color 
name
="dimgray"
>
#696969
</
color
>
 
<!--
暗灰色 
-->

    
<
color 
name
="dimgrey"
>
#696969
</
color
>
 
<!--
暗灰色 
-->

    
<
color 
name
="mediumaquamarine"
>
#66CDAA
</
color
>
 
<!--
中绿色 
-->

    
<
color 
name
="cornflowerblue"
>
#6495ED
</
color
>
 
<!--
菊兰色 
-->

    
<
color 
name
="cadetblue"
>
#5F9EA0
</
color
>
 
<!--
军兰色 
-->

    
<
color 
name
="darkolivegreen"
>
#556B2F
</
color
>
 
<!--
暗橄榄绿
-->

    
<
color 
name
="indigo"
>
#4B0082
</
color
>
 
<!--
靛青色 
-->

    
<
color 
name
="mediumturquoise"
>
#48D1CC
</
color
>
 
<!--
中绿宝石 
-->

    
<
color 
name
="darkslateblue"
>
#483D8B
</
color
>
 
<!--
暗灰蓝色 
-->

    
<
color 
name
="steelblue"
>
#4682B4
</
color
>
 
<!--
钢兰色 
-->

    
<
color 
name
="royalblue"
>
#4169E1
</
color
>
 
<!--
皇家蓝 
-->

    
<
color 
name
="turquoise"
>
#40E0D0
</
color
>
 
<!--
青绿色 
-->

    
<
color 
name
="mediumseagreen"
>
#3CB371
</
color
>
 
<!--
中海蓝 
-->

    
<
color 
name
="limegreen"
>
#32CD32
</
color
>
 
<!--
橙绿色 
-->

    
<
color 
name
="darkslategray"
>
#2F4F4F
</
color
>
 
<!--
暗瓦灰色 
-->

    
<
color 
name
="darkslategrey"
>
#2F4F4F
</
color
>
 
<!--
暗瓦灰色 
-->

    
<
color 
name
="seagreen"
>
#2E8B57
</
color
>
 
<!--
海绿色 
-->

    
<
color 
name
="forestgreen"
>
#228B22
</
color
>
 
<!--
森林绿 
-->

    
<
color 
name
="lightseagreen"
>
#20B2AA
</
color
>
 
<!--
亮海蓝色 
-->

    
<
color 
name
="dodgerblue"
>
#1E90FF
</
color
>
 
<!--
闪兰色 
-->

    
<
color 
name
="midnightblue"
>
#191970
</
color
>
 
<!--
中灰兰色 
-->

    
<
color 
name
="aqua"
>
#00FFFF
</
color
>
 
<!--
浅绿色 
-->

    
<
color 
name
="cyan"
>
#00FFFF
</
color
>
 
<!--
青色 
-->

    
<
color 
name
="springgreen"
>
#00FF7F
</
color
>
 
<!--
春绿色 
-->

    
<
color 
name
="lime"
>
#00FF00
</
color
>
 
<!--
酸橙色 
-->

    
<
color 
name
="mediumspringgreen"
>
#00FA9A
</
color
>
 
<!--
中春绿色 
-->

    
<
color 
name
="darkturquoise"
>
#00CED1
</
color
>
 
<!--
暗宝石绿 
-->

    
<
color 
name
="deepskyblue"
>
#00BFFF
</
color
>
 
<!--
深天蓝色 
-->

    
<
color 
name
="darkcyan"
>
#008B8B
</
color
>
 
<!--
暗青色 
-->

    
<
color 
name
="teal"
>
#008080
</
color
>
 
<!--
水鸭色 
-->

    
<
color 
name
="green"
>
#008000
</
color
>
 
<!--
绿色 
-->

    
<
color 
name
="darkgreen"
>
#006400
</
color
>
 
<!--
暗绿色 
-->

    
<
color 
name
="blue"
>
#0000FF
</
color
>
 
<!--
蓝色 
-->

    
<
color 
name
="mediumblue"
>
#0000CD
</
color
>
 
<!--
中兰色 
-->

    
<
color 
name
="darkblue"
>
#00008B
</
color
>
 
<!--
暗蓝色 
-->

    
<
color 
name
="navy"
>
#000080
</
color
>
 
<!--
海军色 
-->

    
<
color 
name
="black"
>
#000000
</
color
>
 
<!--
黑色 
-->



</
resources
>


 

常见RGB颜色表:

   
 R
 G
 B
 值  
 R
 G
 B
 值  
 R
 G
 B
 值

 黑色
 0
 0
 0
 #000000
 黄色
 255
 255
 0
 #FFFF00
 浅灰蓝色
 176
 224
 230
 #B0E0E6

 象牙黑
 41
 36
 33
 #292421
 香蕉色
 227
 207
 87
 #E3CF57
 品蓝
 65
 105
 225
 #4169E1

 灰色
 192
 192
 192
 #C0C0C0
 镉黄
 255
 153
 18
 #FF9912
 石板蓝
 106
 90
 205
 #6A5ACD

 冷灰
 128
 138
 135
 #808A87
 dougello
 235
 142
 85
 #EB8E55
 天蓝
 135
 206
 235
 #87CEEB

 石板灰
 112
 128
 105
 #708069
 forum gold
 255
 227
 132
 #FFE384          

 暖灰色
 128
 128
 105
 #808069
 金黄色
 255
 215
 0
 #FFD700
 青色
 0
 255
 255
 #00FFFF
          
 黄花色
 218
 165
 105
 #DAA569
 绿土
 56
 94
 15
 #385E0F

 白色
 255
 255
 255
 #FFFFFF
 瓜色
 227
 168
 105
 #E3A869
 靛青
 8
 46
 84
 #082E54

 古董白
 250
 235
 215
 #FAEBD7
 橙色
 255
 97
 0
 #FF6100
 碧绿色
 127
 255
 212
 #7FFFD4

 天蓝色
 240
 255
 255
 #F0FFFF
 镉橙
 255
 97
 3
 #FF6103
 青绿色
 64
 224
 208
 #40E0D0

 白烟
 245
 245
 245
 #F5F5F5
 胡萝卜色
 237
 145
 33
 #ED9121
 绿色
 0
 255
 0
 #00FF00

 白杏仁
 255
 235
 205
 #FFFFCD
 桔黄
 255
 128
 0
 #FF8000
 黄绿色
 127
 255
 0
 #7FFF00

 cornsilk
 255
 248
 220
 #FFF8DC
 淡黄色
 245
 222
 179
 #F5DEB3
 钴绿色
 61
 145
 64
 #3D9140

 蛋壳色
 252
 230
 201
 #FCE6C9          
 翠绿色
 0
 201
 87
 #00C957

 花白
 255
 250
 240
 #FFFAF0
 棕色
 128
 42
 42
 #802A2A
 森林绿
 34
 139
 34
 #228B22

 gainsboro
 220
 220
 220
 #DCDCDC
 米色
 163
 148
 128
 #A39480
 草地绿
 124
 252
 0
 #7CFC00

 ghostWhite
 248
 248
 255
 #F8F8FF
 锻浓黄土色
 138
 54
 15
 #8A360F
 酸橙绿
 50
 205
 50
 #32CD32

 蜜露橙
 240
 255
 240
 #F0FFF0
 锻棕土色
 135
 51
 36
 #873324
 薄荷色
 189
 252
 201
 #BDFCC9

 象牙白
 250
 255
 240
 #FAFFF0
 巧克力色
 210
 105
 30
 #D2691E
 草绿色
 107
 142
 35
 #6B8E23

 亚麻色
 250
 240
 230
 #FAF0E6
 肉色
 255
 125
 64
 #FF7D40
 暗绿色
 48
 128
 20
 #308014

 navajoWhite
 255
 222
 173
 #FFDEAD
 黄褐色
 240
 230
 140
 #F0E68C
 海绿色
 46
 139
 87
 #2E8B57

 old lace
 253
 245
 230
 #FDF5E6
 玫瑰红
 188
 143
 143
 #BC8F8F
 嫩绿色
 0
 255
 127
 #00FF7F

 海贝壳色
 255
 245
 238
 #FFF5EE
 肖贡土色
 199
 97
 20
 #C76114          

 雪白
 255
 250
 250
 #FFFAFA
 标土棕
 115
 74
 18
 #734A12
 紫色
 160
 32
 240
 #A020F0
          
 乌贼墨棕
 94
 38
 18
 #5E2612
 紫罗蓝色
 138
 43
 226
 #8A2BE2

 红色
 255
 0
 0
 #FF0000
 赫色
 160
 82
 45
 #A0522D
 jasoa
 160
 102
 211
 #A066D3

 砖红
 156
 102
 31
 #9C661F
 马棕色
 139
 69
 19
 #8B4513
 湖紫色
 153
 51
 250
 #9933FA

 镉红
 227
 23
 13
 #E3170D
 沙棕色
 244
 164
 96
 #F4A460
 淡紫色
 218
 112
 214
 #DA70D6

 珊瑚色
 255
 127
 80
 #FF7F50
 棕褐色
 210
 180
 140
 #D2B48C
 梅红色
 221
 160
 221
 #DDA0DD

 耐火砖红
 178
 34
 34
 #B22222                  

 印度红
 176
 23
 31
 #B0171F
 蓝色
 0
 0
 255
 #0000FF        

 栗色
 176
 48
 96
 #B03060
 钴色
 61
 89
 171
 #3D59AB        

 粉红
 255
 192
 203
 #FFC0CB
 dodger blue
 30
 144
 255
 #1E90FF        

 草莓色
 135
 38
 87
 #872657
 jackie blue
 11
 23
 70
 #0B1746        

 橙红色
 250
 128
 114
 #FA8072
 锰蓝
 3
 168
 158
 #03A89E        

 蕃茄红
 255
 99
 71
 #FF6347
 深蓝色
 25
 25
 112
 #191970        

 桔红
 255
 69
 0
 #FF4500
 孔雀蓝
 51
 161
 201
 #33A1C9        

 深红色
 255
 0
 255
 #FF00FF
 土耳其玉色
 0
 199
 140
 #00C78C


 

附：颜色获取 --根据RGB值获取颜色

Color Hex Color Codes
http://www.colorcodehex.com/

 