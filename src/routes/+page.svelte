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
		$contactStore = [ { name: inputName, phone: inputPhone }, ...$contactStore]
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
		<h1 class="h1 m-10">Contacts</h1>
		<p>A storage persitent contacts store page</p>
		<label class="label">
			<span class="label-text">Name</span>
			<input class="input" type="text" placeholder="Name" bind:value={inputName} />
		</label>
		<label class="label">
			<span class="label-text">Phone Number</span>
			<input class="input" type="text" placeholder="Phone Number" bind:value={inputPhone} />
		</label>
		<button type="button" class="btn variant-filled" on:click={addContact}>Add contact</button>
		<hr />
		<h2 class="h2">Your contacts:</h2>
		{#each $contactStore as contact, index}
			<div class="card">
				<div class="card-body p-4 text-lg">
					<p class="card-title font-bold">{contact.name}</p>
					<p class="card-subtitle">{contact.phone}</p>
					<button type="button" class="btn btn-sm variant-filled" on:click={() => deleteContact(index)}>Delete</button>
				</div>
			</div>
		{/each}
		{#if $contactStore.length === 0}
			<p>You don't have any contacts yet</p>
		{/if}
	</div>
</div>
