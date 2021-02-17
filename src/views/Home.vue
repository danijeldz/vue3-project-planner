<template>
	<div class="home">
		<FilterNav @filterChange="current = $event" :current="current" />
		<div v-if="filteredProjects.length">
			<div v-for="project in filteredProjects" :key="project.id">
				<SingleProject
					:project="project"
					@delete="handleDelete"
					@complete="handleComplete"
				/>
			</div>
		</div>
	</div>
</template>

<script>
// @ is an alias to /src

import SingleProject from "../components/SingleProject.vue";
import FilterNav from "../components/FilterNav.vue";

export default {
	name: "Home",
	components: {
		SingleProject,
		FilterNav,
	},
	data() {
		return {
			projects: [],
			current: "all",
		};
	},
	computed: {
		filteredProjects() {
			if (this.current === "completed") {
				return this.projects.filter((project) => project.complete);
			}
			if (this.current === "ongoing") {
				return this.projects.filter((project) => !project.complete);
			}
			return this.projects;
		},
	},
	methods: {
		handleDelete(id) {
			this.projects = this.projects.filter((item) => {
				return item.id != id;
			});
		},
		handleComplete(id) {
			let p = this.projects.find((item) => {
				return item.id === id;
			});
			p.complete = !p.complete;
		},
	},
	mounted() {
		fetch("http://localhost:3000/projects")
			.then((res) => res.json())
			.then((data) => (this.projects = data))
			.catch((err) => console.log(err.message));
	},
};
</script>
