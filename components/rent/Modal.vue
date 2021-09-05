<template>
  <div>
    <a-modal
      :width="900"
      :title="title"
      :visible="visible"
      :confirm-loading="confirmLoading"
      @ok="handleOk"
      @cancel="close"
    >
      <a-spin :spinning="loading">
        <a-descriptions title="Change Status" bordered>
          <a-descriptions-item label="Order Process" :span="3">
            <a-radio-group
              v-model="status"
              name="radioGroup"
              @change="onChange"
            >
              <a-radio value="WAITING" :disabled="true" style="color: #aaaba6">
                WAITING
              </a-radio>
              <a-radio
                :disabled="
                  order &&
                  (order.status === 'SHIPPING' ||
                    order.status === 'CANCEL' ||
                    order.status === 'DONE' ||
                    order.status === 'EXPIRED')
                "
                value="CONFIRM"
                style="color: #5d91ff"
              >
                CONFIRM
              </a-radio>
              <a-radio
                :disabled="
                  order &&
                  (order.status === 'DONE' ||
                    order.status === 'CANCEL' ||
                    order.status === 'WAITING' ||
                    order.status === 'EXPIRED')
                "
                value="SHIPPING"
                style="color: purple"
              >
                SHIPPING
              </a-radio>
              <a-radio value="RENTING" :disabled="true" style="color: #ab4f71">
                RENTING
              </a-radio>
              <a-radio
                :disabled="
                  order &&
                  (order.status === 'WAITING' ||
                    order.status === 'CONFIRM' ||
                    order.status === 'CANCEL' ||
                    order.status === 'EXPIRED')
                "
                value="DONE"
                style="color: green"
              >
                DONE
              </a-radio>
            </a-radio-group>
          </a-descriptions-item>
          <a-descriptions-item label="Renting Process" :span="3">
            <a-radio-group
              v-model="status"
              name="radioGroup"
              @change="onChange"
            >
              <a-radio
                :disabled="
                  order &&
                  (order.status === 'SHIPPING' || order.status === 'DONE')
                "
                value="CANCEL"
                style="color: red"
              >
                CANCEL
              </a-radio>
              <a-radio value="EXPIRED" :disabled="true" style="color: orange">
                EXPIRED
              </a-radio>
            </a-radio-group>
          </a-descriptions-item>
        </a-descriptions>
        <a-descriptions title="Order Info" bordered>
          <a-descriptions-item label="Order Code" :span="3">
            PS - {{ order ? order.orderRentName : '' }}
          </a-descriptions-item>
          <a-descriptions-item label="Customer Name" :span="3">
            {{ order ? order.user.username : '' }}
          </a-descriptions-item>
          <a-descriptions-item label="Rent StartDate" :span="2">
            <span>
              {{
                order ? $moment(order.rentalStart).format('YYYY-MM-DD') : ''
              }}</span
            >
          </a-descriptions-item>
          <a-descriptions-item label="Total Rent Days" :span="2">
            {{ order ? order.bookingDate : '' }}
          </a-descriptions-item>
          <a-descriptions-item label="Rent EndDate" :span="2">
            <span
              v-if="
                (order && order.status === 'DONE') ||
                (order && order.status === 'SHIPPING')
              "
            >
              {{
                order ? $moment(order.rentalEnd).format('YYYY-MM-DD') : ''
              }}</span
            >
            <div v-else>
              <a-date-picker
                v-if="order"
                v-model="order.rentalEnd"
                @change="handleChangeRentalEnd"
              />
            </div>
          </a-descriptions-item>
          <a-descriptions-item label="Email Address" :span="2">
            {{ order ? order.email : '' }}
          </a-descriptions-item>
          <a-descriptions-item label="Phone Number" :span="2">
            {{ order ? order.phone : '' }}
          </a-descriptions-item>
          <a-descriptions-item label="Shipping Address" :span="2">
            {{ order ? order.shippingAddress : '' }}
          </a-descriptions-item>
          <a-descriptions-item label="Status" :span="2">
            <a-badge
              :status="setStatus(order ? order.status : '')"
              :text="order ? order.status : ''"
            />
          </a-descriptions-item>
          <a-descriptions-item label="Payment Type" :span="2">
            {{ order ? order.paymentType : '' }}
          </a-descriptions-item>
          <a-descriptions-item label="Grand Total" :span="3">
            {{ formatPrice(order ? order.totalPrice : 0) }}
          </a-descriptions-item>
          <a-descriptions-item label="Extra Rent Money" :span="3">
            <span>{{ formatPrice(extraMoney) }}</span>
          </a-descriptions-item>
          <a-descriptions-item label="Product Info">
            <a-tag
              v-for="(item, index) in order ? order.products : []"
              :key="index"
              color="#D48459"
            >
              {{ item.productName }}
            </a-tag>
          </a-descriptions-item>
        </a-descriptions>
      </a-spin>
    </a-modal>
  </div>
</template>
<script>
export default {
  data() {
    return {
      labelCol: { span: 7 },
      wrapperCol: { span: 17 },
      visible: false,
      confirmLoading: false,
      loading: false,
      orderRentID: '',
      title: '',
      action: '',
      order: null,
      orderUpdate: null,
      status: '',
      isCanChange: false,
      extraMoney: 0,
    }
  },
  mounted() {
    console.log('abc')
  },
  methods: {
    async showEdit(id) {
      try {
        this.loading = true
        this.action = 'edit'
        this.title = 'Order Details'
        this.orderRentID = id
        this.visible = true
        const result = await this.$api.getRent(id, {
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        const productEdit = []
        // eslint-disable-next-line no-unused-expressions
        result.products.length
          ? result.products.forEach((item) => {
              productEdit.push(item.productID)
            })
          : []
        const comboEdit = []
        // eslint-disable-next-line no-unused-expressions
        result.combos.length
          ? result.combos.forEach((item) => {
              comboEdit.push(item.comboID)
            })
          : []
        this.order = {
          orderRentName: result.orderRentName,
          phone: result.phone,
          rentalStart: result.rentalStart,
          rentalEnd: result.rentalEnd,
          bookingDate: result.bookingDate,
          shippingAddress: result.shippingAddress,
          email: result.email,
          note: result.note,
          paymentType: result.paymentType,
          totalPrice: result.totalPrice,
          status: this.$moment(result.rentalEnd).isBefore(this.$moment())
            ? 'EXPIRED'
            : result.status,
          user: result.user,
          products: result.products,
          combos: result.combos,
          createdAt: result.createdAt,
          updateAt: result.updateAt,
        }
        this.orderUpdate = {
          orderRentID: result.orderRentID,
          orderRentName: result.orderRentName,
          phone: result.phone,
          rentalStart: result.rentalStart,
          rentalEnd: result.rentalEnd,
          bookingDate: result.bookingDate,
          shippingAddress: result.shippingAddress,
          lastName: result.lastName,
          firstName: result.firstName,
          totalQuantity: result.totalQuantity,
          combo: comboEdit,
          product: productEdit,
          userID: result.user.userID,
          email: result.email,
          note: result.note,
          paymentType: result.paymentType,
          totalPrice: result.totalPrice,
          status: this.$moment(result.rentalEnd).isBefore(this.$moment())
            ? 'EXPIRED'
            : result.status,
          createAt: result.createAt,
          updateAt: new Date(),
        }
        this.status = this.$moment(result.rentalEnd).isBefore(this.$moment())
          ? 'EXPIRED'
          : result.status
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        this.loading = false
      }
    },
    handleOk(e) {
      this.$confirm({
        title: 'Update Order ? ',
        content: 'Do you sure want to update order ? ',
        onOk: () => this.updateOrder(),
      })
    },

    handleChangeRentalEnd(date) {
      if (this.order && date !== null) {
        const oldPrice = this.order.totalPrice / this.order.bookingDate
        const oldBookingDate = this.order.bookingDate
        this.order.rentalEnd = date
        this.orderUpdate.rentalEnd = date
        this.order.bookingDate = date.diff(
          this.$moment(this.order.rentalStart),
          'days'
        )
        if (this.$moment(this.orderUpdate.rentalEnd).isAfter(this.$moment())) {
          this.orderUpdate.status = 'RENTING'
        }
        this.order.totalPrice =
          oldPrice * date.diff(this.$moment(this.order.rentalStart), 'days')
        this.orderUpdate.totalPrice = this.order.totalPrice
        this.orderUpdate.bookingDate = this.order.bookingDate
        this.extraMoney = oldPrice * (this.order.bookingDate - oldBookingDate)
      }
    },
    async updateOrder() {
      try {
        this.confirmLoading = true
        await this.$api.updateRent(this.orderRentID, this.orderUpdate, {
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        this.$message.success(`Update Order Successfully!`)
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        this.confirmLoading = false
        this.visible = false
        this.$emit('refreshOrder')
      }
    },
    close(e) {
      this.visible = false
      this.isCanChange = false
    },
    formatPrice(money) {
      return new Intl.NumberFormat('vi-VN', {
        style: 'currency',
        currency: 'VND',
      }).format(money)
    },
    setStatus(text) {
      let status = ''
      if (text === 'WAITING') {
        status = 'default'
      } else if (text === 'CONFIRM') {
        status = 'processing'
      } else if (text === 'SHIPPING') {
        status = 'warning'
      } else if (text === 'DONE') {
        status = 'success'
      } else {
        status = 'error'
      }
      return status
    },
    onChange(e) {
      this.status = e.target.value
      this.orderUpdate.status = e.target.value
    },
  },
}
</script>
