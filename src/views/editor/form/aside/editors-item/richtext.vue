<template lang="pug">
  div
    el-form(v-on="$listeners" v-bind="$attrs")
      el-form-item(label="标签名")
        el-input(v-model="formItem.label")
      el-form-item(label="占据列数")
        el-input-number(controls-position="right" size="mini"
          v-model = "formItem.colspan"
          :min="1"
          :max="8")
      //- el-form-item(label="占位文本")
      //-   el-input(v-model="formItem.placeholder")
      el-form-item(label="键名")
        el-input(:value="formItem.key" readonly)
      el-form-item(label="默认值")
        el-input(v-model="formItem.value" type="textarea")
      //- el-form-item(label="禁用")
      //-   el-switch(v-model="formItem.disabled")
      //- el-form-item(label="只读")
      //-   el-switch(v-model="formItem.readonly")

    //- wtf?
    //- editor-rules(:item-rules.sync="formItem.rules" :item-type="formItem.type")
    //- editor-rules(:item-rules="formItem.rules" @update:item-rules="n => formItem.rules = n" :item-type="formItem.type" types="required,length,regexp,sql")

    pre {{formItem}}
</template>

<script>
// todo 扩展可配置项
// import EditorRules from '../editor-rules'
export default {
  // components: { EditorRules },
  props: {
    formItem: {
      type: Object,
      required: true,
    },
  },
};
</script>
