<template lang="html">

	<section class="bar-chart">
		<div class="chart" :id="id"></div>
	</section>

</template>

<script>
import * as d3 from 'd3';
import cloud from 'd3-cloud';

	export default {
		name: 'barChart',
		props: {
			widthSize: {
				type: String
			},
			heightSize: {
				type: String
			},
			id: {
				type: String
			},
			data: {
				type: Array
			},
			marginLeft: {
				type: String
			},
			marginTop: {
				type: String
			},
			marginRight: {
				type: String
			},
			marginBottom: {
				type: String
			}
		},
		mounted () {
			this.drawChart();
		},
		data () {
			return {

			}
		},
		methods: {
			drawChart: function() {

				const element = "#" + this.id;
				var info = this.data;

				var margin = { left: Number(this.marginLeft), top: Number(this.marginTop), right: Number(this.marginRight), bottom: Number(this.marginBottom) };

				var wS = Number(this.widthSize);
				var hS = Number(this.heightSize);

				var w = wS - margin.left - margin.right;
				var h = hS - margin.top - margin.bottom;
				var allData;

					info.forEach(function(d) {

						d.resident = +d.resident;

					});

					allData = info;

					// var arrayLength = allData.length;

					var yMax = d3.max(allData, function(d) {

						return d.resident;

					});

					var yScale = d3.scaleLinear()
						.domain([0, yMax])
						.range([h, 0]);

					var xScale = d3.scaleBand()
						.domain(allData.map(function(d) {

							return d.country;

						}))
						.rangeRound([0, w])
						.padding(0.1);

					var svg = d3.select(element)
						.append("svg")
						.attr("width", w + margin.left + margin.right)
						.attr("height", h + margin.top + margin.bottom)
						.append("g")
						.attr("transform", "translate(" + margin.left + "," + margin.top + ")");

					svg.selectAll("rect")
						.data(allData)
						.enter()
						.append("rect")
						.attr("x", function(d, i) {

							return xScale(d.country);

						})
						.attr("y", function(d) {

							return yScale(0);

						})
						.attr("width", xScale.bandwidth())
						.attr("height", function(d) {

							return h - yScale(0);

						})
						.attr("fill", "teal")
						.attr("class", "bar");

					// Animation
					svg.selectAll("rect")
						.transition()
						.duration(800)
						.attr("y", function(d) {

							return yScale(d.resident);

						})
						.attr("height", function(d) {

							return h - yScale(d.resident);

						})
						.delay(function(d,i){
							return(i*100)
						});

					// X-Axis
					var xAxis = d3.axisBottom(xScale);

					svg.append("g")
						.attr("class", "x axis")
						.attr("transform", "translate(0," + h + ")")
						.call(xAxis)
						.selectAll("text")
						.style("text-anchor", "end")
						.attr("dx", "-.8em")
						.attr("dy", ".15em")
						.attr("transform", function(d) {

							return "rotate(-60)"

						});

					// Y-Axis
					var yAxis = d3.axisLeft(yScale);

					svg.append("g")
					.attr("class", "y axis")
						.call(yAxis);

			}
		},
		computed: {

		}
}


</script>

<style scoped lang="scss">
	.bar-chart {

	}
</style>
