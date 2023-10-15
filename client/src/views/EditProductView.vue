<template>
  <section class="produit">
    <div class="produit__header">
      <h2>Modifier un produit</h2>
    </div>
    <div v-if="!submitted" class="produit__form">
      <label for="productName">Nom
        <input
          id="productName"
          type="text"
          v-model="product.name"
          required
        />
      </label>
      <label for="productType">Catégorie
        <select
          id="productType"
          v-model="product.type"
          required>
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
          v-model.number="product.price"
          required
        />
      </label>
      <label for="productPhoto">Photo
        <input
          type="text"
          id="productPhoto"
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
        <button class="button blue" @click="updateProduct">Modifier</button>
        <router-link to="/catalogue" class="button red">Annuler</router-link>
      </div>
    </div>
  </section>
</template>

<script>
import ProductDataService from '@/services/ProductDataService'
export default {
  props: ['inventory', 'update'],
  data () {
    return {
      product: {},
      message: null,
      id: parseInt(this.$route.params.id)
    }
  },
  methods: {
    updateProduct () {
      ProductDataService.update(this.id, this.product)
        .then((response) => {
          this.update(this.product, this.productIndex)
          this.$router.push({ name: 'catalogue' })
        })
        .catch((e) => {
          this.message = e.response.data.message
        })
    }
  },
  computed: {
    productIndex () {
      return this.inventory.findIndex((item) => item.id === this.id)
    }
  },
  mounted () {
    ProductDataService.get(this.id).then((response) => {
      this.product = response.data
    })
  }
}
</script>
