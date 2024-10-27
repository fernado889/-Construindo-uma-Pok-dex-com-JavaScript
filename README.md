# -Construindo-uma-Pok-dex-com-JavaScript
pokedex/
│
├── index.html
├── style.css
└── script.js
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pokédex</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Pokédex</h1>
        <input type="text" id="search" placeholder="Pesquise um Pokémon...">
        <button id="search-btn">Buscar</button>
        <div id="pokedex"></div>
    </div>

    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    color: #333;
}

.container {
    max-width: 600px;
    margin: 0 auto;
    text-align: center;
    padding: 20px;
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

#search {
    padding: 10px;
    width: 80%;
    margin-bottom: 10px;
}

#search-btn {
    padding: 10px 20px;
}

.pokemon-card {
    border: 1px solid #ccc;
    border-radius: 8px;
    padding: 10px;
    margin: 10px 0;
    background-color: #fafafa;
}
const API_URL = 'https://pokeapi.co/api/v2/pokemon/';

document.getElementById('search-btn').addEventListener('click', async () => {
    const searchTerm = document.getElementById('search').value.toLowerCase();
    const pokemonData = await fetchPokemon(searchTerm);
    displayPokemon(pokemonData);
});

async function fetchPokemon(name) {
    try {
        const response = await fetch(`${API_URL}${name}`);
        if (!response.ok) throw new Error('Pokémon não encontrado');
        const data = await response.json();
        return data;
    } catch (error) {
        alert(error.message);
        return null;
    }
}

function displayPokemon(pokemon) {
    const pokedex = document.getElementById('pokedex');
    pokedex.innerHTML = ''; // Limpa resultados anteriores

    if (pokemon) {
        const pokemonCard = document.createElement('div');
        pokemonCard.className = 'pokemon-card';
        pokemonCard.innerHTML = `
            <h2>${pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)}</h2>
            <img src="${pokemon.sprites.front_default}" alt="${pokemon.name}">
            <p>Altura: ${pokemon.height}</p>
            <p>Peso: ${pokemon.weight}</p>
        `;
        pokedex.appendChild(pokemonCard);
    }
}
