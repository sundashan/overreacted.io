---
title: react-native零散记录
date: 2019-09-11 11:18:59
spoiler: 省略.
cta: 'react-native'
---

1. 导航栏
react-navigation、react-native-navigation各有千秋，看自己选择
2. 蓝牙
react-native-ble-plx
react-native-ble-manager
react-native-bluetooth-serial
都可以看下，我用的是react-native-ble-plx，文档比较全面，同时适配安卓ios
3. 录音
react-native-audio
4. 播放音频
react-native-sound
5. 播放视频
react-native-video
6. 相机
react-native-camera
7. 截图
react-native-view-shot
8. 文字渐变
react-native-text-gradient 或者自己用svg的渐变写下，也很简单，[ 例子见最下方 ]
9. 背景渐变
react-native-linear-gradient
10. 边框阴影
安卓可以用属性elevation，或者react-native-cardview
说到这两个属性就要说到层级关系，比如，给边框加了阴影以后，再想在上面通过position: 'absolute'加一层遮罩，这个时候就发现遮罩无效
注意：
    1. 如果只有zIndex没有elevation,那么zIndex大的在上层
    2. 如果两个组件都有elevation，那么elevation大的在上层
    3. 如果既有zIndex也有elevation，那么以elevation为准

    所以上面说的遮罩无效的问题，解决方法是在遮罩上也加上elevation
11. 本地文件访问
react-native-fs
12. 图表
victory-native： 文档非常全，可配置的属性很多很全，比如折线图的点是否突出显示、折线图的一段标红，比较推荐使用
react-native-echarts：好久之前更新的，安装的时候一直报错，就没用了
react-native-chart-kit： 图表不全，而且折线图上面的点我没找着怎么去掉，不符合我们的需求，去掉
react-native-svg-charts： 蛮好看的，也蛮好用的，可以看看
react-native-charts-wrapper： 忘了为啥没用它
13. UI组件
NativeBase
14. svg
react-native-svg：注意事项：svg要进行transform变换，需要在进行变换的元素上面套一层G
15. 图片相关
    1. 图片上传 react-native-image-picker
    2. 图片预览 react-native-image-zoom-viewer

文字渐变例子：
```jsx
import { Svg, Rect, Defs, LinearGradient, Use, Stop, Text } from 'react-native-svg'
<Svg width="100" height="21" viewBox="0 0 100 21">
    <Defs>
        <LinearGradient
            id="Gradient"
            gradientUnits="userSpaceOnUse"
            x1="0"
            y1="0"
            x2="100"
            y2="0"
        >
            <Stop offset="0" stopColor={this.props.startColor} stopOpacity="1" />
            <Stop offset="1" stopColor={this.props.endColor} stopOpacity="1" />
        </LinearGradient>
        <Text
            id="Text"
            x="50"
            y="15"
            fontFamily="Verdana"
            fontSize="18"
            textAnchor="middle"
            >
        {this.props.value}
        </Text>
    </Defs>
    <Rect x="0" y="0" width="100" height="21" fill="#FFF" />
    <Use href="#Text" fill="url(#Gradient)" />
</Svg>
```


