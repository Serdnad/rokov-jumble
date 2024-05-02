<script lang="ts">
	import WordRow from '$lib/components/WordRow.svelte';

	let mistakes = 0;

	let words = [
		{ word: 'cat', specialLetter: 'a' },
		{ word: 'dog', specialLetter: 'd' },
		{ word: 'larp', specialLetter: 'p' }
	];

	function onMistake() {
		mistakes += 1;
	}
</script>

<main>
	<h1>Christian Rokov's Jumble Fumble</h1>

	<div class="word-grid">
		{#each words.entries() as [i, wordInfo]}
			<WordRow word={wordInfo.word} specialLetter={wordInfo.specialLetter} {onMistake} />
		{/each}
		<!-- 
		<WordRow word="cat" specialLetter="a" {onMistake} />
		<WordRow word="dog" specialLetter="d" {onMistake} />
		<WordRow word="larp" specialLetter="p" {onMistake} /> -->
	</div>

	<p>Mistakes: {mistakes}</p>
</main>

<style lang="scss">
	h1 {
		color: white;
		text-align: center;
	}

	main {
		max-width: 800px;
		margin: auto;
		padding: 16px;
	}

	:global(*) {
		font-family: Arial, Helvetica, sans-serif;
	}

	:global(body) {
		background: #333333;
	}

	.word-grid {
		display: flex;
		flex-direction: column;
		gap: 32px;
	}

	p {
		color: white;
		font-size: x-large;

		text-align: center;
	}
</style>
