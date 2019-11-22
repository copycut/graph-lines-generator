<template>
	<div class="app">
		<div class="demo">
			<ul class="entries">
				<li class="entry" v-for="entry in entries" :key="entry.id">
					{{ entry.title }}
				</li>
			</ul>
			<Graph :size="entries.length" />
			<div class="circle">{{ entries.length }}</div>
		</div>
		<div class="controls">
			<Control @new-entry="addEntry" />
			<ul class="list-of-entries">
				<li class="entry-details" v-for="entry in entries" :key="entry.id">
					<span class="title">{{ entry.title }}</span>
					<span class="remove" @click="removeEntry(entry.id)">×</span>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
import Graph from "./components/graph.vue";
import Control from "./components/control.vue";

export default {
	name: "app",
	components: { Graph, Control },
	data: function() {
		return {
			entries: []
		};
	},
	methods: {
		addEntry(title) {
			this.entries.push({ id: Math.random(), title });
		},
		removeEntry(id) {
			this.entries = this.entries.filter(entry => entry.id !== id);
		}
	}
};
</script>

<style>
html,
body {
	padding: 0;
	margin: 0;
	min-height: 100vh;
}
</style>

<style scoped>
.app {
	font-family: sans-serif;
	font-size: 16px;
	color: #2f3542;
	background-color: #ced6e0;
	display: flex;
	align-items: center;
	justify-content: center;
	height: 100vh;
}

.demo {
	display: flex;
	flex-direction: row;
	align-items: center;
	justify-content: center;
}

.circle {
	display: flex;
	align-items: center;
	justify-content: center;
	background-color: #3742fa;
	height: 80px;
	width: 80px;
	border-radius: 40px;
	font-size: 1.5rem;
	color: #ced6e0;
}

.entries {
	display: flex;
	flex-direction: column;
	padding: 0 16px;
	margin: 0;
	list-style: none;
	align-items: flex-end;
	align-self: stretch;
	flex-grow: 1;
	justify-content: space-between;
}

.entry {
	display: flex;
	font-weight: bold;
	color: #3742fa;
}

.entry:first-child {
	transform: translateY(-50%);
}

.entry:last-child {
	transform: translateY(50%);
}

.entry:first-child:only-child {
	transform: translateY(0);
	height: 100%;
	align-items: center;
}

.controls {
	padding: 16px;
	border-radius: 4px;
	background-color: #dfe4ea;
	margin-left: 16px;
}

.list-of-entries {
	padding: 0;
	margin: 16px 0 0;
	max-height: 150px;
	overflow: auto;
	background-color: #ced6e0;
	border-radius: 4px;
}

.list-of-entries:empty {
	margin: 0;
}

.entry-details {
	display: flex;
	flex-flow: row nowrap;
	align-items: center;
	justify-content: space-between;
}

.title {
	padding: 8px;
}

.remove {
	content: "x";
	padding: 8px;
	color: #ff4757;
	font-weight: bold;
	cursor: pointer;
}

.remove:hover,
.remove:active {
	background-color: #ff4757;
	color: #f1f2f6;
	border-radius: 4px;
}
</style>
