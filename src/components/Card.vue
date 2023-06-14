
<template>
    <div class="container">
      <section class="cart">
        <header class="cart-header">
          <div class="cart-header__title">наименование</div>
          <div class="cart-header__count">количество</div>
          <div class="cart-header__cost">стоимость</div>
        </header>
  
        <section v-for="(product, index) in products" :key="index" class="product">
          <div class="product__img">
            <img :src="product.image" :alt="product.title">
          </div>
          <div class="product__title">{{ product.title }}</div>
          <div class="product__count">
            <div class="count">
              <div class="count__box">
                <input type="number" class="count__input" min="1" max="100" v-model="product.count" @change="updateTotalPrice">
              </div>
              <div class="count__controls">
                <button type="button" class="count__up" @click="increase(index); updateTotalPrice()">
                  <img src="../assets/img/icons/icon-up.svg" alt="Increase">
                </button>
                <button type="button" class="count__down" @click="decrease(index); updateTotalPrice()">
                  <img src="../assets/img/icons/icon-down.svg" alt="Decrease">
                </button>
              </div>
            </div>
          </div>
          <div class="product__price">{{ priceWithCount(product) }} руб.</div>
          <div class="product__controls">
            <button type="button" @click="removeProduct(index); updateTotalPrice()">
              <img src="../assets/img/icons/cross.svg" alt="Delete">
            </button>
          </div>
        </section>
  
        <footer class="cart-footer">
          <div class="cart-footer__count">{{ totalItems }} единицы</div>
          <div class="cart-footer__price">{{ totalPrice }} руб.</div>
        </footer>
        <div class="container add-product">
          <button class='button' @click="newProduct">Add product</button>
        </div>
      </section>
    </div>
  </template>
  
  <script>
    import macbook from '@/assets/img/products/macbook.jpg';
    import apple from '@/assets/img/products/apple-watch.jpg';
    import macpro from '@/assets/img/products/mac-pro.jpg';

  export default {
    data() {
      return {
        products: [
          {
            id: 1,
            title: 'Apple MacBook Air 13',
            count: 1,
            price: 110000,
            image: macbook,
          },
          {
            id: 2,
            title: 'Apple watch',
            count: 1,
            price: 29000,
            image: apple,
          },
          {
            id: 3,
            title: 'Mac Pro',
            count: 1,
            price: 190000,
            image: macpro,
          },
        ],
        totalPrice: 0,
      };
    },
    computed: {
      totalItems() {
        return this.products.reduce((total, product) => total + product.count, 0);
      },
    },
    methods: {
      increase(index) {
        this.products[index].count++;
      },

      decrease(index) {
        if (this.products[index].count !== 1) {
          this.products[index].count--;
        }
      },

      removeProduct(index) {
        this.products.splice(index, 1);
      },

      updateTotalPrice() {
        this.totalPrice = this.products.reduce(
            (total, product) => total + product.price * product.count,
            0
        );
      },

      priceWithCount(product) {
        return product.price * product.count;
      },

      newProduct() {
      const randomProduct = this.generateRandomProduct();
      this.products.push(randomProduct);
      this.updateTotalPrice();
    },

    generateRandomProduct() {
      const randomIndex = Math.floor(Math.random() * this.products.length);
      const randomProduct = this.products[randomIndex];

      const id = this.products.length + 1;
      const title = randomProduct.title;
      const count = 1;
      const price = randomProduct.price;
      const image = randomProduct.image;

      return {
        id,
        title,
        count,
        price,
        image,
      };
    },
      
    },
  };
  </script>
  
  <style lang="scss" scoped>
  @import '../assets/styles/main.scss';
  </style>
  