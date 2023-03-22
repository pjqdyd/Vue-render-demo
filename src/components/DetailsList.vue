<!-- 使用slot render 渲染的的列表 -->
<template>
    <div class="details-list">
      <div v-for="(opt, i) in options" :key="opt.prop">
        <div class="item-box">

          <div class="item-label">{{opt.label}}</div>

          <!-- 自定义插入 -->
          <slot v-if="opt.slot" :name="opt.slot" :scope="{ data: data, opt: opt, index: i }"></slot>
          <!-- 默认展示 -->
          <template v-else>
            <!-- slot函数renderS渲染 -->
            <template v-if="opt.renderS">
              {{ renderToHtml(opt, i) }}
              <slot :name="opt.prop" />
            </template>
            <!-- H函数renderH渲染 -->
            <template v-else-if="opt.renderH">
              <ComponentRender :opt="opt" :data="data" />
            </template>
             <!--普通渲染-->
             <template v-else>
              <div class="item-value">{{data[opt.prop]}}</div>
            </template>
          </template>

        </div>
      </div>
    </div>
  </template>

  <script>
  export default {
    name: 'DetailsList',
    components: {
      ComponentRender: {
          name: 'ComponentRender',
          props: {
              opt: Object,
              data: Object,
          },
          render(h) { // 使用组件自带的render方法提供h函数 (createElement)
            // const h = this.$createElement; // 也可直接获取h函数
            let renderH = this.opt.renderH
            if (typeof renderH === 'function') {
              return renderH(h, this.data[this.opt.prop], this.data)
            }
          },
          // renderError (h, err) { // 渲染出错的提示组件
          //   return h('pre', { style: { color: 'red' }}, err.stack)
          // }
      }
    },
    props: {
      options: {
        type: Array,
        default: () => ([])
      },
      data: {
        type: Object,
        default: () => ({})
      }
    },
    data () {
      return {};
    },
    mounted () {},
    methods: {
      // 使用js动态新增一个slot实现render函数jsx语法渲染
      renderToHtml(opt, index) {
        if (typeof opt.renderS === 'function') {
          try {
            const value = this.data[opt.prop];
            this.$slots[opt.prop] = [opt.renderS(value, this.data, index)];
          } catch (e) {
            console.error(e);
          }
        }
      }
    }
  };
  </script>

  <style scoped>
  .details-list {
    width: 100%;
    max-width: 450px;
    margin: 0 auto;
  }
  .item-box {
        width: 100%;
        padding: 20px;
        box-sizing: border-box;
        background-color: #ffffff;
        text-align: left;
        overflow: hidden;
        color: #142234;
        font-size: 16px;
        border-bottom: 1px solid rgba(20,34,52,.15);
    }
    .item-label {
        font-size: 16px;
        margin-bottom: 15px;
    }
    .item-value {
        font-size: 16px;
    }
  </style>
