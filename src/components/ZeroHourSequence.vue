<template>
  <div
    class="zeroHourSeq"
  >
    <br/>
    <b-tabs
      v-model="activeTab"
      type="is-toggle"
      destroy-on-hide
    >
      <b-tab-item
        v-for="tabObj in tabType"
        :key="tabObj.key"
        :label="tabObj.label"
      >
        <img
          v-if="activeTab === 0"
          :src="getImgSrc()"
        />
        <ZeroHourTable
          v-if="activeTab === 2"
          :elements="elements"
        />
      </b-tab-item>
    </b-tabs>

    <br/>

    <br/>
  </div>
</template>

<script>
import ZeroHourTable from '@/components/ZeroHourTable.vue';

export default {
  name: 'ZeroHourSeq',
  components: {
    ZeroHourTable,
  },
  props: {
    elements: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      activeTab: 0,
      tabType: [{
        label: '길',
        key: 'road',
      }, {
        label: '시계',
        key: 'clock',
      }, {
        label: '테이블',
        key: 'table',
      }],
    };
  },
  computed: {
  },
  watch: {
  },
  methods: {
    getImgSrc() {
      /* eslint-disable */
      // return import(`@/assets/zeroHour/${this.elements}Road.png`).default;
      return require(`@/assets/zeroHour/${this.elements}Road.png`);
    },
  },
};
</script>

<style>
</style>
