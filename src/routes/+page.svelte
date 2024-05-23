<script lang="ts">
	let maxRoll: number | undefined = $state();
	let roll: number | undefined = $state();
	function success(lo?: number, hi?: number) {
		if (!roll || !lo || !hi) return '';
		if (roll >= lo && roll <= hi) {
			return 'success';
		} else {
			return 'fail';
		}
	}
	function rollDie(n: number) {
		roll = undefined;
		maxRoll = n;
		setTimeout(() => {
			roll = Math.floor(Math.random() * n) + 1;
		}, 1500);
	}
</script>

{#snippet die(n)}
	<button onclick={() => rollDie(n)} class={`d${n}`}>
		Roll D{n}
	</button>
{/snippet}

<div class="flex w-full">
	<table>
		<thead>
			<tr><th>Role</th><th>Die</th></tr>
		</thead>
		<tbody>
			<tr><td>Expert</td><td>{@render die(4)}</td> </tr>
			<tr><td>Vanguard</td><td>{@render die(6)}</td></tr>
			<tr><td>Fighter</td><td>{@render die(8)}</td></tr>
			<tr><td>Tank</td><td>{@render die(10)}</td></tr>
		</tbody>
	</table>

	<div class="grid w-16 flex-1 place-content-center">
		<div
			class={`d${maxRoll || 0} ${maxRoll && !roll ? 'animate-spin' : ''} size-40 place-content-center rounded-lg text-center text-8xl`}
		>
			{roll}
		</div>
	</div>

	<table class="flex-2">
		<thead>
			<tr>
				<th>Die Roll</th>
				<th>Outcome</th>
			</tr>
		</thead>
		<tbody>
			<tr class={success(1, 1)}>
				<th>1</th>
				<td>Key (Who You Are)</td>
			</tr>
			<tr class={success(2, 3)}>
				<th>2 3</th>
				<td>Mental Activity</td>
			</tr>
			<tr class={success(3, 5)}>
				<th>3 4 5</th>
				<td>Physical Activity</td>
			</tr>
			<tr class={success(4, 7)}>
				<th>4 5 6 7</th>
				<td>Conflict Resolution</td>
			</tr>
			<tr class={success(5, 9)}>
				<th>5 6 7 8 9</th>
				<td>Strength of Body or Spirit</td>
			</tr>
			<tr class={success(maxRoll, maxRoll)}>
				<th>Max Roll</th>
				<td>Crown (Special Circumstances)</td>
			</tr>
		</tbody>
	</table>
</div>

<article class="prose prose-sm">
	<h1>'Vantage</h1>
	<dl>
		<dt>Advantage</dt>
		<dd>succeed if either roll succeeds</dd>
		<dt>Disadvantage</dt>
		<dd>succeed if both rolls succeed</dd>
	</dl>

	<h1>Moment</h1>
	<p>
		Each player gets a Moment. Can be roll, description, action, or spend. Everyone has an option to
		take a moment before anyone else gets another moment.
	</p>
</article>

<style lang="postcss">
	thead th {
		@apply bg-slate-700 text-white;
	}
	tbody th {
		text-align: right;
	}
	th {
		@apply px-4 py-2;
	}
	td {
		@apply px-4 py-1;
	}
	button {
		@apply m-2 h-12 w-20 rounded-lg;
	}
	.d0 {
		@apply bg-slate-400;
	}
	.d4 {
		@apply bg-rose-500 text-white;
	}
	.d6 {
		@apply bg-yellow-500 text-white;
	}
	.d8 {
		@apply bg-emerald-600 text-white;
	}
	.d10 {
		@apply bg-cyan-600 text-white;
	}
	.success {
		@apply bg-green-200;
	}
	.fail {
		@apply bg-slate-200;
	}
</style>
