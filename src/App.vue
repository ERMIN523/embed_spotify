<template>
  <div class="about w-full text-center">
    <router-link to="/">
      <div
        class="pageheader grid grid-cols-a1f p-4 pt-8 sm:p-12 sm:pb-4 md:pb-6 gap-4 sm:gap-8 items-end"
      >
        <h1
          class="font-bold text-gray-100 my-2 text-4xl sm:text-5xl md:text-6xl lg:text-5r leading-none"
        >
          Moja lista ulubionych piosenek
        </h1>
      </div>
    </router-link>

    <div
      class="bg-161616 text-eeeeee px-t2 sm:px-6 md:px-12 py-t2 md:py-6 text-left min-h-screen"
    >
      <div
        class="bg-transparent grid grid-cols-2 gap-1 py-t1 sm:py-0 sm:px-t4 md:px-t8 sm:flex sm:flex-no-wrap sm:items-center sm:justify-center"
      >
        <router-link
          v-for="navlink in navitems"
          :key="navlink.title"
          :to="navlink.url"
          class="inline-flex items-center justify-center px-4 py-3 sm:m-1 text-base font-medium leading-6 transition-colors duration-300 hover:bg-282828 rounded-md cursor-pointer"
          active-class="bg-222222 shadow"
          >{{ navlink.title }}</router-link
        >
      </div>

      <div id="content"></div>
      <router-view v-slot="{ Component }">
        <transition name="fade" mode="out-in">
          <component :is="Component" />
        </transition>
      </router-view>
    </div>
  </div>
</template>

<script>
export default {
  data() {},
  async created() {
    let copyYear = 2021;
    let currentYear = new Date().getFullYear();
    if (currentYear > copyYear) {
      this.$store.commit("copyYear", copyYear + "–" + currentYear);
    } else {
      this.$store.commit("copyYear", copyYear);
    }
  },
  methods: {
    beforeLeave(element) {
      this.prevHeight = getComputedStyle(element).height;
    },
    enter(element) {
      window.scrollTo({ top: 180, behavior: "smooth" });
      const { height } = getComputedStyle(element);

      element.style.height = this.prevHeight;

      setTimeout(() => {
        element.style.height = height;
      });
    },
    afterEnter(element) {
      element.style.height = "auto";
    },
    scrollToTop() {
      window.scrollTo({ top: 0, behavior: "smooth" });
    },
  },
};
</script>

<style>
html,
body {
  font-family: Jost, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  scroll-behavior: smooth;
}
.pageheader {
  background: rgb(22, 22, 22);

  box-shadow: 0px 0px 10px 0px #000000;
  text-shadow: 0px 0px 3px #000000;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
