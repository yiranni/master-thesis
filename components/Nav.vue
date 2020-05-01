<template>
  <div class="sidenav">
    <a>
      <img src="~/assets/logo.svg" id="logo" />
    </a>
    <div class="menu-items">
      <nuxt-link to="/" >EXPLORE</nuxt-link>
      <nuxt-link to="/about" >ABOUT</nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  name: "Nav",
  data() {
    return {
      activeIndex: "1"
    };
  }
};
</script>

<style scoped>
.sidenav {
  height: 100%;
  width: 160px;
  position: fixed;
  z-index: 1;
  top: 0;
  left: 0;
  background-color: #000000;
  overflow-x: hidden;
  padding-top: 20px;
}

.sidenav a {
  padding: 30px 30px 30px 16px;
  text-decoration: none;
  font-size: 18px;
  color: #838383;
  display: block;
  text-align: right;
  font-weight: bold;
}

.sidenav a:hover {
  color: #f1f1f1;
}


.menu-items {
  margin-top: 16vh;
}

#logo {
  width: 60px;
  height: 60px;
  margin-right: 16px;
  margin-top: 20px;
}
</style>
