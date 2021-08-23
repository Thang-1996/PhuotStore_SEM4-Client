<template>
  <a-spin :spinning="loading">
    <h4 style="margin-top: 25px; color: #d48459">Purchase statistics</h4>
    <a-row :gutter="16">
      <a-col :span="6">
        <a-card title="All Order" :bordered="false">
          <p>{{ orders }}</p>
        </a-card>
      </a-col>
      <a-col :span="6">
        <a-card title="Order Successfully" :bordered="false">
          <p>{{ ordersSuccessfully }}</p>
        </a-card>
      </a-col>
      <a-col :span="6">
        <a-card title="Order Cancel" :bordered="false">
          <p>{{ ordersCancel }}</p>
        </a-card>
      </a-col>
      <a-col :span="6">
        <a-card title="Revenue" :bordered="false">
          <p>{{ formatPrice(revenue) }}</p>
        </a-card>
      </a-col>
    </a-row>
    <h4 style="margin-top: 25px; color: #d48459">Rent statistics</h4>
    <a-row :gutter="16">
      <a-col :span="6">
        <a-card title="All Order" :bordered="false">
          <p>{{ rentorders }}</p>
        </a-card>
      </a-col>
      <a-col :span="6">
        <a-card title="Order Successfully" :bordered="false">
          <p>{{ rentordersSuccessfully }}</p>
        </a-card>
      </a-col>
      <a-col :span="6">
        <a-card title="Order Cancel" :bordered="false">
          <p>{{ rentordersCancel }}</p>
        </a-card>
      </a-col>
      <a-col :span="6">
        <a-card title="Revenue" :bordered="false">
          <p>{{ formatPrice(rentrevenue) }}</p>
        </a-card>
      </a-col>
    </a-row>
  </a-spin>
</template>
<script>
export default {
  layout: 'admin',
  data() {
    return {
      loading: false,
      orders: '',
      ordersSuccessfully: '',
      ordersCancel: '',
      revenue: 0,
      rentorders: '',
      rentordersSuccessfully: '',
      rentordersCancel: '',
      rentrevenue: 0,
    }
  },
  async created() {
    await this.loadPage()
    await this.loadPageRent()
  },
  methods: {
    async loadPage() {
      try {
        this.loading = true
        const result = await this.$api.orderList({
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        console.log(result)
        this.orders = result.content.length
        this.ordersSuccessfully = result.content.filter(
          (item) => item.status === 'DONE'
        ).length
        this.ordersCancel = result.content.filter(
          (item) => item.status === 'CANCEL'
        ).length
        result.content.forEach((item) => {
          const totalPrice = Number(item.totalPrice)
          this.revenue += totalPrice
        })
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        this.loading = false
      }
    },
    async loadPageRent() {
      try {
        this.loading = true
        const result = await this.$api.rentList({
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        this.rentorders = result.content.length
        this.rentordersSuccessfully = result.content.filter(
          (item) => item.status === 'DONE'
        ).length
        this.rentordersCancel = result.content.filter(
          (item) => item.status === 'CANCEL'
        ).length
        result.content.forEach((item) => {
          const totalPrice = Number(item.totalPrice)
          this.rentrevenue += totalPrice
        })
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        this.loading = false
      }
    },
    formatPrice(money) {
      return new Intl.NumberFormat('vi-VN', {
        style: 'currency',
        currency: 'VND',
      }).format(money)
    },
  },
}
</script>
