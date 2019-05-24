<template>
  <div
    id="app"
  >
    <div
      class="top"
    >
      top
    </div>
    <div
      class="body"
    >
      <div
        class="navi"
      >
        <div>
          <router-link to="/">
            main
          </router-link>
        </div>
        <div>
          <router-link to="/ZeroHour">
            menu1
          </router-link>
        </div>
      </div>
      <div
        class="contents"
      >
        <router-view/>
      </div>
    </div>
  </div>
</template>

<script>
import 'buefy/dist/buefy.css';

export default {
  components: {
  },
  data() {
    return {
      checkbox: false,
    };
  },
  computed: {
  },
  methods: {
  },
};
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  .top {
    position: absolute;
    height: 50px;
    width: 100%;
    border-bottom: 1px solid #000;
  }
  .body {
    position: absolute;
    top: 50px;
    width: 100%;
    height: calc(100% - 50px);
  }

  .navi {
    position: absolute;
    border-right: 1px solid #000;
    width: 200px;
    height: 100%;
    background-color: #FFFFFF;
  }
  .contents {
    width: 100%;
    height: 100%;
    margin-left: 200px;
  }
</style>
