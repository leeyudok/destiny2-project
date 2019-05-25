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
        :data="seqData[elements]"
        :narrowed="true"
        default-sort="console1"
      >
        <template
          slot-scope="props"
          v-if="seqData[elements].length / 2 >= props.index + 1"
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
        :data="seqData[elements]"
        :narrowed="true"
        default-sort="console1"
      >
        <template
          slot-scope="props"
          v-if="seqData[elements].length / 2 < props.index + 1"
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
      /* eslint-disable */
      seqData: {
        solar: [
          { console1: '01-02', console2: '06-10', console3: '12-11', seqResult: '초록4', },
          { console1: '01-03', console2: '07-07', console3: '04-12', seqResult: '하늘4', },
          { console1: '01-07', console2: '02-08', console3: '05-06', seqResult: '하양6', },
          { console1: '01-07', console2: '09-05', console3: '10-06', seqResult: '초록6', },
          { console1: '01-10', console2: '05-02', console3: '02-04', seqResult: '노랑2', },
          { console1: '01-11', console2: '06-04', console3: '06-10', seqResult: '노랑6', },
          { console1: '02-06', console2: '08-09', console3: '06-08', seqResult: '보라2', },
          { console1: '02-10', console2: '07-01', console3: '01-07', seqResult: '보라5', },
          { console1: '02-11', console2: '01-06', console3: '06-10', seqResult: '파랑4', },
          { console1: '02-11', console2: '12-08', console3: '09-01', seqResult: '파랑6', },
          { console1: '03-05', console2: '02-04', console3: '09-12', seqResult: '하양7', },
          { console1: '04-06', console2: '09-02', console3: '10-08', seqResult: '하양5', },
          { console1: '04-08', console2: '02-06', console3: '05-05', seqResult: '빨강2', },
          { console1: '05-06', console2: '01-07', console3: '08-09', seqResult: '보라6', },
          { console1: '05-10', console2: '10-01', console3: '04-08', seqResult: '보라4', },
          { console1: '05-11', console2: '03-12', console3: '01-08', seqResult: '하늘6', },
          { console1: '05-11', console2: '07-02', console3: '07-06', seqResult: '노랑5', },
          { console1: '06-07', console2: '04-01', console3: '01-11', seqResult: '초록7', },
          { console1: '06-08', console2: '10-07', console3: '11-09', seqResult: '보라7', },
          { console1: '06-11', console2: '12-08', console3: '05-11', seqResult: '보라1', },
          { console1: '07-04', console2: '03-07', console3: '03-08', seqResult: '빨강7', },
          { console1: '07-08', console2: '01-09', console3: '06-05', seqResult: '하양3', },
          { console1: '07-08', console2: '03-04', console3: '09-12', seqResult: '초록3', },
          { console1: '07-11', console2: '01-09', console3: '07-09', seqResult: '빨강1', },
          { console1: '07-12', console2: '06-05', console3: '02-02', seqResult: '빨강4', },
          { console1: '08-05', console2: '04-10', console3: '04-05', seqResult: '초록1', },
          { console1: '08-07', console2: '04-08', console3: '08-10', seqResult: '파랑3', },
          { console1: '08-10', console2: '07-09', console3: '04-08', seqResult: '하양1', },
          { console1: '08-11', console2: '04-12', console3: '10-10', seqResult: '빨강5', },
          { console1: '08-12', console2: '11-07', console3: '03-01', seqResult: '파랑5', },
          { console1: '09-04', console2: '10-09', console3: '07-02', seqResult: '하늘7', },
          { console1: '09-06', console2: '01-06', console3: '09-08', seqResult: '하늘3', },
          { console1: '09-07', console2: '03-05', console3: '02-08', seqResult: '노랑1', },
          { console1: '09-10', console2: '03-03', console3: '11-06', seqResult: '보라3', },
          { console1: '09-10', console2: '10-03', console3: '08-03', seqResult: '초록2', },
          { console1: '09-10', console2: '11-11', console3: '08-07', seqResult: '파랑1', },
          { console1: '09-11', console2: '07-02', console3: '08-09', seqResult: '하양2', },
          { console1: '09-11', console2: '03-10', console3: '09-05', seqResult: '파랑2', },
          { console1: '10-03', console2: '08-04', console3: '09-12', seqResult: '노랑3', },
          { console1: '10-05', console2: '06-12', console3: '03-12', seqResult: '하늘1', },
          { console1: '10-06', console2: '05-02', console3: '05-07', seqResult: '하늘2', },
          { console1: '10-10', console2: '02-06', console3: '12-08', seqResult: '하늘5', },
          { console1: '11-02', console2: '08-02', console3: '09-02', seqResult: '하양4', },
          { console1: '11-04', console2: '04-06', console3: '12-05', seqResult: '파랑7', },
          { console1: '11-09', console2: '11-04', console3: '09-03', seqResult: '노랑7', },
          { console1: '11-11', console2: '08-07', console3: '01-12', seqResult: '빨강3', },
          { console1: '12-01', console2: '03-10', console3: '12-10', seqResult: '빨강6', },
          { console1: '12-07', console2: '11-07', console3: '01-12', seqResult: '노랑4', },
          { console1: '12-10', console2: '06-05', console3: '01-01', seqResult: '초록5', },
        ],
        arc: [
          { console1: '01-04', console2: '05-07', console3: '06-07', seqResult: '빨강6', },
          { console1: '01-06', console2: '11-03', console3: '05-01', seqResult: '노랑7', },
          { console1: '02-04', console2: '12-10', console3: '08-06', seqResult: '초록3', },
          { console1: '02-09', console2: '04-03', console3: '02-11', seqResult: '초록2', },
          { console1: '02-09', console2: '05-09', console3: '01-08', seqResult: '보라6', },
          { console1: '03-05', console2: '07-06', console3: '04-12', seqResult: '하늘2', },
          { console1: '03-08', console2: '01-08', console3: '03-07', seqResult: '노랑4', },
          { console1: '04-04', console2: '10-03', console3: '04-01', seqResult: '하늘1', },
          { console1: '04-05', console2: '09-07', console3: '01-06', seqResult: '하양4', },
          { console1: '04-06', console2: '06-12', console3: '09-08', seqResult: '보라2', },
          { console1: '04-07', console2: '03-05', console3: '05-01', seqResult: '파랑5', },
          { console1: '04-08', console2: '08-07', console3: '04-08', seqResult: '하양5', },
          { console1: '04-09', console2: '01-01', console3: '11-05', seqResult: '하늘5', },
          { console1: '04-12', console2: '02-07', console3: '02-10', seqResult: '초록6', },
          { console1: '04-12', console2: '05-06', console3: '04-04', seqResult: '노랑1', },
          { console1: '05-04', console2: '05-02', console3: '08-05', seqResult: '빨강5', },
          { console1: '05-04', console2: '11-08', console3: '09-08', seqResult: '하늘7', },
          { console1: '05-08', console2: '03-02', console3: '02-11', seqResult: '빨강7', },
          { console1: '05-09', console2: '01-01', console3: '05-11', seqResult: '빨강1', },
          { console1: '05-09', console2: '06-08', console3: '02-02', seqResult: '빨강4', },
          { console1: '06-02', console2: '12-05', console3: '09-03', seqResult: '파랑3', },
          { console1: '06-02', console2: '01-07', console3: '07-05', seqResult: '보라5', },
          { console1: '06-03', console2: '10-03', console3: '07-03', seqResult: '하양1', },
          { console1: '06-05', console2: '02-12', console3: '07-03', seqResult: '파랑1', },
          { console1: '07-03', console2: '02-09', console3: '09-05', seqResult: '하늘3', },
          { console1: '07-09', console2: '05-12', console3: '10-04', seqResult: '노랑5', },
          { console1: '08-01', console2: '11-02', console3: '07-04', seqResult: '빨강2', },
          { console1: '08-03', console2: '04-09', console3: '05-09', seqResult: '보라7', },
          { console1: '08-09', console2: '09-12', console3: '11-01', seqResult: '초록7', },
          { console1: '08-11', console2: '07-09', console3: '09-05', seqResult: '하늘4', },
          { console1: '08-12', console2: '06-04', console3: '12-06', seqResult: '하양7', },
          { console1: '09-01', console2: '12-04', console3: '11-04', seqResult: '노랑3', },
          { console1: '09-06', console2: '05-03', console3: '08-07', seqResult: '보라4', },
          { console1: '09-10', console2: '11-04', console3: '07-11', seqResult: '노랑2', },
          { console1: '09-11', console2: '12-06', console3: '03-07', seqResult: '파랑4', },
          { console1: '09-11', console2: '01-03', console3: '07-11', seqResult: '파랑7', },
          { console1: '09-12', console2: '07-06', console3: '04-09', seqResult: '보라1', },
          { console1: '10-08', console2: '11-06', console3: '04-02', seqResult: '빨강3', },
          { console1: '10-09', console2: '03-07', console3: '07-12', seqResult: '파랑6', },
          { console1: '10-11', console2: '03-02', console3: '08-07', seqResult: '초록1', },
          { console1: '10-11', console2: '09-03', console3: '04-09', seqResult: '노랑6', },
          { console1: '10-12', console2: '08-04', console3: '12-04', seqResult: '초록5', },
          { console1: '10-12', console2: '09-06', console3: '05-07', seqResult: '하양3', },
          { console1: '11-01', console2: '02-10', console3: '07-01', seqResult: '하양2', },
          { console1: '11-11', console2: '09-11', console3: '03-06', seqResult: '하양6', },
          { console1: '12-05', console2: '11-04', console3: '05-01', seqResult: '하늘6', },
          { console1: '12-07', console2: '01-08', console3: '05-07', seqResult: '보라3', },
          { console1: '12-08', console2: '01-08', console3: '08-03', seqResult: '파랑2', },
          { console1: '12-12', console2: '01-06', console3: '04-01', seqResult: '초록4', },
        ],
        void: [
          { console1: '01-01', console2: '02-12', console3: '08-10', seqResult: '하양1', },
          { console1: '01-01', console2: '04-05', console3: '06-05', seqResult: '빨강2', },
          { console1: '01-02', console2: '01-04', console3: '11-04', seqResult: '빨강1', },
          { console1: '01-07', console2: '12-03', console3: '08-04', seqResult: '노랑3', },
          { console1: '01-10', console2: '07-11', console3: '03-12', seqResult: '빨강6', },
          { console1: '01-12', console2: '01-01', console3: '05-04', seqResult: '하늘3', },
          { console1: '02-04', console2: '01-06', console3: '03-10', seqResult: '초록2', },
          { console1: '02-05', console2: '11-02', console3: '03-05', seqResult: '노랑2', },
          { console1: '02-06', console2: '05-04', console3: '10-03', seqResult: '하늘1', },
          { console1: '02-07', console2: '02-11', console3: '09-10', seqResult: '노랑6', },
          { console1: '02-09', console2: '12-03', console3: '10-02', seqResult: '보라5', },
          { console1: '03-03', console2: '01-03', console3: '06-08', seqResult: '초록7', },
          { console1: '04-03', console2: '02-04', console3: '02-09', seqResult: '초록1', },
          { console1: '04-03', console2: '02-11', console3: '04-07', seqResult: '보라6', },
          { console1: '04-06', console2: '05-06', console3: '05-12', seqResult: '파랑4', },
          { console1: '04-08', console2: '12-08', console3: '09-03', seqResult: '하양5', },
          { console1: '04-09', console2: '09-04', console3: '05-05', seqResult: '노랑1', },
          { console1: '05-06', console2: '07-03', console3: '07-10', seqResult: '빨강5', },
          { console1: '05-09', console2: '11-07', console3: '12-10', seqResult: '보라7', },
          { console1: '06-05', console2: '06-10', console3: '01-01', seqResult: '보라1', },
          { console1: '06-09', console2: '12-10', console3: '08-05', seqResult: '하양7', },
          { console1: '06-11', console2: '11-04', console3: '12-04', seqResult: '하늘4', },
          { console1: '07-02', console2: '08-03', console3: '03-12', seqResult: '하양6', },
          { console1: '07-04', console2: '02-07', console3: '07-09', seqResult: '초록5', },
          { console1: '07-09', console2: '06-05', console3: '05-12', seqResult: '노랑4', },
          { console1: '07-12', console2: '01-02', console3: '05-04', seqResult: '파랑5', },
          { console1: '08-04', console2: '05-08', console3: '09-04', seqResult: '하늘2', },
          { console1: '08-05', console2: '11-08', console3: '11-11', seqResult: '하늘7', },
          { console1: '08-06', console2: '02-09', console3: '02-10', seqResult: '하양3', },
          { console1: '08-06', console2: '09-02', console3: '12-12', seqResult: '노랑5', },
          { console1: '08-10', console2: '05-06', console3: '11-11', seqResult: '빨강3', },
          { console1: '08-12', console2: '05-09', console3: '09-04', seqResult: '하늘6', },
          { console1: '09-03', console2: '12-07', console3: '12-12', seqResult: '초록3', },
          { console1: '09-07', console2: '07-08', console3: '12-07', seqResult: '보라4', },
          { console1: '09-09', console2: '08-10', console3: '08-06', seqResult: '보라2', },
          { console1: '10-02', console2: '03-08', console3: '09-03', seqResult: '하양2', },
          { console1: '10-03', console2: '02-03', console3: '07-11', seqResult: '초록4', },
          { console1: '10-05', console2: '06-02', console3: '03-09', seqResult: '파랑2', },
          { console1: '10-12', console2: '11-05', console3: '08-12', seqResult: '초록6', },
          { console1: '11-01', console2: '05-07', console3: '10-02', seqResult: '파랑6', },
          { console1: '11-04', console2: '02-08', console3: '04-08', seqResult: '파랑7', },
          { console1: '11-04', console2: '04-11', console3: '12-03', seqResult: '노랑7', },
          { console1: '11-04', console2: '05-04', console3: '07-06', seqResult: '빨강4', },
          { console1: '11-06', console2: '12-08', console3: '11-11', seqResult: '빨강7', },
          { console1: '11-07', console2: '06-03', console3: '12-05', seqResult: '보라3', },
          { console1: '11-11', console2: '07-03', console3: '08-11', seqResult: '파랑3', },
          { console1: '12-02', console2: '11-04', console3: '02-11', seqResult: '하늘5', },
          { console1: '12-05', console2: '07-01', console3: '05-07', seqResult: '하양4', },
          { console1: '12-05', console2: '12-11', console3: '04-04', seqResult: '파랑1', },
        ],
      },
    };
  },
  computed: {
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
    cellRenderer(seqResult) {
      if (!seqResult || !this.seqColorFlag) {
        return;
      }
      const color = seqResult.substring(0, 2);
      let resultCls = '';
      if (color == '파랑') {
        resultCls = 'blue';
      } else if (color == '하늘') {
        resultCls = 'sky';
      } else if (color == '빨강') {
        resultCls = 'red';
      } else if (color == '노랑') {
        resultCls = 'yellow';
      } else if (color == '초록') {
        resultCls = 'green';
      } else if (color == '보라') {
        resultCls = 'purple';
      }
      return resultCls;
    },
    console1Duplicate(currIdx) {
      var currWeekData = this.seqData[this.elements];
      var currRow = currWeekData[currIdx];
      var result = false;
      if (currIdx == 0) {
        if (currRow.console1 == currWeekData[currIdx + 1].console1) {
          result = true;
        }
      } else if (currIdx == currWeekData.length - 1) {
        if (currRow.console1 == currWeekData[currIdx - 1].console1) {
          result = true;
        }
      } else {
        if ((currRow.console1 == currWeekData[currIdx + 1].console1)
          || (currRow.console1 == currWeekData[currIdx - 1].console1)
        ) {
          result = true;
        }
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
