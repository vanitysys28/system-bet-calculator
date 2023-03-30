<script>
const binomialCoefficient = (n, k) => {
  if (Number.isNaN(n) || Number.isNaN(k)) return null;
  if (k < 0 || k > n) return 0;
  if (k === 0 || k === n) return 1;
  if (k === 1 || k === n - 1) return n;
  if (n - k < k) k = n - k;
  let res = n;
  for (let j = 2; j <= k; j++) res *= (n - j + 1) / j;
  return Math.round(res);
};

function* combinationN(array, n, outcomes) {
  for (let i = 0; i <= array.length - n; i++) {
    for (const c of combinationN(array.slice(i + 1), n - 1)) {
      if(outcomes[i].outcome == "Gagné") {
	yield [array[i] * c];
	console.log(outcomes[i].outcome)
      }
      if(outcomes[i].outcome == "Void") {
	yield [array[i] * 1];      
}
      if(outcomes[i].outcome == "Perdu") {
	yield [array[i] * 0];      
}
    }
  }
}

const arrayRange = (start, stop, step) =>
    Array.from(
    { length: (stop - start) / step + 1 },
    (value, index) => start + index * step
    );

const systems = arrayRange(2,15,1);
const outcomes = ["Gagné","Perdu","Void"]

let combinations = 2  
let selections = 0 
let combinationReturns = []
let totalStake = 50 
let totalReturn = 0 

$: totalBets = binomialCoefficient(selections, combinations);
$: unitStake = (totalStake / totalBets).toFixed(2)
let odds = []
let values = []

function addOdds(selections) {
odds = new Array(selections).fill()
odds.forEach((val, i) => {
     odds[i] = {"value": "1.8","outcome":"Gagné"};
 })
}

$: if (odds.every(element => element !== null)) {
combinationReturns = [] 
for (const c of combinationN(values, combinations, odds)) {
combinationReturns.push(unitStake * c);
}
}

$: odds, values = odds.map(a => a.value);
$: odds, totalReturn = combinationReturns.reduce(function(a,b) { return a + b; }, 0).toFixed(2)
</script>


<h1>Calculateur Pari Système</h1>

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
Nombre de paris générés: {#if totalBets > 1} {totalBets} {/if}
</div>

<div>
Mise unitaire: {unitStake}
</div>

<div>
Gains: {totalReturn}
</div>
