<template>
  <a-layout-sider
    v-model="collapsed"
    :trigger="null"
    collapsible
    class="sidebar-menu"
  >
    <div class="logo">Phượt Store</div>
    <a-menu class="sidebar-menu" mode="inline" :default-selected-keys="['0']">
      <a-menu-item
        v-for="(item, index) in sideBar"
        :key="index"
        class="sidebar-item"
      >
        <nuxt-link :to="item.path" @click="() => (activeItem = index)">
          <a-icon class="sidebar-icon" :type="item.icon" />
          <span>{{ item.name }}</span>
        </nuxt-link>
      </a-menu-item>
    </a-menu>
  </a-layout-sider>
</template>

<script>
const sideBar = [
  {
    path: '/admin/dashboard',
    name: 'Dashboard',
    icon: 'windows',
    active: false,
  },
  {
    path: '/admin/category',
    name: 'Category',
    icon: 'appstore',
    active: false,
  },
  {
    path: '/admin/product',
    name: 'Product',
    icon: 'code-sandbox',
    active: false,
  },
  {
    path: '/admin/brand',
    name: 'Brand',
    icon: 'apple',
    active: false,
  },
  {
    path: '/admin/blog',
    name: 'Blog',
    icon: 'form',
    active: false,
  },
  {
    path: '/admin/order',
    name: 'Order',
    icon: 'shopping-cart',
    active: false,
  },

  {
    path: '/admin/rent',
    name: 'Order Rent',
    icon: 'shopping',
    active: false,
  },
]
export default {
  name: 'SidebarAdmin.vue',
  data() {
    return {
      collapsed: false,
      sideBar,
      activeItem: '',
    }
  },
}
</script>
