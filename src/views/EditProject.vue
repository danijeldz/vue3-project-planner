<template>
	<h1>Edit Project {{ $route.id }}</h1>
	<form @submit.prevent="handleEdit">
		<label>Title:</label>
		<input type="text" required v-model="title" />
		<label>Details:</label>
		<textarea required v-model="details"></textarea>
		<button>Update Project</button>
	</form>
</template>

<script>
export default {
	props: ["id"],
	data() {
		return {
			title: "",
			details: "",
			uri: "http://localhost:3000/projects/" + this.id,
		};
	},
	methods: {
		handleEdit() {
			fetch(this.uri, {
				method: "PATCH",
				headers: { "Content-Type": "application/json" },
				body: JSON.stringify({
					title: this.title,
					details: this.details,
				}),
			})
				.then(() => {
					this.$router.push({ name: "Home" });
				})
				.catch((err) => {
					console.log(err.message);
				});
		},
	},
	mounted() {
		fetch(this.uri)
			.then((res) => (res = res.json()))
			.then((data) => {
				this.title = data.title;
				this.details = data.details;
			})
			.catch((err) => console.log(err.message));
	},
};
</script>

<style>
</style>