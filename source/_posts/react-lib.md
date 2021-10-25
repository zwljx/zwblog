---
title: React-第三方库和工具
date: 2021-10-25 09:40:09
tags: React
categories: React
---
### React 实用工具使用与第三方库

<!--more-->

#### 工具

##### JSON转换
String转json对象：```JSON.parse()```
json对象转json String：```JSON.stringify()```
##### 百分比计算
```Math.round(value/total*10000)/100.00```
##### 时间格式转换
```
let sdate = new Date(r.create_time).toJSON();
let sdate2 = new Date(+new Date(sdate) + 8 * 3600 * 1000).toISOString().replace(/T/g, ' ').replace(/\.[\d]{3}Z/, '');
```
##### base64转换
```
let string = window.atob(base64String);
//直接atob会乱码，以下两行解决乱码问题
string = escape(string);
string = decodeURIComponent(string);
```
#### 第三方库

##### json组件
```
import ReactJson from 'react-json-view';
<ReactJson src={this.state.jsonSrc} name={null}
       iconStyle="square" collapseStringsAfterLength={30} displayDataTypes={false} collapsed={false}
       style={{fontFamily:'微软雅黑'}}/>
```

##### echarts组件
```
import ReactEcharts from "echarts-for-react";
import echartsTheme from '../../theme/tabletheme1';
componentWillMount(){
       echarts.registerTheme('theme', echartsTheme);
   }
<ReactEcharts option={this.getOption()}
       style={{height: 400}}
       theme="theme"></ReactEcharts>
```

##### cookie操作
```
import cookie from 'react-cookies';
```