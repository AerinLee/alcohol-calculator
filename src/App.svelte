<script>
	import Space from "./components/Space.svelte"
	import Card from "./components/Card.svelte"
	import { fade, fly } from 'svelte/transition';
	
	let show = false;

	function handleMessage(event) {
		console.log(event.detail.title);
		show = true
	}

</script>

<main>
	<h1>술 계산기</h1>
	<p class="message" on:click="{() => {show=false}}">술 종류를 선택하세요.</p>
	<Space value="1"></Space>
	<div class="row">
		<Card on:message={handleMessage} title="소주" emoji="🍾"></Card>
		<Card on:message={handleMessage} title="맥주" emoji= "🍺"></Card>
		<Card on:message={handleMessage} title="세계맥주" emoji= "🍻"></Card>
		<Card on:message={handleMessage} title="막걸리" emoji= "🍚"></Card>
		<Card on:message={handleMessage} title="직접입력" emoji= "🍷"></Card>
	</div>
	<Space value="4"></Space>
	{#if show}
	<div class="row" in:fly={{ y: -100, duration: 1000 }} out:fly={{ y: -100, duration: 1000 }}>
		<Card on:message={handleMessage} title="소주" emoji="🍾"></Card>
		<Card on:message={handleMessage} title="맥주" emoji= "🍺"></Card>
		<Card on:message={handleMessage} title="막걸리" emoji= "🍚"></Card>
		<Card on:message={handleMessage} title="직접입력" emoji= "🍷"></Card>
	</div>
	{/if}

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #2E9639;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		font-family: 'GowunBatang-Bold'
	}

	.message {
		font-size: 1.6rem;
	}

	.row {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}


</style>