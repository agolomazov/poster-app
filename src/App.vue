<template>
  <div id="app">
    <div class="header">
      <h1>Vus.js Poster Store</h1>
    </div>
    <div class="main">
      <div class="products">
        <div
          v-for="(item, index) in items"
          :key="item.title"
          class="product"
        >
          <h4 class="product-title">{{ item.title }}</h4>
          <button
            @click="addItem(index)"
            class="add-to-cart btn"
          >Add to Cart</button>
        </div>
      </div>
      <div class="cart">
        <h2>Shopping Cart</h2>
        <ul v-if="cart.length">
          <li
            v-for="item in cart"
            :key="item.title"
            class="cart-item"
          >
            <div class="item-title">
              {{ item.title }}
            </div>
            <span class="item-qty">{{ item.price | currency }} x {{ item.qty }}</span>
            <button class="btn" @click="inc(item)">+</button>
            <button class="btn" @click="dec(item)">-</button>
          </li>
        </ul>
        <div v-if="cart.length">
          <div>Total: {{ total }}</div>
        </div>
        <div v-else>
          No items in the cart
        </div>
      </div>
    </div>
    <!--<router-view/>-->
  </div>
</template>

<script>
const PRICE = 9.95;

export default {
  name: 'App',
  data() {
    return {
      total: 0,
      items: [
        { id: 1, title: 'title #1' },
        { id: 2, title: 'title #2' },
        { id: 3, title: 'title #3' },
      ],
      cart: [],
    };
  },
  methods: {
    addItem(index) {
      this.total += PRICE;
      const item = this.items[index];
      let found = false;
      for (let i = 0; i < this.cart.length; i += 1) {
        if (this.cart[i].id === item.id) {
          found = true;
          this.cart[i].qty += 1;
        }
      }
      if (!found) {
        this.cart.push({
          ...item,
          qty: 1,
          price: PRICE,
        });
      }
    },
    inc(item) {
      item.qty += 1;
      this.total += PRICE;
    },
    dec(item) {
      this.total -= PRICE;
      item.qty -= 1;

      if (item.qty === 0) {
        let index = this.cart.findIndex((el, index) => {
          if (el.id === item.id) {
            return index;
          }
        });
        this.cart.splice(index, 1);
      }
    },
  },
  filters: {
    currency(value) {
      return '$'.concat(value.toFixed(2));
    },
  },
};
</script>

