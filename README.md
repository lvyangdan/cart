# 前言

学习react,体验一下react框架

## 脚手架

[generator-react-webpack](https://www.npmjs.com/package/generator-react-webpack)

**Installation**

```
npm install -g yo
npm install -g generator-react-webpack
```

**Setting up projects**

```
# Create a new directory, and `cd` into it:
mkdir my-new-project && cd my-new-project
 
# Run the generator
yo react-webpack
```

## 技术

react+react-router+redux+ webpack + ES6 + fetch+antd

## 项目结构

```
│  .babelrc
│  .editorconfig
│  .eslintrc
│  .gitignore
│  .yo-rc.json
│  karma.conf.js
│  package.json
│  prod.server.js
│  server.js
│  shop.json
│  tree.txt
│  webpack.config.js
│  
├─cfg
│      base.js
│      defaults.js
│      dev.js
│      dist.js
│      test.js
│      
├─dist
│          
├─src
│  │  favicon.ico
│  │  index.html
│  │  index.js
│  │  routes.js
│  │  
│  ├─actions
│  │      index.js
│  │      README.md
│  │      
│  ├─api
│  │      shop.json
│  │      
│  ├─components
│  │      Destination.js
│  │      Detail.js
│  │      Index.js
│  │      Main.js
│  │      Plan.js
│  │      
│  ├─config
│  │      base.js
│  │      dev.js
│  │      dist.js
│  │      README.md
│  │      test.js
│  │      
│  ├─constants
│  │      ActionTypes.js
│  │      
│  ├─images
│  │      
│  ├─reducers
│  │      cart.js
│  │      count.js
│  │      history.js
│  │      index.js
│  │      
│  ├─sources
│  │      
│  ├─stores
│  │      
│  └─styles
│          App.css
│          
└─test
            
```

## 目标功能

- [x] 商品浏览页面 -- 完成
- [x] 商品详细页面-- 完成
- [x] 购物车页面-- 完成
- [x] 历史记录页面 -- 完成

## 项目运行
``` 
cd react-shopping

npm install

npm run start //运行命令

npm run dist //打包命令
```
src/index.js
```
import 'core-js/fn/object/assign'
import React from 'react'
import ReactDOM from 'react-dom' // 14.0版本后拆分成react和react-demo
import { createStore ,applyMiddleware } from 'redux'
import { Provider } from 'react-redux'
import Main from './components/Main'
import { createLogger } from 'redux-logger'
import thunk from 'redux-thunk'
import reducer from './reducers'
import 'antd/dist/antd.css'

import './styles/App.css'
import { getAllProducts } from './actions'

const middleware = [ thunk ] // redux-thunk解决异步回调
if (process.env.NODE_ENV !== 'production') {
  middleware.push(createLogger())
}

const store = createStore(reducer,
  applyMiddleware(...middleware) // 中间件
)

store.dispatch(getAllProducts()) //获取全部商品
// Render the main component into the dom
ReactDOM.render(
   <Provider store={ store } >
     <Main />
   </Provider>
  ,document.getElementById('app')
)
```

依赖路由以及主入口文件Main.js

src/components/Main.js
```
import React from 'react'
import {
  BrowserRouter as Router,
  Route,
  Link
} from 'react-router-dom'
import {connect} from 'react-redux'
import Index from './index'
import Destination from './Destination'
import Plan from './Plan'
import Detail from './Detail'
import {Menu, Icon} from 'antd'
const SubMenu = Menu.SubMenu

const Basic = () => (

  <Router >
    <div className="clear container-main">
      <div className="fl">
        <Menu
          style={{width: 240}}
          defaultSelectedKeys={['1']}
          defaultOpenKeys={['sub1']}
          mode="inline"
        >
          <SubMenu key="sub1" title={<span><Icon type="mail"/><span>操作</span></span>}>
            <Menu.Item key="1"><Link to="/">主页</Link></Menu.Item>
            <Menu.Item key="2"><Link to="/about">购物车</Link></Menu.Item>
            <Menu.Item key="3"><Link to="/topics">购买记录</Link></Menu.Item>
          </SubMenu>
        </Menu>
      </div>

      <Route exact path="/" component={Index}/>
      <Route path="/about" component={Destination}/>
      <Route path="/topics" component={Plan}/>
      <Route path="/detail/:topicId" component={Detail} />
    </div>
  </Router>
)

export default connect()(Basic)
```
运用了函数式编程方式：
我们可以看看普通继承和函数式编程的差异，函数编程简洁了不少。也可以看到react-router在4.0版本后发生了一些变化。
![这里写图片描述](http://img.blog.csdn.net/20170525130009551?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmx1ZWJsdWVza3lodWE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 效果图
![这里写图片描述](http://img.blog.csdn.net/20170525133053530?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmx1ZWJsdWVza3lodWE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170525133118375?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmx1ZWJsdWVza3lodWE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170525133131593?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmx1ZWJsdWVza3lodWE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170525133144187?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmx1ZWJsdWVza3lodWE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170525133157422?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmx1ZWJsdWVza3lodWE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
