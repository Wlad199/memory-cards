<template>
	<div class="action">
		<button @click="clearCards" class="action__button button">
			<Icon icon="mdi:clear-octagon-outline" />
			<span>Clear Cards</span>
		</button>
		<button @click="openNewCardForm" class="action__button button">
			<Icon icon="ic:round-add" />
			<span>Add New Card</span>
		</button>
	</div>

	<ul v-if="cardsData.length > 0" class="cards">
		<li v-for="card in cardsData " @click="card.isShown = !card.isShown" class="card"
			:class="{ active: card.isActive, 'show-answer': card.isShown, left: card.isPrevious }" :key="card.id">
			<div class="card__content">
				<div class="card__front">
					<p>
						{{ card.question }}
					</p>
				</div>
				<div class="card__back">
					<p>
						{{ card.answer }}
					</p>
				</div>
			</div>
		</li>
	</ul>

	<div v-if="cardsData.length > 0" class="navigation">
		<button @click="showPreviousCard" class="navigation__button">
			<Icon icon="solar:arrow-left-outline" />
		</button>
		<span class="current">{{ currentActiveCard + 1 }} / {{ cardsData.length }}</span>
		<button @click="showNextCard" class="navigation__button">
			<Icon icon="solar:arrow-right-outline" />
		</button>
	</div>
</template>

<script setup>
import { Icon } from '@iconify/vue';

defineProps({
	cardsData: Array,
	showNextCard: Function,
	showPreviousCard: Function,
	currentActiveCard: Number,
	openNewCardForm: Function,
	clearCards: Function
})


</script>

<style scoped  lang='scss'>
// action =================//
.action {
	display: flex;
	justify-content: center;
	align-items: center;
	gap: 20px;
	margin-bottom: 20px;

	@media (max-width: 550px) {
		font-size: 16px;
		gap: 10px;
	}

	&__button {
		.iconify--mdi {
			font-size: 24px;
			color: #000;
		}

		.iconify--ic {
			font-size: 24px;
			color: #000;
		}
	}
}


//! card =================//

.cards {
	position: relative;
	height: 300px;
	width: 500px;
	max-width: 100%;
	margin-bottom: 20px;
	user-select: none;

	@media (max-width: 550px) {
		width: 300px;
		margin: 0 auto;
		margin-bottom: 10px;
	}
}

.card {
	position: absolute;
	opacity: 0;
	font-size: 24px;
	top: 0;
	left: 0;
	height: 100%;
	width: 100%;
	transform: translateX(50%) rotateY(-10deg);
	transition: transform 0.4s ease, opacity 0.4s ease;

	@media (max-width: 550px) {
		font-size: 18px;
	}

	&.active {
		cursor: pointer;
		opacity: 1;
		z-index: 10;
		transform: translateX(0) rotateY(0deg);
	}

	&.left {
		transform: translateX(-50%) rotateY(10deg);
	}

	&__content {
		box-shadow: 0 1px 10px rgba(0, 0, 0, 0.3);
		border-radius: 4px;
		height: 100%;
		width: 100%;
		position: relative;
		transform-style: preserve-3d;
		transition: transform 0.4s ease;
		text-align: center;
		line-height: 110%;
	}


	&__front,
	&__back {
		backface-visibility: hidden;
		position: absolute;
		top: 0;
		left: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 100%;
		width: 100%;
		background: #fff;
		transition: all 0.3s ease 0s;
		padding: 10px;
	}

	&__front {
		transform: rotateX(0deg);
		z-index: 2;
	}

	&__back {
		transform: rotateX(180deg);
	}
}


.card.show-answer .card__content {
	transform: rotateX(180deg);
}

// navigation =================//
.navigation {
	position: relative;
	z-index: 10;
	display: flex;
	justify-content: center;
	align-items: center;
	gap: 10px;
	font-size: 30px;
	height: 40px;

	span {
		font-size: 24px;
	}

	&__button {
		height: 100%;
		transition: all 0.2s ease 0s;

		&:hover {
			transform: scale(1.1);
		}
	}

	.iconify--solar {
		height: 100%;
	}
}
</style>