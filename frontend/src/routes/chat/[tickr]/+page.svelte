<script lang="ts">
	import { enhance } from '$app/forms';
	import Button from '$lib/components/ui/button/button.svelte';
	import Input from '$lib/components/ui/input/input.svelte';
	import type { ActionData } from '../$types.js';

	export let data;

	type Message = {
		user: string;
		content: string;
	};

	let messageHistory: Message[] = [];

	const appendMessage = (user_type: string, message: string) => {
		messageHistory = [...messageHistory, { user: user_type, content: message }];
	};

	let currentMessage: string;
	export let form: ActionData;

	$: {
		if (form) {
			appendMessage('Assistant', form.llm_response);
		}
	}
</script>

<main class="w-full h-dvh">
	<h1 class="text-3xl font-bold pt-3 text-center">
		Chatting about TICKR: {data.tickr.toUpperCase()}
	</h1>
	<section class="flex w-full h-[calc(100%-116px)] justify-center">
		<ul class="w-4/5 overflow-y-scroll">
			{#each messageHistory as { user, content }}
				<li class="max-w-full flex justify-start p-3">
					<section class="flex flex-col justify-end">
						<span class="font-bold text-left">{user} {user == 'You' ? '👤' : '🤖'}</span>
						<span>{content}</span>
					</section>
				</li>
			{/each}
		</ul>
	</section>
	<form
		method="POST"
		class="flex justify-center pb-7"
		on:submit={() => {
			appendMessage('You', currentMessage);
		}}
		use:enhance
	>
		<div class="flex w-4/5">
			<Input name="query" placeholder="Message assistant" bind:value={currentMessage} />
			<Button type="submit">
				<svg
					width="24"
					height="24"
					viewBox="0 0 24 24"
					fill="none"
					class="text-white dark:text-black"
					><path
						d="M7 11L12 6L17 11M12 18V7"
						stroke="currentColor"
						stroke-width="2"
						stroke-linecap="round"
						stroke-linejoin="round"
					></path></svg
				>
			</Button>
		</div>
	</form>
</main>
