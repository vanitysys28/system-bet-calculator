<script>
const binomialCoefficient = (n, k) => {
  if (Number.isNaN(n) || Number.isNaN(k)) return NaN;
  if (k < 0 || k > n) return 0;
  if (k === 0 || k === n) return 1;
  if (k === 1 || k === n - 1) return n;
  if (n - k < k) k = n - k;
  let res = n;
  for (let j = 2; j <= k; j++) res *= (n - j + 1) / j;
  return Math.round(res);
};

function* combinationN(array, n) {
  if (n === 1) {
    for (const a of array) {
      yield [a];
    }
    return;
  }

  for (let i = 0; i <= array.length - n; i++) {
    for (const c of combinationN(array.slice(i + 1), n - 1)) {
      yield [array[i] * c];
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

let success = '' 
let trial = '' 
let totalStake = ''
let combinationReturns = []
let totalReturn = 0 

$: rows = binomialCoefficient(trial, success);
$: unitStake = (totalStake / rows).toFixed(2)
$: odds = new Array(trial).fill(null)

$: if (odds.every(element => element !== null)) {
combinationReturns = [] 
for (const c of combinationN(odds, success)) {
combinationReturns.push(unitStake * c);
}
console.log(combinationReturns)
}

$: odds, totalReturn = combinationReturns.reduce(function(a,b) { return a + b; }, 0).toFixed(2)
</script>


<h1>Calculateur Pari Système</h1>

<select bind:value={success}>
{#each systems as value}<option {value}>{value}</option>{/each}
</select>

<select bind:value={trial}>
{#each systems as value}<option {value}>{value}</option>{/each}
</select>

<div>
Mise totale:  <input bind:value={totalStake}>
</div>

<div>
{#if odds.length > 2}
Paris:
{#each odds as bet,i}
<div>
Pari {i + 1}:  <input bind:value={bet} placeholder="">
<select>
{#each outcomes as outcome}
<option {outcome}>{outcome}</option>
{/each}
</select>
</div>
{/each}
{/if}
</div>

<div>
Nombre de paris générés: {#if rows > 1} {rows} {/if}
</div>

<div>
Mise unitaire: {unitStake}
</div>

<div>
Gains: {totalReturn}
</div>
