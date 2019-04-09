<template>
	
	<div>
	  <b-jumbotron>

		    <template slot="lead">
		      {{question.question}}
		    </template>

		    <hr class="my-4">


		    <b-list-group>
			  <b-list-group-item v-for="(answer,index) in answers" :key="index" @click="selectAnswer(index)" :class="[selectedAnswer === index ? 'selected' : '']">
			 {{answer}}</b-list-group-item>
			  
			</b-list-group>


		    

		    <b-button variant="primary" @click="submitAnswer">Submit</b-button>
		    <b-button @click.prevent="next" variant="success">Next</b-button>
	  </b-jumbotron>
	</div>

</template>


<script>

import _ from "lodash"
	
	export default {
		props: {
			question:Object,
			next:Function,
			increment:Function
		},

		data() {
			return {
				selectedAnswer : null,
				shuffledAnswers:[]
			}
		},

		watch:{
			question: {
				immediate:true,
				handler() {
					this.selectedAnswer = null
					this.shuffleAnswers()
				}
			}
		},

		methods: {
			selectAnswer(index) {
				this.selectedAnswer=index
				//console.log(index)
			},
			shuffleAnswers() {
				let answers = [...this.question.incorrect_answers,this.question.correct_answer]
				this.shuffledAnswers = _.shuffle(answers)
			},
			submitAnswer() {
				let isCorrect = false
				if(this.selectedAnswer === this.correctIndex) {
					isCorrect = true
				}
				this.increment(isCorrect)
			}
		},

		computed: {
			answers() {
				let answers = [...this.question.incorrect_answers]
				answers.push(this.question.correct_answer)
				return answers
			}
		}
	}
	

</script>

<style scoped>
	
	.list-group {
		margin-bottom:15px;
	}

	.btn {
		margin: 0 5px;
	}

	.list-group-item:hover {
		background:#eee;
		cursor: pointer;
	}

	.selected {
		background-color: #eee;
	}

	.correct {
		background-color: lightgreen;
	}

	.incorrect {
		background-color: lightcoral;
	}

</style>