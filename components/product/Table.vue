<template>
  <a-table :columns="columns" :data-source="products">
    <a-tag
      slot="status"
      slot-scope="text"
      :color="text === 'SHOW' ? 'green' : 'red'"
    >
      {{ text === 'SHOW' ? 'SHOW' : 'HIDDEN' }}</a-tag
    >
    <div slot="images" slot-scope="text">
      <img :src="text" style="width: 300px; height: 100px" :alt="text" />
    </div>
    <span slot="price" slot-scope="text">
      {{ formatPrice(text) }}
    </span>
    <span slot="rental" slot-scope="text">
      {{ formatPrice(text) }}
    </span>
    <span slot="action" slot-scope="record">
      <a-button
        type="primary"
        style="margin-bottom: 10px"
        @click="editProduct(record.id)"
        ><a-icon type="edit"
      /></a-button>
      <a-button type="danger" @click="deleteProduct(record.id)"
        ><a-icon type="delete"
      /></a-button>
    </span>
  </a-table>
</template>

<script>
const columns = [
  {
    title: 'ID',
    dataIndex: 'id',
    key: 'id',
  },
  {
    title: 'Name',
    dataIndex: 'name',
    key: 'name',
  },
  {
    title: 'Image',
    dataIndex: 'images',
    key: 'images',
    scopedSlots: { customRender: 'images' },
  },
  {
    title: 'Description',
    dataIndex: 'desc',
    key: 'desc',
    width: '300px',
  },
  {
    title: 'Quantity',
    dataIndex: 'qty',
    key: 'qty',
  },
  {
    title: 'Price',
    dataIndex: 'price',
    key: 'price',
    scopedSlots: { customRender: 'price' },
  },
  {
    title: 'Rental',
    dataIndex: 'rental',
    key: 'rental',
    scopedSlots: { customRender: 'rental' },
  },
  {
    title: 'Category',
    dataIndex: 'categoryName',
    key: 'categoryName',
  },
  {
    title: 'Brand',
    dataIndex: 'brandName',
    key: 'brandName',
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
    width: '130px',
  },
]

export default {
  name: 'Table',
  props: {
    products: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      columns,
      loading: false,
    }
  },
  methods: {
    editProduct(id) {
      this.$emit('editProduct', id)
    },
    deleteProduct(id) {
      this.$emit('deleteProduct', id)
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
