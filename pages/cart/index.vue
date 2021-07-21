<template>
  <div>
    <table class="sigma_responsive-table">
      <thead>
        <tr>
          <th class="remove-item"></th>
          <th>Product</th>
          <th>Price</th>
          <th>Quantity</th>
          <th>Total</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in cart" :key="index">
          <td class="remove">
            <button
              type="button"
              class="close-btn close-danger remove-from-cart"
            >
              <span></span>
              <span></span>
            </button>
          </td>
          <td data-title="Product">
            <div class="sigma_cart-product-wrapper">
              <img src="assets/img/products/1.jpg" alt="prod1" />
              <div class="sigma_cart-product-body">
                <h6>
                  <a href="#">{{ item.product.productName }}</a>
                </h6>
              </div>
            </div>
          </td>
          <td data-title="Price">
            <strong> {{ item.product.price }}$</strong>
          </td>
          <td class="quantity" data-title="Quantity">
            <input
              v-model="item.quantity"
              type="number"
              class="qty form-control"
            />
          </td>
          <td data-title="Total">
            <strong>{{ item.product.price * item.quantity }}$</strong>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="row">
      <div class="col-lg-5">
        <div class="form-group mb-0">
          <div class="input-group mb-0">
            <input
              type="text"
              class="form-control"
              placeholder="Enter Coupon Code"
              aria-label="Coupon Code"
            />
            <div class="input-group-append">
              <button
                class="sigma_btn-custom shadow-none"
                type="button"
                @click="goToCheckOut()"
              >
                Apply
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      cart: [],
      quantity: '',
    }
  },
  async created() {
    await this.getCart()
  },
  methods: {
    getCart() {
      const cart = localStorage.getItem('cart')
        ? JSON.parse(localStorage.getItem('cart'))
        : []
      this.cart = [...cart]
    },
    goToCheckOut() {
      this.$router.push('/checkout')
    },
  },
}
</script>
