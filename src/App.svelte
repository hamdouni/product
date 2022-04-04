<script>
	let ean = '3017620425035';
	let errmsg = '';
	let data = {};
	let image_url = '';
	let image_nutrition_url = '';
	let product_name = '';
	let nutriscore = '';
	let pnns_groups = '';

	const search = () => {
		fetch("https://fr.openfoodfacts.org/api/v0/product/"+ean+".json")
        .then((r) => {
            if (r.ok) return r.json();
            throw Error("échec récupération inventaire en cours: " + r.statusText);
        })
        .then((d) => {
            if(d !== null) {
            	data = d;
            	image_url = d.product.image_url;
            	product_name = d.product.product_name;
            	image_nutrition_url = d.product.image_nutrition_url;
            	nutriscore = d.product.nutriscore_grade;
            	pnns_groups = d.product.pnns_groups_1 + "/" + d.product.pnns_groups_2;
            }
            else data = {};
        })
        .catch((err) => (errmsg = err));
	}
</script>
<h1>nutriscore {nutriscore}</h1>
<h2>{pnns_groups}</h2>
<h2>{errmsg}</h2>
<input bind:value={ean}/><button on:click={search}>rechercher</button>
<h3>{product_name}</h3>
{#if image_url}
<img src="{image_url}" alt="product"/>
{/if}
<img src="{image_nutrition_url}" alt="nutrition"/>
<pre>
{JSON.stringify(data,null,' ')}
</pre>
