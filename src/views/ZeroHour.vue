<template>
  <div>
    <b-tabs
      v-model="activeTab"
      type="is-boxed"
      :animated="false"
      class="zeroHourElementTab"
    >
      <br/>
      <b-tab-item
        v-for="element in elementList"
        :key="'zeroHourTab_' + element"
        :label="element + ' week'"
      >
        <template
          slot="header"
        >
          <div
            :class="element"
          >
            {{ element }} week
          </div>
        </template>
        <b-tabs
          v-model="activeTab2"
          type="is-toggle"
          :animated="false"
          class="zeroHourTypeTab"
          :class="element"
        >
          <b-tab-item
            v-for="tabObj in tabType"
            :key="tabObj.key"
            :label="tabObj.label"
            class="zeroHourTabItem"
          >
            <ZeroHourLoad
              v-if="activeTab2 === 0"
              :elements="element"
            />
            <ZeroHourClock
              v-if="activeTab2 === 1"
              :elements="element"
            />
            <ZeroHourTable
              v-if="activeTab2 === 2"
              :elements="element"
            />
            <div
              v-if="activeTab2 === 3"
              class="zeroHourTotal"
            >
              <ZeroHourLoad
                :elements="element"
              />
              <ZeroHourTable
                :elements="element"
              />
            </div>
          </b-tab-item>
        </b-tabs>
      </b-tab-item>
    </b-tabs>
  </div>
</template>

<script>
import ZeroHourLoad from '@/components/zeroHour/ZeroHourLoad.vue';
import ZeroHourTable from '@/components/zeroHour/ZeroHourTable.vue';
import ZeroHourClock from '@/components/zeroHour/ZeroHourClock.vue';

export default {
  name: 'ZeroHour',
  components: {
    ZeroHourLoad,
    ZeroHourTable,
    ZeroHourClock,
  },
  data() {
    return {
      notMobile: true,
      activeTab: 0,
      activeTab2: 0,
      elementList: ['void', 'arc', 'solar'],
      tabType: [{
        label: '길',
        key: 'road',
      }, {
        label: '시계',
        key: 'clock',
      }, {
        label: '테이블',
        key: 'table',
      }, {
        label: '종합',
        key: 'total',
      }],
    };
  },
};
</script>

<style>
  .zeroHourTotal {
    display: flex;
    justify-content: center;
  }
  .zeroHourElementTab > .tabs li.is-active a span .arc{
    color: #00C1D1;
  }
  .zeroHourElementTab > .tabs li.is-active a span .solar{
    color: #D15100;
  }
  .zeroHourElementTab > .tabs li.is-active a span .void{
    color: #A800FF;
  }
  .zeroHourTypeTab.arc > .tabs.is-toggle li.is-active a{
    background-color: #00C1D1;
    border-color: #00C1D1;
  }
  .zeroHourTypeTab.solar > .tabs.is-toggle li.is-active a{
    background-color: #D15100;
    border-color: #D15100;
  }
  .zeroHourTypeTab.void > .tabs.is-toggle li.is-active a{
    background-color: #A800FF;
    border-color: #A800FF;
  }
</style>
