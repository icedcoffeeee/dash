<script lang="ts">
	const today = new Date();
	const days = new Date(today.getFullYear(), today.getMonth() + 1, 0).getDate();
	const particular = (i: number) => new Date(today.getFullYear(), today.getMonth(), i + 1);

	type Note = {
		id: number;
		note: string;
		date: Date;
	};

	let inspect = today.getDate() - 1;
	let notes: Note[] = [];
	let updated = 0;
</script>

<button class="text-xl mb-3 py-1 px-2 rounded bg-slate-800">
	{today.toLocaleString('en', { month: 'long', year: 'numeric' })}
</button>

<div class="flex justify-between">
	<div class="w-fit grid grid-cols-7 gap-2">
		{#each Array(days).fill(0) as _, i}
			<div
				class="p-1 size-32 rounded bg-slate-800 hover:scale-110 transition-all"
				class:border={i === inspect}
				class:border-yellow-500={i === inspect}
				class:text-yellow-500={i + 1 === today.getDate()}
				on:click={function () {
					inspect = i;
				}}
				on:keyup={function ({ key }) {
					if (key !== 'Enter') return;
					inspect = i;
				}}
				role="button"
				tabindex="0"
			>
				{particular(i).toLocaleString('en', { weekday: 'narrow' })}
				{i + 1}
				<div class="text-xs">
					<ul class="list-disc pl-4">
						{#key updated}
							{#each notes.filter((n) => n.date.getTime() === particular(i).getTime()) as note}
								<li>{note.note}</li>
							{/each}
						{/key}
					</ul>
				</div>
			</div>
		{/each}
	</div>
	<div class="sticky top-[5rem] w-[30rem] h-[30rem] p-2 rounded bg-slate-600">
		<h2 class="text-xl mb-3">
			{particular(inspect).toLocaleString('en', { weekday: 'short', day: '2-digit' })}
		</h2>
		<ul class="list-disc pl-4">
			{#key updated}
				{#each notes.filter((n) => n.date.getTime() === particular(inspect).getTime()) as note}
					<li
						contenteditable="true"
						bind:innerText={note.note}
						on:keydown={function ({ key }) {
							if (key === 'Backspace' && note.note.length == 1) {
								notes.splice(notes.indexOf(note), 1);
								updated += 1;
							}
						}}
					>
						{note.note}
					</li>
				{/each}
			{/key}
			<li>
				<textarea
					class="w-full bg-transparent align-top"
					placeholder="Add a note"
					on:keyup={function ({ key, currentTarget }) {
						if (key !== 'Enter') return;
						notes.push({
							id: notes[notes.length - 1]?.id ?? -1 + 1,
							note: currentTarget.value,
							date: particular(inspect)
						});
						currentTarget.value = '';
						updated += 1;
					}}
				></textarea>
			</li>
		</ul>
	</div>
</div>
