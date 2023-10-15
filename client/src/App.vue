<template>
  <div class="container">
    <header>
      <div class="header__container">
        <h1>Tea & Vue</h1>
        <nav>
          <router-link to="/" exact active-class="active">Accueil</router-link>
          <router-link to="/catalogue" active-class="active">Produits</router-link>
          <router-link to="/about" exact active-class="active">À propos</router-link>
        </nav>
      </div>
    </header>
    <main>
      <router-view :inventory="inventory" :addInv="addInventory" :removeInv="removeInventory" :update="updateInventory" />
    </main>
    <footer>
      <h5>© 2023 <span class="font-logo">Tea & Vue</span></h5>
    </footer>
  </div>
</template>

<script>
import ProductDataService from '@/services/ProductDataService.js'
export default {
  data () {
    return {
      inventory: []
    }
  },
  methods: {
    addInventory (data) {
      this.inventory.push(data)
    },
    removeInventory (index) {
      this.inventory.splice(index, 1)
    },
    updateInventory (data, index) {
      this.inventory[index] = data
    }
  },
  mounted () {
    ProductDataService.getAll()
      .then((response) => {
        this.inventory = response.data
      })
      .catch((e) => {
        console.log(e)
      })
  }
}
</script>
