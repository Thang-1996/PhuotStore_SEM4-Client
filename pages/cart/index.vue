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
      <tbody v-if="cart.length">
        <tr v-for="(item, index) in cart" :key="index">
          <td class="remove">
            <button
              type="button"
              class="close-btn close-danger remove-from-cart"
              @click="remove(item)"
            >
              <span></span>
              <span></span>
            </button>
          </td>
          <td data-title="Product">
            <div class="sigma_cart-product-wrapper">
              <img :src="JSON.parse(item.product.images)[0]" alt="prod1" />
              <div class="sigma_cart-product-body">
                <h6>
                  <a href="#">{{ item.product.productName }}</a>
                </h6>
              </div>
            </div>
          </td>
          <td data-title="Price">
            <strong v-if="selectType === 'buy'">{{
              formatPrice(item.product.price)
            }}</strong>
            <strong v-if="selectType === 'rent'">{{
              formatPrice(item.product.rental)
            }}</strong>
          </td>
          <td class="quantity" data-title="Quantity">
            <input
              v-model="item.quantity"
              type="number"
              class="qty form-control"
              @change="onChange(item)"
            />
          </td>
          <td data-title="Total">
            <strong v-if="selectType === 'buy'">{{
              formatPrice(item.product.price * item.quantity)
            }}</strong>
            <strong v-if="selectType === 'rent'">{{
              formatPrice(item.product.rental * item.quantity)
            }}</strong>
          </td>
        </tr>
      </tbody>
      <span
        v-else
        style="
          margin-top: 20px;
          width: 100%;
          display: flex;
          justify-content: center;
          align-items: center;
        "
      >
        Empty Cart Item
      </span>
    </table>
    <!--    <div class="row">-->
    <!--      <div class="col-lg-6">-->
    <!--        <a-range-picker @change="onChange" />-->
    <!--      </div>-->
    <!--    </div>-->
    <a-radio-group v-model="selectType" @change="onChangeRadio">
      <a-radio value="rent"> Rent </a-radio>
      <a-radio value="buy"> Buy </a-radio>
    </a-radio-group>
    <div class="row">
      <div class="col-lg-6">
        <div class="form-group mb-0">
          <div class="input-group mb-0">
            <div class="input-group-append">
              <input
                v-if="selectType === 'buy'"
                type="text"
                class="form-control"
                placeholder="Enter Coupon Code"
                aria-label="Coupon Code"
              />
              <a-range-picker
                v-if="selectType === 'rent'"
                @change="onDateChange"
              />
            </div>
          </div>
        </div>
      </div>
      <div class="col-lg-6" style="display: flex; justify-content: flex-end">
        <div class="form-group mb-0">
          <div class="input-group mb-0">
            <div>
              <button
                class="d-none d-lg-block sigma_btn-custom"
                type="button"
                @click="goToCheckOut()"
              >
                CheckOut
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
      rentDate: {},
      selectType: 'buy',
    }
  },
  async created() {
    await this.getCart()
  },
  methods: {
    getCart() {
      if (process.browser) {
        const cart = localStorage.getItem('cart')
          ? JSON.parse(localStorage.getItem('cart'))
          : []
        this.cart = [...cart]
      }
    },
    goToCheckOut() {
      if (Object.keys(this.rentDate).length) {
        this.$router.push({
          name: 'checkout',
          params: { rentDate: this.rentDate },
        })
      } else {
        this.$router.push('/checkout')
      }
    },
    goToCheckOutRent() {},
    formatPrice(money) {
      return new Intl.NumberFormat('vi-VN', {
        style: 'currency',
        currency: 'VND',
      }).format(money)
    },
    onChangeRadio(e) {
      this.selectType = e.target.value
    },
    onChange(item) {
      ;[...this.cart].map((cart) => {
        return cart.product.productID === item.product.productID
      })
      if (process.browser) {
        localStorage.setItem('cart', JSON.stringify(this.cart))
      }
    },
    onDateChange(date, dateString) {
      this.rentDate.startDate = dateString[0]
      this.rentDate.endDate = dateString[1]
    },
    remove(item) {
      this.cart = [...this.cart].filter((cart) => {
        return cart.product.productID !== item.product.productID
      })
      if (process.browser) {
        localStorage.setItem('cart', JSON.stringify(this.cart))
      }
    },
  },
}
</script>
<style lang="scss">
.ant-calendar-picker-input {
  height: 59px;
}
</style>
