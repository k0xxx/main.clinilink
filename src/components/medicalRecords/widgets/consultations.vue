<template>
	<div class="medicalRecordItem card">
		<div class="title">
			<a href="#" v-if="isFullWidget" @click.prevent="$emit('toogle')">
				<span><icon name="arrow-left" class="mr-50"></icon>Назад</span>
			</a>
			<a href="#" @click.prevent="$emit('toogle')">
				<span><icon :name=item.icon class="mr-50"></icon>{{item.title}}</span>
			</a>
			<button class="btn btn-primary" v-on:click="showModal = true"><icon name="plus"></icon></button>
		</div>
		<div class="measurementItemGraph p-75">
			<div v-if="loading" class="p-75 text-center text-primary">
				<icon name="refresh" scale="2" spin></icon>
			</div>
			<div class="text-center pt-100">
				<span class="text-primary">Записей нет</span>
				<button class="btn btn-outline-primary btn-small mx-auto mt-50" v-on:click="showModal = true">
					Добавить новую запись<icon name="plus" class="ml-50"></icon>
				</button>
			</div>
		</div>
		<div v-if="isFullWidget" class="p-75">
			<table>
				<tr>		
					<th>Дата</th>
					<th>Систолическое</th>
					<th>Диастолическое</th>
					<th>Пульс</th>
					<th>Аритмия</th>
					<th>Примечание</th>
					<th></th>
				</tr>
				<tr v-for="measurement in measurementsList">
					<td>{{measurement.date | formatMeasurement}}</td>
					<td>{{measurement.bloodpressure.systolic}}</td>
					<td>{{measurement.bloodpressure.diastolic}}</td>
					<td>{{measurement.bloodpressure.pulse}}</td>
					<td>{{measurement.bloodpressure.arrhythmia}}</td>
					<td>{{measurement.bloodpressure.note}}</td>
					<td>Edit</td>
				</tr>
			</table>
		</div>
		<div v-if="showModal" class="modal">
			<transition name="modal">
				<div class="modal-mask">
					<div class="modal-wrapper">
						<div class="modal-container">
							<div class="modal-header">
								<h3 class="px-100">Добавить значение</h3>
								<a href="#" class="close-icon" v-on:click.prevent="showModal = false">
									<icon name="times" scale="1"></icon> 
								</a>
							</div>
							<div class="modal-body">
								<div class="p-100">
									<form v-on:submit.prevent="addMeasurement">
										<input type="text" name="date" class="form-control text-center" v-model="bloodpressureForm.date">
										<span class="input-group-addon"><i class="fa fa-calendar"></i></span>
										<label for="systolic">Систола</label>
										<input type="number" name="systolic" placeholder="120" class="form-control text-center" min="0" max="400" v-model="bloodpressureForm.systolic" required="">
										<div class="col-2 text-center py-1">/</div>
										<label for="diastolic">Диастола</label>
										<input type="number" name="diastolic" placeholder="80" class="form-control text-center" min="0" max="400" v-model="bloodpressureForm.diastolic" required="">
										<label for="pulse">Пульс</label>
										<input type="number" name="pulse" placeholder="80" class="form-control text-center" min="0" max="400" v-model="bloodpressureForm.pulse" required="">
										<span class="input-group-addon">уд/м</span>
										<label for="arrhythmia">Аритмия</label>
										<select class="form-control text-center" name="arrhythmia" v-model="bloodpressureForm.arrhythmia">
											<option value="Нет" selected="">Нет</option>
											<option value="Обнаружена">Обнаружена</option>
										</select>
										<label for="note">Примечание</label>
										<textarea class="form-control" name="note" rows="1" v-model="bloodpressureForm.note"></textarea>
										<button type="submit" class="btn-primary btn-xlarge mt-100">Добавить</button>
									</form>
								</div>
							</div>
						</div>
					</div>
				</div>
			</transition>
		</div>
	</div>
</template>

<script>
import { baseAPI } from '../../../config';
export default {
	name: 'widgetWeight',
	data() {
		return {
			loading: true,
			endpoint: baseAPI + 'medical_records/',
			item: {title: 'Консультации', icon: 'bed', type: 'consultations'},
			showModal: false,
			medical_recordsList: [],
			bloodpressureForm: {
				date: '',
				systolic: '',
				diastolic: '',
				pulse: '',
				arrhythmia: '',
				note: '',
				type: 'consultations',
			},
		}
	},
	props: ['showWidget', 'isFullWidget'],
	methods: {
		addMedicalRecord: function(){
			this.$http.put(this.endpoint + this.item.type, this.allergiesForm).then((response) => {
				console.log(response);
				//this.bloodpressureForm.date = '';
				this.showModal = false;
				this.medical_recordsList.push(response.data.medicalRecord);
			}, function(err){
				console.log(err);
			})
		},
		getMedicalRecords: function(){
			this.loading = true;
			this.$http.get(this.endpoint + this.item.type).then((response) => {
				console.log(response);
				this.medical_recordsList = response.data.medical_recordsList;
				this.loading = false;
			}, function(err){
				console.log(err);
			})
		}
	},
	created: function(){
		this.getMedicalRecords();
	},
}
</script>

<style>
</style>