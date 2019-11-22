<template>
	<form class="control" @submit.prevent="newEntry">
		<input
			class="input"
			type="text"
			v-model="value"
			placeholder="Type someting and hit enter key"
		/>
	</form>
</template>

<script>
export default {
	data() {
		return {
			value: ""
		};
	},
	methods: {
		newEntry() {
			if (this.value === "") {
				return;
			}

			this.$emit("new-entry", this.value);
			this.$nextTick(() => {
				this.value = "";
			});
		}
	}
};
</script>

<style scoped>
.input {
	font-family: sans-serif;
	font-size: 16px;
	color: #2f3542;
	background-color: #ced6e0;
	border: 1px solid #ced6e0;
	padding: 8px;
}

.input:focus {
	outline: none;
	border: 1px solid #3742fa;
}
</style>
