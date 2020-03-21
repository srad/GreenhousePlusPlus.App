<template>
  <div id="app">
    <b-navbar id="nav" toggleable="sm" type="light" variant="primary" class="navbar-default navbar-static-top">
      <b-navbar-toggle target="nav-text-collapse"></b-navbar-toggle>
      <router-link to="/">
        <b-navbar-brand class="font-weight-bold text-white m-0 p-0">
          Greenhouse++
          <font-awesome-icon icon="hands-helping" class="ml-3"/>
        </b-navbar-brand>
      </router-link>
    </b-navbar>
    <b-container fluid class="pl-3 pr-3 pt-3 pb-2">
      <router-view/>
    </b-container>
  </div>
</template>

<style lang="scss">
@import "assets/main.scss";
</style>
