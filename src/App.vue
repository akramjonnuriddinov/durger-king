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

<script setup lang="ts">
import { ref } from "vue"
import axios from "axios"

// Define types for categories and items
interface Category {
  name: string
  label: string
}

interface MenuItem {
  name: string
  price: number
}

const categories = ref<Category[]>([
  { name: "burgers", label: "Burgers" },
  { name: "drinks", label: "Drinks" },
  { name: "desserts", label: "Desserts" },
])

const selectedCategory = ref<string>("")
const items = ref<MenuItem[]>([])

// Function to select category and fetch items
async function selectCategory(category: string) {
  selectedCategory.value = category
  try {
    const response = await axios.get(
      `http://localhost:3000/api/menu/${category}`
    )
    items.value = response.data
  } catch (error) {
    console.error("Error fetching items:", error)
  }
}

// Function to add item to cart
async function addToCart(item: MenuItem) {
  try {
    await axios.post("http://localhost:3000/api/cart", { item })
    alert(`${item.name} added to your cart!`)
  } catch (error) {
    console.error("Error adding item to cart:", error)
  }
}
</script>
