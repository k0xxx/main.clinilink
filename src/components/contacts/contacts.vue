<template>
	<div id="view">
		<div class="card px-75 py-50 mb-50">
			<form class="d-flex justify-content-between">
				<input type="text" v-model="contactsSearch" v-on:keyup="refreshList" name="search" class="form_input" placeholder="Поиск...">
				<div class="d-flex align-i-baseline">
					<label for="contactType" class="mr-50">Тип контакта</label>
					<select id="contactType" v-on:change="refreshList" v-model="contactsType" class="form_input">
						<option value="">Все</option>
						<option value="0">Пациенты</option>
						<option value="1">Специалисты</option>
						<option value="3">Врачи</option>
					</select>
				</div>
			</form>
		</div>
		<div class="d-flex">
			<div id="contacts" class="withSideBar">
				<contactItem v-for="contact in contactsList" v-bind:key="contact._id" :contact="contact.contactRef" :contactStatus="contact.type" :isSubscribe="contact.subsriber" ></contactItem>
				<infinite-loading :on-infinite="onInfinite" ref="infiniteLoading">
					<span slot="no-more">Всё загружено!</span>
					<span slot="no-results">Вы не добавили еще ни одного друга!</span>
				</infinite-loading>
			</div>
			<contactsFilter></contactsFilter>
		</div>
	</div>
</template>

<script>
import { baseAPI } from '../../config.js'
import InfiniteLoading from 'vue-infinite-loading'
import contactsFilter from './filterContact.vue'
import contactItem from "./contactItem.vue"

export default {
	name:"contacts",
	components: {contactItem, InfiniteLoading, contactsFilter},
	data() {
		return {
			page: 1,
			endpoint: baseAPI + 'contacts',
			contactsList: [],
			contactsType: '',
			contactsSearch: '',
			relationType: '',
		}
	},
	methods: {
		onInfinite() {
			var options = {
				params: {page: this.page, relationType: this.relationType, contactsType: this.contactsType, contactsSearch: this.contactsSearch}
			}
			this.$http.get(this.endpoint, options).then((response) => {
				console.log(response);
				if (response.data.contactsList.length) {
					this.contactsList = this.contactsList.concat(response.data.contactsList);
					this.page++;
					this.$refs.infiniteLoading.$emit('$InfiniteLoading:loaded');
				} else {
					this.$refs.infiniteLoading.$emit('$InfiniteLoading:complete');
				}
		  });
		},
		getQuery(){
			this.relationType = this.$route.query.type;
			this.refreshList();
		},
		refreshList(){
			this.contactsList = [];
			this.page = 1;
      		this.$nextTick(() => {
        		this.$refs.infiniteLoading.$emit('$InfiniteLoading:reset');
      		});
		},
	},
	watch: {
    	'$route': 'getQuery'
  	},
}      
</script>  