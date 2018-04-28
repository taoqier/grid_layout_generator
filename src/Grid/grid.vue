<template>
    <div class="grid" :style="{'grid-template-rows': rowspan, 'grid-template-columns': columnspan, 'grid-row-gap': `${rgap}px`, 'grid-column-gap': `${cgap}px`}">
        <div v-for="n in nums" :key="n" @click="updatefn(n)" ref="cells">{{wcell}}</div>
        <!-- <div :contenteditable="editable" v-for="n in nums" :key="n" @click="edit">{{wcell}}</div> -->
        <!-- <textarea v-for="n in nums" :key="n" v-model="wcell"></textarea> -->
    </div>
</template>

<script>
export default {
    name: 'grid',
    data () {
        return {
            editable: false
        }
    },
    props: {
        nums: {
            type: Number,
            required: true
        },
        rows: {
            type: Number,
            required: true,
            validator: function (value) {
                return value >= 2
            }
        },
        columns: {
            type: Number,
            required: true,
            validator: function (value) {
                return value >= 1
            }
        },
        rgap: {
            type: Number,
            required: true,
            validator: function (value) {
                return value >= 0
            }
        },
        cgap: {
            type: Number,
            required: true,
            validator: function (value) {
                return value >= 0
            }
        },
        wcell: {
            type: Number,
            required: true,
            validator: function (value) {
                return value >= 0
            }
        },
        hcell: {
            type: Number,
            required: true,
            validator: function (value) {
                return value >= 0
            }
        },
    },
    computed: {
        rowspan: function () {
            let len = this.rows
            return new Array(len).fill(1).map((cell) => {
                // return '100px'
                return this.hcell+'px'
            }).reduce((css, item)=>{
                return css + ' '+ item
            })
        },
        columnspan: function () {
            let len = this.columns
            return new Array(len).fill(1).map((cell) => {
                // return '100px'
                return this.wcell+'px'
            }).reduce((css, item)=>{
                return css + ' '+ item
            })
        },
    },
    methods: {
        output () {
            // console.log('666')
            console.log(this.$el.outerHTML)
        },
        // edit () {
        //     this.editable = true
        // },
        updatefn (index) {
            console.log(index + 'resize')
            let sy = 'cell'+index
            console.log(sy)
            console.log(this.$refs.cells[index-1].style.cssText)
        }
    }
}
</script>

<style lang="scss">
.grid{
    display: grid;
    width: 500px;
    height: 600px;
    margin: 10px;
    div{
        display: flex;
        justify-content: center;
        align-items: center;
        resize:horizontal;
        overflow:auto;
        background-color:purple;
    }
}
</style>