struct Pokemon{
    string name;
    uint level;
    string img;
    string pokemonType;
    uint hp;
}
function battle(uint _attackingPokemon, uint _defendingPokemon) public onlyOwnerOf(_attackingPokemon){
    Pokemon storage attacker = pokemons[_attackingPokemon];
    Pokemon storage defender = pokemons[_defendingPokemon];
    
    if (attacker.level >= defender.level) {
        defender.hp -= 10; // O defensor perde pontos de vida
        attacker.level += 2;
    } else {
        attacker.hp -= 10; // O atacante perde pontos de vida
        defender.level += 2;
    }
}
function healPokemon(uint _pokemonId) public onlyOwnerOf(_pokemonId) {
    Pokemon storage pokemon = pokemons[_pokemonId];
    pokemon.hp += 20; // Cura o Pokémon
}
function tradePokemon(uint _pokemonId, address _to) public onlyOwnerOf(_pokemonId) {
    _transfer(msg.sender, _to, _pokemonId);
}
enum Rarity { Common, Rare, Epic, Legendary }
Rarity public rarity;
event PokemonCreated(uint indexed id, string name, address indexed owner);
event Battle(uint attackerId, uint defenderId, address winner);

function createNewPokemon(string memory _name, address _to, string memory _img) public {
    require(msg.sender == gameOwner, "Apenas o dono do jogo pode criar novos Pokemons");
    uint id = pokemons.length;
    pokemons.push(Pokemon(_name, 1, _img));
    _safeMint(_to, id);
    emit PokemonCreated(id, _name, _to);
}
