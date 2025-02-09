# react-native-draggable-grid

[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu) [![LICENSE](https://img.shields.io/badge/license-NPL%20(The%20996%20Prohibited%20License)-blue.svg)](https://github.com/996icu/996.ICU/blob/master/LICENSE)


[中文文档](./README_CN.md)

## Why the fork?
What was attempted:
I attempted to modify the code in such a way that when an object had a key that ended in noMove, that item would become disabled, and could not be moved.

What is working:
When this receives an item whos key ends in `noMove` it does not allow the user to directly move that item (disabled long press)

What is broken:
The user can still move the 'unmovable' item by moving other object in front of or behind the disabled object.

Why:
This was a (failed) attempt to make a draggable item inactive. Unfortunately the original code is written in such a way that you can eaisly disble on click events (what this does), but you are unable (without a significant code overhaul) to remove an item from the map to prevent it from moving when another item gets placed behind or in front of the disbled item.


## Demo

<p align="center">
  <img alt="Issue Stats" width="400" src="https://github.com/SHISME/react-native-draggable-grid/blob/master/example.gif?raw=true">
</p>


## Getting Started

## Installation

```bash
npm install react-native-draggable-grid-with-disable --save
```

## Usage

```javascript

import React from 'react';
import {
  View,
  StyleSheet,
  Text,
} from 'react-native';
import { DraggableGrid } from 'react-native-draggable-grid-with-disable';

interface MyTestProps {

}

interface MyTestState {
  data:{key:string, name:string}[];
}

export class MyTest extends React.Component<MyTestProps, MyTestState>{

  constructor(props:MyTestProps) {
    super(props);
    this.state = {
      data:[
        {name:'1',key:'one'},
        {name:'2',key:'two'},
        {name:'3',key:'three'},
        {name:'4',key:'four'},
        {name:'5',key:'five'},
        {name:'6',key:'six'},
        {name:'7',key:'seven'},
        {name:'8',key:'eight'},
        {name:'9',key:'night'},
        {name:'0',key:'zero noMove'},
      ],
    };
  }

  public render_item(item:{name:string, key:string}) {
    return (
      <View
        style={styles.item}
        key={item.key}
      >
        <Text style={styles.item_text}>{item.name}</Text>
      </View>
    );
  }

  render() {
    return (
      <View style={styles.wrapper}>
        <DraggableGrid
          numColumns={4}
          renderItem={this.render_item}
          data={this.state.data}
        />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  button:{
    width:150,
    height:100,
    backgroundColor:'blue',
  },
  wrapper:{
    paddingTop:100,
    width:'100%',
    height:'100%',
    justifyContent:'center',
  },
  item:{
    width:100,
    height:100,
    borderRadius:8,
    backgroundColor:'red',
    justifyContent:'center',
    alignItems:'center',
  },
  item_text:{
    fontSize:40,
    color:'#FFFFFF',
  },
});


```

## Props

| parameter  | type   | required | description |
| :--------  | :----  | :------- | :---------- |
| numColumns | number | yes      | how many items should be render on one row|
| data       | array  | yes      | data's item must have unique key，item's render will depend on the key|
| renderItem |(item, order:number) => ReactElement| yes | Takes an item from data and renders it into the list |
| itemHeight | number | no       | if not set this, it will the same as itemWidth |
| dragStartAnimation | object | no | custom drag start animation |
| style      | object | no       | grid styles |

## Event Props


| parameter  | type   | required | description |
| :--------  | :----  | :------- | :---------- |
| onItemPress | (item) => void | no      | Function will execute when item on press |
| onDragStart | (startDragItem) => void | no | Function will execute when item start drag |
| onDragRelease | (data) => void | no | Function will execute when item release, and will return the new ordered data |

## Custom Drag Start Animation

If you want to use your custom animation, you can do like this

```javascript

 render() {
    return (
      <View style={styles.wrapper}>
        <DraggableGrid
          numColumns={4}
          renderItem={this.render_item}
          data={this.state.data}
          onDragStart={this.onDragStart}
          dragStartAnimation={{
            transform:[
              {scale:this.state.animatedValue}
            ],
          }}
        />
      </View>
    );
  }

  private onDragStart = () => {
    this.state.animatedValue.setValue(1);
    Animated.timing(this.state.animatedValue, {
      toValue:3,
      duration:400,
    }).start();
  }

```

## Resort item

if you want resort item yourself,you only need change the data's sort, and the draggable-grid will auto resort by your data.

> the data's key must unique
