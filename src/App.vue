<template>
  <v-app>
    <v-app-bar app>
      <router-link
        to="/"
        tag="h4"
        style="font-size: 1.6rem; font-weight: 300; margin-left:43%"
        >Literary association</router-link
      >
      <v-spacer></v-spacer>

      <v-app-bar-nav-icon
        @click.stop="drawer = !drawer"
      ></v-app-bar-nav-icon>

      <template>
        <v-navigation-drawer
          v-model="drawer"
          absolute
          temporary
          right
          style="height: 1000px"
        >
        <!-- <v-list dense>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>
                  <router-link
                    to="/login"
                    style="
                      font-size: 1.2rem;
                      font-weight: 300;
                      line-height: 1.2;
                      color: black;
                    "
                    >Log In</router-link
                  >
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list> -->

          <v-list dense>
            <v-list-item v-if="user !== null">
              <v-list-item-content>
                <v-list-item-title>
                  <router-link
                    to="/index"
                    style="
                      font-size: 1.2rem;
                      font-weight: 300;
                      line-height: 1.2;
                      color: black;
                    "
                    >Upload file</router-link
                  >
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>

          <v-list dense>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>
                  <router-link
                    to="/search"
                    style="
                      font-size: 1.2rem;
                      font-weight: 300;
                      line-height: 1.2;
                      color: black;
                    "
                    >Search</router-link
                  >
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>

          <v-divider></v-divider>

          <v-list-item v-if="user !== null">
            <v-list-item-avatar>
              <v-icon>mdi-logout</v-icon>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title id="logout" @click="logout"
                style="font-size: 1.2rem; font-weight: 300; line-height: 1.2; color: black;"
                >Log out</v-list-item-title
              >
            </v-list-item-content>
          </v-list-item>
        </v-navigation-drawer>
      </template>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
import { mapGetters, mapMutations } from "vuex";

export default {
  data() {
    return {
      drawer: null,
    };
  },
  computed: {
    ...mapGetters(["user"]),
  },
  methods: {
    ...mapMutations(['setUser']),
    logout(){
      this.setUser(null);
      this.$router.push("/")
    }
  }
};
</script>

<style scoped>
div[data-app="true"] {
  width: 100%;
  height: 100%;
  background: url("https://scontent.fbeg5-1.fna.fbcdn.net/v/t1.15752-9/p1080x2048/116878239_3189763244418283_4395864458301874219_n.jpg?_nc_cat=110&ccb=3&_nc_sid=ae9488&_nc_ohc=Qw_84yZ14uoAX-stP-8&_nc_ht=scontent.fbeg5-1.fna&tp=6&oh=9ba60e6170d7105a0368d9315134d48b&oe=6056F3C0")
    no-repeat center center fixed !important;
  background-size: cover;
}

a:hover {
  text-decoration: none;
}
h4:hover {
  cursor: pointer;
}

#logout:hover {
  cursor: pointer;
}
</style>
