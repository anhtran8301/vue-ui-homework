<template>
  <v-app>
    <v-app-bar color="primary" prominent>
      <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Book Store Manager</v-toolbar-title>
      <v-spacer></v-spacer>
      <template v-if="$vuetify.display.mdAndUp">
        <v-btn icon="mdi-magnify" variant="text"></v-btn>
        <v-btn icon="mdi-filter" variant="text"></v-btn>
      </template>
      <v-btn icon="mdi-dots-vertical" variant="text"></v-btn>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" :location="$vuetify.display.mobile ? 'bottom' : undefined" temporary>
      <v-list>
        <v-list>
          <v-list-item to="/login" router-link>
            <v-list-item-content>
              <v-list-item-title>Login</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item to="/chu-de" router-link>
            <v-list-item-content>
              <v-list-item-title>Chủ đề</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item router-link>
            <v-list-item-content>
              <v-list-item-title>Khách hàng</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item router-link>
            <v-list-item-content>
              <v-list-item-title>Nhà xuất bản</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item router-link>
            <v-list-item-content>
              <v-list-item-title>Tác giả</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>
<script>
export default {
  name: 'App',

  components: {},

  data: () => ({
    drawer: false
  }),
}
</script>
