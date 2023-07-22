<script lang="ts">
	import { localStorageStore } from '@skeletonlabs/skeleton'
	import type { Writable } from 'svelte/store'

	interface Contacts {
		name: string
		phone: string
	}

	const contactStore: Writable<Contacts[]> = localStorageStore('contacts', [])

	let inputName = ''
	let inputPhone = ''

	function addContact() {
		$contactStore = [{ name: inputName, phone: inputPhone }, ...$contactStore]
		inputName = ''
		inputPhone = ''
	}

	function deleteContact(index: number) {
		contactStore.update((contacts) => {
			contacts.splice(index, 1)
			return contacts
		})
	}
</script>

<div class="container mx-auto flex justify-center items-center">
	<div class="space-y-5">
		<h1 class="h1 m-10 text-center">Contacts</h1>
		<p class="text-center">A storage persitent contacts store page</p>
		<label class="label">
			<input class="input" type="text" placeholder="Name" bind:value={inputName} />
		</label>
		<label class="label">
			<input class="input" type="text" placeholder="Phone Number" bind:value={inputPhone} />
		</label>
		<div class="flex justify-center">
			<button type="button" class="btn variant-filled" on:click={addContact}>Add contact</button>
		</div>
		<hr />
		<h2 class="h2 text-center">Your contacts:</h2>
		{#each $contactStore as contact, index}
			<div class="card flex justify-between place-items-center">
				<div class="card-body p-4 text-lg">
					<p class="card-title font-bold">{contact.name}</p>
					<p class="card-subtitle">{contact.phone}</p>
				</div>
				<button
					type="button"
					class="btn btn-lg variant-filled m-2 p-6 h-6"
					on:click={() => deleteContact(index)}>Delete</button
				>
			</div>
		{/each}
		{#if $contactStore.length === 0}
			<p class="text-center">You don't have any contacts yet</p>
		{/if}
	</div>
</div>
