<template>
  <div>
    <h1>DurgerKing Menu</h1>
    <button
      v-for="category in categories"
      :key="category.name"
      @click="selectCategory(category.name)"
    >
      {{ category.label }}
    </button>

    <div v-if="items.length">
      <h2>{{ selectedCategory }}</h2>
      <div v-for="item in items" :key="item.name">
        <span>{{ item.name }} - ${{ item.price }}</span>
        <button @click="addToCart(item)">Add to Cart</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"

export default {
  data() {
    return {
      categories: [
        { name: "burgers", label: "Burgers" },
        { name: "drinks", label: "Drinks" },
        { name: "desserts", label: "Desserts" },
      ],
      selectedCategory: "",
      items: [],
    }
  },
  methods: {
    async selectCategory(category) {
      this.selectedCategory = category
      const response = await axios.get(
        `http://localhost:3000/api/menu/${category}`
      )
      this.items = response.data
    },
    async addToCart(item) {
      await axios.post("http://localhost:3000/api/cart", { item })
      alert(`${item.name} added to your cart!`)
    },
  },
}
</script>
