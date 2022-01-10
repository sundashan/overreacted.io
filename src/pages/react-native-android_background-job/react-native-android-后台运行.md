---
title: react-native android 后台运行
date: 2019-09-18 17:41:49
spoiler: 省略.
cta: 'react-native'
---

react-native android APP 后台运行，目前发现以下两种方式：
1. Headless JS

  官网有说明： [Headless JS](https://facebook.github.io/react-native/docs/headless-js-android/)


2. react-native-background-job

  照着readme做就行,上手简单， [react-native-background-job](https://github.com/vikeri/react-native-background-job/)

```jsx
import BackgroundJob from "react-native-background-job"; // 首先引入

BackgroundJob.register({
    jobKey: 'someJob', // 注册作业
    job: () => {
      console.log('发送数据')
    }
});


export default class Dynamic extends React.Component {
    constructor(props) {
        this.state = {
          appState: AppState.currentState, // app的状态: active 前台运行, background 后台运行
        };
    }
    async componentDidMount(){
      AppState.addEventListener('change', this._handleAppStateChange);
    }

    _handleAppStateChange = (nextAppState) => {
      if (
        this.state.appState.match(/inactive|background/) &&
        nextAppState === 'active'
      ) {
        console.log('App has come to the foreground!');
      } else {
          BackgroundJob.schedule({
              jobKey: "someJob", // 切换到后台时，安排作业
              period: 1000, // 运行作业的频率，这个数字不准确
              exact: true,
              allowWhileIdle: true,
          });
      }
      this.setState({appState: nextAppState});
    }
    render() {
      return (
        <View>
        ...
        </View>
      )
    }
}
```