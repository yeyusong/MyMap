<template>
  <div class="map">
	  <div ref='mapbox' class="mapbox"></div>
  </div>
</template>

<script>
	import echarts from 'echarts'
	import 'echarts/map/js/china.js'
	import jsonp from 'jsonp'
	const option = {
		title:{
			text:"疫情地图",
			link:"https://baidu.com",
			subtext:"叶宇松",
			sublink:"https://baidu.com"
		},
		series:[{
			name:'确诊人数',
			//渲染地图
			type:'map',
			map:'china',
			label:{
				show:true,
				color:'black',
				fontSize:10
			},
			// 控制地图的放大/缩小
			zoom:0.8,
			// 设置是否开启放大功能
			roam:true,
			data:[
				
			],
			itemStyle:{
				areaColor:'green',
				borderColor:'yellow'
			},
			emphasis:{
				// 控制鼠标滑过时的背景、字体颜色
				label:{
					
				},
				itemStyle:{
					
				}
			}
		}],
		visualMap:[{
			type:'piecewise',
			show:true,
			// splitNumber:4,
			pieces:[
				{min:10000},
				{min:1000,max:9999},
				{min:100,max:999},
				{min:10,max:99},
				{min:1,max:9}
			],
			
		}],
		tooltip:{
			trigger:'item'
		},
		toolbox:{
			show: true,
			orient: 'vertical',
			left: 'right',
			top: 'center',
			feature: {
				dataView: {readOnly: false},
				restore: {},
				saveAsImage: {}
			}
		}
	};

	
	export default {
		mounted() {
			this.getdata()
			this.mychart = echarts.init(this.$refs.mapbox)
			this.mychart.setOption(option)
			
		},
		methods:{
			getdata(){
				jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',{},(err,data)=>{
					if(!err){
						//请求数据成功了
						// console.log(data)
						let list = data.data.list.map(item=>({
							name:item.name,value:item.value
						}))
						option.series[0].data = list;
						this.mychart.setOption(option)
					}else{
						alert('接口失效了')
					}
				})
			}
		}
	}
</script>


<style scoped>
	.mapbox{
		height: 800px;
		width: 800px;
		/* background-color: red; */
	}
</style>
