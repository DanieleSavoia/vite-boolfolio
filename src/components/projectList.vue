<script>
import axios from 'axios';
export default {
	data() {
		return {
			arrProject: [],
			currentPage: 1,
			nPages: 0,
		};
	},
	methods: {
		changePage(page) {
			this.currentPage = page;
			this.getProject();
		},
		getProject() {
			axios
				.get('http://localhost:8000/api/Project', {
					params: {
						page: this.currentPage,
					},
				})
				.then(response => {
					this.arrProject = response.data.data;
					this.nPages = response.data.last_page;
				});
		},
	},
	created() {
		// richiesta dati al server
		axios
			.get('http://localhost:8000/api/projects', {
				params: {
					page: this.currentPage,
				},
			})
			.then(response => {
				this.arrProject = response.data.data;
				this.nPages = response.data.last_page;
			});
	},
};
</script>

<template>
    <div class="bg-dark-subtle">
	    <h2 class="d-flex justify-content-center mb-5">
		    These are our projects
	    </h2>
	        <ul class="d-flex justify-content-around list-unstyled">
		        <li class="border border-danger p-2" v-for="project in arrProject" :key="project.id">
		            <h3 class="d-flex justify-content-center">
				        {{ project.title }}
			        </h3>
			        <div class="d-flex justify-content-center">
				        <img class="img-thumbnail" src="{{ $project->url_image }}" alt="" style="width: 150px;">
			        </div>
			        <div class="d-flex justify-content-center">
			        	{{ project.creation_date }}
			        </div>
		        </li>
	        </ul>

	    <nav class="d-flex justify-content-center mt-5">
		    <ul class="pagination">
			    <li class="page-item disabled">
				    <a class="page-link">Previous</a>
			    </li>
			    <li
				    v-for="page in nPages"
				    :key="page"
				    class="page-item"
				    :class="{ active: page == currentPage }"
			    >
				    <span class="page-link" @click="changePage(page)">
					    {{ page }}
				    </span>
			    </li>

			    <li class="page-item">
				    <a class="page-link" href="#">Next</a>
			    </li>
		    </ul>
	    </nav>
    </div>	
</template>

<style></style>