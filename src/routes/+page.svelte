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

<main class="bg-[url('/pattern.svg')]">
  <section id="header" class="p-10">
	<h1 class="flex flex-col items-center text-5xl font-bold">Calculateur
	    <span>
		Pari Système
	    </span>
	</h1> 
  </section>
  
   <section id="calculator" class="p-10">
	<div class="flex justify-center mx-auto max-w-4xl">
	<div class="flex flex-col items-center py-5 gap-5 bg-neutral-800 border rounded-md border-neutral-700 p-2 w-screen">
	    <div>
	    <select class="border rounded-md border-neutral-700 bg-neutral-900 p-2 w-12 text-center" bind:value={subset}>
		{#each subsetRange as value}<option {value}>{value}</option>{/each}
	    </select>
	    /
	    <select class="border rounded-md border-neutral-700 bg-neutral-900 p-2 w-12 text-center" bind:value={selections} on:change={addOdds(selections)}>
		{#each selectionRange as value}<option {value}>{value}</option>{/each}
	    </select>
	    </div>
	    <div class="flex justify-center w-full">
		<input class="border rounded-md border-neutral-700 bg-neutral-900 p-2 text-center sm:w-1/4 w-1/2 appearance-none" bind:value={totalStake} type="number">
</div>


    {#each odds as bet,i}
	<div class="flex justify-center sm:gap-20 gap-10 w-full">
	    <input class="border rounded-md border-neutral-700 bg-neutral-900 p-2 sm:w-1/4 w-1/2 text-center appearance-none" bind:value={bet.value} type="number" placeholder="">
	    <select class="border rounded-md border-neutral-700 bg-neutral-900 p-2 sm:w-1/4 w-1/2 text-center" bind:value={bet.outcome}>
	     {#each outcomes as outcome}
	       <option {outcome}>{outcome}</option>
	     {/each}
	     </select>
	 </div>
    {/each}



<div class ="border rounded-md border-neutral-700 w-max bg-neutral-900 py-4 sm:px-20 px-10">
     <div>Mise unitaire: {#if unitStake > 0} {unitStake} {/if}</div>
      <div>Nombre de paris générés: {#if totalBets > 0} {totalBets} {/if}</div>
      <div>Gains: {totalReturn}</div>
</div>
	</div>
	</div>
   </section>
	
 <section id="promotional" class="p-10 bg-neutral-800">
<div class="mx-auto max-w-4xl">
   <h2 class="text-2xl sm:text-3xl font-bold text-center">
	  Envie de tenter ta chance ?
	</h2>


 <h3 class="text-xl sm:text-2xl font-bold mt-4">
     Inscris toi chez Unibet
 </h3>
 <div class="sm:flex gap-2">
 <div class="basis-1/2">
 <p class="mt-4">
     Profite d'un bonus de bienvenue de 100€ si ton premier pari est perdant !
 </p>
 <p class="mt-4">
     Ton bonus est valable pour toute nouvelle inscription, et sera crédité sous 24 heures après le résultat de ton premier pari.
 </p>
 </div>
 <div class="basis-1/2 mt-4">
 <a href="https://www.unibet.fr/register">
   <img class="border rounded-md border-neutral-700 basis-1/2" src="https://i.unibet.fr/original//Bonus-Bienvenue_NEW3-LP_1280x340baf7b939-132f-4754-bfd6-dab28cab41412e8ee789-ca18-43ec-a811-2a367e029646.jpg">
 </a>
 </div>
</div>
 </div>
 </section>
 
 <section id="contact" class="p-10">    
 <h2 class="flex flex-col text-2xl sm:text-3xl font-bold text-center ">
     Tu rencontres un problème
 <span class="font-normal">
     ou tu souhaites proposer une nouvelle fonctionnalité ?
 </span>
 </h2>
 
 <p class="text-center mt-4">
     Contacte-nous par email et fais-nous part de tes idées !
 </p>

 <div class="flex justify-center mt-4">
 <button class="border rounded-md border-neutral-700 p-2 w-max bg-neutral-800 text-center">contact@calculateurparisysteme.gg</button>
 </div>
</section>
 
 <footer class="pt-10">
 <p class="text-center mb-4">
     Made with 🤍 by <a href="https://github.com/vanitysys28/">Guillaume Aubert</a>
 </p>
    </footer>
     
</main>
