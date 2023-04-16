<script>
	import { afterUpdate, tick } from 'svelte';
	import Header from './Header.svelte';
	import './styles.css';
	import { fade } from 'svelte/transition';
	import { v4 as uuid } from 'uuid';

	let px = 100;
	let inputValue = ''; // declare a variable to store the input value
	let chatAssistant = 'Welcome to Bot Chat'
	let scrollDiv 
	let footerdiv
	let botMessages = [
		{
			botid: uuid(),
			bottitle: chatAssistant,
			botdesc: 'bot'
		}
	];

	let userMessages = [
		{
			id: uuid(),
			title: inputValue,
			desc: 'user'
		}
	];

	function handleInput(event) {
		inputValue = event.target.value; // update the variable whenever the input value changes
		console.log('input', inputValue)
	}

	function sendRequest() {
		const api_key = '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'
		const options = {
			method: 'POST',
			headers: {
				Authorization: `Bearer ${api_key}`,
				'Content-Type': 'application/json',
			},
			body: JSON.stringify({
				model: "gpt-3.5-turbo",
				messages: [{role: "user", content: inputValue}]
			})
		} 
		postRequest(options)
	}

	async function postRequest(options) {
		const response = await fetch('https://api.openai.com/v1/chat/completions', options);
		const data = await response.json();

		console.log('res::', data.choices[0].message.content)

		chatAssistant = data.choices[0].message.content
		px += 200

		updateMessages()
		
		return {
			status: 200,
			content: data.choices[0].message.content,
			user: `${data.choices[0].message.role} bot`
		};

	}

	function updateMessages() {
		botMessages = [
			...botMessages,
			{
				botid: uuid(),
				bottitle: chatAssistant,
				botdesc: 'bot'
			}
		];

		userMessages = [
			...userMessages,
			{
				id: uuid(),
				title: inputValue,
				desc: 'user'
			}
		];
		inputValue = ''
		
	}

	afterUpdate(async () => {
		console.log('scroll', scrollDiv)
		
		await tick()
		console.log('ticked', scrollDiv)

		scrollDiv.scrollIntoView(false)

	})
	
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
	<link href="https://cdn.jsdelivr.net/npm/daisyui@2.51.5/dist/full.css" rel="stylesheet" type="text/css" />
</svelte:head>

<div class="app">
	<Header bind:this={footerdiv} />
</div>

<div bind:this={scrollDiv}>
	<ul>
		{#each botMessages as { botid, bottitle, botdesc }, j (botid)}
			{#each userMessages as { id, title, desc }, i (id)}
	
				{#if i === j} 
				{#key id}
				{#if title &&  desc === 'user'}
				<li style="width: 100px">
					<div transition:fade={{duration: 1000}} class="chat chat-end">
						<div class="chat-image avatar">
						<div class="w-10 rounded-full">
							<!-- svelte-ignore a11y-missing-attribute -->
							<!-- <img src="/images/stock/photo-1534528741775-53994a69daeb.jpg" /> -->
						</div>
						</div>
						<div class="chat-header">
						Darth User
						<time class="text-xs opacity-50">12:46</time>
						</div>
						<div class="chat-bubble">{title}</div>
						<div class="chat-footer opacity-50">
						Seen at 12:46
						</div>
					</div>
				</li>
				{/if}
			{/key}
	
	
			{#key botid}
				<li style="width: 100px">
					<div transition:fade={{duration: 500}} class="chat chat-start">
						<div class="chat-image avatar">
						<div class="w-10 rounded-full">
							<!-- svelte-ignore a11y-missing-attribute -->
							<!-- <img src="/images/stock/photo-1534528741775-53994a69daeb.jpg" /> -->
						</div>
						</div>
						<div class="chat-header">
						AnakinSky Bot
						<time class="text-xs opacity-50">12:45</time>
						</div>
						<div class="chat-bubble">{bottitle}</div>
						<div class="chat-footer opacity-50">
						Greetings earthling...
						</div>
					</div>
				</li>
			{/key}
			{/if}
			{/each}
		{/each}
	</ul>
</div>

<div class="footer-div">
	<footer>
		<div class="form-control">
			<div class="input-group">
			  <input bind:value={inputValue} on:input={handleInput} type="text" placeholder="ask the robot..." class="input input-bordered" style="width: 800px"  />
			  <button class="btn btn-square" on:click={sendRequest}>
				<!-- <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" /></svg> -->
				<svg height="70%" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
					<path fill="none" stroke="#000000" stroke-width="2" d="M18.3482396,15.9535197 C18.7664592,15.0561341 19,14.0553403 19,13 C19,9.13400675 15.8659932,6 12,6 C8.13400675,6 5,9.13400675 5,13 C5,14.1167756 5.2615228,15.1724692 5.72666673,16.1091793 L5.72666673,16.1091793 M12,3 C12.5522847,3 13,2.55228475 13,2 C13,1.44771525 12.5522847,1 12,1 C11.4477153,1 11,1.44771525 11,2 C11,2.55228475 11.4477153,3 12,3 Z M12,23 C12.5522847,23 13,22.5522847 13,22 C13,21.4477153 12.5522847,21 12,21 C11.4477153,21 11,21.4477153 11,22 C11,22.5522847 11.4477153,23 12,23 Z M12,6 L12,3 M9,14 C9.55228475,14 10,13.5522847 10,13 C10,12.4477153 9.55228475,12 9,12 C8.44771525,12 8,12.4477153 8,13 C8,13.5522847 8.44771525,14 9,14 Z M15,14 C15.5522847,14 16,13.5522847 16,13 C16,12.4477153 15.5522847,12 15,12 C14.4477153,12 14,12.4477153 14,13 C14,13.5522847 14.4477153,14 15,14 Z M6,18.9876876 L5,16 C5,16 5.07242747,15.2283988 5.5,15.5 C6.43069361,16.0911921 8.57396448,17 12,17 C15.5536669,17 17.6181635,16.0844828 18.5,15.5 C18.8589052,15.262117 19,16 19,16 L18,18.9876876 C18,18.9876876 17.0049249,20.9999997 12,21 C6.99507512,21.0000003 6,18.9876876 6,18.9876876 Z"/>
				  </svg>
			</button>
			</div>
		  </div>
	  </footer>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		position: sticky;
		top: 0;
		z-index: 100;
		background: #f3f4fa;
		
		/* background: radial-gradient( 50% 50% at 50% 50%, rgba(255, 255, 255, 0.75) 0%, rgba(255, 255, 255, 0) 100% ), linear-gradient(180deg, var(--color-bg-0) 0%, var(--color-bg-1) 15%, var(--color-bg-2) 50%); */
	}
	.footer-div {
		background: #ecf0f8;
		position: sticky;
		bottom: 0px;
	    /* bottom: 600px; */
		margin-top: 50%;
		/* margin-bottom: 50%; */
	}
	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 12px;
	}
</style>
