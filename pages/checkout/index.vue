<template>
  <a-spin :spinning="loading">
    <form method="post">
      <div class="row">
        <div class="col-xl-7">
          <h4 v-if="rentDate">Rent Details</h4>
          <h4 v-else>Pay Details</h4>
          <div class="row">
            <div v-if="rentDate" class="form-group col-xl-12">
              <label>Rent Time <span class="text-danger">*</span></label>
              <input
                type="text"
                :placeholder="`From Date :  ${rentDate.startDate}   -   To Date :  ${rentDate.endDate}      |        ${calcDayRent} Day Rent`"
                name="fname"
                class="form-control"
                required=""
                readonly
              />
            </div>
            <div v-if="rentDate" class="form-group col-xl-12">
              <label>Deposits <span class="text-danger">*</span></label>
              <input
                type="text"
                :placeholder="`${formatPrice(deposist)}`"
                name="fname"
                class="form-control"
                required=""
                readonly
              />
            </div>
            <div class="form-group col-xl-6">
              <label>Name <span class="text-danger">*</span></label>
              <input
                v-model="billingDetails.firstName"
                type="text"
                placeholder="Name"
                name="fname"
                class="form-control"
                required=""
              />
            </div>
            <div class="form-group col-xl-6">
              <label>Phone Number <span class="text-danger">*</span></label>
              <input
                v-model="billingDetails.phone"
                type="text"
                placeholder="Phone Number"
                name="phone"
                class="form-control"
                required=""
              />
            </div>
            <div class="form-group col-xl-6">
              <label>Email Address <span class="text-danger">*</span></label>
              <input
                v-model="billingDetails.email"
                type="email"
                placeholder="Email Address"
                name="email"
                class="form-control"
                required=""
              />
            </div>
            <div class="form-group col-xl-6">
              <label>Shipping Address <span class="text-danger">*</span></label>
              <input
                v-model="billingDetails.shippingAddress"
                type="text"
                placeholder="Shipping Address"
                name="shippingAddress"
                class="form-control"
                required=""
              />
            </div>
            <div class="form-group col-xl-12 mb-0">
              <label>Order Notes</label>
              <textarea
                v-model="billingDetails.note"
                name="name"
                rows="5"
                class="form-control"
                placeholder="Order Notes (Optional)"
              ></textarea>
            </div>
          </div>
          <!-- Buyer Info End -->
        </div>
        <div class="col-xl-5 checkout-billing">
          <!-- Order Details Start -->
          <table class="sigma_responsive-table">
            <thead>
              <tr>
                <th>Product</th>
                <th>Quantity</th>
                <th>Total</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in cart" :key="index">
                <td data-title="Product">
                  <div class="sigma_cart-product-wrapper">
                    <div class="sigma_cart-product-body">
                      <h6>
                        <a href="#">{{ item.product.productName }}</a>
                      </h6>
                    </div>
                  </div>
                </td>
                <td data-title="Quantity">{{ item.quantity }}</td>
                <td data-title="Total">
                  <strong v-if="!rentDate"
                    >{{
                      formatPrice(
                        Number(item.product.price) * Number(item.quantity)
                      )
                    }}
                  </strong>
                  <strong v-else>{{
                    formatPrice(item.product.rental * item.quantity)
                  }}</strong>
                </td>
              </tr>
            </tbody>
            <tr class="total">
              <td>
                <h6 class="mb-0">Grand Total</h6>
              </td>
              <td>{{ billingDetails.totalQuantity }}</td>
              <td>
                <strong>{{ formatPrice(billingDetails.totalPrice) }}</strong>
              </td>
            </tr>
          </table>
          <div class="pay_ment" style="display: flex">
            <input
              v-model="billingDetails.paymentType"
              type="radio"
              value="PAYNOW"
              placeholder="Shipping Address"
              name="shippingAddress"
              class="form-control"
              required=""
              style="margin-top: 5px"
            />
            <label style="margin-left: 10px">PAYNOW</label>
            <input
              v-model="billingDetails.paymentType"
              type="radio"
              value="ONLINE"
              placeholder="Shipping Address"
              name="shippingAddress"
              class="form-control"
              required=""
              style="margin-top: 5px; margin-left: 100px"
            />
            <label style="margin-left: 10px">ONLINE</label>
          </div>
          <div v-if="billingDetails.paymentType !== 'PAYNOW'">
            <no-ssr>
              <paypal-checkout
                env="sandbox"
                :amount="calcUSD.toString()"
                currency="USD"
                locale="en_US"
                :button-style="myStyle"
                :client="paypal"
                :items="myItems"
                @payment-completed="payment_completed_cb"
              >
                >
              </paypal-checkout>
            </no-ssr>
          </div>
          <button
            v-else
            type="button"
            class="sigma_btn-custom primary btn-block"
            @click="payment()"
          >
            Place Order
          </button>
          <!-- Order Details End -->
        </div>
      </div>
    </form>
  </a-spin>
</template>

<script>
import { generateHash } from 'random-hash'

export default {
  name: 'index.vue',
  data() {
    return {
      loading: false,
      cart: [],
      grandTotal: 0,
      user: {},
      deposist: 0,
      calcUSD: 0,
      billingDetails: {
        orderName: '',
        note: '',
        status: 'WAITING',
        totalQuantity: 0,
        totalPrice: 0,
        firstName: '',
        lastName: '',
        email: '',
        shippingAddress: '',
        paymentType: 'PAYNOW',
        phone: '',
        userID: '',
        product: [],
        combo: [],
      },
      myItems: [],
      paypal: {
        sandbox:
          'AWz5WR3B6eRDLTiOv8l4tfCCJsPnAPgQwuhEjwSGwKDe4FBxGwhLOVfacgtI9Lvgi83mXDRnpcC7goQg',
      },
      myStyle: {
        label: 'checkout',
        size: 'responsive',
        shape: 'pill',
        color: 'gold',
      },
    }
  },
  computed: {
    userID() {
      return this.$auth.$storage.getUniversal('token').userID
        ? this.$auth.$storage.getUniversal('token').userID
        : ''
    },
    rentDate() {
      return this.$route.params.rentDate ? this.$route.params.rentDate : ''
    },
    calcDayRent() {
      return this.$route.params.rentDate
        ? this.$moment(this.$route.params.rentDate.endDate).diff(
            this.$moment(this.$route.params.rentDate.startDate),
            'days'
          )
        : ''
    },
  },
  async created() {
    await this.getCart()
  },
  methods: {
    async payment_completed_cb(res) {
      if (res.state === 'approved') {
        this.billingDetails.paymentType = 'ONLINE'
        this.billingDetails.status = 'SHIPPING'
        await this.checkout()
      }
    },
    async getUserInfo() {
      this.loading = true
      try {
        const result = await this.$api.getUser(this.userID, {
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        this.user = result
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        this.loading = false
      }
    },
    async getCart() {
      if (process.browser) {
        await this.getUserInfo()
        this.billingDetails.firstName = this.user.username
        this.billingDetails.shippingAddress = this.user.address
        this.billingDetails.phone = this.user.phone
        this.billingDetails.email = this.user.email
        const cart = localStorage.getItem('cart')
          ? JSON.parse(localStorage.getItem('cart'))
          : []
        const user = this.$auth.$storage.getUniversal('token')
        this.cart = [...cart]
        cart.forEach((item) => {
          let itemPer = 0
          if (this.rentDate) {
            itemPer = Number(item.product.rental * item.quantity)
          } else {
            itemPer = Number(item.product.price * item.quantity)
          }
          this.billingDetails.totalQuantity += Number(item.quantity)
          this.billingDetails.totalPrice += itemPer

          this.billingDetails.product.push(item.product.productID)
          this.billingDetails.userID = user.userID
          this.billingDetails.orderName = generateHash()
          this.calcUSD += item.quantity * Math.ceil(item.product.price / 23000)
          if (this.rentDate) {
            this.billingDetails.totalPrice =
              this.billingDetails.totalPrice *
              this.$moment(this.rentDate.endDate).diff(
                this.$moment(this.rentDate.startDate),
                'days'
              )
            this.deposist = item.product.rental * item.quantity * 10
          }

          const paypalItem = {
            name: item.product.productName,
            description: item.product.productDesc,
            quantity: item.quantity,
            price: Math.ceil(item.product.price / 23000).toString(),
            currency: 'USD',
          }
          this.myItems.push({ ...paypalItem })
        })
      }
    },
    payment() {
      this.$confirm({
        title: 'Are you sure to order ? ',
        content: 'Please check the billing information before order ?',
        okText: 'Submit',
        cancelText: 'Cancel',
        onOk: this.checkout,
      })
    },
    async checkout() {
      try {
        this.loading = true
        if (this.rentDate) {
          delete this.billingDetails.orderName
          this.billingDetails.orderRentName = generateHash()
          this.billingDetails.rentalStart = this.$moment(
            this.rentDate.startDate
          )
          this.billingDetails.rentalEnd = this.$moment(this.rentDate.endDate)
          this.billingDetails.bookingDate = this.$moment(
            this.rentDate.endDate
          ).diff(this.$moment(this.rentDate.startDate), 'days')
          this.billingDetails.totalPrice =
            Number(this.billingDetails.totalPrice) + Number(this.deposist)
          await this.$api.rentPlace(this.billingDetails, {
            headers: {
              Authorization: this.$auth.$storage.getUniversal('token').token,
            },
          })
        } else {
          this.billingDetails.createAt = Date.now()
          await this.$api.orderPlace(this.billingDetails, {
            headers: {
              Authorization: this.$auth.$storage.getUniversal('token').token,
            },
          })
        }
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        localStorage.removeItem('cart')
        this.loading = false
        this.$message.success(`Payment Successfully!`)
        this.resetBilling()
        if (this.rentDate) {
          await this.$router.push({
            name: 'profile',
            params: { id: this.user.userID },
            query: { type: 'rent' },
          })
        } else {
          await this.$router.push({
            name: 'profile',
            params: { id: this.user.userID },
            query: { type: 'order' },
          })
        }
      }
    },
    formatPrice(money) {
      return new Intl.NumberFormat('vi-VN', {
        style: 'currency',
        currency: 'VND',
      }).format(money)
    },
    resetBilling() {
      this.billingDetails = {
        orderName: '',
        note: '',
        status: 'WAITING',
        totalQuantity: 0,
        totalPrice: 0,
        firstName: '',
        lastName: '',
        email: '',
        shippingAddress: '',
        paymentType: 'PAYNOW',
        phone: '',
        userID: '',
        product: [],
        combo: [],
      }
    },
  },
}
</script>
