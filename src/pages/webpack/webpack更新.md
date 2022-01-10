---
title: webpack更新
date: 2021-08-21 19:42:56
spoiler: 省略.
cta: 'js'
---

写得比较乱，纯属记载，没有顺序。
初始版本：是以前公司的webpack3，一直没有更新，这次看这个项目比较简单就拿这个试试了。
![](./images/1.png)
使用webpack5以后
![](./images/2.png)
加了cacheDirectory: true后
![](./images/3.png)
加入compress-webpack-plugin后，第一次打包和第二次打包
![](./images/4.png)
加入css-minimizer-webpack-plugin后
![](./images/5.png)
加入terser-webpack-plugin后
![](./images/6.png)
使用splitChunks: {chunks: 'all'}后
![](./images/7.png)
使用devtool: 'cheap-module-source-map',并且修改mode: 'production'后，之前一直搞错了，没在production下面。。。。不过再让我敲一遍截图一遍也嫌麻烦，有个对比就行
![](./images/8.png)
修改mode: 'development',
![](./images/9.png)
修改devtool: 'eval-source-map',后
![](./images/10.png)
修改devtool： false后
![](./images/11.png)
修改devtool：inline-source-map后
![](./images/12.png)
devtool: 'inline-cheap-source-map',
![](./images/13.png)
devtool： eval
![](./images/14.png)
改成mode：production后
![](./images/15.png)
在TerserPlugin中添加paraller：4 并没有变快
new TerserPlugin({parallel: 4}),
![](./images/16.png)
修改paraller为true
![](./images/17.png)
加入terserOptions: {
                    compress: {
                        unused: true,
                        drop_debugger: true,
                        drop_console: true,
                        dead_code: true
                    }
                }后，速度变慢了，包有没有变小还没看到
![](./images/18.png)
加入BundleAnalyzerPlugin后，分析页面，然后进行别的方面的性能优化

添加thread-loader
![](./images/19.png)
使用fast-sass-loader报错了，去除

加入cache-loader，第一次
![](./images/20.png)
![](./images/21.png)
第二次，哇，显著优化！！！
![](./images/22.png)
修改了cache-loader和thread-loader的位置，第一次，
![](./images/23.png)
第二次
![](./images/24.png)
更新一个tree-shaking的问题
![](./images/25.png)
第一次，
![](./images/26.png)
第二次
![](./images/27.png)
卧槽卧槽卧槽，加了文件缓存，惊为天人！！！太牛逼了吧！！！
![](./images/28.png)
第一次
![](./images/29.png)
第二次
![](./images/30.png)
splitChunks加入cacheGroups
![](./images/31.png)
第一次
![](./images/32.png)
第二次
![](./images/33.png)
额，加了PurgecssPlugin后，居然变成了53s！！！体积我没看，但是这个时间太不可了
![](./images/34.png)
第一次
![](./images/35.png)
第二次也要49s，可怕
![](./images/36.png)

懒得总结了，回头再说，太长了，看得我脑壳疼