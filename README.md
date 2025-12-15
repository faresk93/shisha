# shisha

Description
-----------

Ce dépôt contient des ressources pour une simulation de shisha destinée à être intégrée dans une page HTML.

But
----

Fournir une simulation simple qui modélise l'effet des paramètres (charbon, tabac, foyer, longueur du tuyau, réglages d'aération) sur :
- la température de la chicha
- le volume de fumée produit
- l'intensité de la saveur

Paramètres d'entrée (pour l'interface HTML)
-------------------------------------------------

- `bowl_type` : type de foyer (ex: "phunnel", "vortex", "traditional")
- `tobacco_weight_g` : poids du tabac (grammes)
- `coal_count` : nombre de charbons
- `coal_type` : type de charbon (ex: "quick-light", "natural")
- `airflow_setting` : réglage d'aération (0-100)
- `hose_length_cm` : longueur du tuyau (cm)
- `ambient_temp_c` : température ambiante (°C)

Format d'entrée attendu (JSON)
--------------------------------

Exemple :

```
{
	"bowl_type": "phunnel",
	"tobacco_weight_g": 15,
	"coal_count": 2,
	"coal_type": "natural",
	"airflow_setting": 70,
	"hose_length_cm": 150,
	"ambient_temp_c": 22
}
```

Sorties produites par la simulation
------------------------------------

- `temperature_c` : température estimée du foyer (°C)
- `smoke_volume_l` : volume de fumée par bouffée (litres)
- `flavor_intensity` : score 0-10 de l'intensité perçue
- `recommended_adjustments` : tableau de suggestions (ex: augmenter charbons, réduire flux d'air)

Exemple de sortie (JSON)
------------------------

```
{
	"temperature_c": 220,
	"smoke_volume_l": 0.8,
	"flavor_intensity": 7.2,
	"recommended_adjustments": [
		"Réduire airflow_setting à 60",
		"Ajouter 1 charbon si température < 200°C"
	]
}
```

Exemple d'intégration HTML
--------------------------

Voici un exemple minimal d'interface HTML qui envoie les paramètres à la simulation (en local ou via une API) et affiche les résultats :

```html
<form id="simForm">
	<select name="bowl_type">
		<option value="phunnel">Phunnel</option>
		<option value="vortex">Vortex</option>
		<option value="traditional">Traditional</option>
	</select>
	<input type="number" name="tobacco_weight_g" value="15" />
	<input type="number" name="coal_count" value="2" />
	<input type="range" name="airflow_setting" min="0" max="100" value="70" />
	<button type="submit">Simuler</button>
</form>

<pre id="results"></pre>

<script>
document.getElementById('simForm').addEventListener('submit', async (e) => {
	e.preventDefault();
	const form = new FormData(e.target);
	const payload = Object.fromEntries(form.entries());
	// convertir types numériques
	payload.tobacco_weight_g = Number(payload.tobacco_weight_g);
	payload.coal_count = Number(payload.coal_count);
	payload.airflow_setting = Number(payload.airflow_setting);

	// Remplacer l'URL par celle de la simulation (locale ou API)
	const res = await fetch('/simulate', {
		method: 'POST',
		headers: { 'Content-Type': 'application/json' },
		body: JSON.stringify(payload)
	});
	const json = await res.json();
	document.getElementById('results').textContent = JSON.stringify(json, null, 2);
});
</script>
```

Notes
-----

- La simulation fournie ici est conçue pour l'interface/visualisation et n'est pas une modélisation scientifique exhaustive.
- Ajustez les formules et coefficients selon vos données expérimentales.

Contact
-------

Pour suggestions ou contributions, ouvrez une issue ou un pull request.
