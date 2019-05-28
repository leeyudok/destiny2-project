<template>
  <div
    class="zeroHourLoad"
  >
    <img
      :src="getImgSrc()"
    />
    <br/>
    <img
      src="@/assets/zeroHour/room.png"
    />
    <br/>
    <img
      src="@/assets/zeroHour/machineIndex.png"
    />
  </div>
</template>

<script>
import arcRoad from '@/assets/zeroHour/arcRoad.png';
import solarRoad from '@/assets/zeroHour/solarRoad.png';
import voidRoad from '@/assets/zeroHour/voidRoad.png';

export default {
  name: 'ZeroHourLoad',
  components: {
  },
  props: {
    elements: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
    };
  },
  mounted() {
  },
  computed: {
  },
  watch: {
  },
  methods: {
    getImgSrc() {
      if (this.elements === 'arc') {
        return arcRoad;
      }
      if (this.elements === 'void') {
        return voidRoad;
      }
      return solarRoad;
    },
  },
  beforeDestroy() {
  },
};
</script>

<style>
</style>
