<template>
    <div @click="fire" :class="필터 + ' filter-item'" :style="`background-image:url(${이미지})`">
        <slot></slot>
    </div> 
</template>

<script>
export default {
    name : 'FilterBox',
    data(){
        return{
            msg : 'hello',
        }
    },
    methods : {
        fire(){
            this.emitter.emit('필터적용', this.필터)
        }
    },
    props : {
        필터 : String,
        이미지 : String,
    }
}
</script>

<style>
.filter-item {
    width: 100px;
    height: 100px;
    margin: 10px 10px 10px auto;
    padding: 8px;
    display: inline-block;
    color : white;
    background-size: cover;
    background-position : center;
}
</style>