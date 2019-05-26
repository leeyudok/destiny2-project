<template>
  <div
    class="zeroHourTable"
  >
    <b-field
      class="checkbox-wrapper"
      grouped
      group-multiline
    >
      <div v-for="(column, index) in columns"
           :key="index"
           class="control">
        <b-checkbox
          v-if="column.field !== 'seqResult' && column.field !== 'console1'"
          v-model="column.visible"
          :disabled="simpleModeFlag"
        >
          {{ column.label }}
        </b-checkbox>
      </div>
      <b-checkbox
        class="control seqColorCheckbox"
        v-model="seqColorFlag"
      >
        시퀀스 색상
      </b-checkbox>
      <b-checkbox
        class="control seqColorCheckbox"
        v-model="simpleModeFlag"
      >
        심플 모드
      </b-checkbox>
    </b-field>
    <br/>
    <div
      class="b-table-wrapper"
    >
      <b-table
        class="zeroSeqTable left"
        :data="getSeq(`${elements}`)"
        :narrowed="true"
        default-sort="console1"
      >
        <template
          slot-scope="props"
          v-if="getSeq(`${elements}`).length / 2 >= props.index + 1"
        >
          <b-table-column
            v-for="(column, index) in columns"
            :key="index + '1'"
            :field="column.field"
            :label="column.label"
            :visible="column.visible"
          >
            <div
              v-if="column.field === 'seqResult' && seqColorFlag"
              class="cell"
              :class="cellRenderer(props.row[column.field])"
            >
              {{ props.row[column.field] }}
            </div>
            <div
              v-else-if="column.field === 'console1' &&
              simpleModeFlag && console1Duplicate(props.index)"
              class="cell"
              :class="cellRenderer(props.row.seqResult)"
            >
              {{ props.row[column.field] }} ( {{ props.row.console2 }} )
            </div>
            <div
              v-else-if="simpleModeFlag"
              class="cell"
              :class="cellRenderer(props.row.seqResult)"
            >
              {{ props.row[column.field] }}
            </div>
            <div
              v-else
            >
              {{ props.row[column.field] }}
            </div>
          </b-table-column>
        </template>
      </b-table>
      <b-table
        class="zeroSeqTable right"
        :data="getSeq(`${elements}`)"
        :narrowed="true"
        default-sort="console1"
      >
        <template
          slot-scope="props"
          v-if="getSeq(`${elements}`).length / 2 < props.index + 1"
        >
          <b-table-column
            v-for="(column, index) in columns"
            :key="index + '2'"
            :field="column.field"
            :label="column.label"
            :visible="column.visible"
          >
            <div
              v-if="column.field === 'seqResult' && seqColorFlag"
              class="cell"
              :class="cellRenderer(props.row[column.field])"
            >
              {{ props.row[column.field] }}
            </div>
            <div
              v-else-if="column.field === 'console1' &&
              simpleModeFlag && console1Duplicate(props.index)"
              class="cell"
              :class="cellRenderer(props.row.seqResult)"
            >
              {{ props.row[column.field] }} ( {{ props.row.console2 }} )
            </div>
            <div
              v-else-if="simpleModeFlag"
              class="cell"
              :class="cellRenderer(props.row.seqResult)"
            >
              {{ props.row[column.field] }}
            </div>
            <div
              v-else
            >
              {{ props.row[column.field] }}
            </div>
          </b-table-column>
        </template>
      </b-table>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
  name: 'ZeroHourTable',
  props: {
    elements: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      activeTab: 0,
      defaultSortDirection: 'asc',
      seqColorFlag: true,
      simpleModeFlag: false,
      columns: [
        {
          label: '콘솔1',
          field: 'console1',
          visible: true,
        },
        {
          label: '콘솔2',
          field: 'console2',
          visible: true,
        },
        {
          label: '콘솔3',
          field: 'console3',
          visible: true,
        },
        {
          label: '시퀀스',
          field: 'seqResult',
          visible: true,
        },
      ],
    };
  },
  computed: {
    ...mapGetters('ZeroHour', [
      'solarSeq',
      'arcSeq',
      'voidSeq',
    ]),
  },
  watch: {
    simpleModeFlag(flag) {
      if (flag) {
        this.seqColorFlag = true;
        this.columns[1].visible = false;
        this.columns[2].visible = false;
      } else {
        this.seqColorFlag = true;
        this.columns[1].visible = true;
        this.columns[2].visible = true;
      }
    },
  },
  methods: {
    getSeq(type) {
      return this[`${type}Seq`];
    },
    cellRenderer(seqResult) {
      if (!seqResult || !this.seqColorFlag) {
        return;
      }
      const color = seqResult.substring(0, 2);
      let resultCls = '';
      if (color === '파랑') {
        resultCls = 'blue';
      } else if (color === '하늘') {
        resultCls = 'sky';
      } else if (color === '빨강') {
        resultCls = 'red';
      } else if (color === '노랑') {
        resultCls = 'yellow';
      } else if (color === '초록') {
        resultCls = 'green';
      } else if (color === '보라') {
        resultCls = 'purple';
      }
      // eslint-disable-next-line
      return resultCls;
    },
    console1Duplicate(currIdx) {
      const currWeekData = this[`${this.elements}Seq`];
      const currRow = currWeekData[currIdx];
      let result = false;
      if (currIdx === 0) {
        if (currRow.console1 === currWeekData[currIdx + 1].console1) {
          result = true;
        }
      } else if (currIdx === currWeekData.length - 1) {
        if (currRow.console1 === currWeekData[currIdx - 1].console1) {
          result = true;
        }
      } else if ((currRow.console1 === currWeekData[currIdx + 1].console1)
        || (currRow.console1 === currWeekData[currIdx - 1].console1)
      ) {
        result = true;
      }
      return result;
    },
  },
};
</script>

<style>
  .seqColorCheckbox {
    top: -1px;
  }
  .checkbox-wrapper {
    justify-content: center !important;
  }
  .b-table-wrapper {
    text-align: center;
  }
  .zeroHourTable > th {
    display: table-cell;
    vertical-align: inherit;
  }
  .zeroSeqTable {
    display: inline-block;
    width: 280px;
    margin: 0 0 0 0;
  }
  .zeroSeqTable > .table-wrapper {
    height: 100%;
  }
  .zeroSeqTable.right > .table-wrapper > table > thead {
    display: none;
  }
  .cell.blue {
    background-color: blue;
    color: white;
  }
  .cell.red {
    background-color: red;
    color: white;
  }
  .cell.sky {
    background-color: deepskyblue;
    color: white;
  }
  .cell.yellow {
    background-color: yellow;
  }
  .cell.green {
    background-color: green;
    color: white;
  }
  .cell.purple {
    background-color: purple;
    color: white;
  }
</style>
