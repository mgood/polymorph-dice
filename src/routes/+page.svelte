<script lang="ts">
	import polymorphLogo from '$lib/assets/polymorph.avif';

	let maxRoll: number | undefined = $state();
	let dice: (number | undefined)[] = $state([undefined]);
	let mustMatch = $state(1);
	let isAdvantage = $derived(mustMatch === 1 && dice.length === 2);
	let isDisadvantage = $derived(mustMatch === 2 && dice.length === 2);
	function success(lo?: number, hi?: number) {
		if (!lo || !hi) return '';
		// Array methods don't seem to work reliably on the Proxy object, so take a snapshot
		const diceSnap = $state.snapshot(dice);
		if (!diceSnap.every(Boolean)) return '';
		const inRange = (n: number | undefined) => n && n >= lo && n <= hi;
		const matched = diceSnap.filter(inRange).length;
		if (matched >= mustMatch) {
			return 'success';
		} else {
			return 'fail';
		}
	}

	function toggleVantage(match: number) {
		if (dice.length === 2 && match === mustMatch) {
			dice.length = 1;
			mustMatch = 1;
		} else {
			dice.length = 2;
			mustMatch = match;
		}
	}

	$effect(() => {
		if (!maxRoll) return;
		// if all the dice already have a value, we don't need to roll
		let allDefined = $state.snapshot(dice).every(Boolean);
		if (allDefined) return;
		const timeout = setTimeout(() => {
			if (!maxRoll) return;
			for (let i = 0; i < dice.length; i++) {
				if (dice[i]) continue;
				dice[i] = Math.floor(Math.random() * maxRoll) + 1;
			}
		}, 1500);
		return () => clearTimeout(timeout);
	});

	function rollDie(n: number) {
		// set dice to a new cleared array to force the effect to trigger
		let cleared: typeof dice = [];
		cleared.length = dice.length;
		dice = cleared;
		maxRoll = n;
	}
</script>

{#snippet roll(n)}
	<button onclick={() => rollDie(n)} class="roll d{n}">
		Roll D{n}
	</button>
{/snippet}

<div class="flex w-full">
	<div>
		<table>
			<thead>
				<tr><th>Role</th><th>Roll</th></tr>
			</thead>
			<tbody>
				<tr><td>Expert</td><td>{@render roll(4)}</td> </tr>
				<tr><td>Vanguard</td><td>{@render roll(6)}</td></tr>
				<tr><td>Fighter</td><td>{@render roll(8)}</td></tr>
				<tr><td>Tank</td><td>{@render roll(10)}</td></tr>
			</tbody>
		</table>

		<button onclick={() => toggleVantage(1)} class="vantage" class:enabled={isAdvantage}>
			Advantage
		</button>

		<button onclick={() => toggleVantage(2)} class="vantage" class:enabled={isDisadvantage}>
			Disdvantage
		</button>
	</div>

	<div class="grid w-16 flex-1 place-content-center gap-12">
		{#each dice as roll}
			<div class="die d{maxRoll || 0}" class:animate-spin={maxRoll && !roll}>
				{roll}
			</div>
		{/each}
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
				<td>🗝️ Key (Who You Are)</td>
			</tr>
			<tr class={success(2, 3)}>
				<th>2 3</th>
				<td>🧠 Mental Activity</td>
			</tr>
			<tr class={success(3, 5)}>
				<th>3 4 5</th>
				<td>🏃‍♀️ Physical Activity</td>
			</tr>
			<tr class={success(4, 7)}>
				<th>4 5 6 7</th>
				<td>✋ Conflict Resolution</td>
			</tr>
			<tr class={success(5, 9)}>
				<th>5 6 7 8 9</th>
				<td>💪 Strength of Body or Spirit</td>
			</tr>
			<tr class={success(maxRoll, maxRoll)}>
				<th>Max Roll</th>
				<td>👑 Crown (Special Circumstances)</td>
			</tr>
		</tbody>
		<tfoot>
			<tr>
				<td colspan="2" align="right">
					<a href="https://9thlevel.com/pages/polymorph">
						<img src={polymorphLogo} alt="built with polymorph" class="h-10" />
					</a>
				</td>
			</tr>
		</tfoot>
	</table>
</div>

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
	.roll {
		@apply m-2 h-12 w-20 rounded-lg text-white;
	}
	.die {
		@apply size-28 place-content-center rounded-lg text-center text-8xl text-white;
	}
	.d0 {
		@apply bg-slate-400;
	}
	.d4 {
		@apply bg-rose-500;
	}
	.d6 {
		@apply bg-yellow-500;
	}
	.d8 {
		@apply bg-emerald-600;
	}
	.d10 {
		@apply bg-cyan-600;
	}
	.success {
		@apply bg-green-200;
	}
	.fail {
		@apply bg-slate-200;
	}
	.vantage {
		@apply m-2 rounded-lg border-2 border-slate-600 p-2;
	}
	.vantage.enabled {
		@apply bg-slate-600 text-white;
	}
</style>
