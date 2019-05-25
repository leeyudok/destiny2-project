<template>
  <div>
    <b-tabs v-model="activeTab">
      <b-tab-item label="room">
        <img
          src="@/assets/zeroHour/room.png"
        />
        <br/>
        <img
          src="@/assets/zeroHour/machineIndex.png"
        />
        <br/>
        <img
          src="@/assets/zeroHour/clock.png"
        />
      </b-tab-item>
      <b-tab-item
        v-for="element in elementList"
        :key="'zeroHourTab_' + element"
        :label="element + ' week'"
      >
        <ZeroHourSeq
          :elements="element"
        />
      </b-tab-item>
    </b-tabs>
  </div>
</template>

<script>
import ZeroHourSeq from '@/components/ZeroHourSequence.vue';

export default {
  name: 'ZeroHour',
  components: {
    ZeroHourSeq,
  },
  data() {
    return {
      activeTab: 0,
      elementList: ['solar', 'arc', 'void'],
    };
  },
};
</script>
