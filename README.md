# 利用 react、AntDisign 制作的一个新闻站点

## 一、头条新闻的数据借口：

从聚合数据爬取：

PostMan 的使用：

## 二、测试环境的调试：

主要介绍不同的浏览模式下的调试方式

主要使用 Chrome 里面的模拟器进行移动端的测试

**注： 下面写的内容主要针对的是逻辑部分的内容，具体细节请参考源码和注释**

## 三、PC 端页头的开发

图标的下载：iconfinder

结构目录

![](https://ws1.sinaimg.cn/large/006c6oKBgy1fspzyx3b8aj308r0c43yz.jpg)

在 component 下创建 pc_header.js //注意命名规范化

react 的基本写法：

首先是引入组件

```js
import React from 'react';
import { Row, Col } from 'antd';
```

下面是基本的框架（套路）写法：

把大的框架搭好往里面填充即可：

```js
class PCHeader extends React.Component{
    render(){
        return(

        );
    };

}
```

下面布局用 Antsign 里面的 Flex 进行布局(基于 24 栅格布局)

新建 pc_index.js 把我们的 pc_header.js 导进去。

```js
import React from 'react';
import PCHeader from './pc_header';

export default class PCIndex extends React.component {
  render() {
    return (
      <div>
        <PCHeader />
      </div>
    );
  }
}
```

新建 root.js ，接着把 pc_index.js 导入到 root.js

```js
import React from 'react';
import ReactDom from 'react-dom';
import { Router, Route, hashHistory } from 'react-router';

import PCIndex from './components/px_index';

export default class PCIndex extends React.component {
  render() {
    return (
      <div>
        <PCIdex />
      </div>
    );
  }
}

// 出口：

ReactDOM.render(<Root />, document.getElementById('mainContainer'));
```

导航用 antd 里面的 menu

首先是导入 API 进来

import {Menu,Icon} from 'antd';

然后在布局里面加入(其他项目，直接 copy 即可)：

```js
<Row>
  <Col span={16}>
    <Menu.Item>
      <Menu.Item>
        <Icon type="appstore" />头条
      </Menu.Item>
    </Menu.Item>
  </Col>
</Row>
```

选中状态的指定（Menu）：selectedKeys={[this.state.current]} 参见 antd 里面 Menu 文档

然后我们对 current 初始化：

//做一个构造函数
constructor(){
super(); //基类初始化
this.state ={
current: 'top' //初始化 current 的默认指定的 key 是 top
}
}

#### 手机端页头开发：

手机端需要适配，也就是响应式开发， 我们需要加载一个组件：

react-reponsive npm 安装即可

在 root.js 做判断就可以了。 具体细节可以看源代码注释。

## 四、PC 端页头的开发

和页头逻辑结构一样，先写一个 pc_footer.js 然后将其导入到 pc_index.js 里面

移动端同理

## 五、注册功能的开发

fetch 框架的使用： npm install fetch --save

### fetch 兼容性

所有的 ie 都不支持 fetch()方法,所以，考虑兼容性，需要对 fetch()使用 polyfill；

使用 Fetch Polyfil 来实现 fetch 功能：

> npm install whatwg-fetch –save

对于 ie，还要引入 Promise：

> npm install promise-polyfill –save-exact

考虑到跨域问题，需要使用 Jsonp，那么还需要 fetch-jsonp:

> npm install fetch-jsonp

至此，则有：

```js
import 'whatwg-fetch';
import Promise from 'promise-polyfill';
import fetchJsonp from 'fetch-jsonp';

fetchJsonp('/users.jsonp')
  .then(function(response) {
    return response.json();
  })
  .then(function(json) {
    console.log('parsed json', json);
  })
  .catch(function(ex) {
    console.log('parsing failed', ex);
  });
```

### Fetch API

Fetch使用说明

```js
fetch(url, options).then(function(response) { 
// handle HTTP response
}, function(error) {
 // handle network error
})
```

说明：
a. fetch api返回的是一个promise对象
b.Options:

- method(String): HTTP请求方法，默认为GET
- body(String): HTTP的请求参数
- headers(Object): HTTP的请求头，默认为{}
- credentials(String): 默认为omit,忽略的意思，也就是不带cookie;还有两个参数，same-origin，意思就是同源请求带- cookie；include,表示无论跨域还是同源请求都会带cookie


c.第一个then函数里面处理的是response的格式，这里的response具体如下：

![](https://ws1.sinaimg.cn/large/006c6oKBgy1fsvc2157tjj30jg0dct9o.jpg)


- status(number): HTTP返回的状态码，范围在100-599之间
- statusText(String): 服务器返回的状态文字描述，例如Unauthorized,上图中返回的是Ok
- ok(Boolean): 如果状态码是以2开头的，则为true
- headers: HTTP请求返回头
- body: 返回体，这里有处理返回体的一些方法



### fetch 常用情况

#### 请求 json

```js
 fetch('http://xxx/xxx.json').then(res => {
        return res.json();
    }).then(res => {
        console.log(res);
    })
```


#### 请求文本

```js
fetch('/xxx/page').then(res => {
        return res.text();
    }).then(res => {
        console.log(res);
    })
```

#### 发送普通 json 数据

```js
fetch('/xxx', {
        method: 'post',
        body: JSON.stringify({
            username: '',
            password: ''
        })
    });
```

#### 发送form 表单数据

```js
var form = document.querySelector('form');
    fetch('/xxx', {
        method: 'post',
        body: new FormData(form)
    });
```

#### 获取图片

```js
fetch('/xxx').then(res => {
        return res.blob();
    }).then(res => {
        document.querySelector('img').src = URL.createObjectURL(imageBlob);
    })
```


#### 上传

```js
var file = document.querySelector('.file')
    var data = new FormData()
    data.append('file', file.files[0])
    fetch('/xxx', {
      method: 'POST',
      body: data
    })
```




### 需要用到的 antd 的 API 有：

- Menu,
- Icon,
- Tabs,
- message,
- Form,
- Input,
- Button,
- CheckBox,
- Modal



#### 注意表单的接收： 在最后要进行一个form的二次构造：

const WrappedNormalLoginForm = Form.create()(NormalLoginForm);

ReactDOM.render(<WrappedNormalLoginForm />, mountNode);

经过 Form.create 包装的组件将会自带 this.props.form 属性，this.props.form 提供的 API 如下：

>使用 getFieldsValue getFieldValue setFieldsValue 等时，应确保对应的 field 已经用 getFieldDecorator 注册过了。


** getFieldDecorator	用于和表单进行双向绑定，以前用getFieldProps **

经过 getFieldDecorator 包装的控件，表单控件会自动添加 value（或 valuePropName 指定的其他属性） onChange（或 trigger 指定的其他属性），数据同步将被 Form 接管，这会导致以下结果：

- 你不再需要也不应该用 onChange 来做同步，但还是可以继续监听 onChange 等事件。

- 你不能用控件的 value defaultValue 等属性来设置表单域的值，默认值可以用 getFieldDecorator 里的 initialValue。

- 你不应该用 setState，可以使用 this.props.form.setFieldsValue 来动态改变表单值。
















### react 原理和算法

Virtual DOM

