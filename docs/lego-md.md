---
创建日期: 2024-01-31
Author:
  - 陈新宇
---
# 门户管理中心编辑器

> 门户管理中心编辑器是一个使用vue3.3 + ts + vite开发的可视化低代码编辑器 ，该项目依托于门户管理中心作为入口，为门户管理中心提供拖拽生成静态html页面和对外生成组件库供生成的html引入的能力。
> 项目基于vue3.3，该版本提供了更完备的ts类型支持，支持导入其他ts文件中的类型作为props参数。
> 该项目提供了完备的ts类型， 所有组件、逻辑、方法、hooks均基于ts开发，项目中的所有配置均可参考对应位置的`model`文件中的ts定义进行开发。
## 项目功能

- 💡 打包生成组件库供html使用
- 🚀 可视化拖拽生成页面
## 1. 项目配置与运行
### 环境配置
```
node 版本 16.17
包管理器： pnpm 8.1.0
```
### 安装
```bash  
$ pnpm install  
```  
### 开发
```bash  
$ pnpm run dev  
```  
### 启动
> 注意： 该项目为门户管理中心的编辑器，本地开发时需要配合门户管理中心使用，步骤如下：  
> 1.打开并启动门户管理中心的开发环境并登录（如果不登录，编辑器接口不通）  
> 2.启动编辑器的开发环境  
> 3.在门户管理中心模板管理页面点击新建或修改模板按钮跳转到编辑器对应的开发页面
### 构建
本项目打包可以生成两种产物，分别对应不同的命令：物料库(组件库)包和编辑器包，
- 物料库包作为组件库（类似element-ui）提供给编辑器和生成的html使用，其中在编辑器包在构建时会自动将物料包包含其中，而html页面使用新的物料包需要每次将打包后将生成的`demo-cms/lib/lego-materials`文件夹上传到服务器中使用(提供给后端)。
- 编辑器包作为构建器页面的主体使用，生成dist包，编辑器本身已经包含物料库的内容，无需额外引入（正常发版）。

每次开发完成打包后分别执行 `npm run build:[env]` 和 `npm run gen:materials` 两个命令，具体说明如下
```bash  
# 打包编辑器：测试环境
$ npm run build:test
  
# 打包编辑器：开发环境  
$ npm run build:production
  
# 打包组件库（组件库不区分测试、生产环境）,生成文件直接提供给后端放到服务器
$ npm run gen:materials
  
# 配合webstorm TreeInfoTip插件，自动生成materials/core/**/** 的目录中文注释  
$ npm run renderDirDescription
```  

## 2.项目目录文件
使用webstorm启动项目时，可以安装TreeInfotip插件，可以直接在项目目录中直接查看目录的中文注释

```shell  
|-- fosung  
    |-- .env.development // 开发环境配置  
    |-- .env.production //  生产配置  
    |-- .env.test // 测试环境配置  
    |-- .eslintignore  
    |-- .eslintrc.cjs  
    |-- .gitignore  
    |-- README.md  
    |-- index.html  
    |-- package.json  
    |-- pnpm-lock.yaml  
    |-- tsconfig.json  
    |-- tsconfig.node.json  
    |-- uno.config.ts // unocss配置  
    |-- vite.config.cms.ts // 组件库打包时的配置  
    |-- vite.config.ts // 编辑器打包时的配置  
    |-- build  
    |   |-- proxy.ts // 代理配置  
    |   |-- wrapperEnv.ts  
    |-- demo-cms // 存放打包后的组件库并用于开发环境预览  
    |   |-- data.js // 存放生成的json供demo.html使用,可以在生成的页面中复制json数据到该文件中,做到无需发版查看组件效果  
    |   |-- demo1.html // 对应生产上预览的html文件,可以浏览器打开该文件在本地预览效果  
    |   |-- lib // 组件库打包后的文件  
    |       |-- vue.js // vue3文件,供组件库引用,无需每次发给后端  
    |       |-- lego-materials // 打包生成的组件库, 组件库打包后将该文件夹直接发给后端上传到服务器中即可  
    |           |-- style.css  
    |           |-- vite.svg  
    |           |-- website.js  
    |           |-- website.js.map  
    |-- dist // 打包编辑器生成的包  
    |-- public  
    |   |-- vite.svg  
    |-- src  
    |   |-- App.vue  
    |   |-- main.ts  
    |   |-- api // 接口  
    |   |   |-- model // 接口的数据类型,model目录下的每个类型文件对应api目录下的一个同名的接口文件  
    |   |-- assets // 资源  
    |   |   |-- icons // 图标资源,可以直接使用unocss的图标导入功能导入  
    |   |   |-- img  // 图片资源  
    |   |-- components // 公共的组件,主要是提供给编辑器使用的组件,区别于提供给组件库的组件  
    |   |   |-- form // 核心之一,编辑器右侧的表单渲染器  
    |   |   |   |-- index.ts // 统一的导出路径  
    |   |   |   |-- componentMap // 表单渲染器支持的组件映射,表单渲染器每次新增组件需要在此添加类型  
    |   |   |   |   |-- index.ts  
    |   |   |   |-- components // 表单渲染器中的组件库  
    |   |   |   |-- core // 表单渲染器的核心逻辑  
    |   |   |       |-- FormGenerator.tsx  
    |   |   |       |-- FormGeneratorItem.tsx  
    |   |   |       |-- FormGroup.tsx  
    |   |   |       |-- index.less  
    |   |   |       |-- model.ts  
    |   |   |-- render // 核心之一,渲染器,用于渲染组件树  
    |   |       |-- ComponentRender.vue  
    |   |       |-- ComponentWrapper.vue  
    |   |-- const // 包含一些常量,vue编辑器和组件库中提供的provide的key,编辑器或组件库中使用的默认值  
    |   |   |-- basic.ts // 项目中基础的一些常量  
    |   |   |-- provide.ts // 向组件库中提供的provide的key  
    |   |   |-- channelData // 项目中某些组件对应的栏目的id需要写死,且在测试和生产环境对应的id不同  
    |   |   |   |-- data.dev.ts // 测试/开发环境使用的channelId  
    |   |   |   |-- data.production.ts // 生产环境使用的channelId  
    |   |   |   |-- index.ts // 统一的导出路径,根据环境变量导出对应的channelId  
    |   |   |   |-- model.ts // channelId的类型,每次添加一个新的频道id都需要在此添加类型  
    |   |   |-- defaultData // 项目中某些组件的默认值  
    |   |       |-- materialsMapFields.ts // 组件库中的组件的mapFields的默认值, 用于规定从后端获取对应的字段及其名称  
    |   |       |-- templateDefault.ts // 模板的默认值  
    |   |-- materials // 核心,组件库  
    |   |   |-- build.ts // 打包组件库时的入口文件  
    |   |   |-- index.ts // 统一的导出路径,编辑器多数情况下使用该路径导入组件库的内容,但是打包组件库时使用的是index.cms.ts  
    |   |   |-- components // 组件库中用到的基础组件  
    |   |   |   |-- BasicPagination.vue  
    |   |   |   |-- BasicTitle2.vue  
    |   |   |   |-- DraggableBlock.vue  
    |   |   |   |-- IframeBase.vue  
    |   |   |   |-- ListImageContent.vue  
    |   |   |   |-- VideoPlayer.vue  
    |   |   |   |-- BasicCarousel  
    |   |   |   |   |-- carousel-item.less  
    |   |   |   |   |-- carousel-item.vue  
    |   |   |   |   |-- carousel.less  
    |   |   |   |   |-- carousel.vue  
    |   |   |   |   |-- config.ts  
    |   |   |   |   |-- index.vue  
    |   |   |   |   |-- hooks  
    |   |   |   |       |-- carousel-item.ts  
    |   |   |   |       |-- carousel.ts  
    |   |   |   |       |-- constants.ts  
    |   |   |   |       |-- instance.ts  
    |   |   |   |       |-- use-carousel-item.ts  
    |   |   |   |       |-- use-carousel.ts  
    |   |   |   |       |-- useFlattedChildren.ts  
    |   |   |   |       |-- useOrderedChildren.ts  
    |   |   |   |-- BasicImgBox  
    |   |   |   |   |-- config.ts  
    |   |   |   |   |-- index.vue  
    |   |   |   |-- BasicTitle  
    |   |   |   |   |-- config.ts  
    |   |   |   |   |-- index.vue  
    |   |   |   |-- siderBar  
    |   |   |       |-- index.vue  
    |   |   |       |-- index_old.vue  
    |   |   |-- core // 组件库向外暴露的组件,组件库的核心  
    |   |   |   |-- common // 普通组件  
    |   |   |   |-- layout // 布局组件  
    |   |   |-- model // 类型文件  
    |   |   |   |-- components.ts  
    |   |   |   |-- config.ts  
    |   |   |   |-- material.ts  
    |   |   |   |-- props.ts  
    |   |   |   |-- wrapper.ts  
    |   |   |-- wrapper // 所有组件外层包裹的div上设置的属性在此定义  
    |   |       |-- schemas.ts  
    |   |-- router // 路由  
    |   |   |-- index.ts  
    |   |-- store // 全局存储,所有的数据的存储和交互都在store中进行  
    |   |   |-- cmsData.ts // 栏目数据  
    |   |   |-- editor.ts // 核心,编辑器的数据,包括组件树,当前选中的组件,当前选中的组件的配置项等  
    |   |   |-- index.ts  
    |   |   |-- zoom.ts // 编辑器缩放逻辑  
    |   |   |-- model // 类型文件  
    |   |       |-- editor.ts  
    |   |-- style 样式文件  
    |   |   |-- drag.less  
    |   |   |-- index.less  
    |   |   |-- materials  
    |   |       |-- base.less  
    |   |       |-- reset.less  
    |   |-- utils // 基本的工具函数和hooks  
    |   |   |-- buildUUID.ts  
    |   |   |-- getIsBasicSchema.ts  
    |   |   |-- tree.ts  
    |   |   |-- materials // 组件库中用到的工具函数和hooks  
    |   |   |   |-- getUsageEnv.ts  
    |   |   |   |-- isCommonComponent.ts  
    |   |   |   |-- transformConfigToMaterials.ts  
    |   |   |   |-- transformPosition.ts  
    |   |   |   |-- useCmsProps.ts  
    |   |   |   |-- useDomStyle.ts  
    |   |   |   |-- useHoverComponent.ts  
    |   |   |   |-- useLocation.ts  
    |   |   |   |-- useModel.ts  
    |   |   |   |-- useTab.ts  
    |   |   |   |-- useUserInfo.ts  
    |   |   |-- request  
    |   |       |-- index.ts  
    |   |       |-- instance.ts  
    |   |-- views // 页面  
    |       |-- demoGround // demo页面,用于测试新增的组件等  
    |       |   |-- index.vue  
    |       |-- editor // 编辑器页面  
    |           |-- CenterCanvas.vue // 编辑器中间的画布  
    |           |-- EditorConfigure.vue // 编辑器右侧的表单渲染器和配置项  
    |           |-- HeaderBar.vue  // 编辑器顶部的工具栏  
    |           |-- LeftSelector.vue // 编辑器左侧的组件库  
    |           |-- index.vue // 编辑器页面的入口文件  
    |           |-- components // 编辑器页面中用到的组件  
    |               |-- ComponentModal.vue // 模板编辑模式下保存自定义组件的弹窗  
    |               |-- ComponentSelect.vue // 选择组件库中的组件  
    |               |-- ComponentSelectModal.vue // 组件编辑模式下开始时选择组件的弹窗  
    |               |-- ComponentTree.vue // 当前编辑器中拖拽出的组件生成的组件树  
    |               |-- JsonModal.vue // json编辑弹窗  
    |               |-- PageModal.vue // 模板编辑模式下是保存模板的弹窗 组件编辑模式下是保存自定义组件的弹窗  
    |               |-- TemplateModal.vue // 保存为自定义模板的弹窗  
    |-- types // 全局的类型文件  
        |-- components.d.ts  
        |-- global.d.ts  
        |-- shims.d.ts  
        |-- vite-env.d.ts  
```  

### 打包文件
`/demo-cms:`   执行`npm run gen:materials` 后生成的物料组件库所在的目录，具体文件夹为`demo-cms/lib/lego-materials`, 打包后将该文件夹发送给后端上传服务器即可，测试环境与生产环境均引入服务器上的该文件夹，类似`element-ui`的形式。
`/dist: ` 打包编辑器生成的文件，正常部署即可
### 配置文件
`/vite.config.ts: ` `npm run build:[env]` 命令执行使用的配置文件，包含项目启动本地服务与打包时的配置，生产环境与测试环境均使用该配置文件。
`/vite.config.cms.ts：`  `npm run gen:materials` 执行时的使用配置文件，主要是对于组件库的打包配置。
`.env.development:` 开发环境下使用的环境属性。
`.env.test:`  测试环境下使用的环境属性。
`.env.production:`  生产环境下使用的环境属性。
### 核心文件
`/src/components:`  公共的组件,主要是提供给编辑器页面使用的组件（区别于提供给组件库的组件），物料库
`/src/components/form：` 主要是用于编辑器右侧表单配置组件，用于编辑器编辑时配置组件的属性，使用方式类似vben框架表单配置。
`/src/components/render：`  主要用于组件在编辑器中的展示效果，主要提供组件选中、hover时的样式；组件删除、保存为组件、拖拽等功能的按钮；并提供开发时必要的属性提供给每个组件，如当前组件的id，当前组件的配置属性等。
`/src/const:`  项目中的常量配置。
`/src/const/channelData` 为项目中默写固定的栏目id，如灯塔党建要闻对应栏目的id，该文件夹区分生产环境与开发环境，不同环境下对应的后台的id不同，因此要将其进行区分。
`/src/const/defaultData: ` 项目的基础模板配置，当点击进入一个空模板时默认会加载该json数据作为默认模板。
`/src/views/editor: ` 编辑器的主要布局结构与所有全局的基础操作均在该文件夹下，该部分分为**头部、物料库、中心绘制区域、表单编辑器**四块内容，涵盖了页面的初始化逻辑、提供给所有组件的基本的数据、编辑器中的操作按钮、组件树、保存/编辑json等功能按钮、拖拽功能等一系列基础功能。
### 物料库
物料库(`src/materials`)是该项目的核心区域，整个物料库可以视为一个单独的项目，该目录的主要功能有两块：`1. 为编辑器提供基本的组件库，即编辑器左侧的组件列表以及中心渲染区域的展示样式 2. 作为打包后的组件库供生成的html引入，实现客户端最终页面的渲染。`
`/src/materials/core：`  所有对外展示的组件存放的位置，组件分为布局组件与普通组件，分别对应该文件夹下`layout`和`common` 两个目录。
`/src/materials/components：` 物料库公共组件，区分于`/src/componnets`，该目录下的组件只供 `/src/materials/core` 下的组件使用，作为组件的基础组件使用。
`/src/materials/build.ts: ` 物料库打包时的入口文件，包含对所有物料组件的注册使用。
### store
该项目使用pinia作为状态管理库，为了保证数据与逻辑的统一，关键的数据存储以及组件逻辑均在store中定义并向外提供，所有的状态管理文件放置在`/src/store`目录下.
`/src/store/editor：` 编辑器中的组件状态，包括拖拽生成的组件树、当前组件、当前组件的属性、更新某个组件方法等功能，具体对应状态以及功能在代码中均有注释。
`/src/store/cmsData：` 存放cms中的所有数据，根据前后端定义的规范，获取的cms相关的数据，供所有公共组件使用，一般用于组件编辑时选择对应的栏目、内容等。
具体的页面结构与状态管理的关系如下：
![](https://raw.githubusercontent.com/AlexxChan/img-store/main/202405291604337.png)
## 3. 开发
### 开发新的物料
所有物料组件都放在`/src/materials/core/[common|layout]` 目录下，每个单独的物料组件的配置以及代码均单独放在一个同名文件夹中，该文件夹名称即为组件名。
每个单独的物料组件文件夹包含三部分内容，配置文件（`config.ts`  ）、组件本身（`index.vue`）、封面（`thumbnail.png`）。
#### 配置文件
配置文件`config.ts` 作为组件的基本配置，包含了组件的标题、组件封面、组件的`props` 属性、以及组件在编辑器中可编辑的属性、`props` 的默认值等一系列属性配置。
配置文件默认导出两部分内容:
1. `接口PropsType`:   项目采用vue3.3开发，所以可以ts类型作为props的传递给`index.vue` 作为props属性来定义组件所需传入的属性（[相关文档](https://cn.vuejs.org/guide/typescript/composition-api.html#typing-component-props)）。
2. `config属性`： 提供给编辑器的物料组件相关的属性，如标题、封面、表单配置、组件props的默认值等。具体可配置的内容参见`/src/materials/model/config.ts` 文件中的`ConfigType`类型。
   配置文件导出的两部分内容中，均有`cmsProps` 属性，该属性为前后端数据对接的内容，`config`中该属性为一个对象，对象中每一个值都包含`type`、`params`等属性；`type` 目前对应四种数据结构，对应的类型为`/src/materials/model/props.ts` 中`CmsPropsContent`类型，分别对应栏目数据，内容列表、栏目二维列表、分页数据等。
#### 组件本身
组件本身依照正常的`vue3`组件开发即可，需要注意组件的props需要使用`vue3+ts`的形式声明，从当前目录下的`config.ts`文件导入。
```ts
import { PropsType } from "./config"  
import { computed } from "vue"  
  
const props = defineProps<PropsType>()
```

除此之外，对于`cmsProps`的获取也提供了hooks来简化对应操作，
```ts
// config
export interface PropsType extends CommonComponentPropsExtends {  
  height: string  
  cmsProps: {  
    channelData: ChannelData  
    // 第二个数据，内容列表  
    contentList: ContentList  
  }  
}

const config: ConfigType<'common', PropsType> = {  
  ...
  // cms属性  
  cmsProps: {  
    channelData: {  
      type: 'channelData',  
      mapFields: {  
      },  
      params: {  
        id: '',  
      },  
      value: {  
        title: '党建要闻',  
        link: 'https://www.baidu.com',  
      },  
    },  
    contentList: {  
      type: 'contentList',  
      params: [  
        {          
          channelId: '',  
          count: 10  
        }  
      ],  
      mapFields: {},  
      value: [  
      ]}  },  
}  
  
export default config


// index.vue
// 可以直接获取cms中的数据作为响应式数据使用
const { contentList, channelData } = useCmsProps(props)
```
#### 开发布局（layout）组件
布局组件与普通组件并无太大区别。
与普通组件的区别在于，布局组件中所有的可拖拽区域均引入位于`/src/materials/components/DraggableBlock.vue`的`DraggableBlock`组件进行开发，布局组件负责提供放置`DraggableBlock`组件的位置。
每个`DraggableBlock`组件均是一个独立的可拖拽的区域，在每个布局组件中需要放置几个拖拽区域，则放置几个对应的`DraggableBlock`组件，例如一个左右布局的组件，则需要在组件中放置两个`DraggableBlock` 组件作为拖拽区域。
组件与`DraggableBlock`的关系如下:

![网页形式](https://raw.githubusercontent.com/AlexxChan/img-store/main/202405290936618.png)