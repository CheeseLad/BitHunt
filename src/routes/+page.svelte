<script lang="ts">
	import type { PageData } from './$types';
	import { DoorOpen, Crown, LogOut, LogIn, UserPlus } from 'lucide-svelte';
	import Thumb from '$lib/assets/bitthumb.svelte';

	import Toasts from '$lib/toasts/Toasts.svelte';
	import { addToast } from '$lib/toasts/store.js';

	export let data: PageData;

	let prize = '';

	if (data.status == 200) {
		if (data.event.prizecount > 1) {
			// Check if last letter is 's'
			if (data.event.prize.slice(-1) == 's') {
				prize = data.event.prize + 'es';
			} else {
				prize = data.event.prize + 's';
			}
		}
	}

	if (data.log == 'Logged out Successfully') {
		addToast({
			message: data.log,
			type: 'success',
			dismissible: true,
			timeout: 5000
		});
	}

	export let register = () => {
			window.location.href = '/register';
		},
		login = () => {
			window.location.href = '/login';
		};
</script>

<head>
	<title>Redbrick Treasure Hunt</title>
</head>

<Toasts />

<div class="container">
	<Thumb />

	<h1 class="title verylarge nogap">Redbrick Treasure Hunt</h1>

	{#if data.status == 200}
		<div class="row justify-content-center">
			<div class="col">
				<h2 class="subtitle medium nogap"><strong>Today's Event:</strong> {data.event.name}</h2>

				<h2 class="subtitle medium nogap">
					<strong>Today's Prize:</strong> One of {data.event.prizecount}
					{prize}!
				</h2>
			</div>
		</div>

		{#if data.validCookie == false}
			<div class="row justify-content-center">
				<div class="container">
					<div class="row justify-content-center">
						<div class="col-md-6">
							<button class="cspp" on:click={() => register()}>
								Register
							</button>
						</div>
						<div class="col-md-6">
							<button class="cspp" on:click={() => login()}>
								Login
							</button>
						</div>
					</div>
				</div>
			</div>
		{:else}
			<div class="row justify-content-center">
				<div class="container-fluid">
					<div class="row justify-content-center">
						<div class="col-lg-6">
							<button class="cspp" on:click={() => (window.location.href = '/stage')}>
								<DoorOpen color="var(--green)" size="32"/>
								Enter Event
							</button>
						</div>

						<div class="col-lg-6">
							<button class="cspp" on:click={() => (window.location.href = '/logout')}>
								<LogOut color="var(--green)" size="32"/>
								Logout
							</button>
						</div>
					</div>
				</div>
			</div>
		{/if}
	{:else}
		<h2 class="subtitle large nogap"><strong>No Event Today</strong></h2>
		<p class="subtitle medium nogap">Please check back later.</p>
	{/if}
</div>
