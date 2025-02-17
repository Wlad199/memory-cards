<template>
	<div class="add-card">
		<h1 class="add-card__title"></h1>
		<div class="add-card__actions">
			<button @click="openNewCardForm" class="action__button button">
				<Icon icon="ic:baseline-close" />
				<span>Cancel</span>
			</button>
			<button @click="sendData" class="action__button button">
				<Icon icon="ic:round-add" />
				<span>Create Card</span>
			</button>
		</div>
		<div class="questions">
			<label class="questions__question">
				<p>Question</p>
				<textarea v-model="newQuestion" placeholder="Enter question..."></textarea>
			</label>
			<label class="questions__answer">
				<p>Answer</p>
				<textarea v-model="newAnswer" placeholder="Enter Answer..."></textarea>
			</label>
		</div>
	</div>
</template>

<script setup>
import { Icon } from '@iconify/vue'
import { ref } from 'vue';

defineProps({
	openNewCardForm: Function
})

const emit = defineEmits(['sendDataForm'])

let newQuestion = ref('')
let newAnswer = ref('')

const sendData = () => {
	emit('sendDataForm', { newQuestion, newAnswer })
	newQuestion.value = ''
	newAnswer.value = ''
}

</script>

<style scoped  lang='scss'>
.add-card {
	&__title {}

	&__actions {
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 20px;
		margin-bottom: 20px;
	}

	@media (max-width: 550px) {
		width: 300px;
		margin: 0 auto;
		display: block;
	}
}

.questions {
	p {
		font-size: 22px;
		margin-bottom: 10px;
	}

	&__question {
		display: block;
		margin-bottom: 20px;
	}

	&__answer {}

	textarea {
		border: 1px solid #aaa;
		border-radius: 3px;
		font-size: 16px;
		padding: 12px;
		width: 500px;
		resize: vertical;
		font-size: 18px;

		@media (max-width: 550px) {
			width: 300px;
		}
	}

}
</style>