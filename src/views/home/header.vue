<template>
  <div class="header">
    <div>
      吃了么
    </div>
    <van-dropdown-menu active-color="#1989fa">
      <!-- v-model在vue3中会变为:modelValue -->
      <van-dropdown-item
        :modelValue="category"
        :options="categoryList"
        @change="change"
      />
    </van-dropdown-menu>
  </div>
</template>

<script lang="ts">
import { CATEGORY_TYPES } from "@/typings/home";
import { defineComponent, PropType, reactive, ref, toRefs } from "vue";

export default defineComponent({
  props: {
    category: {
      type: Number as PropType<CATEGORY_TYPES>, // 类型断言 PropType是vue提供的
    },
  },
  emits: ["setCurrentCategory"],
  // props 是父组件传递的参数；context 中有attr slots emit
  setup(props, context) {
    // reactive 响应式方法，可以创建响应式数据
    let state = reactive({
      categoryList: [
        { text: "热销榜", value: CATEGORY_TYPES.All },
        { text: "单人精彩套餐", value: CATEGORY_TYPES.BREAKFAST },
        { text: "冰爽饮品限时特惠", value: CATEGORY_TYPES.LUNCH },
        { text: "精选热菜", value: CATEGORY_TYPES.AFTERNOON },
        { text: "爽口凉菜", value: CATEGORY_TYPES.DINNER },
      ],
    });

    function change(value: CATEGORY_TYPES) {
      context.emit("setCurrentCategory", value); // 调用父组件方法
    }

    return {
      // toRefs 解构state，并且返回响应式数据
      // ref可以包装单个数据,并返回响应式的数据
      ...toRefs(state),
      change,
    };
  },
});
</script>

<style lang="scss" scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9999;
  background: #409eff;
  color: #fff;
  padding: 0 10px;
  width: calc(100% - 20px);
  height: 55px;
  line-height: 55px;
}
</style>

<style>
.van-dropdown-menu__bar {
  background-color: #409eff !important;
  color: #fff;
}
.van-dropdown-menu__title {
  color: #fff !important;
}
</style>
