<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">

    <h3>使用命名slot渲染函数或者h函数渲染展示详情信息:</h3>

    <DetailsList :options="options" :data="details">
      <template slot="age" slot-scope="{ scope }">
        <span style="color: green;">{{ scope.data.age }}</span>
      </template>
    </DetailsList>

  </div>
</template>

<script>
import DetailsList from './components/DetailsList.vue'

export default {
  name: 'App',
  components: {
    DetailsList
  },
  data() {
    return {
      details: {
        name: '小明',
        age: '18',
        tel: '12345678',
        phone: '13100000000',
        address: '中国'
      },
      options: [
        {
          label: '姓名:',
          prop: 'name' // 默认展示
        },
        {
          label: '年龄:',
          prop: 'age',
          slot: 'age' // 自定义插入
        },
        {
          label: '电话:',
          prop: 'tel', // 使用slot渲染函数jsx
          renderS: (value, data) => {
            const address = data.address || '未知';
            return (
              <div class="tel" onClick={this.handleClick.bind(this, value)}>
                <span style="color: red;">{address}</span>
                <span> - </span>
                <span>{value}</span>
              </div>
            );
          }
        },
        {
          label: '手机:',
          prop: 'phone', // 使用h函数渲染
          renderH: (h, value, data) => {
            // const h = this.$createElement; //  不使用子组件ComponentRender组件的render函数，也可直接获取h函数来执行渲染
            const address = data.address || '未知';
            return h('div', {
              class: 'phone',
              on: {
                click: () => this.handleClick(value)
              },
              }, [
                  h('span', {style: 'color: green;'}, address),
                  h('span', '-'),
                  h('span', value),
              ]);
          }
        },
      ]
    }
  },
  methods: {
    handleClick(msg) {
      alert(msg)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
.tel,.phone {
  width: 100%;
  font-size: 18px;
  font-weight: 600;
  cursor: pointer;
}
</style>
