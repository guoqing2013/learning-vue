1. 实例
	new Vue({
		el: '#app',
		template: '<div>{fruit}</div>'
		data: {
			fruit: 'apple',
		}
	});

2. 组件
	// 全局组件
	Vue.component('my-component', {

	});

	//局部组件
	var myHeader = {
		template: '<p>hello hello world</p>'
		data: function() { 
			return {
				a: 1,
				b: 2
			}
		}
	};

	new Vue({
		el: '#app',
		components: {
			'my-header': myHeader
		}
	});


3. vue基本概念

	全局api
	实例选项
	实例属性/方法
	指令
	内置组件
