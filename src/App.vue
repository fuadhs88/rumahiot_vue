<template>
  <v-content>
    <v-app light>
      <v-toolbar color="white" fixed app :clipped-left="clipped">
        <img src="/static/img/logos/banner-toolbar.png" height="50px" alt="Rumah IoT">
        <v-spacer></v-spacer>
        <v-toolbar-items class="align-center">
          <v-btn flat :to="{name:'landing_page'}" value="beranda" active-class>Beranda</v-btn>
        </v-toolbar-items>
        <v-toolbar-items class="hidden-sm-and-down align-center" >
          <div>
            <v-btn dark color="light-blue darken-1">Masuk</v-btn>
          </div>
        </v-toolbar-items>
        <v-btn class="hidden-md-and-up"
               icon
               light
               @click.stop="rightDrawer = !rightDrawer"
        >
          <v-icon>menu</v-icon>
        </v-btn>
      </v-toolbar>
      <v-slide-x-transition>
      <router-view></router-view>
      </v-slide-x-transition>
    </v-app>
  </v-content>
</template>
