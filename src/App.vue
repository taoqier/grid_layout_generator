<template>
  <div id="app" class="app">
    <input type="number" v-model.number="config.rows">行
    <input type="number" v-model.number="config.columns">列
    横间距：<input type="number" v-model.number="config.rgap">
    纵间距：<input type="number" v-model.number="config.cgap">
    <br>
    单元格宽：<input type="number" v-model.number="config.wcell">
    单元格高：<input type="number" v-model.number="config.hcell">
    <button @click="gen">生成布局</button>
    <!-- <Grid :nums="nums" :rows="row" :columns="column" :rgap="rgap" :cgap="cgap"></Grid> -->
    <Grid v-bind="config" :nums="nums" ref="grids"></Grid>
  </div>
</template>

<script>
import Grid from './Grid/grid.vue'
export default {
    name: 'app',
    data () {
        return {
            title: 'Vue.js',
            config: {
                rows: 2,
                columns: 2,
                rgap: 1,
                cgap: 1,
                wcell: 100,
                hcell: 100,
            }
        }
    },
    components: {
        Grid
    },
    computed: {
        nums: function () {
            return Number.parseInt(this.config.rows) * Number.parseInt(this.config.columns)
        }
    },
    methods: {
        gen () {
            // console.log('2333')
            console.log(this.$refs.grids.output())
        }
    }
}
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  text-align: center;
  color: #fff;
  margin-top: 60px;
  background-color: pink;
}
</style>