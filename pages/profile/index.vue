<template>
  <a-spin :spinning="loading">
    <a-tabs
      :default-active-key="defaultActiveTabs"
      tab-position="left"
      @change="tabChange"
    >
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
            <a-input v-model="user.password" type="password" />
          </a-form-model-item>
          <a-form-model-item label="Address">
            <a-input v-model="user.address" />
          </a-form-model-item>
          <a-form-model-item label="Phone">
            <a-input v-model="user.phone" />
          </a-form-model-item>
          <a-form-model-item label="Email">
            <a-input v-model="user.email" />
          </a-form-model-item>
          <a-form-model-item :wrapper-col="{ span: 3, offset: 21 }">
            <a-button type="primary" @click="updateUser">
              Update User
            </a-button>
          </a-form-model-item>
        </a-form-model>
      </a-tab-pane>
      <a-tab-pane key="order" tab="Order Info" force-render>
        <h6 v-if="orderID !== ''" style="color: #d48459">
          ORDER CODE : PS - {{ order.orderName }}
        </h6>
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
                  Have you want change the billing infomation please click Edit,
                  or click the cancel to cancel the order
                </p>
                <a-button type="primary" @click="visible = true">
                  Edit Order</a-button
                >
                <a-button type="danger" @click="updateOrder('CANCEL')">
                  CANCEL ORDER</a-button
                >
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
                  Your order is being prepared. Please check your shipping
                  address before we ship it. Orders during delivery cannot be
                  edited or canceled
                </p>
                <a-button type="primary" @click="visible = true">
                  Edit Order</a-button
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
                  Have you received your order, Click the order received order
                  button to confirm the order
                </p>
                <a-button type="primary" @click="updateOrder('DONE')">
                  Order received
                </a-button>
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
          <a-tab-pane key="DONE" tab="DONE">
            <a-list
              v-if="order.status === 'DONE'"
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
                  Order has been delivered successfully
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
            <a-tag
              slot="status"
              slot-scope="text"
              :color="setOrderStatusColor(text)"
            >
              {{ text }}</a-tag
            >
            <span slot="totalPrice" slot-scope="text">
              {{ formatPrice(text) }}
            </span>
            <span slot="createAt" slot-scope="text">
              {{ text }}
            </span>
            <span slot="action" slot-scope="record">
              <a-button type="primary" @click="editOrder(record.orderID)"
                ><a-icon type="edit"
              /></a-button>
            </span>
          </a-table>
        </div>
      </a-tab-pane>
      <a-tab-pane key="rent" tab="Rent Info">
        <h6 v-if="orderRentID !== ''" style="color: #d48459">
          ORDER CODE : PS - {{ orderRentID.orderRentName }}
        </h6>
        <a-tabs
          v-if="orderRentID !== ''"
          default-active-key="WAITING"
          @change="changeTab"
        >
          <a-tab-pane key="WAITING" tab="WAITING">
            <a-list
              v-if="orderRent.status === 'WAITING'"
              item-layout="horizontal"
              :data-source="dataRent"
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
                <div style="color: #d48459">
                  {{ formatPrice(item.rental) }}
                </div>
              </a-list-item>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">{{ dataRent.length }} product</p>
                <div>
                  <p style="color: #d48459">
                    Deposist :
                    {{
                      formatPrice(orderRent.totalPrice - orderRent.totalRental)
                    }}
                  </p>
                  <p style="color: #d48459">
                    Total Rental : {{ formatPrice(orderRent.totalRental) }}
                  </p>
                  <p style="color: #d48459">
                    Total Price : {{ formatPrice(orderRent.totalPrice) }}
                  </p>
                </div>
              </div>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">
                  Have you want change the billing infomation please click Edit,
                  or click the cancel to cancel the order
                </p>
                <a-button type="primary" @click="visible = true">
                  Edit Rent</a-button
                >
                <a-button type="danger" @click="updateOrderRent('CANCEL')">
                  CANCEL Rent</a-button
                >
              </div>
            </a-list>
            <p v-else style="color: #d48459; text-align: center">
              This Status Have No Order
            </p>
          </a-tab-pane>
          <a-tab-pane key="CONFIRM" tab="CONFIRM">
            <a-list
              v-if="orderRent.status === 'CONFIRM'"
              item-layout="horizontal"
              :data-source="dataRent"
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
                <div style="color: #d48459">
                  {{ formatPrice(item.rental) }}
                </div>
              </a-list-item>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">{{ dataRent.length }} product</p>
                <p style="color: #d48459">
                  Total Price : {{ formatPrice(orderRent.totalPrice) }}
                </p>
              </div>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">
                  Your order is being prepared. Please check your shipping
                  address before we ship it. Orders during delivery cannot be
                  edited or canceled
                </p>
                <a-button type="primary" @click="visible = true">
                  Edit Rent</a-button
                >
              </div>
            </a-list>
            <p v-else style="color: #d48459; text-align: center">
              This Status Have No Order
            </p>
          </a-tab-pane>
          <a-tab-pane key="SHIPPING" tab="SHIPPING">
            <a-list
              v-if="orderRent.status === 'SHIPPING'"
              item-layout="horizontal"
              :data-source="dataRent"
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
                <div style="color: #d48459">
                  {{ formatPrice(item.rental) }}
                </div>
              </a-list-item>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">{{ dataRent.length }} product</p>
                <p style="color: #d48459">
                  Total Price : {{ formatPrice(orderRent.totalPrice) }}
                </p>
              </div>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">
                  Have you received your order, Click the order received order
                  button to confirm the order
                </p>
                <a-button type="primary" @click="updateOrderRent('DONE')">
                  Rent received
                </a-button>
              </div>
            </a-list>
            <p v-else style="color: #d48459; text-align: center">
              This Status Have No Order
            </p>
          </a-tab-pane>
          <a-tab-pane key="CANCEL" tab="CANCEL">
            <a-list
              v-if="orderRent.status === 'CANCEL'"
              item-layout="horizontal"
              :data-source="dataRent"
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
                <div style="color: #d48459">
                  {{ formatPrice(item.rental) }}
                </div>
              </a-list-item>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">{{ dataRent.length }} product</p>
                <p style="color: #d48459">
                  Total Price : {{ formatPrice(orderRent.totalPrice) }}
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
          <a-tab-pane key="DONE" tab="DONE">
            <a-list
              v-if="orderRent.status === 'DONE'"
              item-layout="horizontal"
              :data-source="dataRent"
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
                <div style="color: #d48459">
                  {{ formatPrice(item.rental) }}
                </div>
              </a-list-item>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">{{ dataRent.length }} product</p>
                <p style="color: #d48459">
                  Total Price : {{ formatPrice(orderRent.totalPrice) }}
                </p>
              </div>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">
                  Order has been delivered successfully
                </p>
              </div>
            </a-list>
            <p v-else style="color: #d48459; text-align: center">
              This Status Have No Order Rent
            </p>
          </a-tab-pane>
          <a-tab-pane key="RENTING" tab="RENTING">
            <a-list
              v-if="orderRent.status === 'RENTING'"
              item-layout="horizontal"
              :data-source="dataRent"
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
                <div style="color: #d48459">
                  {{ formatPrice(item.rental) }}
                </div>
              </a-list-item>

              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">{{ dataRent.length }} product</p>
                <p style="color: #d48459">
                  Total Price : {{ formatPrice(orderRent.totalPrice) }}
                </p>
              </div>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">Order has been renting now</p>
              </div>
              <div style="display: flex; justify-content: space-between">
                <p style="color: #d48459">
                  Have you want change the billing infomation please click Edit,
                  or click the cancel to cancel the order
                </p>
                <a-button type="primary" @click="visible = true">
                  Edit Rent</a-button
                >
              </div>
            </a-list>
            <p v-else style="color: #d48459; text-align: center">
              This Status Have No Order Rent
            </p>
          </a-tab-pane>
          <a-button slot="tabBarExtraContent" @click="backToListRent">
            <a-icon type="arrow-left" /> Back To List
          </a-button>
        </a-tabs>
        <div v-else>
          <h6 style="color: #d48459; text-align: center">ORDER LIST</h6>
          <a-table :columns="columnsRent" :data-source="orderRentList">
            <a-tag
              slot="status"
              slot-scope="text"
              :color="setStatusColor(text)"
            >
              {{ text }}</a-tag
            >
            <span slot="totalPrice" slot-scope="text">
              {{ formatPrice(text) }}
            </span>
            <span slot="rentalStart" slot-scope="text">
              {{ $moment(text).format('YYYY-MM-DD') }}
            </span>
            <span slot="rentalEnd" slot-scope="text">
              {{ $moment(text).format('YYYY-MM-DD') }}
            </span>
            <span slot="bookingDate" slot-scope="text"> {{ text }} days </span>
            <span slot="action" slot-scope="record">
              <a-button
                type="primary"
                @click="editOrderRent(record.orderRentID)"
                ><a-icon type="edit"
              /></a-button>
            </span>
          </a-table>
        </div>
      </a-tab-pane>
    </a-tabs>
    <a-modal v-model="visible" title="Edit Rent Time" @ok="handleOk">
      <template v-if="dataRent.length">
        <a-form-model
          :model="orderRentUpdate"
          :label-col="{ span: 7 }"
          :wrapper-col="{ span: 17 }"
        >
          <a-form-model-item label="Rent Date End">
            <a-date-picker
              v-model="orderRentUpdate.rentalEnd"
              :disabled-date="disabledEndDate"
            />
          </a-form-model-item>
        </a-form-model>
      </template>
      <template v-else>
        <a-form-model
          :model="orderUpdate"
          :label-col="{ span: 7 }"
          :wrapper-col="{ span: 17 }"
        >
          <a-form-model-item label="Shipping Address">
            <a-input
              v-model="orderUpdate.shippingAddress"
              placeholder="Enter Shipping Address"
            />
          </a-form-model-item>
          <a-form-model-item label="Phone Number">
            <a-input
              v-model="orderUpdate.phone"
              placeholder="Enter Phone Number"
            />
          </a-form-model-item>
          <a-form-model-item label="Email Address">
            <a-input
              v-model="orderUpdate.email"
              placeholder="Enter Email Address"
            />
          </a-form-model-item>
        </a-form-model>
      </template>
    </a-modal>
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
    scopedSlots: { customRender: 'totalPrice' },
  },
  {
    title: 'Total Quantity',
    dataIndex: 'totalQuantity',
    key: 'totalQuantity',
  },

  {
    title: 'Status',
    dataIndex: 'status',
    key: 'status',
    scopedSlots: { customRender: 'status' },
  },

  {
    title: 'Action',
    key: 'action',
    scopedSlots: { customRender: 'action' },
  },
]
const columnsRent = [
  {
    title: 'ID',
    dataIndex: 'orderRentID',
    key: 'orderRentID',
  },
  {
    title: 'Order Code',
    dataIndex: 'orderRentName',
    key: 'orderRentName',
  },
  {
    title: 'Grand Total',
    dataIndex: 'totalPrice',
    key: 'totalPrice',
    scopedSlots: { customRender: 'totalPrice' },
  },
  {
    title: 'Total Quantity',
    dataIndex: 'totalQuantity',
    key: 'totalQuantity',
  },
  {
    title: 'Rent Start',
    dataIndex: 'rentalStart',
    key: 'rentalStart',
    scopedSlots: { customRender: 'rentalStart' },
  },
  {
    title: 'Rent End',
    dataIndex: 'rentalEnd',
    key: 'rentalEnd',
    scopedSlots: { customRender: 'rentalEnd' },
  },
  {
    title: 'Rent Days',
    dataIndex: 'bookingDate',
    key: 'bookingDate',
    scopedSlots: { customRender: 'bookingDate' },
  },
  {
    title: 'Status',
    dataIndex: 'status',
    key: 'status',
    scopedSlots: { customRender: 'status' },
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
      defaultActiveTabs: '',
      data: [],
      dataRent: [],
      columns,
      columnsRent,
      labelCol: { span: 3 },
      wrapperCol: { span: 21 },
      user: {},
      loading: false,
      orderID: '',
      order: {},
      orderList: [],
      orderRentID: '',
      orderRent: {},
      orderRentList: [],
      products: [],
      orderUpdate: {},
      orderRentUpdate: {},
      visible: false,
      visibleRent: false,
    }
  },
  computed: {
    userID() {
      return this.$route.params.id ? this.$route.params.id : ''
    },
  },
  beforeMount() {
    const prevPath = this.$route.name
    const typeOrder = this.$route.query
    if (prevPath === 'profile' && typeOrder.type === 'order') {
      this.defaultActiveTabs = 'order'
    } else if (prevPath === 'profile' && typeOrder.type === 'rent') {
      this.defaultActiveTabs = 'rent'
    } else {
      this.defaultActiveTabs = 'user'
    }
  },
  async created() {
    await this.loadPage()
    await this.getOrderList()
    await this.getRentList()
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
        delete this.user.roles
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
        await this.loadPage()
      }
    },
    tabChange(key) {
      if (key === 'order' && this.userID !== '') {
        this.getOrderList()
      } else if (key === 'rent' && this.userID !== '') {
        this.getRentList()
      } else {
        this.loadPage()
      }
    },
    async backToList() {
      await this.getOrderList()
      this.orderID = ''
      this.products = []
    },
    async backToListRent() {
      await this.getRentList()
      this.orderRentID = ''
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
      } catch (e) {
        if (e.response.data) {
          this.$message.warning(e.response.data.details)
        }
      } finally {
        this.loading = false
      }
    },
    async getRentList() {
      try {
        this.loading = true
        const result = await this.$api.getRentByUser(this.userID, {
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        this.orderRentList = [...result.content]
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
    async editOrderRent(id) {
      this.orderRentID = id
      try {
        this.loading = true
        const result = await this.$api.getRent(id, {
          headers: {
            Authorization: this.$auth.$storage.getUniversal('token').token,
          },
        })
        this.orderRent = { ...result }
        this.dataRent = [...result.products].reduce((acc, item) => {
          const product = {
            productDesc: item.productDesc,
            productName: item.productName,
            price: item.price,
            rental: item.rental,
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
        this.orderRentUpdate = {
          orderRentID: result.orderRentID,
          orderRentName: result.orderRentName,
          phone: result.phone,
          shippingAddress: result.shippingAddress,
          bookingDate: result.bookingDate,
          rentalStart: result.rentalStart,
          totalRental: result.totalRental,
          rentalEnd: result.rentalEnd,
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
    showModal() {
      this.visible = true
    },
    async handleInfoOrder() {
      try {
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
        this.visible = false
      }
    },
    handleOk(e) {
      if (this.dataRent.length) {
        this.$confirm({
          title: 'Update Order Rent ? ',
          content:
            'Do you sure want to renting the order it will change the total payment ? ',
          onOk: () => this.rentingOrder(),
        })
      } else {
        this.$confirm({
          title: 'Update Order ? ',
          content: 'Do you sure want to update order info ? ',
          onOk: () => this.handleInfoOrder(),
        })
      }
    },
    async rentingOrder() {
      try {
        this.loading = true
        const newDiffDay = this.orderRentUpdate.rentalEnd.diff(
          this.$moment(this.orderRentUpdate.rentalStart),
          'days'
        )
        const deposist =
          this.orderRentUpdate.totalPrice - this.orderRentUpdate.totalRental
        const oneOfMoney =
          Number(this.orderRentUpdate.totalRental) /
          Number(this.orderRentUpdate.bookingDate)
        const newMoney = oneOfMoney * newDiffDay
        this.orderRentUpdate.totalRental = newMoney
        this.orderRentUpdate.totalPrice = deposist + newMoney
        this.orderRentUpdate.bookingDate = newDiffDay
        this.orderRentUpdate.status = 'RENTING'
        await this.$api.updateRent(this.orderRentID, this.orderRentUpdate, {
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
        await this.editOrderRent(this.orderRentID)
        this.visible = false
      }
    },
    disabledEndDate(endValue) {
      const startValue = this.$moment(this.orderRentUpdate.rentalEnd)
      if (!endValue || !startValue) {
        return false
      }
      return startValue.valueOf() >= endValue.valueOf()
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
    async updateOrderRent(status) {
      try {
        this.orderRentUpdate.status = status
        this.loading = true
        await this.$api.updateRent(this.orderRentID, this.orderRentUpdate, {
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
        await this.editOrderRent(this.orderRentID)
      }
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
      } else if (text === 'EXPIRED') {
        color = 'purple'
      } else {
        color = 'red'
      }
      return color
    },
    setOrderStatusColor(text) {
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
