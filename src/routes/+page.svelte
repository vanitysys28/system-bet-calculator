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

//for (const c of combinationN(array, 4)) {
//console.log(c);
//}

const arrayRange = (start, stop, step) =>
    Array.from(
    { length: (stop - start) / step + 1 },
    (value, index) => start + index * step
    );

const systems = arrayRange(2,15,1);

let success = '' 
let trial = '' 
let stake = ''
let bets = []

$: rows = binomialCoefficient(trial, success);
$: unitStake = (stake / rows).toFixed(2)
$: betRow = arrayRange(1,trial,1)
</script>


<h1>Calculateur Pari Système</h1>

<select bind:value={success}>
{#each systems as value}<option {value}>{value}</option>{/each}
</select>

<select bind:value={trial}>
{#each systems as value}<option {value}>{value}</option>{/each}
</select>

<div>
Mise totale:  <input bind:value={stake}>
</div>

<div>
Paris:
{#each betRow as bet}
<div>
Pari {bet}:  
</div>
{/each}
</div>

<div>
Nombre de paris: {#if rows > 1} {rows} {/if}
</div>

<div>
Mise unitaire: {unitStake}
</div>

<div>
Gains: 
</div>
