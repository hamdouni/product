<script>

	import ScoreItem from './lib/score.svelte';

	let ean = '3017620425035';
	let errmsg = '';
	let data;
	let name = '';
	let qty = 0;
	let brand = '';
	let short = '';
	let nutriscore = '';
	let novascore = '';
	let ecoscore = '';
	let nutrigras = '';
	let nutrisel = '';
	let nutrisatur = '';
	let nutrisucr = '';
	let ingredients = '';

	const search = () => {
		errmsg = '';
		// fetch("https://fr.openfoodfacts.org/api/v0/product/"+ean+".json")
		fetch("/product/"+ean+".json")
        .then((r) => {
            if (r.ok) return r.json();
            throw Error("échec récupération produit: " + r.statusText);
        })
        .then((d) => {
            if(d !== null) {
            	data = d;
            	short = (d.product_name!=null)?d.product_name:'';
				name = (d.product_name_fr!=null)?d.product_name_fr:'';
				qty = (d.quantity!=null)?d.quantity:'';
				brand = (d.brands!=null)?d.brands:'';
            	nutriscore = d.nutriscore_grade;
				novascore  = d.nova_groups;
				ecoscore   = d.ecoscore_grade;
				nutrigras = d.nutrient_levels.fat;
				nutrisel =  d.nutrient_levels.salt;
				nutrisatur =  d.nutrient_levels['saturated-fat'];
				nutrisucr =  d.nutrient_levels.sugars;
				ingredients = (d.ingredients_text_fr != null)?d.ingredients_text_fr:'';
			}
            else data = null	;
        })
        .catch((err) => (errmsg = err));
	}
</script>

<main>

<div class="search">
	<input bind:value={ean}/><button on:click={search}>rechercher</button>
	<h2>{errmsg}</h2>
</div>

<hr>

{#if data}
<div class="result">
	<h2>{name}</h2>
	<div class="scores">
		<ScoreItem type="nova-group" value="{novascore}"/>
		<ScoreItem type="nutriscore" value="{nutriscore}"/>
		<ScoreItem type="ecoscore" value="{ecoscore}"/>
	</div>
	<p>{brand}</p>
	<p>{qty}</p>
	<div class="nutriment">
		<div class="{nutrisel}">sel</div>
		<div class="{nutrigras}">gras</div>
		<div class="{nutrisatur}">gras saturé</div>
		<div class="{nutrisucr}">sucres</div>
	</div>
	{#if ingredients}
	<p>ingrédients: {ingredients}</p>
	{/if}
</div>
{/if}

</main>

<style>
	main {
		max-width: 31rem;
	}
	.scores {
		display: flex;
		flex-direction: row;
		justify-content: left;
	}

	.nutriment {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}
	.nutriment div {
		font-size: 1rem;
		font-weight: bold;
		width: 4rem;
		height: 4rem;
		border-radius: 3rem;
		border-width: 4px;
		border-style: solid;
		display: flex;
		justify-content: center;
		align-items: center;
		text-align: center;
	}
	.low {
		border-color: #038141;
		color: #038141;
	}
	.moderate {
		border-color: #fecb02;
		color: #fecb02;
	}
	.high {
		border-color: #df1f1f;
		color: #df1f1f;
	}
</style>
