<template>
  <section class="produit">
    <div class="produit__header">
      <h2>Ajouter un produit</h2>
    </div>
    <div v-if="!submitted" class="produit__form">
      <label for="productName">Nom
        <input
          id="productName"
          type="text"
          placeholder="Ajouter un nom de produit"
          v-model="product.name"
          required
        />
      </label>
      <label for="productType">Catégorie
        <select
          id="productType"
          v-model="product.type"
          required>
            <option value="" selected>--Sélectionner une option--</option>
            <option value="Thé vert">Thé vert</option>
            <option value="Thé noir">Thé noir</option>
            <option value="Tisane">Tisane</option>
            <option value="Thé chaï">Chai</option>
        </select>
      </label>
      <label for="productPrice">Prix
        <input
          id="productPrice"
          type="number"
          placeholder="Ajouter un prix"
          v-model.number="product.price"
          required
        />
      </label>
      <label for="productPhoto">Photo
        <input
          type="text"
          id="productPhoto"
          placeholder=""
          v-model="product.photo"
          required
        />
      </label>
      <label for="productDescription">Description
        <textarea
          id="productDescription"
          rows="10"
          placeholder="Ajouter une description"
          v-model="product.description"
          required>
        </textarea>
      </label>
      <div v-show="message" class="error">
        <h3>Veuillez remplir tous les champs !</h3>
        <p>{{ message }}</p>
      </div>
      <div class="optionButtons center">
        <button class="button blue" @click="saveProduct">Enregistrer</button>
        <router-link to="/catalogue" class="button red">Annuler</router-link>
      </div>
    </div>
    <div v-else class="produit__confirmation">
      <h3>Le produit a été ajouté au catalogue !</h3>
      <div class="optionButtons">
        <button class="button yellow" type="button" @click="newProduct">Ajouter un nouveau produit</button>
        <router-link to="/catalogue" class="button red">Retourner au catalogue</router-link>
      </div>
    </div>
  </section>
</template>

<script>
import ProductDataService from '@/services/ProductDataService'
export default {
  props: ['addInv'],
  data () {
    return {
      submitted: false,
      product: {
        name: null,
        photo: null,
        price: null,
        description: null,
        type: null
      },
      message: null
    }
  },
  methods: {
    saveProduct () {
      ProductDataService.create(this.product)
        .then(response => {
          this.product.id = response.data.id
          this.addInv(this.product)
          this.submitted = true
          this.message = null
        })
        .catch(e => {
          this.message = e.response.data.message
        })
    },
    newProduct () {
      this.submitted = false
      this.product = {}
    }
  }
}
</script>
