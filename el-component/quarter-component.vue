<template>
    <div>
        <mark v-show="showSeason"
              @click.stop="showSeason=false">
        </mark>
        <el-input placeholder="请选择季度" v-model="showValue" style="width:138px;" @focus="showSeason=true">
            <i slot="prefix" class="el-input__icon el-icon-date"></i>
        </el-input>
        <el-card class="box-card"
                v-show="showSeason">
            <div slot="header" class="clearfix" style="text-align:center;padding:0">
                <button type="button"
                        aria-label="前一年"
                        class="el-picker-panel__icon-btn el-date-picker__prev-btn el-icon-d-arrow-left"
                        @click="prev">
                </button>
                <span role="button" class="el-date-picker__header-label">{{year}}年</span>
                <button type="button"
                        aria-label="后一年"
                        @click="next"
                        class="el-picker-panel__icon-btn el-date-picker__next-btn el-icon-d-arrow-right">
                </button>
            </div>
            <div class="text item">
                <el-button type="text"
                           size="medium"
                           class="text-con fl"
                           @click="selectSeason(0)">第一季度</el-button>
                <el-button type="text"
                           size="medium"
                           class="text-con fr"
                           @click="selectSeason(1)">第二季度</el-button>
            </div>
            <div class="text item">
                <el-button type="text"
                           size="medium"
                           class="text-con fl"
                           @click="selectSeason(2)">第三季度</el-button>
                <el-button type="text"
                           size="medium"
                           class="text-con fr"
                           @click="selectSeason(3)">第四季度</el-button>
            </div>
        </el-card>
    </div>
</template>

<script>
/**
 * @file:  View 组件 季度选择控件
 * @description: UI组件  可选择季节
 * @api: valueArr : 季度value defalut['01-03', '04-06', '07-09', '10-12'] 默认值待设置
 * @api: defaultValue : 默认选中值 defalut: '202001-202003' 默认值待设置
 */

export default {
  name: 'Login',
  components: {
  },
  props: {
    valueArr: {
      default: () => {
        return ['01-03', '04-06', '07-09', '10-12']
      },
      type: Array
    },
    getValue: {
      default: () => {},
      type: Function
    },
    defaultValue: {
      default: '',
      type: String
    }
  },
  data () {
    return {
      showSeason: false,
      season: '',
      year: new Date().getFullYear(),
      showValue: ''
    }
  },
  created () {
    if (this.defaultValue) {
      var value = this.defaultValue
      var arr = value.split('-')
      this.year = arr[0].slice(0, 4)
      var str = arr[0].slice(4, 6) + '-' + arr[1].slice(4, 6)
      var arrAll = this.valueArr
      this.showValue = `${this.year}年${arrAll.indexOf(str) + 1}季度`
    }
  },
  watch: {
    defaultValue: function (value, oldValue) {
      var arr = value.split('-')
      this.year = arr[0].slice(0, 4)
      var str = arr[0].slice(4, 6) + '-' + arr[1].slice(4, 6)
      var arrAll = this.valueArr
      this.showValue = `${this.year}年${arrAll.indexOf(str) + 1}季度`
    }
  },
  methods: {
    one () {
      this.showSeason = false
    },
    prev () {
      this.year = this.year * 1 - 1
    },
    next () {
      this.year = this.year * 1 + 1
    },
    selectSeason (i) {
      var that = this
      that.season = i + 1
      var arr = that.valueArr[i].split('-')
      that.getValue(that.year + arr[0] + '-' + that.year + arr[1])
      that.showSeason = false
      this.showValue = `${this.year}年${this.season}季度`
    }
  }
}
</script>

<style scoped>
  mark {
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        background: rgba(0, 0, 0, 0);
        z-index: 999;
    }

    .box-card {
        width: 322px;
        padding: 0 3px 20px;
        margin-top: 10px;
        position: fixed;
        z-index: 9999
    }

    .text {
        text-align: center;
    }

    .text-con {
        width: 40%;
        color: #606266
    }

    .fl {
        float: left;
    }

    .fr {
        float: right;
    }
</style>