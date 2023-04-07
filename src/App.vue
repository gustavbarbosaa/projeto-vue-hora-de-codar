<template>
  <nav>
    <Navbar :logo="logoImg" :alt="altImg"/>
  </nav>
  <router-view/>
  <Footer />
</template>

<script>
import Navbar from './components/Navbar.vue';
import Footer from './components/Footer.vue';

export default {
  components: {
    Navbar,
    Footer
  },
  data() {
    return {
      logoImg: "/img/logo.png",
      altImg: "Logo de hamburguer",
    }
  }
}

</script>

<style>

* {
  font-family: Helvetica, sans-serif;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.mainContainer {
  margin: 50px;
  min-height: 250px;
}

h1 {
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}

</style>
