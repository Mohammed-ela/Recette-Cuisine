<template>
    <main>
      <h1>Mes recettes</h1>
      <form @submit.prevent="addRecipe">
        <label for="nom">Nom de la recette :</label>
        <input type="text" id="nom" v-model="newRecipe.nom" required>
        
        <label for="ingredients">Ingrédients :</label>
        <textarea id="ingredients" v-model="newRecipe.ingredients" required></textarea>
        
        <label for="preparation">Préparation :</label>
        <textarea id="preparation" v-model="newRecipe.preparation" required></textarea>
        
        <label for="temps">Temps de préparation (en min) :</label>
        <input type="number" id="temps" v-model.number="newRecipe.temps" required>
        
        <label for="nombre">Nombre de personnes :</label>
        <input type="number" id="nombre" v-model.number="newRecipe.nombre" required>

        <!-- Div messages d'erreur -->
        <div class="error" v-if="error">{{ error }}</div>
        
        <button type="submit">Ajouter la recette</button>
      </form>
      
      <ul>
        <li v-for="recipe in recipes" :key="recipe.id">
          <h2>{{ recipe.nom }}</h2>
          <p><strong>Ingrédients :</strong> {{ recipe.ingredients }}</p>
          <p><strong>Préparation :</strong> {{ recipe.preparation }}</p>
          <p><strong>Temps de préparation :</strong> {{ recipe.temps }} minutes</p>
          <p><strong>Nombre de personnes :</strong> {{ recipe.nombre }}</p>
          <button @click="deleteRecipe(recipe.id)">Supprimer</button>
        </li>
      </ul>
    </main>
</template>

<script>
export default {
  data() {
    return {
      newRecipe: {
        id: null,
        nom: '',
        ingredients: '',
        preparation: '',
        temps: 0,
        nombre: 0
      },
      recipes: [],
      error: '' // msg d'erreur
    };
  },
  mounted() {
    // Récupérer les recettes depuis le localStorage lors du montage du composant
    const recipesFromLocalStorage = localStorage.getItem('recipes');
    if (recipesFromLocalStorage) {
      this.recipes = JSON.parse(recipesFromLocalStorage);
    }
  },
  methods: {
    addRecipe() {
      if (this.validateForm()) {
        this.newRecipe.id = Date.now();
        this.recipes.push({ ...this.newRecipe });
        this.newRecipe = {
          id: null,
          nom: '',
          ingredients: '',
          preparation: '',
          temps: 0,
          nombre: 0
        };
        this.error = ''; // Rmet msg d'erreur vide
        this.saveRecipesToLocalStorage();
      } else {
        this.error = 'Veuillez remplir tous les champs du formulaire.';
      }
    },
    deleteRecipe(id) {
      this.recipes = this.recipes.filter(recipe => recipe.id !== id);
      this.saveRecipesToLocalStorage();
    },
    saveRecipesToLocalStorage() {
      localStorage.setItem('recipes', JSON.stringify(this.recipes));
    },
    validateForm() {
      return (
        this.newRecipe.nom &&
        this.newRecipe.ingredients &&
        this.newRecipe.preparation &&
        this.newRecipe.temps > 0 &&
        this.newRecipe.nombre > 0
      );
    }
  }
};
</script>

<!-- style suplementaire -->
<style scoped>
.error {
  color: red;
  margin-bottom: 10px;
}
</style>
