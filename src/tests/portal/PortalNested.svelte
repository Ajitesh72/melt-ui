<script lang="ts" context="module">
	import Dialog from './Dialog.svelte';
	import Popover from './Popover.svelte';
	import Select from './Select.svelte';

	const components = {
		dialog: Dialog,
		popover: Popover,
		select: Select,
	} as const;

	export type Structure = {
		name: keyof typeof components;
		children?: Structure[];
	};
	export const structure: Structure = {
		name: 'dialog',
		children: [
			{
				name: 'dialog',
				children: [
					{
						name: 'select',
					},
				],
			},
			{
				name: 'popover',
				children: [{ name: 'dialog' }, { name: 'popover' }],
			},
			{ name: 'select' },
		],
	};
</script>

<script lang="ts">
	export let cmp: Structure = structure;

	$: resolvedCmp = components[cmp.name];
	export let isRoot = true;
</script>

{#if isRoot}
	<main>
		<svelte:component this={resolvedCmp}>
			{#each cmp.children ?? [] as child}
				<svelte:self cmp={child} isRoot={false} />
			{/each}
		</svelte:component>
	</main>
{:else}
	<svelte:component this={resolvedCmp}>
		{#each cmp.children ?? [] as child}
			<svelte:self cmp={child} isRoot={false} />
		{/each}
	</svelte:component>
{/if}
