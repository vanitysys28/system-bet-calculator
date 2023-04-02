<script>
import * as C from 'js-combinatorics';

const createArrayRange = (start, stop, step) =>
    Array.from(
    { length: (stop - start) / step + 1 },
    (value, index) => start + index * step
    );

let combinations = 2  
let selections = 4 
let totalStake = 100   
let combinationReturns = []
let totalReturn = 0 

const systems = createArrayRange(2,15,1);
const outcomes = ["Gagné","Perdu","Void"]

let odds = []
let values = []

$: totalBets =  [...combinationIterator].length 
$: unitStake = (totalStake / totalBets).toFixed(2)

function addOdds(selections) {
    odds = new Array(selections).fill()
    odds.forEach((odd, i) => {
    odds[i] = {"value": "1.8","outcome":"Gagné"};
    })
}

// TODO: Find a way to make odds appear without calling the function
addOdds(selections)

function checkOutcomes(odd, i) {
    if (odds[i].outcome == "Gagné") {
			  return odds[i].value
		}
		if (odds[i].outcome == "Perdu") {
			  return 0
		}
		if (odds[i].outcome == "Void") {
			  return 1
		}
}

$: combinationIterator = C.Combination.of(values, combinations);

$: if (totalStake) {
combinationReturns = []
for (let i = 0; i <= [...combinationIterator].length - 1; i++) {
combinationReturns.push([...combinationIterator][i].reduce((a, b) => a * b ) * unitStake)
}
}

$: odds, values = odds.map(checkOutcomes); 
$: odds, totalReturn = combinationReturns.reduce((a,b) => a + b).toFixed(2)
</script>


<h1 class="text-3xl font-bold underline">Calculateur Pari Système</h1>

<select bind:value={combinations}>
{#each systems as value}<option {value}>{value}</option>{/each}
</select>

<select bind:value={selections} on:change={addOdds(selections)}>
{#each systems as value}<option {value}>{value}</option>{/each}
</select>

<div>
Mise totale:  <input bind:value={totalStake}>
</div>

<div>
Paris:
{#each odds as bet,i}
<div>
Pari {i + 1}:  <input bind:value={bet.value} placeholder="">
<select bind:value={bet.outcome}>
{#each outcomes as outcome}
<option {outcome}>{outcome}</option>
{/each}
</select>
</div>
{/each}
</div>

<div>
Nombre de paris générés: {#if totalBets > 0} {totalBets} {/if}
</div>

<div>
Mise unitaire: {#if unitStake > 0} {unitStake} {/if}
</div>

<div>
Gains: {totalReturn}
</div>

