<template>
  <div>
    <Header />

    <main class="flex justify-center items-center">
      <Nuxt />
    </main>
    <br />
    <Footer />
  </div>
</template>

<script>
import Header from "../components/Common/Header/index.vue";
import Footer from "../components/Common/Footer/index.vue";
import Chat from "../components/Chat/index.vue";

export default {
  components: {
    Header,
    Footer,
  },
  data() {
    return {
      activeLink: null,
    };
  },
  computed: {},
  methods: {
    setActive(link) {
      this.activeLink = link;
    },
  },
};
</script>

<style>
.nav-link:hover {
  text-decoration: underline;
  color: white;
  background-color: #f24b5a;
  padding: 11px;
}

.nav-link.active {
  background-color: #f24b5a;
  padding: 11px;
}
</style>
