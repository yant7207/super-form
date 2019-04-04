<template>
  <el-form
    :model="value"
    v-bind="formConfig"
    class="dynamic-form"
  >
    <el-row
      v-for = " (row , index) in layout.rows"
      :key="index">
      <el-col
        v-for = " (item , i) in row"
        :key = "i"
        :span = "item.span">
    <dynamic-form-item
      v-if="value[item.key]!==undefined"
      :key="item.key"
      :item="item"
      v-bind="item"
      :value="value[item.key]"
      :style="{'min-width':columnMinWidth}"
      @input="handleInput($event, item.key)"
    ></dynamic-form-item>
      </el-col>
    </el-row>
    <slot></slot>

  </el-form>
</template>

<script>
export default {
  props: {
    formConfig: {
      type: Object,
      required: true,
    },
    value: {
      type: Object,
      required: true,
    },
    columnMinWidth: {
      type: String,
    },
  },
  computed: {
    // 对组件进行布局，分行分列
    layout() {
      debugger;
      const layoutCofig = {};
      const cols = this.formConfig.cols ? this.formConfig.cols : 2;
      const colspan = 24 / cols;
      layoutCofig.colspan = colspan;
      const rows = [];
      let row = [];
      let occupy = 0;
      this.formConfig.formItemList.forEach((item) => {
        const span = item.colspan ? item.colspan : 1;
        item.span = span * colspan;
        if ((occupy + span) > cols) {
          rows.push(row);
          row = [];
          row.push(item);
          occupy = span;
        } else {
          row.push(item);
          occupy += span;
        }
      });
      rows.push(row);
      layoutCofig.rows = rows;
      return layoutCofig;
    },
  },
  mounted() {
    this.setDefaultValue();
  },
  methods: {
    handleInput(val, key) {
      // 这里element-ui没有上报event，直接就是value了
      this.$emit('input', { ...this.value, [key]: val });
    },
    setDefaultValue() {
      const formData = { ...this.value };
      // 设置默认值
      this.formConfig.formItemList.forEach(item => {
        const { key, value } = item;
        if (formData[key] === undefined || formData[key] === null) {
          formData[key] = value;
        }
      });
      this.$emit('input', { ...formData });
    },
  },
};
</script>

<style lang="less">
// .dynamic-form.el-form--inline {

//   // .block {
//   //   padding-right: 10%;
//   // }

//   .el-form-item {
//     display: inline-flex;
//     // margin-right: 0;
//     // padding-left: 10px;

//     .el-form-item__content {
//       flex: 1;
//       display: inline-flex;
//       align-items: center;

//       .el-slider {
//         width: 100%
//       }
//     }

//     .el-date-editor.el-input, .el-date-editor.el-input__inner, .el-select, .el-cascader {
//       width: 100%;
//     }
//   }
// }
</style>
