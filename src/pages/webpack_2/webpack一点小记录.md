---
title: webpack一点小记录
date: 2021-08-12 20:56:32
spoiler: 省略.
cta: 'js'
---

webpack5 跟 webpack4 有点差别，把之前的文件又拿出来改了改
<!-- more -->
webpack.config.js
```jsx
const path = require('path')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { VueLoaderPlugin } = require('vue-loader')
const MiniCssExtractPlugin = require('mini-css-extract-plugin') // 支持在开发中加载热更新的css文件
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
const CompressionPlugin = require("compression-webpack-plugin")

module.exports = {
    entry: {
        path: path.resolve(__dirname, '../src/main.js')
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'js/[name].[hash:8].js',
        chunkFilename: 'js/[id].[chunkhash].js', // js放到js文件夹下面
        clean: true,  // 每次构建之前清理文件夹
        assetModuleFilename: 'images/[hash:8][ext][query]'  // 图片类都放到images文件夹下面
    },
    mode: 'development',
    resolve: { // 配置如何解析模块 Configure how modules are resolved
        alias: { // 更轻松地为import或require某些模块创建别名
            // 'vue$': 'vue/dist/vue.runtime.esm.js',
            '@': path.resolve(__dirname, '../src')
        },
        extensions: ['*', '.js', '.json', '.vue']  // 尝试按顺序解析这些扩展。如果多个文件共享相同的名称但具有不同的扩展名，webpack 将使用数组中第一个列出的扩展名解析文件并跳过其余文件。
    },
    module: {
        rules: [
            {
                test: /\.vue$/,
                loader: 'vue-loader'
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env']
                    }
                }
            }, {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
                // test: /.s?css$/,
                // use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
            }, {
                test: /\.scss$/,
                use: [
                    'style-loader', {
                        loader: MiniCssExtractPlugin.loader,
                        options: {
                            esModule: false,
                        }
                    }, 'css-loader', 'sass-loader'
                ]
            }, {
                test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
                type: 'asset/resource'
            }, {
                test: /\.svg$/,
                type: 'asset/inline'
            }, {
                test: /\.txt$/,
                type: 'asset/source'
            }, {
                test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
                type: 'asset/resource'
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({  // 这是一个webpack插件，它简化了 HTML 文件的创建以服务于你的webpack包。这对于webpack在文件名中包含哈希值的包尤其有用，该哈希值会更改每次编译。您可以让插件为您生成 HTML 文件，使用lodash模板提供您自己的模板或使用您自己的加载程序。
            template: path.resolve(__dirname, '../public/index.html')
        }),
        new VueLoaderPlugin(), // 热更新
        new MiniCssExtractPlugin({ //这个插件将 CSS 提取到单独的文件中。它为每个包含 CSS 的 JS 文件创建一个 CSS 文件。它支持按需加载 CSS 和 SourceMaps。
            filename: "css/[name].css",
            chunkFilename: "css/[id].css",
          }),
        new BundleAnalyzerPlugin(),  // 分析
        new CompressionPlugin(), // 压缩。准备资产的压缩版本以使用内容编码为它们提供服务。
        new webpack.DllReferencePlugin({
            context: __dirname,
            // manifest: require('./wendor-manifest.json')
        })
    ]
}
```


webpack.prod.js
```
const CopyWebpackPlugin = require('copy-webpack-plugin')
const path = require('path')
const TerserPlugin = require('terser-webpack-plugin')
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin') // css压缩
const { merge } = require('webpack-merge')
const common = require('./webpack.config.js')

module.exports = merge(common, {
    mode: 'production',
    devtool: 'cheap-module-source-map',
    plugins: [
        new CopyWebpackPlugin({
            patterns: [{
                from: path.resolve(__dirname, '../public'),
                to: path.resolve(__dirname, 'public')
            }]
        })
    ],
    optimization: { //  优化
        minimizer: [
            new TerserPlugin(),
            new CssMinimizerPlugin()
        ],
        splitChunks: {
            chunks: 'all',
            // cacheGroups: {
            //     libs: {
            //         name: 'chunk-libs',
            //         test: /[\\/]node_modules[\\/]/,
            //         priority: 10,
            //         chunks: 'initial'
            //     }
            // }
        }
    }
})
```