<template>
	<div
		class="menu-view"
		v-show="isMenuVisible"
	>

	<img v-if="isFirstGame"
		class="menu-view__logo"
		src="../assets/images/logo.png" alt=""
		@load="logoLoaded"
	/>

	<GameSummary v-else />
	
	<p class="menu-view__highscore">Highscore: {{ highscore }}</p>

	<div class="menu-view__options">

		<MenuOption 
			label="Sounds"
			math="1 * 1"
			:condition="audioEnabled"
		/>

		<MenuOption 
			:label="'Type 2 * 2 to ' + (isFirstGame ? 'Begin' : 'Retry')"
			single-line
			important
		/>

		<MenuOption 
			label="Music"
			math="3 * 3"
			:condition="musicEnabled"
		/>

	</div>
	
	<TheInput
		:fields="menuInput"
		class="menu-view__input"
		style="margin-top: 20px"
	/>
		
	<footer class="menu-view__about">
		© 2018 Grzegorz Dzięgelewski. Version {{ version }}
	</footer>

	</div>
</template>

<script>
	import TheInput from '@/components/TheInput';
	import MenuOption from '@/components/MenuOption';
	import GameSummary from '@/components/GameSummary';
	import { mapState, mapGetters } from 'vuex';

	export default {
		name: 'MenuView',
		components: {
			TheInput,
			MenuOption,
			GameSummary,
		},

		data() {
			return {
				isLogoLoaded: false,
			};
		},

		computed: {
			...mapState([
				'menuInput',
				'audioEnabled',
				'musicEnabled',
				'highscore',
			]),

			...mapGetters([
				'isFirstGame',
			]),

			isMenuVisible() {
				return this.isLogoLoaded || !this.isFirstGame;
			},

			version() {
				return VERSION;
			},
		},

		methods: {
			logoLoaded() {
				this.isLogoLoaded = true;
			},
		},

	};
</script>

<style lang='scss' scoped>
	@import 'src/assets/styles/shared';
	.menu-view {
		text-align: center;
		z-index: 3;

		&__logo {
			width: 100%;
			max-width: 600px;
			text-align: center;
			margin: 5% auto;
		}

		&__highscore {
			@extend %small-font;
			margin: 0 0 4%;
			color: $score-color;
			font-weight: bold;
		}

		&__options {
			display: flex;
			justify-content: space-around;
		}

		&__about {
			display: none;
			@include screen {
				display: block;
				position: absolute;
				right: 10px;
				bottom: 0;
				color: #AAA;
				font-size: 10px;
			}
		}
	}

	
</style>