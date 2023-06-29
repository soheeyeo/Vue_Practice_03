<template>
    <div>
        <Post :게시물="게시물데이터[i]" v-for="(a, i) in 게시물데이터" :key="i" />
    </div>
</template>

<script>
import Post from './Post.vue';

export default {
    name : 'ContainerBox',
    components : {
        Post,
    },
    props : {
        게시물데이터 : Array,
    },
}
</script>

<style>

</style>