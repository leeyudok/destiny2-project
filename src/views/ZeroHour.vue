<template>
  <div>
    <b-tabs v-model="activeTab">
      <b-tab-item label="room">
        <img
          src="@/assets/zeroHour_room.png"
        />
      </b-tab-item>
      <b-tab-item label="matrix">
        Lorem ipsum dolor sit amet.
      </b-tab-item>
    </b-tabs>
  </div>
</template>

<script>
export default {
  name: 'ZeroHour',
  components: {
  },
  data() {
    return {
      activeTab: 0,
    };
  },
};
</script>
