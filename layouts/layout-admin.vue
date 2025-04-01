<template>
  <div id="app">
    <v-app>
      <!-- Side-bar -->
      <v-navigation-drawer v-model="drawer" app clipped>
        <v-list active-class="pink--text">
          <!-- Logo in side-bar -->
          <v-list-item class="py-3">
            <v-img
              v-if="!$vuetify.breakpoint.mobile"
              height="220px"
              width="220px"
              contain
              :src="require('@/assets/images/icon.svg')"
            />
          </v-list-item>

          <!-- List of menu items -->
          <div v-for="(link, i) in items" :key="i">
            <v-list-item
              v-if="!link.items"
              :key="i"
              active-class="secondary --light"
              :to="link.to"
            >
              <v-list-item-action>
                <v-icon>{{ link.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-title v-text="link.title" />
            </v-list-item>
            <!-- Nested menu items -->
            <v-list-group v-else :key="link.title" no-action>
              <template v-slot:activator>
                <v-list-item active-class="secondary --light" class="group-item">
                  <v-list-item-action>
                    <v-icon>{{ link.icon }}</v-icon>
                  </v-list-item-action>
                  <v-list-item-content>
                    <v-list-item-title>{{ link.title }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </template>
              <v-list-item
                v-for="sublink in link.items"
                :key="sublink.title"
                :to="sublink.to"
              >
                <v-list-item-action>
                  <v-icon>{{ sublink.icon }}</v-icon>
                </v-list-item-action>
                <v-list-item-content>
                  <v-list-item-title>{{ sublink.title }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-group>
          </div>
        </v-list>
      </v-navigation-drawer>

      <!-- App-bar -->
      <v-app-bar app clipped-left>
        <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <v-img
          contain
          width="100%"
          max-width="145px"
          height="50%"
          :src="require('@/assets/images/logo.svg')"
        ></v-img>
        <v-toolbar-title> Manager</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-list color="transparent">
          <v-list-item v-if="user" class="pa-3 py-0">
            <v-chip block outlined color="#FFEB3B">
              <v-icon left> mdi-account-circle-outline </v-icon>
              {{ user.email }}
            </v-chip>
          </v-list-item>
        </v-list>
        <v-menu
          v-model="menu"
          :disabled="true"
          :open-on-hover="true"
          :close-on-click="true"
          :close-on-content-click="true"
        >
          <template v-slot:activator="{ on }">
            <v-btn outlined dark v-on="on" @click="logoutUser()">
              Logout
            </v-btn>
          </template>
        </v-menu>
      </v-app-bar>

      <!-- Main content -->
      <v-main>
        <v-container class="fill-height" fluid>
          <slot />
        </v-container>
      </v-main>
    </v-app>
  </div>
</template>

<script>
export default {
  name: "LayoutAdmin",
  data: () => ({
    drawer: null,
    menu: true,
  }),
  computed: {
    user() {
      return this.$store?.getters['auth/getUser']
    },
    items() {
      return [
        {
          icon: 'mdi-view-dashboard',
          title: 'Home',
          to: '/administration/dashboard',
        },
        {
          icon: 'mdi-cube-outline',
          title: 'Reporting',
          to: '/analytics/trends',
          items: [
            { title: 'Trends', to: '/analytics/trends', icon: 'mdi-cube-outline' },
            { title: 'Engagement', to: '/analytics/engagement', icon: 'mdi-puzzle' },
            { title: 'Comparator (demo)', to: '/analytics/comparator', icon: 'mdi-chart-areaspline' },
          ],
        },
        {
          icon: 'mdi-radio-tower',
          title: 'Algorithm',
          to: '/administration/algorithm',
        },
        {
          icon: 'mdi-account-cash',
          title: 'Subscription',
          to: '/administration/team',
          items: [
            { icon: 'mdi mdi-account-multiple', title: 'Team', to: '/administration/team' },
            { icon: 'mdi-human-greeting', title: 'Profile', to: '/administration/profile' },
          ],
        },
        {
          icon: 'mdi-owl',
          title: 'Privacy',
          to: '/administration/terms/privacy',
          items: [
            { title: 'Privacy', to: '/administration/terms/privacy', icon: 'mdi-owl' },
            { title: 'Terms Of Use', to: '/administration/terms/termsofuse', icon: 'mdi-shield' },
          ],
        },
        {
          icon: 'mdi-chat',
          title: 'Support',
          to: '/administration/contact-us',
        },
      ]
    },
  },
  created() {
    if (this.$vuetify) {
      this.$vuetify.theme.dark = true
    }
  },
  mounted() {
    this.items.push({
      icon: 'mdi-panorama-fisheye',
      title: 'Super Admin',
      to: '/administration/super',
    })
  },
  methods: {
    logoutUser() {
      this.$store?.dispatch('auth/logoutAdmin')
      setTimeout(() => {
        this.$dialog?.message.success('Logged Out!', {
          position: 'top',
          color: 'green',
          timeout: 3000,
        })
      }, 800)
    },
  },
}
</script>

<style>
body,
#app {
  min-height: 100vh;
}

.text-nowrap {
  white-space: nowrap;
}
.group-item {
  padding: 0 !important;
}
</style>
