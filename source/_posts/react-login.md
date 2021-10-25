---
title: React-登录
date: 2021-10-22 17:15:30
tags: React
categories: React
---
### react 登录功能
<!--more-->

#### 登录功能实现

##### 页面-账号密码输入框、登录按钮
采用Form表单：
```
import {Button,Form,Input} from 'antd';

const layout = {
	labelCol: { span: 8 },
	wrapperCol: { span: 16 },
	};
	
<Form
             {...layout}
             name="basic"
             initialValues={{ remember: true }}
             onFinish={this.onFinish}
             onFinishFailed={this.onFinishFailed}
         >
             <Form.Item
		label="Username"
		name="username"
		rules={[{ required: true, message: 'Please input your username!' }]}
             >
                 <Input />
             </Form.Item>

             <Form.Item
                 label="Password"
		name="password"
		rules={[{ required: true, message: 'Please input your password!' }]}
	>
		<Input.Password />
	</Form.Item>

	<Form.Item {...tailLayout}>
		<Button type="primary" htmlType="submit">
				Login
		</Button>
	</Form.Item>
</Form>
```
##### 登录验证及cookie操作
```
onFinish = (values) =>{
       console.log(values);
       let url = "http://***.***.com/login";
       fetch(url, {
           method: 'POST',
           headers: {
               'Access-Control-Allow-Origin': '*',
               'Access-Control-Allow-Methods':'POST, GET, OPTIONS, DELETE, PUT',
               'Access-Control-Allow-Credentials':'true',
               'Accept': 'application/json',
               'Content-Type': 'application/json;charset=utf-8',
               'mode':'no-cors',
               'Access-Control-Allow-Headers':'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Requested-With,Token,x-access-token',
           },
           credentials: 'include',
           body: JSON.stringify(values),
       }).then(response => response.json())
           .then(responseJson => {
               //处理登录请求结果
               if (responseJson.code === "0") {
                   //cookie有效期1个小时
                   cookie.save('user', values.username,{path:"/",maxAge:3600});
                   message.success("login success");
                   this.props.history.push('/home', null);
               }else {
                   message.error("login failed");
               }
               console.log(responseJson);
           }).catch(function (e) {
           console.log("Oops, error");
           message.error("login failed")
       });
   }

   onFinishFailed = (errorInfo ) =>{
       console.log('Failed:', errorInfo);
   }
```

##### 登录态失效判断及跳转
```
componentWillMount(){
       console.log("user = " + cookie.load("user"));
       if (cookie.load("user") === undefined || cookie.load("user") === null) {
           this.props.history.push('/login', null);
       }
   }
```
- 注意：页面跳转失效处理如下
```
import {withRouter} from 'react-router-dom'; 
class HomePage extends Component{} 
export default withRouter(HomePage);
```

#### 权限
根据cookie中存储的用户信息，判断是否对页面上某些元素可见
```
let hasPermission = this.hasPermission(cookie.load("user"));
hasPermission = (user) => {
      if (user === "admin"){
          return true;
      } else {
          return false;
      }
   }
{hasCallPermission?<div>……</div>:null}
```