<script>
 import pkg from 'js-combinatorics';
 const { combination } = pkg;

 const createArrayRange = (start, stop, step) =>
     Array.from(
	 { length: (stop - start) / step + 1 },
	 (value, index) => start + index * step
     );

 let subset = 2  
 let selections = 4 
 let totalStake = 100   
 let combinationReturns = []
 let totalReturn = 0 

 $: subsetRange = createArrayRange(2,selections - 1,1);
 $: selectionRange = createArrayRange(subset + 1,15,1);
 const outcomes = ["Gagné","Perdu","Void"]

 let odds = []
 let values = []

 function addOdds(selections) {
     odds = new Array(selections).fill()
     odds.forEach((odd, i) => {
	 odds[i] = {"value": "1.8","outcome":"Gagné"};
     })
 }

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

 $: combinations = combination(values, subset);

 $: if (totalStake) {
     combinationReturns = []
     combinations.forEach(function(combination){
	 combinationReturns.push(combination.reduce((a, b) => a * b ) * unitStake)
     })
 }

 $: totalReturn = combinationReturns.reduce((a,b) => a + b).toFixed(2)
 $: totalBets =  combinations.length 
 $: unitStake = (totalStake / totalBets).toFixed(2)
 $: values = odds.map(checkOutcomes);

</script>

<main>
    <div>    
	<h1 class="text-3xl font-bold">Calculateur
	    <span>
		Pari Système
	    </span>
	</h1>
 </div> 

 <div>
 <select bind:value={subset}>
{#each subsetRange as value}<option {value}>{value}</option>{/each}
</select>

<select bind:value={selections} on:change={addOdds(selections)}>
{#each selectionRange as value}<option {value}>{value}</option>{/each}
</select>

<div>
<input class="text-black" bind:value={totalStake}>
</div>

<div>
{#each odds as bet,i}
<div>
<input class="text-black" bind:value={bet.value} placeholder="">
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
 </div>

 <h2>
     Envie de tenter ta chance ?
 </h2>

 <span>
     Inscris toi chez Unibet
 </span>
 <p>
     Profite d'un bonus de bienvenue de 100€ si ton premier pari est perdant !
 </p>
 <p>
     Ton bonus est valable pour toute nouvelle inscription, et sera crédité sous 24 heures après le résultat de ton premier pari.
 </p>

 <span>
     Tu rencontres un problème
 </span>
 <span>
     ou tu souhaites proposer une nouvelle fonctionnalité ?
 </span>

 <p>
     Contacte-nous par email et fais-nous part de tes idées !
 </p>

 <p>
     Made with 🤍 by Guillaume Aubert
 </p>
     
</main>
