<template>
	<div id="view" class="noSideBar">
		<div id="measurements" v-bind:class="{fullWidget: isFullWidget}">
			<widgetWeight v-if="activeWidgets.wWeight.show" :showWidget="activeWidgets.wWeight.show" :isFullWidget="activeWidgets.wWeight.full" @toogle="toogle('wWeight')"></widgetWeight>
			<widgetGlucose v-if="activeWidgets.wGlucose.show" :showWidget="activeWidgets.wGlucose.show" :isFullWidget="activeWidgets.wGlucose.full" @toogle="toogle('wGlucose')"></widgetGlucose>
			<widgetBloodpressure v-if="activeWidgets.wBloodpressure.show" :showWidget="activeWidgets.wBloodpressure.show" :isFullWidget="activeWidgets.wBloodpressure.full" @toogle="toogle('wBloodpressure')"></widgetBloodpressure>
			<widgetCholesterol v-if="activeWidgets.wCholesterol.show" :showWidget="activeWidgets.wCholesterol.show" :isFullWidget="activeWidgets.wCholesterol.full" @toogle="toogle('wCholesterol')"></widgetCholesterol>
			<widgetHeight v-if="activeWidgets.wHeight.show" :showWidget="activeWidgets.wHeight.show" :isFullWidget="activeWidgets.wHeight.full" @toogle="toogle('wHeight')"></widgetHeight>
		</div>
	</div>
</template>

<script>
import widgetWeight from './widgets/weight.vue';
import widgetGlucose from './widgets/glucose.vue';
import widgetBloodpressure from './widgets/bloodpressure.vue';
import widgetCholesterol from './widgets/cholesterol.vue';
import widgetHeight from './widgets/height.vue';

export default {
	name: 'measurements',
	data() {
		return {
			activeWidgets: {
				wWeight: {full: false, show: true},
				wGlucose: {full: false, show: true},
				wBloodpressure: {full: false, show: true},
				wCholesterol: {full: false, show: true},
				wHeight: {full: false, show: true},
			},
			isFullWidget: false,
		}
	},
	components: {
		widgetWeight,
		widgetGlucose,
		widgetBloodpressure,
		widgetCholesterol,
		widgetHeight,
	},
	methods: {
		toogle: function(el){
			//console.log(this.activeWidgets[el].full);
			if(!this.activeWidgets[el].full){
				this.isFullWidget = true;
				for(var widget in this.activeWidgets){
					this.activeWidgets[widget].show = false;
				}
				this.activeWidgets[el].show = true;
				this.activeWidgets[el].full = true;
			}else{
				this.isFullWidget = false;
				for(var widget in this.activeWidgets){
					this.activeWidgets[widget].show = true;
				}
				this.activeWidgets[el].full = false;
			}
		}
	}
}
</script>

<style>
#measurements{
	column-count: 3;
}
#measurements.fullWidget{
	column-count: 1;
}
.measurementItem{
	display: inline-block;
    width: 100%;
}
.measurementItem .title{
	display: flex;
    justify-content: space-between;
    font-size: 1.3rem;
	border-bottom: 1px solid #ddd;
	align-items: center;
	padding: 0.75rem 0.75rem 0.25rem;
}
table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
}
td, th {
    border: 1px solid #dddddd;
    text-align: left;
	padding: 8px;
}
tr:nth-child(even) {
    background-color: #f5f5f5;
}
td.editBtns{
	padding: 0;
    text-align: center;
    width: 84px;
}
td.editBtns a{
	display: inline-block;
	padding: 8px 14px;
}
td.editBtns a:hover{
	background: #ddd;
}
td.editBtns a:hover.remove{
	color: #f50057;
}
</style>