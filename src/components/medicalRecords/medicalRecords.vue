<template>
	<div id="view">
		<div id="medicalRecords" v-bind:class="{fullWidget: isFullWidget}">
			<widgetConsultations v-if="activeWidgets.wConsultations.show" :showWidget="activeWidgets.wConsultations.show" :isFullWidget="activeWidgets.wConsultations.full" @toogle="toogle('wConsultations')"></widgetConsultations>
			<widgetAnalyzes v-if="activeWidgets.wAnalyzes.show" :showWidget="activeWidgets.wAnalyzes.show" :isFullWidget="activeWidgets.wAnalyzes.full" @toogle="toogle('wAnalyzes')"></widgetAnalyzes>
			<widgetInjuries v-if="activeWidgets.wInjuries.show" :showWidget="activeWidgets.wInjuries.show" :isFullWidget="activeWidgets.wInjuries.full" @toogle="toogle('wInjuries')"></widgetInjuries>

			<widgetDiseases v-if="activeWidgets.wDiseases.show" :showWidget="activeWidgets.wDiseases.show" :isFullWidget="activeWidgets.wDiseases.full" @toogle="toogle('wDiseases')"></widgetDiseases>
			<widgetOperations v-if="activeWidgets.wOperations.show" :showWidget="activeWidgets.wOperations.show" :isFullWidget="activeWidgets.wOperations.full" @toogle="toogle('wOperations')"></widgetOperations>
			<widgetGeneralInformation v-if="activeWidgets.wGeneralInformation.show" :showWidget="activeWidgets.wGeneralInformation.show" :isFullWidget="activeWidgets.wGeneralInformation.full" @toogle="toogle('wGeneralInformation')"></widgetGeneralInformation>
			
			<widgetAllergies v-if="activeWidgets.wAllergies.show" :showWidget="activeWidgets.wAllergies.show" :isFullWidget="activeWidgets.wAllergies.full" @toogle="toogle('wAllergies')"></widgetAllergies>
			<widgetImmunizations v-if="activeWidgets.wImmunizations.show" :showWidget="activeWidgets.wImmunizations.show" :isFullWidget="activeWidgets.wImmunizations.full" @toogle="toogle('wImmunizations')"></widgetImmunizations>
			<widgetMedicaments v-if="activeWidgets.wMedicaments.show" :showWidget="activeWidgets.wMedicaments.show" :isFullWidget="activeWidgets.wMedicaments.full" @toogle="toogle('wMedicaments')"></widgetMedicaments>	
			
			
		</div>
	</div>
</template>

<script>
import widgetDiseases from './widgets/diseases.vue';
import widgetConsultations from './widgets/consultations.vue';
import widgetAllergies from './widgets/allergies.vue';
import widgetImmunizations from './widgets/immunizations.vue';
import widgetMedicaments from './widgets/medicaments.vue';
import widgetInjuries from './widgets/injuries.vue';
import widgetOperations from './widgets/operations.vue';
import widgetAnalyzes from './widgets/analyzes.vue';
import widgetGeneralInformation from './widgets/generalInformation.vue';
export default {
	name: 'medicalRecords',
	components: {
		widgetConsultations,
		widgetAllergies,
		widgetDiseases,
		widgetImmunizations,
		widgetMedicaments,
		widgetInjuries,
		widgetOperations,
		widgetAnalyzes,
		widgetGeneralInformation
	},
	data() {
		return {
			activeWidgets: {
				wConsultations: {full: false, show: true},
				wDiseases: {full: false, show: true},
				wAllergies: {full: false, show: true},
				wImmunizations: {full: false, show: true},
				wMedicaments: {full: false, show: true},
				wInjuries: {full: false, show: true},
				wOperations: {full: false, show: true},
				wAnalyzes: {full: false, show: true},
				wGeneralInformation: {full: false, show: true},
			},
			isFullWidget: false,
		}
	},
	methods: {
		toogle: function(el){
			console.log(this.activeWidgets[el].full);
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
#medicalRecords{
	column-count: 3;
}
#medicalRecords.fullWidget{
	column-count: 1;
}
.medicalRecordItem{
	display: inline-block;
    width: 100%;
	min-height: 280px;
	font-size: 14px;
}
.medicalRecordItem .title{
	display: flex;
    justify-content: space-between;
    font-size: 1.3rem;
	border-bottom: 1px solid #ddd;
	align-items: center;
	padding: 0.75rem 0.75rem 0.25rem;
}
.medicalRecordList-item{
	margin-bottom: 0.5rem;
    border-bottom: 1px solid #ddd;
    padding-bottom: 0.25rem;
}
/*.medicalRecordList{

}

.medicalRecordList-item:last-child{
	border-bottom: none;
}*/
</style>