<template>
  <section class="catalogue">
    <div class="catalogue__header">
      <h2>Catalogue de nos thés</h2>
      <router-link to="/new-product" class="button bg-grey">Ajouter un produit</router-link>
    </div>
    <div class="catalogue__produits">
      <div v-for="(product, i) in inventory" :key="i">
        <div class="catalogue__produit">
          <!-- Product image-->
          <img :src="require(`@/assets/img/${product.photo}`)" :alt="product.name" />
          <!-- Product detail -->
          <h4>{{product.name}}</h4>
          <p>
            <svg
              stroke="currentColor"
              fill="currentColor"
              stroke-width="0"
              viewBox="0 0 256 256"
              height="1em"
              width="1em"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M205.41,151.07a60.9,60.9,0,0,1-31.83,8.86,71.71,71.71,0,0,1-27.36-5.66A55.55,55.55,0,0,0,136,186.51V216a8,8,0,0,1-8.53,8,8.18,8.18,0,0,1-7.47-8.25V203.31L81.38,164.69A52.5,52.5,0,0,1,63.44,168a45.82,45.82,0,0,1-23.92-6.67C17.73,148.09,6,117.62,8.27,79.79a8,8,0,0,1,7.52-7.52c37.83-2.23,68.3,9.46,81.5,31.25A46,46,0,0,1,103.74,132a4,4,0,0,1-6.89,2.43l-19.2-20.1a8,8,0,0,0-11.31,11.31l53.88,55.25c.06-.78.13-1.56.21-2.33a68.56,68.56,0,0,1,18.64-39.46l50.59-53.46a8,8,0,0,0-11.31-11.32l-49,51.82a4,4,0,0,1-6.78-1.74c-4.74-17.48-2.65-34.88,6.4-49.82,17.86-29.48,59.42-45.26,111.18-42.22a8,8,0,0,1,7.52,7.52C250.67,91.65,234.89,133.21,205.41,151.07Z"
              ></path>
            </svg>
            {{product.type}}
          </p>
          <p>
            <svg
              stroke="currentColor"
              fill="currentColor"
              stroke-width="0"
              viewBox="0 0 256 256"
              height="1em"
              width="1em"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M172,108a12,12,0,0,1-12,12H96a12,12,0,0,1,0-24h64A12,12,0,0,1,172,108Zm-12,28H96a12,12,0,0,0,0,24h64a12,12,0,0,0,0-24Zm76-12A104.11,104.11,0,0,1,132,228H47.67A19.69,19.69,0,0,1,28,208.33V124a104,104,0,0,1,208,0Zm-24,0a80,80,0,0,0-160,0v80h80A80.09,80.09,0,0,0,212,124Z"
              ></path>
            </svg>
            {{product.description}}
          </p>
          <p>
            <svg
              stroke="currentColor"
              fill="currentColor"
              stroke-width="0"
              viewBox="0 0 288 512"
              height="1em"
              width="1em"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M209.2 233.4l-108-31.6C88.7 198.2 80 186.5 80 173.5c0-16.3 13.2-29.5 29.5-29.5h66.3c12.2 0 24.2 3.7 34.2 10.5 6.1 4.1 14.3 3.1 19.5-2l34.8-34c7.1-6.9 6.1-18.4-1.8-24.5C238 74.8 207.4 64.1 176 64V16c0-8.8-7.2-16-16-16h-32c-8.8 0-16 7.2-16 16v48h-2.5C45.8 64-5.4 118.7.5 183.6c4.2 46.1 39.4 83.6 83.8 96.6l102.5 30c12.5 3.7 21.2 15.3 21.2 28.3 0 16.3-13.2 29.5-29.5 29.5h-66.3C100 368 88 364.3 78 357.5c-6.1-4.1-14.3-3.1-19.5 2l-34.8 34c-7.1 6.9-6.1 18.4 1.8 24.5 24.5 19.2 55.1 29.9 86.5 30v48c0 8.8 7.2 16 16 16h32c8.8 0 16-7.2 16-16v-48.2c46.6-.9 90.3-28.6 105.7-72.7 21.5-61.6-14.6-124.8-72.5-141.7z"
              ></path>
            </svg>
            {{product.price}} CAD
          </p>
          <div class="optionButtons">
            <router-link :to="{ name: 'edit-product', params: { id: product.id } }" class="button blue">Modifier</router-link>
            <button class="button red" @click="openModale(product)" >Supprimer</button>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showModale">
      <div class="modal">
        <div class="modal-content">
          <h3>Voulez-vous vraiment supprimer ce produit ?</h3>
          <div class="optionButtons center">
            <button class="button red" @click="deleteProduct">Supprimer</button>
            <button class="button blue" @click="backProduct">Annuler</button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import ProductDataService from '@/services/ProductDataService'
export default {
  props: ['inventory', 'removeInv'],
  data () {
    return {
      showModale: false,
      productToDelete: null
    }
  },
  methods: {
    backProduct () {
      this.showModale = false
    },
    openModale (product) {
      this.productToDelete = product
      this.showModale = true
    },
    deleteProduct () {
      console.log(this.productToDelete)
      if (this.productToDelete) {
        ProductDataService.delete(this.productToDelete.id)
          .then(() => {
            const productIndex = this.inventory.findIndex(product => product.id === this.productToDelete.id)
            this.removeInv(productIndex)
            this.productToDelete = null
            this.showModale = false
          })
          .catch(error => {
            console.error(error)
          })
      }
    }
  }
}
</script>
