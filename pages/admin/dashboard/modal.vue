<template>
  <a-modal
    v-model="visible"
    :width="1400"
    title="Order List"
    :ok-button-props="{ props: { disabled: true } }"
    :cancel-button-props="{ props: { disabled: true } }"
  >
    <div style="width: 100%; display: flex; padding-bottom: 20px">
      <a-icon
        style="font-size: 20px; cursor: pointer; color: purple; padding: 0 10px"
        type="car"
      />
      SHIPPING
      <a-icon
        style="font-size: 20px; cursor: pointer; color: red; padding: 0 10px"
        type="close-circle"
      />
      CANCEL
      <a-icon
        style="
          font-size: 20px;
          display: flex;
          justify-content: center;
          align-items: center;
          cursor: pointer;
          color: green;
          padding: 0 10px;
        "
        type="check-circle"
      />
      DONE
      <a-icon
        style="font-size: 20px; cursor: pointer; color: blue; padding: 0 10px"
        type="check"
      />
      CONFIRM
    </div>

    <a-spin :spinning="loading">
      <a-table :columns="columns" :data-source="ordersRender">
        <a-tag slot="status" slot-scope="text" :color="setStatusColor(text)">
          {{ text }}</a-tag
        >
        <span slot="code" slot-scope="text">PS - {{ text }}</span>
        <span
          slot="user"
          slot-scope="text"
          style="text-transform: capitalize"
          >{{ text.username }}</span
        >
        <span slot="price" slot-scope="text">{{ formatPrice(text) }}</span>
        <div
          slot="products"
          slot-scope="record"
          style="max-width: 250px; display: flex; flex-wrap: wrap"
        >
          <a-tag
            v-for="(item, index) in record"
            :key="index"
            style="margin: 5px"
            color="#D48459"
          >
            {{ item.productName }}
          </a-tag>
        </div>
        <span slot="action" slot-scope="record">
          <template v-if="record.status === 'CONFIRM'">
            <a-icon
              style="
                font-size: 20px;
                cursor: pointer;
                color: purple;
                padding-right: 10px;
              "
              type="car"
              @click="confirmBeforeChange(record, 'SHIPPING')"
            />
            <a-icon
              style="font-size: 20px; cursor: pointer; color: red"
              type="close-circle"
              @click="confirmBeforeChange(record, 'CANCEL')"
            />
          </template>
          <template v-if="record.status === 'SHIPPING'">
            <a-icon
              style="
                font-size: 20px;
                display: flex;
                justify-content: center;
                align-items: center;
                cursor: pointer;
                color: green;
              "
              type="check-circle"
              @click="confirmBeforeChange(record, 'DONE')"
            />
          </template>
          <template v-if="record.status === 'WAITING'">
            <a-icon
              style="
                font-size: 20px;
                cursor: pointer;
                color: blue;
                padding-right: 10px;
              "
              type="check"
              @click="confirmBeforeChange(record, 'CONFIRM')"
            />
            <a-icon
              style="font-size: 20px; cursor: pointer; color: red"
              type="close-circle"
              @click="confirmBeforeChange(record, 'CANCEL')"
            />
          </template>
        </span>
      </a-table>
    </a-spin>
  </a-modal>
</template>
<script>
const columns = [
  {
    title: 'Customer Name',
    dataIndex: 'user',
    key: 'user',
    scopedSlots: { customRender: 'user' },
  },
  {
    title: 'Phone Number',
    dataIndex: 'phone',
    key: 'phone',
  },
  {
    title: 'Email Address',
    dataIndex: 'email',
    key: 'email',
  },
  {
    title: 'Grand Total',
    dataIndex: 'totalPrice',
    key: 'totalPrice',
    scopedSlots: { customRender: 'price' },
  },
  {
    title: 'Payment Type',
    dataIndex: 'paymentType',
    key: 'paymentType',
  },
  {
    title: 'Status',
    dataIndex: 'status',
    key: 'status',
    scopedSlots: { customRender: 'status' },
  },
  {
    title: 'Product List',
    dataIndex: 'products',
    key: 'products',
    scopedSlots: { customRender: 'products' },
  },
  {
    title: '',
    key: 'action',
    scopedSlots: { customRender: 'action' },
  },
]
export default {
  data() {
    return {
      loading: false,
      columns,
      visible: false,
      ordersRender: [],
    }
  },
  methods: {
    showModal(apiPath, status) {
      this.getOrderWaiting(apiPath, status)
      this.visible = true
    },
    confirmBeforeChange(order, status) {
      this.$confirm({
        title: 'Are you sure to confirm this status ? ',
        content: 'Please check before change status ?',
        okText: 'Submit',
        cancelText: 'Cancel',
        onOk: () => {
          this.handleClick(order, status)
          this.visible = false
        },
      })
    },
    async getOrderWaiting(apiPath, status) {
      if (apiPath === 'order') {
        try {
          this.loading = true
          const result = await this.$api.filterOrderByStatus(status, {
            headers: {
              Authorization: this.$auth.$storage.getUniversal('token').token,
            },
          })
          this.ordersRender = result.content
        } catch (e) {
          if (e.response.data) {
            this.$message.warning(e.response.data.details)
          }
        } finally {
          this.loading = false
        }
      } else {
        try {
          this.orderRender = []
          this.loading = true
          const result = await this.$api.filterRentByStatus(status, {
            headers: {
              Authorization: this.$auth.$storage.getUniversal('token').token,
            },
          })
          this.ordersRender = result.content
        } catch (e) {
          if (e.response.data) {
            this.$message.warning(e.response.data.details)
          }
        } finally {
          this.loading = false
        }
      }
    },
    handleOk(e) {
      this.visible = false
    },
    async handleClick(order, status) {
      if (order.orderRentID !== undefined) {
        try {
          this.loading = true
          const orderRentUpdate = { ...order }
          orderRentUpdate.status = status
          const productEdit = []
          // eslint-disable-next-line no-unused-expressions
          orderRentUpdate.products.length
            ? orderRentUpdate.products.forEach((item) => {
                productEdit.push(item.productID)
              })
            : []
          const comboEdit = []
          // eslint-disable-next-line no-unused-expressions
          orderRentUpdate.combos.length
            ? orderRentUpdate.combos.forEach((item) => {
                comboEdit.push(item.comboID)
              })
            : []
          const rent = {
            orderRentID: orderRentUpdate.orderRentID,
            orderRentName: orderRentUpdate.orderRentName,
            phone: orderRentUpdate.phone,
            rentalStart: orderRentUpdate.rentalStart,
            rentalEnd: orderRentUpdate.rentalEnd,
            bookingDate: orderRentUpdate.bookingDate,
            shippingAddress: orderRentUpdate.shippingAddress,
            lastName: orderRentUpdate.lastName,
            firstName: orderRentUpdate.firstName,
            totalQuantity: orderRentUpdate.totalQuantity,
            combo: comboEdit,
            product: productEdit,
            userID: orderRentUpdate.user.userID,
            email: orderRentUpdate.email,
            note: orderRentUpdate.note,
            paymentType: orderRentUpdate.paymentType,
            totalPrice: orderRentUpdate.totalPrice,
            status: this.$moment(orderRentUpdate.rentalEnd).isBefore(
              this.$moment()
            )
              ? 'EXPIRED'
              : orderRentUpdate.status,
            createAt: orderRentUpdate.createAt,
            updateAt: new Date(),
          }
          await this.$api.updateRent(rent.orderRentID, rent, {
            headers: {
              Authorization: this.$auth.$storage.getUniversal('token').token,
            },
          })
        } catch (e) {
          if (e.response.data) {
            this.$message.warning(e.response.data.details)
          }
        } finally {
          this.loading = false
          this.$emit('refreshData')
        }
      }
      if (order.orderID !== undefined) {
        try {
          this.loading = true
          // eslint-disable-next-line no-use-before-define
          const orderUpdate = { ...order }
          orderUpdate.status = status
          const productOrderEdit = []
          // eslint-disable-next-line no-unused-expressions
          orderUpdate.products.length
            ? orderUpdate.products.forEach((item) => {
                productOrderEdit.push(item.productID)
              })
            : []
          const comboOrderEdit = []
          // eslint-disable-next-line no-unused-expressions
          orderUpdate.combos.length
            ? orderUpdate.combos.forEach((item) => {
                comboOrderEdit.push(item.comboID)
              })
            : []
          const orderEdit = {
            orderID: orderUpdate.orderID,
            orderName: orderUpdate.orderName,
            phone: orderUpdate.phone,
            shippingAddress: orderUpdate.shippingAddress,
            lastName: orderUpdate.lastName,
            firstName: orderUpdate.firstName,
            totalQuantity: orderUpdate.totalQuantity,
            combo: comboOrderEdit,
            product: productOrderEdit,
            userID: orderUpdate.user.userID,
            email: orderUpdate.email,
            note: orderUpdate.note,
            paymentType: orderUpdate.paymentType,
            totalPrice: orderUpdate.totalPrice,
            status: orderUpdate.status,
            createAt: orderUpdate.createAt,
            updateAt: new Date(),
          }
          await this.$api.updateOrder(orderEdit.orderID, orderEdit, {
            headers: {
              Authorization: this.$auth.$storage.getUniversal('token').token,
            },
          })
        } catch (e) {
          if (e.response.data) {
            this.$message.warning(e.response.data.details)
          }
        } finally {
          this.loading = false
          this.$emit('refreshData')
        }
      }
    },
    editOrder(id) {
      this.$emit('editOrder', id)
    },
    formatPrice(money) {
      return new Intl.NumberFormat('vi-VN', {
        style: 'currency',
        currency: 'VND',
      }).format(money)
    },
    setStatusColor(text) {
      let color = ''
      if (text === 'WAITING') {
        color = '#ebc638'
      } else if (text === 'CONFIRM') {
        color = 'purple'
      } else if (text === 'SHIPPING') {
        color = 'blue'
      } else if (text === 'DONE') {
        color = 'green'
      } else {
        color = 'red'
      }
      return color
    },
  },
}
</script>
