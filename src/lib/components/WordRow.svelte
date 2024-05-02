<script lang="ts">
	export let word: string;
	export let specialLetter: string;
	export let onMistake: () => void;

	let guessStatus: 'guessing' | 'correct' | 'incorrect' = 'guessing';

	export let guesses: string[] = [];

	let container: HTMLDivElement;

	function onChangeText(e: Event) {
		// check if textfield was just backspaced
		if ((e.target as HTMLInputElement).value === '') {
			(e.target as HTMLElement).previousSibling?.focus();
			return;
		}

		(e.target as HTMLElement).nextSibling?.focus();
	}

	$: {
		if (guesses.length === word.length && guesses.every((letter) => letter !== '')) {
			const guess = guesses.join('');
			if (guess.toUpperCase() === word.toUpperCase()) {
				guessStatus = 'correct';

				console.log('win');
			} else {
				onMistake();
				guessStatus = 'incorrect';

				container.childNodes.forEach((element) => {
					element.style.animation = 'jiggle 0.5s infinite';
				});

				setTimeout(() => {
					container.childNodes.forEach((element) => {
						element.style.animation = '';
					});
				}, 1000);
				console.log('lose');
			}
		}
	}
</script>

<div class="container">
	<p>{word.split('').join(' ')}</p>

	<div bind:this={container} class="box-row">
		{#each word.split('').entries() as [i, letter]}
			<input
				class="{guessStatus === 'correct' ? 'correct' : ''} {specialLetter === letter
					? 'special'
					: ''}"
				bind:value={guesses[i]}
				on:input={onChangeText}
				disabled={guessStatus === 'correct'}
				maxlength="1"
			/>
		{/each}
	</div>
</div>

<style lang="scss">
	@keyframes -global-jiggle {
		0% {
			transform: rotate(0deg);
		}
		25% {
			transform: rotate(5deg);
		}
		50% {
			transform: rotate(-5deg);
		}
		75% {
			transform: rotate(5deg);
		}
		100% {
			transform: rotate(0deg);
		}
	}

	.container {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: space-between;
		gap: 8px;
	}

	p {
		text-transform: uppercase;
		font-size: xx-large;
		color: white;

		margin: 0;
	}

	.box-row {
		display: flex;
		gap: 8px;

		input {
			margin: 0;
			padding: 0;

			&.correct {
				&.special {
					background: green;
					background: orange;
				}

				background: green;
				border: 3px solid darkgreen;
			}

			text-align: center;
			text-transform: uppercase;

			background: none;
			width: 40px;
			height: 40px;

			border: 3px solid white;
			border-radius: 4px;

			font-size: xx-large;
			color: white;
		}
	}
</style>
