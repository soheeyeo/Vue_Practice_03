<template>
    <div style="padding : 10px">
        <h4>팔로워</h4>
        <input placeholder="?" />
        <div class="post-header" v-for="(a, i) in follower" :key="i">
            <div class="profile" style="`backgroud-image:url(${a.image})`"></div>
            <span class="profile-name">{{a.name}}</span>
        </div>
    </div>
</template>

<script>
import { onMounted, ref, toRefs, computed } from 'vue';
import axios from 'axios';

export default {
    name : 'myPage',
    props: {
        one : Number,
    }, 
    setup(props){
        let follower = ref([]);

        let { one } = toRefs(props);
        console.log(one.value);

        let 결과 = computed(()=>{ return follower.value.length });
        console.log(결과.value)

        onMounted(()=>{
            axios.get('/follower.json').then((a)=>{
                follower.value = a.data;
            })
        })

        return {follower};
    },
    data(){
        return{

        }
    }
}
</script>

<style>

</style>