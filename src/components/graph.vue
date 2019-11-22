<template>
	<svg
		xmlns="http://www.w3.org/2000/svg"
		:viewBox="viewBox"
		class="graph"
		:style="svgStyle"
	>
		<g v-for="index in size" :key="index" fill="none">
			<circle class="dot" cx="0" :cy="circleY(index)" :r="dotSize"></circle>
			<path class="curve" :d="path(index)"></path>
		</g>
	</svg>
</template>

<script>
export default {
	name: "graph",
	props: {
		size: {
			type: Number,
			default: () => 0,
			validator: size => size >= 0
		}
	},
	computed: {
		height() {
			const { size } = this.getEdges();
			const height = 25 * size;
			return height < 80 ? 80 : height;
		},
		width: () => 80,
		dotSize: () => 4,
		viewBox() {
			return `0 0 ${this.width} ${this.height}`;
		},
		svgStyle() {
			return {
				height: `${this.height}px`,
				width: `${this.width}px`
			};
		}
	},
	methods: {
		getEdges(index = 0) {
			let size = this.size;

			/* If there is one only point, use the 3 points case with the second position */
			if (this.size < 2) {
				size = 3;
				index = 0;
			}

			return { size, index };
		},
		getPositionY(index, size) {
			const positionPercentage = Math.abs((index - 1) / (size - 1)) * 100;
			return (this.height * positionPercentage) / 100;
		},
		path(originalIndex) {
			const { size, index } = this.getEdges(originalIndex);
			const startPointY = this.getPositionY(index, size);
			const middlePointY = this.height / 2;
			const line = {
				start: {
					x: 0,
					y: startPointY
				},
				end: {
					x: this.width,
					y: middlePointY
				}
			};
			const curve = {
				start: {
					x: this.width / 2,
					y: startPointY
				},
				end: {
					x: this.width / 2,
					y: middlePointY
				}
			};
			const lineStart = `M${line.start.x}, ${line.start.y}`;
			const lineEnd = `${line.end.x}, ${line.end.y}`;
			const curveStart = `C${curve.start.x}, ${curve.start.y}`;
			const curveEnd = `${curve.end.x}, ${curve.end.y}`;

			return `${lineStart} ${curveStart} ${curveEnd} ${lineEnd}`;
		},
		circleY(originalIndex) {
			const { size, index } = this.getEdges(originalIndex);
			return this.getPositionY(index, size);
		}
	}
};
</script>

<style scoped>
.graph {
	overflow: visible;
}
.dot {
	fill: #3742fa;
}

.curve {
	stroke-width: 2;
	stroke-linejoin: round;
	stroke: #3742fa;
}
</style>
