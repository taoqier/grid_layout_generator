<template>
  <div id="app" class="app" @mouseup="end">
    <input type="number" v-model.number="config.rows">行
    <input type="number" v-model.number="config.columns">列
    横间距：<input type="number" v-model.number="config.rgap">
    纵间距：<input type="number" v-model.number="config.cgap">
    <br>
    <!-- 单元格宽：<input type="number" v-model.number="config.wcell">
    单元格高：<input type="number" v-model.number="config.hcell"> -->
    <button @click="gen">生成布局</button>
    <!-- <Grid :nums="nums" :rows="row" :columns="column" :rgap="rgap" :cgap="cgap"></Grid> -->
    <Grid v-bind="config" :nums="nums" ref="grids"></Grid>
    <!-- <div class="resize" @mousedown="start" @mousemove="move" :style="style" ></div> -->
    <!-- <div class="draged" draggable="true" @dragend="dragend"></div> -->
    <!-- <div v-drag="1" class="draged"></div> -->
    <!-- <div v-drag=1 class="draged"></div> -->
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
                // wcell: 100,
                // hcell: 100,
            },
            moveable: false,
            style:{
                top:'',
                left:'',
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
        },
        start (e) {
            this.moveable = true
        },
        move (e) {
            if (this.moveable) {
                // console.log(e.clientX)
                console.log(e)
                this.style.left = e.target.client + 'px'
                this.style.top = e.pageY + 'px'
            }
        },
        end (e) {
            this.moveable = false
        },
        // dragend (e) {
        //     console.log(e)
        // }
    },
    // directives: {
    //   //拖拽&&缩放指令
    //   drag:
    //     (el,binding) => {
    //       //绑定默认样式
    //       el.style.zIndex = 9
    //       el.style.backgroundColor = 'rgba(0,0,0,1)'
    //       //如果为编辑状态
    //       if(binding.value){
    //         console.log('init bind')
    //         //定义该元素的 top left width height
    //         let x,y,w,h
    //         //鼠标的起始和结束坐标
    //         let cx_start,cy_start,cx_end,cy_end
    //         //判断鼠标样式
    //         el.onmousemove = (e) => {
    //           //获取鼠标当前位置
    //           let cx_now = e.clientX
    //           let cy_now = e.clientY
    //           console.log('move')
    //           //获取div右下角相对浏览器的位置
    //           let {top:el_top,left:el_left,width:el_width,height:el_height} = el.getBoundingClientRect()
    //           let el_bottom_height = el_top + el_height
    //           let el_right_width = el_left + el_width
    //           //判断鼠标是否在div下边界
    //           let mouse_in_bottom = (cy_now <= el_bottom_height + 5 && cy_now >= el_bottom_height - 5)
    //           //判断鼠标是否在div右边界
    //           let mouse_in_right = (cx_now <= el_right_width + 5 && cx_now >= el_right_width - 5)
    //           if(mouse_in_bottom && mouse_in_right){
    //             el.style.cursor = 'se-resize'
    //           }else if(mouse_in_right){
    //             el.style.cursor = 'e-resize'
    //           }else if(mouse_in_bottom){
    //             el.style.cursor = 's-resize'
    //           }else{
    //             el.style.cursor = 'move'
    //           }
    //         }
    //         el.onmousedown = (e) => {
    //           let mouse = el.style.cursor
    //           console.log('click')
    //           //更改默认样式
    //           el.style.backgroundColor = 'rgba(0,0,0,.5)'
    //           el.style.zIndex = 99
    //         //   console.log( el.style )
    //         //   console.log( el.style.left )
    //           //对象解构赋值
    //         //   let {left:el_x,top:el_y,width:el_w,height:el_h} = el.style
    //           let {left:el_x,top:el_y,width:el_w,height:el_h} = el.getBoundingClientRect()
    //           console.log( el_x, el_y, el_w, el_h)
    //           x = el_x
    //           y = el_y
    //           w = el_w
    //           h = el_h
    //           cx_start = e.clientX
    //           cy_start = e.clientY
    //           //绑定移动事件
    //           document.onmousemove = (e) => {
    //             cx_end = e.clientX
    //             cy_end = e.clientY
    //             //默认左下方向配置
    //             let x_move = cx_end - cx_start
    //             let y_move = cy_end - cy_start
    //             let direct = ['width','height']
    //             let pos = [w,h]
    //             let move = [x_move,y_move]
    //             let limit = 100
    //             //判断鼠标的类型进行对应的操作
    //             switch (mouse) {
    //               case 'e-resize':
    //                 direct = ['width']
    //                 pos = [w]
    //                 move = [x_move]
    //                 break;
    //               case 's-resize':
    //                 direct = ['height']
    //                 pos = [h]
    //                 move = [y_move]
    //                 break;
    //               case 'move':
    //                 direct = ['left','top']
    //                 pos = [x,y]
    //                 limit = 0
    //                 break;
    //             }
    //             handle_div(direct,pos,move,limit)
    //             // console.log(direct,pos,move,limit)
    //           }
    //           //取消移动事件
    //           document.onmouseup = (e) => {
    //             //还原默认样式
    //             el.style.zIndex = 9
    //             el.style.backgroundColor = 'rgba(0,0,0,1)'
    //             document.onmousemove = null
    //           }
    //           /**
    //            * 操作DOM位置和大小方法
    //            * @param direct 方向
    //            * @param pos 尺寸/坐标
    //            * @param move 拖动距离
    //            * @param limit 限定范围
    //            */
    //           function handle_div(direct,pos,move,limit) {
    //               console.log('handle_div')
    //             for(let i=0;i<direct.length;i++) {
    //               let val = parseInt(pos[i]) + move[i]
    //               val = val <= limit ? limit : val
    //             //   console.log(val)
    //               el.style[direct[i]] = val + 'px'
    //             }
    //           }
    //         }
    //       }
    //       else{
    //         // console.log('xxx')
    //         el.style.cursor = 'default'
    //         //移除点击事件
    //         el.onmousedown = null
    //         el.onmousemove = null
    //       }
    //     }
        
    // } 
}
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  text-align: center;
  color: #fff;
  padding: 60px;
  background-color: pink;
  position: relative;
}
.resize{
    width: 200px;
    height: 100px;
    background-color: steelblue;
    cursor: move;
    position: absolute;
    top: 100px;
    left: 100px;
}
.draged{
    width: 200px;
    height: 100px;
    background-color: purple;
    /* cursor: move; */
    position: absolute;
    top: 300px;
    left: 300px;
}
</style>