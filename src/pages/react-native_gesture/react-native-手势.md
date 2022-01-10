---
title: react-native 手势
date: 2019-09-24 17:01:04
spoiler: 省略.
cta: 'react-native'
---


1. react-native-gesture-handler

  官方示例： [react-native-gesture-handler](https://github.com/kmagiera/react-native-gesture-handler/)

2. PanResponder

  简单示例如下，还不知道有没有什么坑，后面再补， [PanResponder](https://facebook.github.io/react-native/docs/panresponder/)，效果图后面再补。。。。


```jsx
import React from 'react';
import {
  StyleSheet,
  View,
  PanResponder,
} from 'react-native';

export default class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
        top: 100,
        left: 100,
        bg: 'blue',
        distance: 0,
        transValue: 1,
    };
    this.panResponder = PanResponder.create({

      onStartShouldSetPanResponderCapture: (evt, gestureState) => true,
      onMoveShouldSetPanResponderCapture: (evt, gestureState) => true,

        onStartShouldSetPanResponder: () => true,
        onMoveShouldSetPanResponder: ()=> true,
        onPanResponderGrant: ()=>{
          this._top = this.state.top
          this._left = this.state.left
          this.setState({bg: 'red'})
        },
        onPanResponderMove: (evt,gs)=>{
          console.log(evt.nativeEvent.touches.length)
          let touches = evt.nativeEvent.touches; 
          if (touches.length === 2) { // 双指 缩放
            this.process(touches[0].pageX, touches[0].pageY,
              touches[1].pageX, touches[1].pageY);
          } else { // 单指 移动
            console.log(gs.dx+' '+gs.dy)
            this.setState({
              top: this._top+gs.dy,
              left: this._left+gs.dx
            })
          }          
        },
        onPanResponderRelease: (evt,gs)=>{
          console.log('yellow')
          this.setState({
            bg: 'yellow',
            top: this._top+gs.dy,
            left: this._left+gs.dx
          })
        }
      })
  }

  process(x1, y1, x2, y2) {
    let distance = this.calcDistance(x1, y1, x2, y2);

    console.log('distance:' + distance)
    if (distance > this.state.distance) {
      console.log('放大')
      this.setState({
        transValue: 2,
      })
    } else {
      console.log('缩小')
      this.setState({
        transValue: 0.5,
      })
    }
    this.setState({
      distance: distance,
    })
  }

  calcDistance(x1, y1, x2, y2) {
    let dx = Math.abs(x1 - x2)
    let dy = Math.abs(y1 - y2)
    return Math.sqrt(Math.pow(dx, 2) + Math.pow(dy, 2));
  }

  render() {
    return(
        <View
            {...this.panResponder.panHandlers}
            style={[styles.rect,{
                "backgroundColor": this.state.bg,
                "top": this.state.top,
                "left": this.state.left,
                'transform': [{scale: this.state.transValue}]
            }]}
        >
        </View>
    )
    }
}

const styles = StyleSheet.create({
    rect: {
        width: 300,
        height: 300,
    },
});

```