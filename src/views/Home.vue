<template>
  <div class="home">
    home
    <hello-world>
      123
    </hello-world>
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue';

export default {
  name: 'Home',
  components: {
    HelloWorld,
  },
};
</script>
