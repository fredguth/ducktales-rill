<script lang="ts">
	import { Table as TableIcon } from "lucide-svelte/icons" ;
    import CaretSort from "svelte-radix/CaretSort.svelte";
	import Check from "svelte-radix/Check.svelte";
	import PlusCircled from "svelte-radix/PlusCircled.svelte";

	import { tick } from "svelte";
	import { cn } from "$lib/utils.js";
	import * as Avatar from "$lib/components/ui/avatar/index.js";
	import { Button } from "$lib/components/ui/button/index.js";
	import * as Command from "$lib/components/ui/command/index.js";
	import * as Dialog from "$lib/components/ui/dialog/index.js";
	import { Input } from "$lib/components/ui/input/index.js";
	import { Label } from "$lib/components/ui/label/index.js";
	import * as Popover from "$lib/components/ui/popover/index.js";
	import * as Select from "$lib/components/ui/select/index.js";

	let className: string | undefined | null = undefined;
	export { className as class };


    const tables = [
        {
            label: "Table 1",
            comment: "table1 bla bal bla",
        },
        {
            label: "Table 2",
            comment: "table2 bla bal bla",
        },
    ];

    
	let open = false;

    
    let selectedTable = tables[0];

    export let selected = selectedTable.label;
    
    $: selected = selectedTable.label;

	function closeAndRefocusTrigger(triggerId: string) {
		open = false;

		tick().then(() => document.getElementById(triggerId)?.focus());
	}
</script>


	<Popover.Root bind:open let:ids>
		<Popover.Trigger asChild let:builder>
			<Button
				builders={[builder]}
				variant="outline"
				role="combobox"
				aria-expanded={open}
				aria-label="Select a team"
				class={cn("w-[200px] justify-between", className)}
			>
				<TableIcon class="mr-2 h-5 w-5" />
					
				{selectedTable.label}
				<CaretSort class="ml-auto h-4 w-4 shrink-0 opacity-50" />
			</Button>
		</Popover.Trigger>
		<Popover.Content class="w-[500px] p-0">
			<Command.Root>
				<Command.Input placeholder="Procure tabela..." />
				<Command.List>
					<Command.Empty>Não encontrada.</Command.Empty>
					{#each tables as table}
                                <Command.Item
                                onSelect={() => {
                                    selectedTable = table;
                                    closeAndRefocusTrigger(ids.trigger);
                                }}
                                value={table.label}
                                class="flex flex-col items-start space-y-1 px-4 py-2 text-sm"
                            >
                            <div class="flex items-start gap-3">
								<TableIcon class="size-5" />
								<div class="grid gap-0.5">
									<p>
										{table.label}
									</p>
									<p class="text-xs text-muted-foreground" data-description>
										{table.comment}
									</p>
								</div>
							</div>

                            </Command.Item>

						
					{/each}
				</Command.List>
				
			</Command.Root>
		</Popover.Content>
	</Popover.Root>
	
