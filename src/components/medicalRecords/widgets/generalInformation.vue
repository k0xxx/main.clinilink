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
		<div v-if="loading" class="p-75 text-center text-primary">
			<icon name="refresh" scale="2" spin></icon>
		</div>
		<div v-else-if="!isFullWidget" class="p-75">
			<div v-if="generalInformationForm.activity_level" class="mb-25">
				<strong>Уровень активности:</strong>
				{{generalInformationForm.activity_level}}.
			</div>
			<div v-if="generalInformationForm.smoking_years || generalInformationForm.alcohol_years || generalInformationForm.drugs_years" class="mb-25">
				<strong>Вредные привычки:</strong>
				<div v-if="generalInformationForm.smoking_years">Курение {{generalInformationForm.smoking_years}} лет, по {{generalInformationForm.smoking_counter}} пачек в день.</div>
				<div v-if="generalInformationForm.alcohol_years">Употребление алкоголя {{generalInformationForm.alcohol_years}} лет.</div>
				<div v-if="generalInformationForm.drugs_years">Употребление наркотиков {{generalInformationForm.drugs_years}} лет.</div>
			</div>
			<div v-if="generalInformationForm.food_settings" class="mb-25">
				<strong>Питание:</strong>
				{{generalInformationForm.food_settings}}
			</div>
			<div v-if="generalInformationForm.sleep_type" class="mb-25">
				<strong>Качество сна:</strong>
				{{generalInformationForm.sleep_type}}, длительность {{generalInformationForm.sleep_duration}} часов.
			</div>
			<div v-if="generalInformationForm.stress_type" class="mb-25">
				<strong>Стрессовые ситуации:</strong>
				{{generalInformationForm.stress_type}}
			</div>
		</div>
		<div v-else-if="isFullWidget" class="p-75">
			<form v-on:submit.prevent="saveGeneralInformation">
				<div class="d-flex align-i-baseline mb-75">
					<label for="activity_level" class="mr-50">Уровень активности</label>
					<select id="activity_level" name="activity_level" v-model="generalInformationForm.activity_level" class="form_input">
						<option value="">Не выбрано</option>
						<option value="Подвижный">Подвижный</option>
						<option value="Малоподвижный">Малоподвижный</option>
						<option value="Сидячий">Сидячий</option>
						<option value="Работаю на даче">Работаю на даче</option>
					</select>
				</div>
				<div class="d-flex flex-column mb-75">
					<label for="physical_culture" class="mb-50">Физическая культура</label>
					<textarea id="physical_culture" class="form_input" name="physical_culture" v-model="generalInformationForm.physical_culture" rows="2" placeholder="Опишите ваши занятия спортом..."></textarea>
				</div>
				<div class="d-flex flex-column mb-75">
					<label for="food_settings" class="mb-50">Питание</label>
					<textarea id="food_settings" class="form_input" name="food_settings" v-model="generalInformationForm.food_settings" rows="2" placeholder="Примерное меню на день..."></textarea>
				</div>
				<div class="d-flex align-i-baseline mb-75">
					<label for="work_type" class="mr-50">Уровень тяжести работы</label>
					<select id="work_type" class="form_input" name="work_type" v-model="generalInformationForm.work_type">
						<option value="">Не выбрано</option>
						<option value="Легкий">Легкий</option>
						<option value="Средний">Средний</option>
						<option value="Тяжелый">Тяжелый</option>
					</select>
				</div>
				<div class="d-flex align-i-baseline mb-75">
					<label for="sleep_type" class="mr-50">Качество сна</label>
					<select id="sleep_type" name="sleep_type" class="form_input" v-model="generalInformationForm.sleep_type">
						<option value="">Не выбрано</option>
						<option value="Быстрое засыпание">Быстрое засыпание</option>
						<option value="Медленное засыпание">Медленное засыпание</option>
					</select>
				</div>
				<div class="d-flex align-i-baseline mb-75">
					<label for="sleep_duration" class="mr-50">Продолжительность сна</label>
					<div class="d-flex align-i-baseline">
						<input id="sleep_duration" name="sleep_duration" type="number" class="form_input" v-model="generalInformationForm.sleep_duration">
						<span class="input-group-addon" id="sleep_duration-addon">Часов в сутки</span>
					</div>
				</div>
				<div class="d-flex align-i-baseline mb-75">
					<label for="stress_type" class="mr-50">Стрессовые ситуации</label>
					<select id="stress_type" class="form_input" name="stress_type" v-model="generalInformationForm.stress_type">
						<option value="">Не выбрано</option>
						<option value="Частые">Частые</option>
						<option value="Не частые">Не частые</option>
						<option value="Редкие">Редкие</option>
						<option value="Постоянные">Постоянные</option>
					</select>
				</div>
				<div class="d-flex align-i-baseline mb-75">
					<label for="sex_life" class="mr-50">Половая жизнь</label>
					<select id="sex_life" class="form_input" name="sex_life" v-model="generalInformationForm.sex_life">
						<option value="">Не выбрано</option>
						<option value="Наличие постоянного партнера">Наличие постоянного партнера</option>
						<option value="Беспорядочные половые связи">Беспорядочные половые связи</option>
						<option value="Отсутствие половой жизни">Отсутствие половой жизни</option>
					</select>
				</div>
				<div class="d-flex align-i-baseline mb-75">
					<label for="sex_contraception" class="mr-50">Методы контрацепции</label>
					<select class="form_input" name="sex_contraception" v-model="generalInformationForm.sex_contraception">
						<option value="">Не выбрано</option>
						<option value="Презервативы">Презервативы</option>
						<option value="Гормональные средства">Гормональные средства</option>
						<option value="Спермициды">Спермициды</option>
						<option value="Без предохранения">Без предохранения</option>
						<option value="Другое">Другое</option>
					</select>
				</div>
				<div class="d-flex align-i-baseline mb-75">
					<label for="home_pets" class="mr-50">Наличие домашних животных</label>
					<select id="home_pets" class="form_input" name="home_pets" v-model="generalInformationForm.home_pets">
						<option value="">Не выбрано</option>
						<option value="Есть животные">Есть животные</option>
						<option value="Нет животных">Нет животных</option>
					</select>
				</div>

				<label for="bad_habbits" class="mb-50">Вредные привычки</label>
				<div class="card p-75">
					<div class="d-flex align-i-baseline mb-75">
						<label for="smoking_years" class="mr-50">Курение</label>
						<div class="d-flex">
							<div class="d-flex align-i-baseline mr-50">
								<input type="number" class="form_input p-0 text-center" name="smoking_years" v-model="generalInformationForm.smoking_years" id="smoking_years" min="0" max="99" maxlength="2" value="10">
								<span class="input-group-addon" id="smoking_years-addon">Лет</span>
							</div>
							<div class="d-flex align-i-baseline">
								<input type="number" class="form_input p-0 text-center" name="smoking_counter" v-model="generalInformationForm.smoking_counter" id="smoking_counter" min="0" max="99" maxlength="2" value="2">
								<span class="input-group-addon" id="smoking_counter-addon">Пачек в день</span>
							</div>
						</div>
					</div>
					<div class="d-flex align-i-baseline mb-75">
						<label for="alcohol_years" class="mr-50">Алкоголь</label>
						<div class="d-flex">
							<input type="number" class="form_input p-0 text-center" name="alcohol_years" v-model="generalInformationForm.alcohol_years" id="alcohol_years" min="0" max="99" maxlength="2" value="1">
							<span class="input-group-addon" id="alcohol_years-addon">Лет</span>
						</div>
					</div>
					<div class="d-flex align-i-baseline mb-75">
						<label for="drugs_years" class="mr-50">Наркотики</label>
						<div class="d-flex">
							<input type="number" class="form_input p-0 text-center" name="drugs_years" v-model="generalInformationForm.drugs_years" id="drugs_years" min="0" max="99" maxlength="2" value="0">
							<span class="input-group-addon" id="drugs_years-addon">Лет</span>
						</div>
					</div>
				</div>
				<button class="btn btn-primary mx-auto" type="submit">Сохранить</button>	
			</form>
		</div>
	</div>
</template>

<script>
import { baseAPI } from '../../../config';
export default {
	name: 'widgetGeneralInformation',
	data() {
		return {
			loading: true,
			endpoint: baseAPI + 'medical_records/general_information',
			item: {title: 'Общая информация', icon: 'bed', type: 'generalInformation'},
			generalInformationForm: {
				activity_level: '',
				physical_culture: '',
				food_settings: '',
				work_type: '',
				sleep_type: '',
				sleep_duration: '',
				stress_type: '',
				sex_life: '',
				sex_contraception: '',
				home_pets: '',
				smoking_years: '',
				smoking_counter: '',
				alcohol_years: '',
				drugs_years: '',
			},
		}
	},
	props: ['showWidget', 'isFullWidget'],
	methods: {
		saveGeneralInformation: function(){
			this.$http.post(this.endpoint, this.generalInformationForm).then((response) => {
				console.log(response);
			}, function(err){
				console.log(err);
			})
		},
		getGeneralInformation: function(){
			this.loading = true;
			this.$http.get(this.endpoint).then((response) => {
				this.generalInformationForm = response.data.generalInformation;
				this.loading = false;
			}, function(err){
				console.log(err);
			})
		}
	},
	created: function(){
		this.getGeneralInformation();
	},
}
</script>

<style>

</style>