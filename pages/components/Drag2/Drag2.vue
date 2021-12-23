<template>
	<view style="margin-top: 50px;">
		<prettyChange :list="list" @change="change"></prettyChange>
	</view>
</template>

<script>
	import prettyChange from './pretty-exchange/pretty-exchange.vue'
	export default {
		components:{
			prettyChange
		},
		data() {
			return {
				list:[{
					name:'小张',
					src:'https://s.cn.bing.net/th?id=ODL.8acd6796e431785e9623a48ef791095b&w=146&h=146&c=7&rs=1&qlt=80&pid=RichNav',
					color:'#E5716A'
				},{
					name:'小q',
					color:'#D3D240',
					src:'https://tse1-mm.cn.bing.net/th?id=OIP.OzTCtonjrdsbnZ8tbeYM4AAAAA&w=150&h=160&c=8&rs=1&qlt=90&pid=3.1&rm=2'
				},{
					name:'小星',
					color:'#89CB5B',
					src:'https://s.cn.bing.net/th?id=ODL.7b61652172fdf6eee5d579d645e13bdb&w=146&h=146&c=7&rs=1&qlt=80&pid=RichNav'
				},{
					name:'小王',
					color:'#78DCAE',
					src:'https://tse2-mm.cn.bing.net/th/id/OIP.fPtV12SSVlxTIlLoqm5F2wAAAA?w=176&h=180&c=7&o=5&pid=1.7'
				},{
					name:'小宝',
					color:'#6EABE7',
					src:'https://tse1-mm.cn.bing.net/th/id/OIP.pWnNuLzgsLmnXQj_2w2YzAAAAA?w=180&h=180&c=7&o=5&pid=1.7'
				}]
			}
		},
		onLoad() {
			
		},
		methods: {
			change(e) {
				console.log(e)
			}
		}
	}
</script>

<style>
	
</style>
