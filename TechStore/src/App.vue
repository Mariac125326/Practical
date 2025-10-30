<template>
  <v-app>
    <v-app-bar color="primary" prominent>
      <v-app-bar-title class="text-h5 font-weight-bold">
        TechStore - Product Catalog
      </v-app-bar-title>
      <v-spacer></v-spacer>
      <v-badge class="mr-3" :content="cartItemCount" color="error" overlap>
        <v-icon size="large">mdi-cart</v-icon>
      </v-badge>
    </v-app-bar>

    <v-main>
      <v-container fluid>
        <v-row>
          <!-- Products Section -->
          <v-col cols="12" md="8">
            <v-card>
              <v-card-title class="text-h5">Available Products</v-card-title>
              <v-card-text>
                <v-row>
                  <v-col v-for="product in products" :key="product.id" cols="12" sm="6" md="4">
                    <v-card elevation="2">
                      <v-card-title>{{ product.name }}</v-card-title>
                      <v-card-subtitle>
                        R{{ product.price.toFixed(2) }}
                      </v-card-subtitle>
                      <v-card-text>
                        <div class="text-caption">
                          Stock: {{ product.stock }} units
                        </div>
                      </v-card-text>
                      <v-card-actions>
                        <v-btn color="primary" :disabled="product.stock === 0" @click="addToCart(product)">
                          Add to Cart
                        </v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-col>
                </v-row>
              </v-card-text>
            </v-card>
          </v-col>

          <!-- Cart Section -->
          <v-col cols="12" md="4">
            <v-card>
              <v-card-title class="text-h5">Shopping Cart</v-card-title>
              <v-card-text>
                <v-list v-if="cart.length > 0">
                  <v-list-item v-for="item in cart" :key="item.id" class="mb-2">
                    <v-list-item-title>{{ item.name }}</v-list-item-title>
                    <v-list-item-subtitle>
                      R{{ item.price.toFixed(2) }} x {{ item.quantity }}
                    </v-list-item-subtitle>
                    <template v-slot:append>
                      <div class="text-subtitle-1 font-weight-bold">
                        R{{ (item.price * item.quantity).toFixed(2) }}
                      </div>
                    </template>
                  </v-list-item>
                </v-list>
                <v-alert v-else type="info" variant="tonal">
                  Your cart is empty
                </v-alert>

                <v-divider class="my-4"></v-divider>

                <!-- Discount Code -->
                <v-text-field v-model="discountCode" label="Discount Code" placeholder="Enter SAVE10" density="compact"
                  variant="outlined" @input="applyDiscount"></v-text-field>

                <v-alert v-if="discountApplied" type="success" variant="tonal" density="compact" class="mb-4">
                  10% discount applied!
                </v-alert>

                <!-- Cart Summary -->
                <div class="text-body-1 mb-2">
                  <strong>Subtotal:</strong>
                  <span class="float-right">R{{ subtotal.toFixed(2) }}</span>
                </div>
                <div class="text-body-1 mb-2">
                  <strong>VAT (15%):</strong>
                  <span class="float-right">R{{ VAT.toFixed(2) }}</span>
                </div>
                <div v-if="discount > 0" class="text-body-1 mb-2 text-success">
                  <strong>Discount (10%):</strong>
                  <span class="float-right">-R{{ discount.toFixed(2) }}</span>
                </div>
                <v-divider class="my-2"></v-divider>
                <div class="text-h6">
                  <strong>Total:</strong>
                  <span class="float-right">R{{ total.toFixed(2) }}</span>
                </div>

                <v-btn v-if="cart.length > 0" color="success" block size="large" class="mt-4" @click="checkout">
                  Checkout
                </v-btn>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>

    <v-snackbar v-model="snackbar" :timeout="2000" color="success">
      {{ snackbarText }}
    </v-snackbar>
  </v-app>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import axios from 'axios'

const products = ref([])

const cart = ref([])
const discountCode = ref('')
const discountApplied = ref(false)
const snackbar = ref(false)
const snackbarText = ref('')
const VAT_AMOUNT = ref(15)

onMounted(() => {
  loadProducts()
})

async function loadProducts() {
  const {data} = await axios.get('http://127.0.0.1:60000/api/TechStore/GetStoreItems')
  products.value = data.data;
}

function saveProducts() {
  // create Update function in BE
}

const cartItemCount = computed(() => {
  return cart.value.reduce((total, item) => total + item.quantity, 0)
})

const subtotal = computed(() => {
  return cart.value.reduce((total, item) => total + item.price * item.quantity, 0)
})

const VAT = computed(() => {
  if (discountApplied.value) {
    return subtotal.value * (VAT_AMOUNT.value / 100)
  }
  return subtotal.value * (VAT_AMOUNT.value / 100)
})

const discount = computed(() => {
  if (discountApplied.value) {
    return subtotal.value * 0.1
  }
  return 0
})

const total = computed(() => {
  return subtotal.value + VAT.value - discount.value
})

function addToCart(product) {
  const existingItem = cart.value.find(item => item.id === product.id)

  if (existingItem) {
    existingItem.quantity++
  } else {
    cart.value.push({
      ...product,
      quantity: 1
    })
  }

  const productInList = products.value.find(p => p.id === product.id)
  if (productInList) {
    productInList.stock--
  }

  snackbarText.value = `${product.name} added to cart!`
  snackbar.value = true
}

function applyDiscount() {
  if (discountCode.value.toUpperCase() === 'SAVE10') {
    discountApplied.value = true
  } else {
    discountApplied.value = false
  }
}

function checkout() {
  snackbarText.value = 'Order placed successfully! Stock updated.'
  snackbar.value = true

  // Clear cart after a delay
  setTimeout(() => {
    cart.value = []
    discountCode.value = ''
    discountApplied.value = false
    saveProducts()
  }, 2000)
}
</script>

<style scoped>
.float-right {
  float: right;
}
</style>
