<template>
  <a-table :columns="columns" :data-source="blogs">
    <a-tag
      slot="status"
      slot-scope="text"
      :color="text === 'SHOW' ? 'green' : 'red'"
    >
      {{ text === 'SHOW' ? 'SHOW' : 'HIDDEN' }}</a-tag
    >
    <span slot="desc" slot-scope="text">
      {{ text }}
    </span>
    <div slot="thumbnail" slot-scope="text">
      <img :src="text" alt="text" style="width: 100px; height: 100px" />
    </div>
    <span slot="action">
      <a-button type="primary"><a-icon type="edit" /></a-button>
      <a-button type="danger"><a-icon type="delete" /></a-button>
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
    title: 'Title',
    dataIndex: 'title',
    key: 'title',
  },
  {
    title: 'Description',
    dataIndex: 'desc',
    key: 'desc',
    scopedSlots: { customRender: 'desc' },
    width: '500px',
  },
  {
    title: 'Thumbnail',
    dataIndex: 'thumbnail',
    key: 'thumbnail',
    scopedSlots: { customRender: 'thumbnail' },
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
  name: 'Table',
  props: {
    blogs: {
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
}
</script>
