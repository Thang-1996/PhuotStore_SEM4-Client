<template>
  <a-spin :spinning="loading">
    <div class="category">
      <a-card title="Search" class="card-category">
        <orderFilter @search="search" />
      </a-card>
      <a-card title="Order List" class="card-category-list">
        <orderTable :orders="orders" @editOrder="editOrder" />
      </a-card>
    </div>
    <orderModal ref="modalOrder" @refreshOrder="refreshOrder" />
  </a-spin>
</template>
<script>
export default {
  layout: 'admin',
  data() {
    return {
      visible: false,
      loading: false,
      orders: [],
      columns: [],
    }
  },
  created() {
    this.loadPage()
  },
  methods: {
    async loadPage() {
      this.loading = true
      try {
        const result = await this.$api.orderList({
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        this.data = [...result.content]
        this.transformData(this.data)
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        this.loading = false
      }
    },
    search(obj) {
      const data = [...this.data].filter((item) => {
        return item.firstName.toLowerCase().includes(obj.search.toLowerCase())
      })
      this.transformData(data)
    },
    transformData(data) {
      this.orders = [...data].reduce((acc, item, index) => {
        const obj = {
          key: index + 1,
          id: item.orderID,
          orderName: item.orderName,
          status: item.status,
          totalQty: item.totalQuantity,
          totalPrice: item.totalPrice,
          note: item.note,
          firstName: item.firstName,
          lastName: item.lastName,
          email: item.email,
          address: item.shippingAddress,
          phone: item.phone,
          paymentType: item.paymentType,
          user: item.user,
          product: item.product,
          combo: item.combo,
        }
        acc.push(obj)
        return acc
      }, [])
    },
    async refreshOrder() {
      await this.loadPage()
    },
    editOrder(id) {
      this.$refs.modalOrder.showEdit(id)
    },
  },
}
</script>
