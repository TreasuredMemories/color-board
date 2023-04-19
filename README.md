### 概述

color-board是一个**全端**支持，免费的，开源的取色工具。您可以基于此进行二次开发，样式升级，功能拓展等。

- 基于Color Ui样式库
- 基于Uniapp

### 特点

- 即开即用
- 体积小，功能较丰富，代码简单

### github

https://github.com/TreasuredMemories/color-board

### 插件市场
https://ext.dcloud.net.cn/plugin?id=11879

### 部署

App.vue引入样式文件

```vue
<style>
	/*每个页面公共css */
	@import url("@/static/icon/icon.css");
	@import url("@/static/css/main.css");
</style>
```

简单导入部署即可，如下：

```vue
<template>
	<view>
		<colorBoard></colorBoard>
	</view>
</template>

<script>
	import colorBoard from '@/components/color-board/color-board'
	export default {
		components:{
			colorBoard
		},
		data() {
			return {
				
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
```

> 注：部分样式文件引用自ColorUi组件库，请在static目录下查看,

### 预览



<img src="http://static.notesbook.site/file/img/2023/04/19/9b21c049-4afe-4688-b975-51d30069b3de-X1.jpg" alt="notesbook_board" style="zoom: 67%;" />

<img src="http://static.notesbook.site/file/img/2023/04/19/ffaaea4e-1006-4b06-84d2-06c006a3fec1-X1.jpg" alt="notesbook_board" style="zoom:67%;" />*

### 真机预览

![gh_dc85a6943c6a_258-removebg-preview](http://static.notesbook.site/file/img/2023/04/19/7ed1c6a3-b592-4f37-9107-61432db61213-X1.jpg)

### 其它推荐

微信小程序**notesbook**中体验



<img src="http://static.notesbook.site/file/img/2023/04/19/a891e260-072d-4af6-a5e1-60b2fdc05bb8-X1.jpg" alt="微信图片_20230419202147" style="zoom:25%;" /><img src="http://static.notesbook.site/file/img/2023/04/19/02e4f2c4-5205-4697-9d36-84845dff67a6-X1.jpg" alt="微信图片_20230419202139" style="zoom: 25%;" />



