<script lang="ts">
	import type { PageData } from './$types';

	import { ChevronLeft, Pencil, Save } from 'lucide-svelte';

	export let data: PageData;

	let disabled = true;
	let isActive = false;

	if (data.event.active == true) {
		isActive = true;
	}

	console.log(isActive);

	export let toggleEdit = () => {
		disabled = !disabled;
	};
</script>

<head>
	<title>Redbrick Treasure Hunt [🛡️] - View {data.event.name}</title>
</head>

<h1 class="title large">{data.event.name}</h1>

<div class="buttons">
	<button class="cspp" on:click={() => (window.location.href = '/admin/events')}>
		<ChevronLeft color="var(--green)" size="32" />
		Back
	</button>

	<button class="cspp" on:click={() => toggleEdit()}>
		<Pencil color="var(--green)" size="32" />
		Edit {data.event.name}
	</button>
</div>

<form method="post">
	<input type="hidden" name="id" value={data.event.id} />

	<label for="name">
		Name
		<span class="required">*</span>
	</label>
	<input type="text" name="name" id="name" value={data.event.name} {disabled} required />

	<label for="date">
		Date
		<span class="required">*</span>
	</label>
	<input type="date" name="date" id="date" value={data.event.date} {disabled} required />

	<label for="location">
		Location
		<span class="required">*</span>
	</label>
	<input
		type="text"
		name="location"
		id="location"
		value={data.event.location}
		{disabled}
		required
	/>

	<label for="description">
		Description
		<span class="required">*</span>
	</label>
	<textarea name="description" id="description" {disabled} required
		>{data.event.description}</textarea
	>

	<label for="prize">
		Prize
		<span class="required">*</span>
	</label>
	<input type="text" name="prize" id="prize" value={data.event.prize} {disabled} required />

	<label for="prizecount">
		Prize Count
		<span class="required">*</span>
	</label>
	<input
		type="number"
		name="prizecount"
		id="prizecount"
		value={data.event.prizecount}
		{disabled}
		required
	/>

	<label for="status">
		Status
		<span class="required">*</span>
	</label>
	<select name="status" id="status" {disabled} required>
		<option value="active" selected={isActive}>Active</option>
		<option value="inactive" selected={!isActive}>Inactive</option>
	</select>

	<button class="cspp" {disabled}>
		<Save color="var(--green)" size="32" />
		Save
	</button>
</form>
