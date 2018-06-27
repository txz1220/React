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


下面布局用Antsign 里面的Flex进行布局(基于24栅格布局)


新建pc_index.js   把我们的pc_header.js导进去。

```js
import React from 'react';
import PCHeader from './pc_header'

export default class PCIndex extends React.component{
    render(){
        return(
            <div>
            <PCHeader></PCHeader>
            </div>
        )
    }
}
```

新建root.js ，接着把pc_index.js 导入到root.js

```js
import React from 'react';
import ReactDom from 'react-dom';
import {Router,Route,hashHistory} from 'react-router';

import PCIndex from './components/px_index';

export default class PCIndex extends React.component{
    render(){
        return(
            <div>
            <PCIdex></PCIdex>
            </div>
        )
    }
}

// 出口：

ReactDOM.render(
    <Root/>, document.getElementById('mainContainer'));

```


导航用antd里面的menu  

首先是导入API进来

import {Menu,Icon} from 'antd';

然后在布局里面加入(其他项目，直接copy即可)：

```js
<Row>
    <Col span={16}>
        <Menu.Item>
            <Menu.Item>
                <Icon type="appstore"/>头条
            </Menu.Item>
        </Menu.Item>
    </Col>
</Row>
```

选中状态的指定（Menu）：selectedKeys={[this.state.current]}  参见antd 里面Menu文档

然后我们对current初始化：

//做一个构造函数
constructor(){
    super(); //基类初始化
    this.state ={
        current: 'top'   //初始化current的默认指定的key是top
    }
}


#### 手机端页头开发：


手机端需要适配，也就是响应式开发， 我们需要加载一个组件：

react-reponsive   npm 安装即可

在root.js 做判断就可以了。 具体细节可以看注释。



