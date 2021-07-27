<template>
  <a-spin :spinning="loading">
    <a-tabs default-active-key="1">
      <a-tab-pane key="1" tab="User Info">
        <a-form-model
          :model="user"
          :label-col="labelCol"
          :wrapper-col="wrapperCol"
        >
          <a-form-model-item label="User Name">
            <a-input v-model="user.username" />
          </a-form-model-item>
        </a-form-model>
      </a-tab-pane>
      <a-tab-pane key="2" tab="Order Info" force-render>
        Content of Tab Pane 2
      </a-tab-pane>
    </a-tabs>
  </a-spin>
</template>

<script>
export default {
  name: 'index.vue',
  data() {
    return {
      user: {
        username: '',
      },
      loading: false,
    }
  },
}
</script>
