<template>
	<div class="container">
		<h1>Memory Cards</h1>
		<CardsList v-if="!showCreateCard" :cardsData="cardsData" :showNextCard="showNextCard"
			:showPreviousCard="showPreviousCard" :currentActiveCard="currentActiveCard" :openNewCardForm="openNewCardForm"
			:clearCards="clearCards" />
		<AddNewCard :openNewCardForm="openNewCardForm" v-else @sendDataForm="addNewCard" />
	</div>
</template>

<script setup>
import { nanoid } from 'nanoid'
import { ref } from 'vue';
import CardsList from './components/CardsList.vue';
import AddNewCard from "./components/AddNewCard.vue";
import { onMounted } from 'vue';


onMounted(() => {
	const savedCards = JSON.parse(localStorage.getItem('cards'))
	if (savedCards) {
		cardsData.value = savedCards
	}
})

const saveCardsToLocalStorage = () => {
	localStorage.setItem('cards', JSON.stringify(cardsData.value))
}

const cardsData = ref([
	//{
	//	question: 'What must a variable begin with?',
	//	answer: 'A letter, $ or _',
	//	id: 1,
	//	isActive: true,
	//	isShown: false,
	//	isPrevious: false
	//},
	//{
	//	question: 'What is a variable?',
	//	answer: 'Container for a piece of data',
	//	id: 2,
	//	isActive: false,
	//	isShown: false,
	//	isPrevious: false
	//},
	//{
	//	question: 'Example of Case Sensitive Variable',
	//	answer: 'thisIsAVariable',
	//	id: 3,
	//	isActive: false,
	//	isShown: false,
	//	isPrevious: false
	//}
])

let currentActiveCard = ref(0)

let showCreateCard = ref(false)

const openNewCardForm = () => {
	showCreateCard.value = !showCreateCard.value
}

const showNextCard = () => {
	if (currentActiveCard.value < cardsData.value.length - 1) {
		currentActiveCard.value = currentActiveCard.value + 1
		cardsData.value[currentActiveCard.value].isActive = true
		cardsData.value[currentActiveCard.value - 1].isActive = false
		cardsData.value[currentActiveCard.value - 1].isShown = false
		cardsData.value[currentActiveCard.value - 1].isPrevious = true
	}
}
const showPreviousCard = () => {
	if (currentActiveCard.value > 0) {
		currentActiveCard.value = currentActiveCard.value - 1
		cardsData.value[currentActiveCard.value].isActive = true
		cardsData.value[currentActiveCard.value + 1].isActive = false
		cardsData.value[currentActiveCard.value].isPrevious = false
	}
}

const addNewCard = ({ newQuestion, newAnswer }) => {
	if (newQuestion.value && newAnswer.value) {
		cardsData.value.push({
			question: newQuestion.value,
			answer: newAnswer.value,
			id: nanoid(),
			isActive: cardsData.value.length === 0 ? true : false,
			isShown: false,
			isPrevious: false,
		})
		openNewCardForm()
		saveCardsToLocalStorage()
	}
}

const clearCards = () => {
	cardsData.value = []
	saveCardsToLocalStorage()
}
</script>
