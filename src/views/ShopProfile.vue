<template>
  <div>
    <span>Log out: </span> 
    <button @click="handleLogout">Logout</button>
    <!-- Item Listings -->
    <div class="item-list">
      <h2>Items</h2>
      <div v-for="item in products" :key="item.id" class="item-container">
        <div class="item-info">
          <img :src="item.image" :alt="item.name" style="width: 100px; height: 100px;" />
          <p>{{ item.name }} - ₱{{ item.price }}</p>
          <button @click="addToCart(item)">Add to Cart</button>
        </div>
      </div>
    </div>

    <!-- Shopping Cart -->
    <div>
      <h2>Cart</h2>
      <div v-if="cart.length === 0">Your cart is empty</div>
      <div v-else>
        <div v-for="(item, index) in cart" :key="index">
          <img :src="item.product.image" :alt="item.product.name" style="width: 50px; height: 50px;" />
          <p>{{ item.product.name }} - ₱{{ item.product.price }} x {{ item.quantity }}</p>
          <p>Update</p>
          <button @click="decreaseQuantity(index)">-</button>&nbsp;
          <button @click="increaseQuantity(index)">+</button> &nbsp; | &nbsp;
          <button @click="removeFromCart(index)">Remove</button>
        </div>
        <p>Total: ₱{{ totalPrice }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: 'Parcel 1', price: 5, image: require('@/assets/1.jpg') },
        { id: 2, name: 'Parcel 2', price: 10, image: require('@/assets/2.png') },
        { id: 3, name: 'Parcel 3', price: 25, image: require('@/assets/3.jpg')}
      ],
      cart: []
    };
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((total, item) => {
        return total + (item.product.price * item.quantity);
      }, 0);
    }
  },
  methods: {
    addToCart(item) {
      const foundIndex = this.cart.findIndex(cartItem => cartItem.product.id === item.id);
      if (foundIndex !== -1) {
        this.cart[foundIndex].quantity++;
      } else {
        this.cart.push({ product: item, quantity: 1 });
      }
    },
    removeFromCart(index) {
      this.cart.splice(index, 1);
    },
    increaseQuantity(index) {
      this.cart[index].quantity++;
    },
    decreaseQuantity(index) {
      if (this.cart[index].quantity > 1) {
        this.cart[index].quantity--;
      }
    },
    handleLogout() {
      localStorage.removeItem('token')
      this.$router.push('/')
    }
  }
};
</script>

<style scoped>
.item-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.item-container {
  margin: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.item-info {
  text-align: center;
}

.add-to-cart-btn {
  margin-top: 10px;
}
</style>
