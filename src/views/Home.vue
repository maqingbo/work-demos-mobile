<template>
  <div class="home">
    <div class="wrap">
      <div>
        <ul
          v-for="v in tableData"
          :key="v.id"
          class="row"
          :style="firstColStyle(v)"
        >
          <li>
            {{ v.cityScale }}
          </li>
        </ul>
      </div>
      <div>
        <ul
          v-for="v in tableData"
          :key="v.id"
          class="row"
          :style="{
            height: `${rowHeight}px`,
            'line-height': `${rowHeight}px`,
          }"
        >
          <li>{{ v.title }}</li>
          <li>{{ v.currentYear }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import testData from './test.json'

export default {
  name: 'home',
  components: {},
  props: {},
  data () {
    return {
      cols: [],
      tableData: testData,
      rowHeight: 40
    }
  },
  computed: {},
  watch: {},
  created () { },
  mounted () {
    this.init()
  },
  methods: {
    init () {
      // this.cols = [
      //   { value: 'id', label: 'id' },
      //   { value: 'cityScale', label: 'cityScale' },
      //   { value: 'title', label: 'title' },
      //   { value: 'currentYear', label: 'currentYear' },
      //   { value: 'lastYear', label: 'lastYear' },
      //   { value: 'beforeLastYear', label: 'beforeLastYear' },
      //   { value: 'changeLastYear', label: 'changeLastYear' },
      //   { value: 'changeBeforeLastYear', label: 'changeBeforeLastYear' }
      // ]

      // 确定 rowSpan
      const tableData = this.tableData
      // 找到第一列的值并去重
      const firstColVal = Array.from(new Set(tableData.map(v => v.cityScale)))
      for (const val of firstColVal) {
        const sameValArr = tableData.filter(v => v.cityScale === val)
        if (sameValArr.length) {
          for (let index = 0; index < sameValArr.length; index++) {
            const item = sameValArr[index]
            const itemIndex = tableData.findIndex(j => j.id === item.id)
            if (itemIndex > -1) {
              tableData[itemIndex].rowSpan = index + 1
            }
          }
        }
      }
      this.tableData = tableData
      console.log('%c this.tableData: ', 'background-color: pink', this.tableData.map(v => v.rowSpan))
    },
    firstColStyle (v) {
      const rowHeight = this.rowHeight
      const obj = {
        height: `${rowHeight * v.rowSpan}px`,
        lineHeight: `${rowHeight}px`,
        width: '100px',
        backgroundColor: '#fff'
      }
      if (v.rowSpan > 1) {
        obj.position = 'relative'
        obj['z-index'] = v.rowSpan
        obj.left = 0
      }
      return obj
    }
  }
}
</script>

<style lang='scss' scoped>
.home {
  width: 100%;
  height: 100%;
  // background-color: pink;
  .wrap {
    display: flex;
    font-size: 12px;
    > div {
      flex: 1;
      border-right: 1px solid #cacdd1;
      &:first-child {
        flex: 0 0 100px;
      }
    }
    .row {
      border-bottom: 1px solid #cacdd1;
      &:first-child {
        border-top: 1px solid #cacdd1;
        font-weight: bold;
      }
      display: flex;
      justify-content: space-between;
      align-items: center;
      > li {
        flex: 1;
        // height: 40px;
        // line-height: 40px;
        border-left: 1px solid #cacdd1;
        text-align: center;
        &:first-child {
          padding-left: 0;
          border-left: none;
        }
        &:last-child {
          padding-right: 0;
          border-right: none;
        }
      }
    }
  }
}
</style>
