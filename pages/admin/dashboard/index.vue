<template>
  <a-spin :spinning="loading">
    <h4 style="margin-top: 25px; color: #d48459">Purchase statistics</h4>
    <a-row :gutter="16">
      <a-col :span="4">
        <a-card title="Order Successfully" :bordered="false">
          <a slot="extra" @click="showModalList('order', 'done')"
            ><a-icon type="profile" />
          </a>
          <span>{{ ordersSuccessfully }}</span>
        </a-card>
      </a-col>
      <a-col :span="4">
        <a-card title="Order Shipping" :bordered="false">
          <a slot="extra" @click="showModalList('order', 'shipping')"
            ><a-icon type="profile" />
          </a>
          <span>{{ ordersShipping }}</span>
        </a-card>
      </a-col>
      <a-col :span="4">
        <a-card title="Order Waiting" :bordered="false">
          <a slot="extra" @click="showModalList('order', 'waiting')"
            ><a-icon type="profile" />
          </a>
          <span>{{ ordersWaiting }}</span>
        </a-card>
      </a-col>
      <a-col :span="4">
        <a-card title="Order Cancel" :bordered="false">
          <a slot="extra" @click="showModalList('order', 'cancel')"
            ><a-icon type="profile" />
          </a>
          <span>{{ ordersCancel }}</span>
        </a-card>
      </a-col>
      <a-col :span="4">
        <a-card title="Order Confirm" :bordered="false">
          <a slot="extra" @click="showModalList('order', 'confirm')"
            ><a-icon type="profile" />
          </a>
          <span>{{ ordersConfirm }}</span>
        </a-card>
      </a-col>
      <a-col :span="4">
        <a-card title="Revenue" :bordered="false">
          <span>{{ formatPrice(revenue) }}</span>
        </a-card>
      </a-col>
    </a-row>
    <h4 style="margin-top: 25px; color: #d48459">Rent statistics</h4>
    <a-row :gutter="16">
      <a-col :span="4">
        <a-card title="Rent Successfully" :bordered="false">
          <a slot="extra" @click="showModalList('rent', 'done')"
            ><a-icon type="profile" />
          </a>
          <span>{{ rentordersSuccessfully }}</span>
        </a-card>
      </a-col>
      <a-col :span="4">
        <a-card title="Rent Shipping" :bordered="false">
          <a slot="extra" @click="showModalList('rent', 'shipping')"
            ><a-icon type="profile" />
          </a>
          <span>{{ rentordersShipping }}</span>
        </a-card>
      </a-col>
      <a-col :span="4">
        <a-card title="Rent Waiting" :bordered="false">
          <a slot="extra" @click="showModalList('rent', 'waiting')"
            ><a-icon type="profile" />
          </a>
          <span>{{ rentordersWaiting }}</span>
        </a-card>
      </a-col>
      <a-col :span="4">
        <a-card title="Rent Cancel" :bordered="false">
          <a slot="extra" @click="showModalList('rent', 'cancel')"
            ><a-icon type="profile" />
          </a>
          <span>{{ rentordersCancel }}</span>
        </a-card>
      </a-col>
      <a-col :span="4">
        <a-card title="Rent Confirm" :bordered="false">
          <a slot="extra" @click="showModalList('rent', 'confirm')"
            ><a-icon type="profile" />
          </a>
          <span>{{ rentorderConfirm }}</span>
        </a-card>
      </a-col>
      <a-col :span="4">
        <a-card title="Revenue" :bordered="false">
          <span>{{ formatPrice(rentrevenue) }}</span>
        </a-card>
      </a-col>
    </a-row>
    <DashboardModal ref="dashboard" @refreshData="refreshData" />
  </a-spin>
</template>
<script>
import DashBoardModal from './modal'

export default {
  components: {
    DashboardModal: DashBoardModal,
  },
  layout: 'admin',
  data() {
    return {
      loading: false,
      orders: '',
      ordersSuccessfully: '',
      ordersWaiting: '',
      ordersCancel: '',
      ordersConfirm: '',
      ordersShipping: '',
      revenue: 0,
      rentorders: '',
      rentordersSuccessfully: '',
      rentordersWaiting: '',
      rentordersShipping: '',
      rentorderConfirm: '',
      rentordersCancel: '',
      rentrevenue: 0,
      orderRender: [],
    }
  },
  async created() {
    await this.loadPage()
    await this.loadPageRent()
  },

  methods: {
    async refreshData() {
      await this.loadPage()
      await this.loadPageRent()
    },
    async loadPage() {
      try {
        this.loading = true
        const result = await this.$api.orderList({
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        this.orders = result.content.length
        this.ordersSuccessfully = result.content.filter(
          (item) => item.status === 'DONE'
        ).length
        this.ordersWaiting = result.content.filter(
          (item) => item.status === 'WAITING'
        ).length
        this.ordersShipping = result.content.filter(
          (item) => item.status === 'SHIPPING'
        ).length
        this.ordersConfirm = result.content.filter(
          (item) => item.status === 'CONFIRM'
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
        this.rentordersWaiting = result.content.filter(
          (item) => item.status === 'WAITING'
        ).length
        this.rentordersSuccessfully = result.content.filter(
          (item) => item.status === 'DONE'
        ).length
        this.rentordersCancel = result.content.filter(
          (item) => item.status === 'CANCEL'
        ).length
        this.rentordersShipping = result.content.filter(
          (item) => item.status === 'SHIPPING'
        ).length
        this.rentorderConfirm = result.content.filter(
          (item) => item.status === 'CONFIRM'
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
    showModalList(apiPath, status) {
      this.$refs.dashboard.showModal(apiPath, status)
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
