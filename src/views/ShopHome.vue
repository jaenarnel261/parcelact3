<template>
  <div class="container">
    <!-- Item Listings -->
    <div class="content">
      <span v-if="isLoggedIn">Welcome, User!</span>
      <div>
        <span>Log in: </span>
        <button @click="handleClick">{{ isLoggedIn ? 'Logout' : 'Login' }}</button>
        <h2>Items</h2>
        <div class="item-list" v-if="products.length > 0">
          <div v-for="item in products" :key="item.id" class="item-container">
            <div class="item-info">
              <img :src="item.image" alt="Item Image" style="width: 100px; height: 100px;" />
              <p>{{ item.name }} - ₱{{ item.price }}</p>
            </div>
            <div class="add-to-cart-btn">
              <button @click="addToCart(item)">Add to Cart</button>
            </div>
          </div>
        </div>
        <div v-else>
          No items available.
        </div>
      </div>
      <div v-if="isLoggedIn">
        <h3>Cart</h3>
        <p>Total Price: ₱{{ totalPrice }}</p>
        <ul>
          <li v-for="(cartItem, index) in cart" :key="index">
            {{ cartItem.product.name }} - ₱{{ cartItem.product.price }} (Quantity: {{ cartItem.quantity }})
          </li>
        </ul>
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
      cart: [],
      isLoggedIn: false // Assuming initially the user is not logged in
    };
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((total, item) => {
        return total + item.product.price * item.quantity;
      }, 0);
    }
  },
  methods: {
    addToCart(item) {
      if (this.isLoggedIn) {
        const foundIndex = this.cart.findIndex((cartItem) => cartItem.product.id === item.id);
        if (foundIndex !== -1) {
          this.cart[foundIndex].quantity++;
        } else {
          this.cart.push({ product: item, quantity: 1 });
        }
      } else {
        const confirmed = window.confirm('Please log in to add items to your cart. Would you like to log in now?');
        if (confirmed) {
          localStorage.setItem('token', '12345');
          this.isLoggedIn = true;
          this.$router.push({ name: 'shop' });
          setTimeout(() => {
            window.confirm('You are now logged in. You can add items to your cart.');
          }, 100);
        }
      }
    },
    handleClick() {
      if (this.isLoggedIn) {
        localStorage.removeItem('token');
        this.isLoggedIn = false;
        this.cart = []; // Clear the cart on logout
      } else {
        localStorage.setItem('token', '12345');
        this.isLoggedIn = true;
        this.$router.push({ name: 'shop' });
        setTimeout(() => {
          window.confirm('You are now logged in. You can add items to your cart.');
        }, 100);
      }
    }
  }
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Adjust as needed */
}

.content {
  text-align: center;
}

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
  width: 200px; /* Adjust item container width */
}

.item-info {
  text-align: center;
}

.add-to-cart-btn {
  margin-top: 10px;
}
</style>
