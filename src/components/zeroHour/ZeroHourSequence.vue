<template>
  <div
    class="zeroHourSeq"
  >
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
        <ZeroHourClock
          v-if="activeTab === 1"
          :elements="elements"
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
import ZeroHourTable from '@/components/zeroHour/ZeroHourTable.vue';
import ZeroHourClock from '@/components/zeroHour/ZeroHourClock.vue';

export default {
  name: 'ZeroHourSeq',
  components: {
    ZeroHourTable,
    ZeroHourClock,
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
      return require(`@/assets/zeroHour/${this.elements}Road.png`);
    },
  },
};
</script>

<style>
</style>
