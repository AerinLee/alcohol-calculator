<script>
	import Space from "./components/Space.svelte"
	import Card from "./components/Card.svelte"
	import { fade, fly } from 'svelte/transition';
	import { sojus } from './constants/soju'
	import { beers, beer_amount } from './constants/beer'
	import Select from 'svelte-select';
	
	let show = false;
	let alcohol_type;			// 술 종류
	let bottle; 				// 몇 병 마셨는지
	let selected_brand; 		// 어떤 브랜드 술인지
	let selected_capacity; 		// 용량
	let selected_degree; 		// 도수
	let result;
	let current_amount;

	const SOJU_NORMAL = 16.5 * 360;
	const SOJU_CLASSIC = 20.1 * 360;
	const BEER_CAN_SMALL = 4.5 * 355; 
	const BEER_CAN_LARGE = 4.5 * 500; 
	const BEER_PET = 4.5 * 1600; 

	function handleMessage(event) {
		console.log(event.detail.title);
		alcohol_type = event.detail.title;
		show = true;
		result = false;
		bottle = null;
	}

	function handleSubmit() {
		current_amount = selected_capacity * selected_degree * bottle;
		result = true;
	}

	function handleSojuSelect(event) {
		result = false;
		selected_brand = event.detail.value
		selected_capacity = event.detail.capacity
		selected_degree = event.detail.degree
	}

	function handleBeerSelect(event) {
		result = false;
		selected_brand = event.detail.value
		selected_degree = event.detail.degree
	}

	function handleBeerAmountSelect(event) {
		result = false;
		selected_capacity = event.detail.value
	}

	

</script>

<main>
	<h1>술 계산기</h1>
	<p class="message" on:click="{() => {show=false}}">술 종류를 선택하세요.</p>
	<Space h_value="1"></Space>
	<div class="row">
		<Card on:message={handleMessage} title="소주" emoji="🍾"></Card>
		<Card on:message={handleMessage} title="맥주" emoji= "🍺"></Card>
		<Card on:message={handleMessage} title="세계맥주" emoji= "🍻"></Card>
		<Card on:message={handleMessage} title="막걸리" emoji= "🍚"></Card>
		<Card on:message={handleMessage} title="직접입력" emoji= "🍷"></Card>
	</div>
	<Space h_value="4"></Space>
	{#if show}
		<div in:fly={{y: -100, duration: 1000}} out:fade={{duration : 1000}}>
			<p class="message" on:click="{() => {show=false}}">어떤 술을 얼만큼 마셨나요?</p>
			<form class="input-form" on:submit|preventDefault={handleSubmit}>
				{#if alcohol_type === '소주'}
					<div class="select-item">
						<Select items={sojus} placeholder="소주 이름 검색" on:select={handleSojuSelect}></Select>
					</div>
				{:else if alcohol_type === '맥주'}
					<div class="select-item">
						<Select items={beers} placeholder="맥주 이름 검색" on:select={handleBeerSelect}></Select>
					</div>
					<div class="select-item">
						<Select items={beer_amount} placeholder="용량 선택" on:select={handleBeerAmountSelect}></Select>
						
					</div>
				{:else if alcohol_type === "직접입력"}
					<input class="bottle" type=number bind:value={selected_degree} min="0" placeholder="도수(%)">
					<input class="bottle" type=number bind:value={selected_capacity} min="0" placeholder="용량(ml)">
				{/if}

				{#if selected_brand === '기타'}
					<input class="bottle" type=number bind:value={selected_degree} min="0" placeholder="도수(%)">
					<input class="bottle" type=number bind:value={selected_capacity} min="0" placeholder="용량(ml)">
				{/if}
				<input class="bottle" type=number step=0.1 bind:value={bottle} min="0" placeholder="몇 병/캔?">
				<span class="measure">(병/캔)</span>
			
				<button class="calc_btn" disabled={!bottle} type=submit>계산하기</button>
			</form>
		</div>
		<Space h_value="1"></Space>
		{#if selected_brand && selected_brand != '기타'}
			<p>{selected_brand}의 도수는 <strong>{selected_degree}%</strong> 입니다.</p>
		{/if}
		{#if selected_brand && bottle}
			<p>총 <strong>{Math.round(selected_capacity * bottle)}ml</strong>의 
				{alcohol_type === '직접입력' ? '술' : alcohol_type}을 마셨네요...</p>
		{/if}

	{/if}

	<Space h_value="4" />

	{#if result}
	<div  in:fly={{y: -100, duration: 1000}} out:fly={{y: -100, duration : 500}}>
		<p class="message" on:click="{() => {show=false}}">당신이 마신 술은...</p>
		
		<div>
			<p>일반 소주 <strong>{(current_amount / SOJU_NORMAL).toFixed(2)}</strong>병</p>
			<p>참이슬 클래식 <strong>{(current_amount / SOJU_CLASSIC).toFixed(2)}</strong>병</p>
			<p>맥주 작은 캔 <strong>{(current_amount / BEER_CAN_SMALL).toFixed(2)}</strong>캔</p>
			<p>맥주 큰 캔 <strong>{(current_amount / BEER_CAN_LARGE).toFixed(2)}</strong>캔</p>
			<p>맥주 피쳐 <strong>{(current_amount / BEER_PET).toFixed(2)}</strong>병</p>
		</div>

		<p class="message">과 같습니다.</p>
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

	.select-item {
		width: 15rem;
		font-family: 'GowunBatang-Bold';
		--height: 3rem;
		text-align: left;
		margin: 0 0 0 0.5rem
	}

	.input-form {
		display: flex;
		justify-content: center;
	}

	.measure {
		align-self: center;
		font-size: 1.4rem;
		margin: 0 1rem 0 0;
		
	}

	.bottle {
		height: 3rem;
		padding: 0 0.5rem;
		margin: 0 0 0 1rem;
		width: 8rem;
		font-size: 14px;
		font-weight: bold;
	}

	.calc_btn {
		width: 6rem;
		font-size: 1rem;
		color: black;
		border: 2px solid rgb(46 125 50);
		border-radius: 0.4rem;
		background-color: white;
	}

	.calc_btn:hover {
		background-color: rgb(46 125 50 / 50%);
		color: white;
	}

	strong {
		font-weight: bold;
	}


	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}


</style>