<template>
	<view>
		<view :id="id" :change:id="echarts.loadId" :class="className" :style="{height:height,width:width}" :student="student" :change:student="echarts.loadStudent"/>
	</view>
</template>

<script>
	export default {
		name: 'chart',
		props: {
		    className: {
		      type: String,
		      default: 'chart'
		    },
		    id: {
		      type: String,
		      default: 'chart'
		    },
		    width: {
		      type: String,
		      default: '300px'
		    },
		    height: {
		      type: String,
		      default: '500px'
		    },
			student: {
			  type: Array
			}
		}
	}
	
</script>

<script module="echarts" lang="renderjs">
	import * as echarts from 'echarts'
	
	// import * as echarts from 'echarts/core';
	// import {
	//   TitleComponent,
	//   TooltipComponent,
	//   GridComponent,
	//   LegendComponent
	// } from 'echarts/components';
	// import { BarChart } from 'echarts/charts';
	// import { CanvasRenderer } from 'echarts/renderers';
	
	// echarts.use([
	//   TitleComponent,
	//   TooltipComponent,
	//   GridComponent,
	//   LegendComponent,
	//   BarChart,
	//   CanvasRenderer
	// ]);
	
	export default {
		data() {
			return {
				chart: null,
				student: [],
				id: ''
			}
		},
		mounted() {
			this.initChart()
		},
		beforeDestroy() {
		    if (!this.chart) {
		      return
		    }
		    this.chart.dispose()
		    this.chart = null
		  },
		methods: {
			loadId(newValue, oldValue, ownerInstance, instance) {
				this.id = newValue
			},
			loadStudent(newValue, oldValue, ownerInstance, instance) {
				this.student = newValue
			},
			initChart() {
				const yAxisData = []
				for (let i = 0; i < this.student.length; i++) {
					yAxisData.push(this.student[i].real_name)
				}
				var mainContainer = document.getElementById(this.id)
				
				this.chart = echarts.init(mainContainer)
				var that = this
				window.onresize = function() {
				    resizeMainContainer();
				    that.chart.resize();
				}
				var option = {
				    tooltip: {
				        trigger: 'axis',
				        axisPointer: {
				        type: 'shadow'
				        }
				    },
				    legend: {
				        data: ['课时完成率', '答疑次数',  '签到次数', /*'测验完成率', '测验正确率',*/ '考试完成率', '考试正确率'],
						top: '5',
						x: 'center',
						width: 350,
				    },
				    grid: {
						top: '13%',
				        left: '3%',
				        right: '9%',
				        bottom: '3%',
				        containLabel: true
				    },
					xAxis: [{
					    type: 'value',
						show: false,
						splitLine: {
							show: false
						}
					}],
				    yAxis: [{
				        type: 'category',
						axisLabel: {
						    textStyle: {
						        color: 'dimgrey'
						    }
						},
						axisLine: {
							lineStyle: {
								color: '#FF7F50'
							}
						},
				        data: yAxisData
				    }],
				    series: [
						{
							name: '课时完成率',
							type: 'bar',
							barGap: 0,
							itemStyle: {
								color: '#fda085'
							},
							emphasis: {
								focus: 'series'
							},
							data: []
						}, 
						{
							name: '答疑次数',
							type: 'bar',
							barGap: 0,
							itemStyle: {
								color: '#fbb17a'
							},
							emphasis: {
								focus: 'series'
							},
							data: []
				        },
						{
							name: '签到次数',
							type: 'bar',
							barGap: 0,
							itemStyle: {
								color: '#f6d365'
							},
							emphasis: {
								focus: 'series'
							},
							data: []
						},
						/*{
							name: '测验完成率',
							type: 'bar',
							barGap: 0,
							itemStyle: {
								color: '#fddb92'
							},
							emphasis: {
								focus: 'series'
							},
							data: []
						},
						{
							name: '测验正确率',
							type: 'bar',
							barGap: 0,
							itemStyle: {
								color: '#eee6b6'
							},
							emphasis: {
								focus: 'series'
							},
							data: []
						},*/
						{
							name: '考试完成率',
							type: 'bar',
							barGap: 0,
							itemStyle: {
								color: '#e0f2db'
							},
							emphasis: {
								focus: 'series'
							},
							data: []
						},
						{
							name: '考试正确率',
							type: 'bar',
							barGap: 0,
							itemStyle: {
								color: '#d1fdff'
							},
							emphasis: {
								focus: 'series'
							},
							data: []
						}
					]
				}
				for (let i = 0; i < this.student.length; i++) {
					option.series[0].data.push(this.student[i].study_progress)
					option.series[1].data.push(this.student[i].qanumber)
					option.series[2].data.push(this.student[i].signinnumber)
					option.series[3].data.push(this.student[i].exam_pass)
					option.series[4].data.push(this.student[i].exam_correct)
				}
				this.chart.setOption(option)
			}
		}
	}
</script>

<style>
	
</style>