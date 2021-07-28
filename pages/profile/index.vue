<template>
  <a-spin :spinning="loading">
    <a-tabs default-active-key="user" tab-position="left" @change="tabChange">
      <a-tab-pane key="user" tab="User Info">
        <a-form-model
          :model="user"
          :label-col="labelCol"
          :wrapper-col="wrapperCol"
        >
          <a-form-model-item label="User Name">
            <a-input v-model="user.username" />
          </a-form-model-item>
          <a-form-model-item label="Password">
            <a-input v-model="user.password" />
          </a-form-model-item>
          <a-form-model-item label="Address">
            <a-input v-model="user.address" />
          </a-form-model-item>
          <a-form-model-item label="Phone">
            <a-input v-model="user.phone" />
          </a-form-model-item>
          <a-form-model-item :wrapper-col="{ span: 3, offset: 21 }">
            <a-button type="primary" @click="updateUser">
              Update User
            </a-button>
          </a-form-model-item>
        </a-form-model>
      </a-tab-pane>
      <a-tab-pane key="order" tab="Order Info" force-render>
        <a-tabs
          v-if="orderID !== ''"
          default-active-key="WAITING"
          @change="changeTab"
        >
          <a-tab-pane key="WAITING" tab="WAITING">
            <a-list
              v-if="order.status === 'WAITING'"
              item-layout="horizontal"
              :data-source="data"
            >
              <a-list-item slot="renderItem" slot-scope="item">
                <a-list-item-meta :description="item.productDesc">
                  <a slot="title">{{ item.productName }}</a>
                  <a-avatar slot="avatar" src="assets/img/products/0.jpg" />
                </a-list-item-meta>
                <div style="margin-right: 20px; color: #d48459">
                  Brand : {{ item.brand }}
                </div>
                <div style="margin-right: 20px; color: #d48459">
                  Category : {{ item.category }}
                </div>
                <div style="color: #d48459">{{ formatPrice(item.price) }}</div>
              </a-list-item>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">{{ data.length }} product</p>
                <p style="color: #d48459">
                  Total Price : {{ formatPrice(order.totalPrice) }}
                </p>
              </div>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">
                  Have you received your order, if you have received it, please
                  click the confirm button
                </p>
                <a-button type="primary" @click="updateOrder('SHIPPING')">
                  Shipping Now</a-button
                >
              </div>
            </a-list>
            <p v-else style="color: #d48459; text-align: center">
              This Status Have No Order
            </p>
          </a-tab-pane>
          <a-tab-pane key="SHIPPING" tab="SHIPPING">
            <a-list
              v-if="order.status === 'SHIPPING'"
              item-layout="horizontal"
              :data-source="data"
            >
              <a-list-item slot="renderItem" slot-scope="item">
                <a-list-item-meta :description="item.productDesc">
                  <a slot="title">{{ item.productName }}</a>
                  <a-avatar slot="avatar" src="assets/img/products/0.jpg" />
                </a-list-item-meta>
                <div style="margin-right: 20px; color: #d48459">
                  Brand : {{ item.brand }}
                </div>
                <div style="margin-right: 20px; color: #d48459">
                  Category : {{ item.category }}
                </div>
                <div style="color: #d48459">{{ formatPrice(item.price) }}</div>
              </a-list-item>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">{{ data.length }} product</p>
                <p style="color: #d48459">
                  Total Price : {{ formatPrice(order.totalPrice) }}
                </p>
              </div>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">
                  Have you received your order,click the confirm button to
                  confirm or cancel button to cancel the order
                </p>
                <a-button type="primary" @click="updateOrder('CONFIRM')">
                  CONFIRM NOW
                </a-button>
                <a-button type="danger" @click="updateOrder('CANCEL')">
                  CANCEL NOW
                </a-button>
              </div>
            </a-list>
            <p v-else style="color: #d48459; text-align: center">
              This Status Have No Order
            </p>
          </a-tab-pane>
          <a-tab-pane key="CONFIRM" tab="CONFIRM">
            <a-list
              v-if="order.status === 'CONFIRM'"
              item-layout="horizontal"
              :data-source="data"
            >
              <a-list-item slot="renderItem" slot-scope="item">
                <a-list-item-meta :description="item.productDesc">
                  <a slot="title">{{ item.productName }}</a>
                  <a-avatar slot="avatar" src="assets/img/products/0.jpg" />
                </a-list-item-meta>
                <div style="margin-right: 20px; color: #d48459">
                  Brand : {{ item.brand }}
                </div>
                <div style="margin-right: 20px; color: #d48459">
                  Category : {{ item.category }}
                </div>
                <div style="color: #d48459">{{ formatPrice(item.price) }}</div>
              </a-list-item>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">{{ data.length }} product</p>
                <p style="color: #d48459">
                  Total Price : {{ formatPrice(order.totalPrice) }}
                </p>
              </div>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">
                  Have you received your order, if you have received it, please
                  click the confirm button
                </p>
              </div>
            </a-list>
            <p v-else style="color: #d48459; text-align: center">
              This Status Have No Order
            </p>
          </a-tab-pane>
          <a-tab-pane key="CANCEL" tab="CANCEL">
            <a-list
              v-if="order.status === 'CANCEL'"
              item-layout="horizontal"
              :data-source="data"
            >
              <a-list-item slot="renderItem" slot-scope="item">
                <a-list-item-meta :description="item.productDesc">
                  <a slot="title">{{ item.productName }}</a>
                  <a-avatar slot="avatar" src="assets/img/products/0.jpg" />
                </a-list-item-meta>
                <div style="margin-right: 20px; color: #d48459">
                  Brand : {{ item.brand }}
                </div>
                <div style="margin-right: 20px; color: #d48459">
                  Category : {{ item.category }}
                </div>
                <div style="color: #d48459">{{ formatPrice(item.price) }}</div>
              </a-list-item>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">{{ data.length }} product</p>
                <p style="color: #d48459">
                  Total Price : {{ formatPrice(order.totalPrice) }}
                </p>
              </div>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">
                  Have you received your order, if you have received it, please
                  click the confirm button
                </p>
              </div>
            </a-list>
            <p v-else style="color: #d48459; text-align: center">
              This Status Have No Order
            </p>
          </a-tab-pane>
          <a-button slot="tabBarExtraContent" @click="backToList">
            <a-icon type="arrow-left" /> Back To List
          </a-button>
        </a-tabs>
        <div v-else>
          <h6 style="color: #d48459; text-align: center">ORDER LIST</h6>
          <a-table :columns="columns" :data-source="orderList">
            <span slot="action" slot-scope="record">
              <a-button type="primary" @click="editOrder(record.orderID)"
                ><a-icon type="edit"
              /></a-button>
            </span>
          </a-table>
        </div>
      </a-tab-pane>
    </a-tabs>
  </a-spin>
</template>

<script>
const columns = [
  {
    title: 'ID',
    dataIndex: 'orderID',
    key: 'orderID',
  },
  {
    title: 'Order Code',
    dataIndex: 'orderName',
    key: 'orderName',
  },
  {
    title: 'Grand Total',
    dataIndex: 'totalPrice',
    key: 'totalPrice',
  },
  {
    title: 'Total Quantity',
    dataIndex: 'totalQuantity',
    key: 'totalQuantity',
  },
  {
    title: 'Pay Date',
    dataIndex: 'createAt',
    key: 'createAt',
  },
  {
    title: 'Status',
    dataIndex: 'status',
    key: 'status',
  },
  {
    title: 'Order Note',
    dataIndex: 'note',
    key: 'note',
  },
  {
    title: 'Action',
    key: 'action',
    scopedSlots: { customRender: 'action' },
  },
]
export default {
  name: 'index.vue',
  data() {
    return {
      data: [],
      columns,
      labelCol: { span: 3 },
      wrapperCol: { span: 21 },
      user: {},
      loading: false,
      orderID: '',
      order: {},
      orderList: [],
      products: [],
      orderUpdate: {},
    }
  },
  computed: {
    userID() {
      return this.$route.params.id ? this.$route.params.id : ''
    },
  },
  async created() {
    await this.loadPage()
  },
  methods: {
    async loadPage() {
      this.loading = true
      try {
        const result = await this.$api.getUser(this.userID, {
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        this.user = result
        this.user.password = ''
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        this.loading = false
      }
    },
    async updateUser() {
      try {
        this.loading = true
        await this.$api.updateUser(this.userID, this.user, {
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
      }
    },
    tabChange(key) {
      if (key === 'order' && this.userID !== '') {
        this.getOrderList()
      } else {
        this.loadPage()
      }
    },
    backToList() {
      this.orderID = ''
      this.products = []
    },
    formatPrice(money) {
      return new Intl.NumberFormat('vi-VN', {
        style: 'currency',
        currency: 'VND',
      }).format(money)
    },
    async getOrderList() {
      try {
        this.loading = true
        const result = await this.$api.getOrderByUser(this.userID, {
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        this.orderList = [...result.content]
        console.log(result.content)
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        this.loading = false
      }
    },
    async editOrder(id) {
      this.orderID = id
      try {
        this.loading = true
        const result = await this.$api.getOrder(id, {
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        this.order = { ...result }
        this.data = [...result.products].reduce((acc, item) => {
          const product = {
            productDesc: item.productDesc,
            productName: item.productName,
            price: item.price,
            brand: item.brand.brandName,
            category: item.category.categoryName,
          }
          acc.push(product)
          return acc
        }, [])
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
        this.orderUpdate = {
          orderID: result.orderID,
          orderName: result.orderName,
          phone: result.phone,
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
          status: result.status,
          createAt: result.createAt,
          updateAt: new Date(),
        }
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        this.loading = false
      }
    },
    changeTab(key) {
      console.log(key)
    },
    async updateOrder(status) {
      try {
        this.orderUpdate.status = status
        this.loading = true
        await this.$api.updateOrder(this.orderID, this.orderUpdate, {
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
        await this.editOrder(this.orderID)
      }
    },
  },
}
</script>
