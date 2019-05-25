<template>
  <div
    id="app"
  >
    <div
      class="top"
    >
      <img
        src="@/assets/menuIcon.png"
        class="logoImg"
        @click="logoClick"
      />
      <img
        src="@/assets/topLogo.png"
        class="topLogoImg"
        @click="topLogoClick"
      />
    </div>
    <div
      class="body"
    >
      <div
        class="navi"
        :class="{ collapsed: collapsed }"
      >
        <div>
          <router-link to="/">
            main
          </router-link>
        </div>
        <div>
          <router-link to="/ZeroHour">
            개시 시간
          </router-link>
        </div>
      </div>
      <div
        class="contents"
        :class="{ collapsed: collapsed }"
      >
        <router-view/>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {
  },
  data() {
    return {
      collapsed: true,
    };
  },
  computed: {
  },
  methods: {
    logoClick() {
      this.collapsed = !this.collapsed;
    },
    topLogoClick() {
      this.$router.push('/');
    },
  },
};
</script>

<style>
  .top {
    position: absolute;
    height: 50px;
    width: 100%;
    border-bottom: 1px solid #000;
  }
  .logoImg {
    position: absolute;
    width: 40px;
    height: 40px;
    left: 0;
    cursor: pointer;
  }
  .topLogoImg {
    padding-left: 50px;
    cursor: pointer;
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
    z-index: 1;
  }
  .navi.collapsed {
    width: 50px;
  }
  .navi > div {
    height: 50px;
    border: 1px solid #2e2e2e;
    border-radius: 4px;
    margin: 1px;
  }
  .contents {
    width: 100%;
    height: 100%;
    padding-left: 50px;
  }
  @media (min-width: 1000px) {
    .contents {
      padding-left: 200px;
    }
  }
  .contents.collapsed {
    padding-left: 50px;
  }
</style>
