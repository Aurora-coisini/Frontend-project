# 项目实现（以后台管理系统为例）

## 项目创建

```shell
cnpm create vue@latest
```

这样就会创建一个vue3的项目，我们接下来只需要命名我们的项目

然后选择项目的依赖包：可以都不选，我们自己手动创建也是可以的

然后回车，根据提示

```shell
cd my-project
cnpm install
cnpm run dev
```

这里面的三步分别进行了移动到项目文件夹、下载所需依赖、项目启动

这样就创建了我们的项目

项目里面我们常用的就是`src`目录下的文件夹及文件

我再次总结项目结构

**src里面的目录：**

`assets`：放静态资源的，里面常有styles目录、images目录、icons目录……

`components`：放我们自己创建的组件，通常每个组件会实现一个业务逻辑（**实现一个功能、一个部件**……）

`router`：专门存放路由有关的文件夹，里面都是一些路由配置

`store`：状态管理——pinia、vuex等状态管理（组件与组件之间的数据交互……）

`views`：页面级组件——**对应路由的页面**

`utils`：放一些工具函数

然后外面就是App.vue、main.js

这两个文件，我们要搞清楚，

`App.vue`：作为根组件，一般是挂载路由……

`main.js`：创建vue的应用实例，实现挂载以及一些功能（路由、element-plus、icons……）的使用

## 路由搭建

首先

```shell
cnpm install vue-router
```

安装路由

然后创建一个router目录，里面包含一个index.js

`index.js`里面首先

```vue
import {createRouter,createWebHashHistory} from 'vue-router'
```

然后**制定路由规则**

routes作为路由的一个属性，主要作用就是定义路由规则，路由规则是一个数组，里面的每一个成员都是对象

对象的属性包含了path（自己设定的路径）、name（对应路径的网页名字）、component（挂载相应的组件）

其中component需要引入相应的组件，我们才可以挂载，可以采用异步引入或同步引入

1.异步引入

![image-20251126201326813](C:\Users\25940\AppData\Roaming\Typora\typora-user-images\image-20251126201326813.png)

2.同步引入

直接在文件开头import

![image-20251126201357018](C:\Users\25940\AppData\Roaming\Typora\typora-user-images\image-20251126201357018.png)

接着创建路由实例，通过对象的解构赋值

![image-20251126201437495](C:\Users\25940\AppData\Roaming\Typora\typora-user-images\image-20251126201437495.png)

路由匹配模式常用——createWebHashHistory()

最后需要把这个路由实例导出

![image-20251126201543436](C:\Users\25940\AppData\Roaming\Typora\typora-user-images\image-20251126201543436.png)

编写完index.js

我们要在views目录里面创建路由对应的页面

![image-20251126201635892](C:\Users\25940\AppData\Roaming\Typora\typora-user-images\image-20251126201635892.png)

可以先不写页面内容

然后去到`main.js`在里面**引入我们创建的路由实例**

```vue
import router from '@/router'
```

**然后要使用这个路由实例**

![image-20251126201806993](C:\Users\25940\AppData\Roaming\Typora\typora-user-images\image-20251126201806993.png)

最后需要展示路由的内容，否则匹配了没有展示

需要在相应的组件里面引入，并且显示

```vue
import {RouterView} from 'vur-router'

<RouterView/>
```

## 组件搭建

首先**安装element-plus（去官网找）**

需要什么组件可以去element-plus里面找是否有自己所需的

然后需要做的就是复制粘贴

注意了**element-plus的引入有三种方式（看自己的需求去官网找）**

**首先找的就是布局容器**

但是这个侧边栏（有的时候这个高度可能不是100%）

我们需要在App.vue里面加上CSS

```css
html,
body,
#app{
	height:100%;
	width:100%;
}
```

这样才可以确保我们的页面会在整个视图上面完整显示（占视图100%）

需要注意的是，我们使用了element-plus里面的元件，那就最好不要动人家原有的标签，（最好不要删），只是在**标签内部插入我们自己创建的组件即可**

创建自己的组件的原则就是：看这个功能块需要扩充，那我就创建一个组件，在组件里面又找需要使用的元件，复制粘贴上去

主要是设计样式！

## 组件之间传递数据——pinia

可以查找官网看怎么用

1.安装pinia

```shell
cnpm install pinia -D
```

2.再main.js里面引入，并且**创建pinia实例使用这个实例**

```vue
import {createPinia} from 'pinia'

const pinia=createPinia();
app.use(pinia);
```

3.新建一个stores目录，里面创建一个index.js

文件里面首先定义并导出一个**仓库实例**

仓库实例需要defineStore()去创建

而defineStore()里面重要的就是他的参数是一个回调函数，

函数里面**用ref/reactive来定义数据**（我们需要使用的、需要交互的）,这个数据是对象类型的

```vue
const userInfo=ref({
属性:值
……
})
```

**用computed定义需计算的变量**

**直接用函数来修改定义的数据**

最后返回`return`**需要外部访问的这些数据和方法**

![image-20251126214150882](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251126214150882.png)

4.写完index.js之后，我们需要去到相应的组件里面使用仓库

首先引入我们所创建的仓库实例

获取创建实例（需要再创建一个属于这个组件的小仓库，根据我们创建的那个仓库实例来创建）

然后在该组件的script里面解构（需要使用到computed方法）这个仓库，把需要用的拿来用

```vue
import {useXXXX} from '@/stores'

const store=useXXXX();
const handle_click=()=>{
	store.state.数据=xxx；
}
const abc=computed(()=>store.state.数据)
```

我们可以这样理解，我们知道pinia就像是一个仓库一样，我们要创建仓库，在仓库里面储存我们会用到的（会被交互）的数据、方法，然后别的组件如果是想要访问或是修改仓库里面的数据，都必须要先创建一下这个仓库的实例（就好比是管理员登记）

defineStore()：给仓库「注册备案」——**定义仓库的规则**（有什么数据、能做什么操作），但此时仓库还没「开门使用」；

useXxxStore()：找管理员「登记领凭证」——创建 Store 实例 = **拿到仓库的「钥匙」**，只有拿到钥匙才能访问 / 修改仓库里的东西；

Store 实例：仓库的「专属钥匙」——全项目唯一（单例），不管在哪领钥匙，拿到的都是同一把，保证数据全局一致；

注意要访问仓库的数据，分两种情况，

1.不改变仓库的某数据的值，就直接用computed()来回调函数返回该数据

2.改变仓库里数据的值，就需要编写函数，直接修改仓库里某数据的值

## 网络请求——axios

1.安装

```shell
cnpm install axios -D
```

2.在那个组件用，就在那个组件引入

```vue
import axios from 'axios'
```

3.使用axios，axios()函数的返回值是一个promise对象，它的参数也是一个对象

对象的属性有网络接口的`url`，以及网络请求的方式`method`

![image-20251127151148486](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127151148486.png)

然后由于axios（）方法的结果是一个promise对象，那我们想要读取这个异步操作的结果就需要使用到then()方法

操作成功就需要打印成功的结果

![image-20251127152716489](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127152716489.png)

但是我们要做好网络请求的准备

也就是说：

**前端要学会制造假数据**

这里的制造假数据，不是让我们把数据写死，而是还是**要有交互的请求的流程**，要根据接口文档跑通，还要制造出数据

有一种工具，拦截住请求，然后把伪造的数据来根据接口文档传递过来——`mock.js`

去官网上面看怎么用

1.安装

```shell
cnpm install mockjs -D
```

2.创建目录api

在`api`目录下创建一个`mock.js`

需要在里面引入mock

```
import Mock from 'mockjs'
```

然后使用Mock对象提供的mock()方法

方法的参数分别是：拦截的路径（正则表达式）、网络请求的方法、**具体的返回数据的函数**

而这个获取的数据需要我们自己去伪造

那我们就在`api`目录下再建一个目录`mockData`专门放我们伪造的数据

我们先来创建我们伪造的第一个数据，需要home.js来装载

```javascript
//home.js
export default{
	请求的api:()=>
	{
		return {
			code:200,
			data:{
				tableData(这是我们需要的那个数据的名字):[
				{
				数据1
				},
				{
				数据2
				},
				{
				数据3
				},
				{
				数据4
				},
				]
			}
		}
	}
}
```

回到mock.js

再把我们创建的假数据引入

![image-20251127161940319](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127161940319.png)

最后最重要的就是

**我们需要在main.js里面把我们创建的mock引入，不然是不会生效的**

![image-20251127160058671](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127160058671.png)

我们现在把网络请求的结果改成我们这个伪造的假数据

![image-20251127160604907](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127160604907.png)

然后把本来我们写的死数据替换成这个

## axios的二次封装

我们之前讲的是哪个组件需要，就在那个组件里面写入，但是如果我有很多个组件都需要呢？难道我没一个组件都去写一遍吗？这未免太麻烦了

所以我们需要对axios进行一个二次封装

去官网找该怎么做

首先需要在api目录下，新建一个`request.js`文件——axios的二次封装

这个文件需要引入axios

然后首先进行的就是axios的拦截（在官网上面找拦截器怎么写）

然后就是请求之后的处理……

### 基础封装——request.js

这里面我们不直接复制粘贴，而是先创建一个自定义的axios实例，然后对这个实例进行拦截

![image-20251127163544494](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127163544494.png)

在对实例拦截的时候，我们需要按需修改请求之前和之后的操作

请求之前——拦截

请求之后——……

最后默认导出**导出的 `request()` 函数**——是文件载体

可以理解为就是把axios封装打包成一个request()函数

总的来说就是进行了![image-20251127170547872](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127170547872.png)

### 接口管理——api.js

把项目中的**所有接口地址、请求参数封装成函数**

为的就是返回这样一个axios，集中管理，避免接口散落在组件

![image-20251127170706449](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127170706449.png)

### 全局挂载——main.js

最后还要去main.js里面把api.js引入，然后所有接口函数挂载到 Vue 全局，这样就不需要在一个一个组件中去引入了

![image-20251127170904159](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127170904159.png)

组件若是想要使用这个全局挂载的api就直接用this.$api调用

或者是在vue3里面使用

![image-20251127172312401](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127172312401.png)

然后把页面上的tableData.val换成这个请求的结果，这样就可以接受从网络来的数据了

![image-20251127172652334](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127172652334.png)

## 项目的配置文件

一套完整的配置包括：环境配置、接口请求、Mock数据等

首先明确**配置的核心目的**
所有的配置都是为了解决实际问题的，就比如：

1.开发和测试时可能用模拟数据，上线时不可以用mock数据，需要切换到真实接口

2.不同环境（开发、测试、线上）的接口地址不同，需要去切换

以上两个原因，都涉及到环境不同，需要切换接口，**但是我又不想去手动的更改代码，那我就需要配置文件来根据环境自动地去变换接口地址**

3.接口请求需要统一处理（就比如说什么错误提示、参数格式调整之类的）

所以这第三个原因也告诉了要配置些什么

### 步骤一：创建环境配置文件config

集中管理不同环境的基础地址、mock开关等配置，

这样就可以实现：根据环境不同自动切换接口数据，而不需手动更改代码

思路：

1.首先区分环境，看有哪些可能出现的环境：开发、测试、生产

2.为每一个环境都需要定义：基础接口地址、mock地址

3.需要暴露全局的mock开关（因为是否用mock数据是要根据环境+mock开关来决定的）

![image-20251127200137130](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127200137130.png)

但是记住，我们需要首先获取当前的一个环境

```javascript
const env=import.meta.env ||"prod";
```

要么是本地的环境（开发、测试），要么就是线上环境

然后把这个文件里面定义的都抛出，还有一个全局的mock开关

![image-20251127200337084](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127200337084.png)

### 步骤二：封装接口请求工具request

统一处理接口请求（比如基础地址设置、错误提示、参数转换……），就可以在出现问题的时候，有应对措施

思路：

1.创建axios实例（见axios的二次封装）

这里需要设置基础地址

![image-20251127200827770](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127200827770.png)

2.根据环境和mock开关，来动态的切换接口地址

![image-20251127200646206](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127200646206.png)

zhuyimock开关也是在这个文件里面定义的

![image-20251127200711066](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127200711066.png)

默认是config里面的mock值，但是如果有单独的mock请求（mock的值与config中的不一样），那就可以覆盖config里面的mock

### 步骤三：定义接口列表api

这里面集中管理所有接口的路径和配置

思路：

1.每个接口对应一个函数

函数内部调用封装好的request()方法，并且返回

2.request()方法里面需要配置接口路径、请求方法、是否启用mock等

![image-20251127201007074](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127201007074.png)

### 步骤四：配置mock数据

开发环境和测试环节时，无需等待后端接口，用模拟数据调试

思路：

1.用mockjs生成模拟数据

2.拦截网络请求（拦截指定接口路径，返回模拟数据）

![image-20251127201234829](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127201234829.png)

### 步骤五：全局注册接口main

在main.js里面全局引入api，让所有的组件都可以方便的调用接口，就不需要每个组件里面再次导入

![image-20251127201352316](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127201352316.png)

### 步骤六：在组件中使用接口

vue2里面可以用this.$api来使用

但是vue3需要在生命周期函数里面调用函数来获取相应接口内部的数据

![image-20251127201608546](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251127201608546.png)

## 使用echarts

1.安装

```shell
cnpm install echarts -D
```

2.图表的配置（渲染）

```javascript
import * as echarts from 'echarts'
import {reactive} from 'vue'

const observer=ref(null);//接收观察器实例对象

//柱状图和折线图的公共配置
const xOptions=reactive({
	//图例文字颜色
	textStyle:{
		color:"#333"
    },
    legend:{},
    grid:{//设置图表距离容器div的上下左右距离
        left:"20%"
    },
    //提示框
    tooltip:{
		trigger:"axis"
    },
    xAxis:{
		type:"category",
        data:[],
        axisLine:{
			lineStyle:{color:"#17b3a3"}
        },
        axisLabel:{
            interval:0,
            color:"#333"
        }
    },
    yAxis:[
		{
            type:"value",
            axisLine:{
				linStyle:{color:"#17b3a3"}
            }
        }
    ],
    color:			['#2ec7c9','#b6a2de','#5ab1ef','#ffb980','#d87a80','8d98b3'],
    series:[],
})

//饼状图的配置
const pieOptions=reactive({
	tooltip:{
		trigger:"item"
    },
    legend:{},
    color:[
        "#0f78f4",
        "#dd536b",
        "#9462e5",
        "#a6a6a6",
        "#e1bb22",
        "#39c362",
        "#3ed1cf"
    ],
    series:[]
})
```

这些配置可以在官网找到

3.开始实现

（1）整体流程

·从接口处获取图表的数据（home.js、api.js、mock.js）

·配置Echarts的基础参数（折线图、柱状图、饼状图）的图表配置（坐标轴、图例、样式等可以在官网找，或者直接复制粘贴上面的内容）

·将接口数据转换为Echarts所需的格式（也就是配置x轴和series对应接口的哪个数据）

·初始化图表实例，并渲染到页面

首先第一二步我们已经知道怎么做了

然后就是第三步将`接口数据转换`首先我们要在该组件获取接口数据，再来进行数据的转换对应

那么就需要在script里面写好函数`getChartData()`

这个函数里面就像是我们之前做的那样，先通过proxy.$api.getXXXXX()来获取接口数据，但是这里有所不同的是，因为整个图表的数据包含了x轴的时间数据、每一条折线的数据……有很多数据，所以我们采用对象的解构赋值，把getChartData里面的每一个属性单独领出来操作

```javascript
const {orderData}=await proxy.$api.getChartData();
```

接着就是进行数据的转换，让数据和图表的各部分相对应：

x轴对应时间数据

```javascript
xOptions.xAxis.data=orderData.date;
```

series：表示图表中具体要展示的数据内容和图表的类型

```javascript
xOptions.series=Object.keys(orderData.data[0]).map(key=>{
return {
	name:key,//由键来确定每条折线的名字（作为标识）
	data:orderData.data.map(item=>item[key]),//每条折线中具体的值，根据折线的名字来绑定折线的具体数据
	type:'line'//图表类型：折线图
}
})
```

第三步到此就结束了

然后就是第四步：需要创建图表实例，并且初始化

我们通过echarts.init()函数来进行创建实例并初始化

```javascript
proxy.$refs['echart']` 是用于获取模板中标记了 `ref="echart"
```

`proxy` 是通过 `getCurrentInstance()` 获取的当前组件实例的代理对象，相当于 Vue2 中的 `this`，用于访问组件的属性和方法。

`$refs` 是 Vue 提供的一个特殊对象，用于**访问模板中通过 `ref` 属性标记的 DOM 元素或组件实例。**

## elemen-plus侧边栏

注意了这个侧边栏高度设置的问题

我试过这些方法：

1.

```css
//在App.vue里面进行
html,
body,
#app{
	width:100%;
	height:100%;
	overflow:hidden;
}
```

改成这个倒是没有滚动条了，但是我发现我只要点开检查（f12）就会看不到完整的页面，连滚动条都没有（这玩意儿该有的时候有没有了）

![image-20251128175508612](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251128175508612.png)

2.

```css
//Aside.vue里面进行以下样式设置
.el-menu{
	height:100vh;
}
//或者是在App.vue中进行
html,
body{
	width:auto;
	height:auto
}
#app{
	width:100%;
	height:100%;
	overflow:hidden;
}
//或者是
html,
body，
#app{
	width:100%;
	height:100%;
}
```

但是这无法避免的是都有滚动条！怎么调整都除不掉

而且滚动条划到最上面和最下面都可以发现白边（上下有）

![image-20251128183341474](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251128183341474.png)

![image-20251128183350128](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251128183350128.png)

3.这个方法消除了之前那个滚动条

```css
//在App.vue中添加
html,
body{
  width:auto;
  height:auto;
  margin:0;
  padding: 0;
}
#app{
  width:100%;
  height:100%;
  overflow:hidden;
  /* 设置超出部分被剪裁 */
}
```

这个就解除了之前的问题

当然还要相应的调整Main里面的卡片大小（可能是因为太高了所以无可避免有滚动条

但是我又发现了新问题，就是这个方法的高度设置无法传递到其他组件

我跳转到其他页面的时候侧边栏高度又回到了原来的样子

![image-20251128185058292](C:/Users/25940/AppData/Roaming/Typora/typora-user-images/image-20251128185058292.png)

4.最后我采取了这个方法，刚好消除了滚动条，解决了目前遇到的所有问题

```javascript
//App.vue
html,
body{
  margin:0;
  padding: 0;
}
#app{
  width:100%;
  height:100%;
  overflow:hidden;
  /* 设置超出部分被剪裁 */
}
//Aside.vue
.el-menu {
    height:97vh;
}
```

## User.js

```javascript
import Mock from 'mockjs'
 
// get请求从config.url获取参数，post从config.body中获取参数
function param2Obj(url) {
    const search = url.split('?')[1]
    if (!search) {
        return {}
    }
    return JSON.parse(
        '{"' +
        decodeURIComponent(search)
            .replace(/"/g, '\\"')
            .replace(/&/g, '","')
            .replace(/=/g, '":"') +
        '"}'
    )
}
let List = []
const count = 200
//模拟200条数据
for (let i = 0; i < count; i++) {
    List.push(
        Mock.mock({
            id: Mock.Random.guid(),
            name: Mock.Random.cname(),
            addr: Mock.mock('@county(true)'),
            'age|18-60': 1,
            birth: Mock.Random.date(),
            sex: Mock.Random.integer(0, 1)
        })
    )
}

export default {
    /**
     * 获取列表
     * 要带参数 name, page, limt; name可以不填, page,limit有默认值。
     * @param name, page, limit
     * @return {{code: number, count: number, data: *[]}}
     */
    getUserList: config => {
        //limit默认是10，因为分页也是默认一页10个
        const { name, page = 1, limit =10 } = param2Obj(config.url)
        console.log('name:' + name, 'page:' + page, '分页大小limit:' + limit)
        const mockList = List.filter(user => {
            if (name && user.name.indexOf(name) === -1 ) //如果name存在就根据name筛选数据
                return false
            return true
        })
        //分页
        const pageList = mockList.filter((item, index) => index < limit * page && index >= limit * (page - 1))
        return {
            code: 200,
            data:{
            	count: mockList.length,//数据总条数需要返回
            	list: pageList
            }
        }
    },
 /**
     * 批量删除
     * @param config
     * @return {{code: number, data: {message: string}}}
     */
   deleteUser: config => {
        const { id } = param2Obj(config.url)
        if(!id){
			return{
				code:-999,
                message:"参数不正确"
            }
        }
       else{
            List = List.filter(u => u.id!==id)
        	return {
           	 	code: 200,
                message: '删除成功'
            }
        }
    },


```

新增用户的一个对话框的实现

```vue
<el-dialog 
v-model="dialogVisible"
:title="action == 'add' ? '新建用户' : '编辑用户'" 
width="35%"
:before-close="handleClose">
        <!-- 表单Form -->
        <!-- ref=form:为了通过this.$refs调用组件的方法 -->
        <!--需要注意的是设置了el-form是行内元素，所以会对el-select的样式造成影响，我们通过给他设置一个class=select-clearn在CSS中进行处理-->
        <el-form :inline="true" :model="formUser" :rules="rules" ref="userForm" >
        <el-row>
        	<el-col :span="12">
         	 <!-- 每一项表单域:el-form-item -->
         	 	<el-form-item label="姓名" prop="name">
           		 <el-input placeholder="请输入姓名" v-model="formUser.name"></el-input>
         		 </el-form-item>
         	</el-col>
			<el-col :span="12">
          		<el-form-item label="年龄" prop="age">
            	<el-input placeholder="请输入年龄" v-model="formUser.age"></el-input>
          		</el-form-item>
			</el-col>
            </el-row>
            <el-row>
                <el-col :span="12">
          			<el-form-item 
  class="select-clearn"label="性别" prop="sex">
           		 <el-select v-model="formUser.sex" placeholder="请选择">
             		 <el-option label="男" :value="1"></el-option>
                  	<el-option label="女" :value="0"></el-option>
            	</el-select>
          		</el-form-item>
               </el-col>
           
		<el-col :span="12">
          <el-form-item label="出生日期" prop="birth">
              <el-date-picker type="date" placeholder="请选择日期" v-model="formUser.birth" value-format="yyyy-MM-DD" style="width:100%">
              </el-date-picker>
            </el-form-item>
                </el-col>
            </el-row>
            
          <el-row>
          <el-form-item label="地址" prop="addr">
            <el-input placeholder="请输入地址" v-model="formUser.addr"></el-input>
          </el-form-item>
            </el-row>
         <el-row style="justify-content:flex-end">
          <el-form-item>
             <el-button type="primary" @click="handleCancel">取消</el-button>
              <el-button type="primary" @click="onSUbmit">确定</el-button>
             </el-form-item>
            </el-row>
        </el-form>
      </el-dialog>


```

