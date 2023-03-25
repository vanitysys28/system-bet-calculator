<script>
const binomialCoefficient = (k, n) => {
  if (Number.isNaN(n) || Number.isNaN(k)) return NaN;
  if (k < 0 || k > n) return 0;
  if (k === 0 || k === n) return 1;
  if (k === 1 || k === n - 1) return n;
  if (n - k < k) k = n - k;
  let res = n;
  for (let j = 2; j <= k; j++) res *= (n - j + 1) / j;
  return Math.round(res);
};

const systems = [2,3,4,5,6,7,8,9,10,11,12,13,14,15];

let success = '' 
let trial = '' 
let stake = ''

$: rows = binomialCoefficient(success, trial);
$: unitStake = (stake / rows).toFixed(2)
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
Nombre de paris: {#if rows > 1} {rows} {/if}
</div>

<div>
Mise unitaire: {unitStake}
</div>
