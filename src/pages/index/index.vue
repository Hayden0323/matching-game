<template>
	<view class="content" :style="{minHeight: windowHeight + 'px'}">
		<view class="header">
			<span> Matching Game </span>
		</view>
		<view class="main">
			<view class="gameBoard">
				<Card v-for="(item, index) in randomSymbols" 
							:key="index" 
							:margin="((windowWidth - (48 * 4)) / (5 * 2)) + 'px'" 
							width="48px"
							height="48px"
							fontSize="36px" 
							:title="item" 
							cover="❓" 
							:isShow="isOpen[index]"
							@card-click="cardClickHandler(index)">
				</Card>
			</view>
		</view>
		<view class="footer">
			<span> {{
				isEnded
					? `Congrats! You have completed in ${steps} steps.`
					: `You have tried ${steps} times.`
			}} </span>
			<Card v-if="isEnded"
						margin="5px"
						width="120px"
						height="25px"
						fontSize="15px" 
						title="Try Again" 
						cover="❓" 
						:isShow=true
						@card-click="initGame">
			</Card>
		</view>
	</view>
</template>

<script>
	import Card from '../../components/Card.vue'
	export default {
		components: {
			Card,
		},
		data() {
			return {
				windowWidth: 0,
				windowHeight: 0,
				cardSymbols: ["🐱", "🐽", "🎃", "🌎", "👗", "👠", "👕" ,"⚽️"],
				randomSymbols: [],
				isOpen: [],
				firstPickIndex: null,
				secondPickIndex: null,
				steps: 0,
				isEnded: false
			}
		},
		onLoad() {
			this.getWindowInfo();
			this.getRandomSymbols()
		},
		methods: {
			getWindowInfo () {
				const { windowWidth, windowHeight } = uni.getSystemInfoSync();
				this.windowHeight = windowHeight
				this.windowWidth = windowWidth
			},
			
			getRandomSymbols () {
				let newArray = [...this.cardSymbols, ...this.cardSymbols];
				let len = newArray.length;
				for (let i = 0; i < len; i++) {
					let randomIndex = Math.floor(Math.random() * len)
					let tmp = newArray[i];
					newArray[i] = newArray[randomIndex];
					newArray[randomIndex] = tmp;
				}
				for (let i = 0; i < newArray.length; i++) {
					this.isOpen.push(false);
				}
				this.randomSymbols = newArray;
			},
			
			cardClickHandler (index) {
				let isOpen = [...this.isOpen];
				
				if (isOpen[index] == true) return;
				if (this.firstPickIndex !== null && this.secondPickIndex !== null) return;
				
				isOpen[index] = true;
				if (this.firstPickIndex == null && this.secondPickIndex == null) {
					this.isOpen = isOpen;
					this.firstPickIndex = index;
				} else if (this.firstPickIndex !== null && this.secondPickIndex == null) {
					this.isOpen = isOpen;
					this.secondPickIndex = index;
				}
				
				this.steps++;
				this.calculateGameResult();
			},
			
			calculateGameResult () {
				if (this.firstPickIndex !== null && this.secondPickIndex !== null) {
					
					if (this.isOpen.filter(v => v == false).length == 0) {
						this.isEnded = true;
					}
					
					let firstSymbol = this.randomSymbols[this.firstPickIndex];
					let secondSymbol = this.randomSymbols[this.secondPickIndex];
					
					if (firstSymbol !== secondSymbol) {
						// inCorrect
						let isOpen = [...this.isOpen];
						setTimeout(() => {
							isOpen[this.firstPickIndex] = false;
							isOpen[this.secondPickIndex] = false;
							this.isOpen = isOpen;
							this.firstPickIndex = null;
							this.secondPickIndex = null;
						}, 500)
					} else {
						// correct
						this.firstPickIndex = null;
						this.secondPickIndex = null;
					}
				}
			},
			
			initGame () {
				this.isOpen = [];
				this.steps = 0;
				this.isEnded = false;
				this.getRandomSymbols();
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		justify-content: center;
		background-color: yellow;
	}
	.header {
		display: flex;
		flex:1;
		justify-content: center;
		align-items: center;
		background-color: #eee;
	}
	.header span {
		font-weight: bold;
		font-size: 36px;
	}
	.main {
		display: flex;
		flex:3;
		background-color: #fff;
	}
	.footer {
		display: flex;
		flex:1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		background-color: #eee;
	}
	.footer span {
		font-size: 15px;
	}
	.gameBoard {
		display: flex;
		flex: 1;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: center;
		align-content: center;
	}

</style>
