<template>
    <div style="padding : 10px">
        <h4>팔로워</h4>
        <input placeholder="?" @input="search($event.target.value)"/>
        <div class="post-header" v-for="(a, i) in follower" :key="i">
            <div class="profile" style="`backgroud-image:url(${a.image})`"></div>
            <span class="profile-name">{{a.name}}</span>
        </div>
    </div>
</template>

<script>
import { onMounted, ref } from 'vue';
import axios from 'axios';
// import { useStore } from 'vuex';

export default {
    name : 'myPage',
    // props: {
    //     one : Number,
    // }, 
    setup(){
        let follower = ref([]);
        let followerOriginal = ref([]);

        // let { one } = toRefs(props);
        // console.log(one.value);

        // let re = computed(()=>{ return follower.value.length });
        // console.log(re.value);

        // let store = useStore();
        // console.log(store.state.name);
        // console.log(store.commit());

        onMounted(()=>{
            axios.get('/follower.json').then((a)=>{
                follower.value = a.data;
                followerOriginal.value = [...a.data];
            })
        });

        function search(keyword){
            let searchedFollower = followerOriginal.value.filter((i)=>{
                return i.name.indexOf(keyword) != -1;
            });
            follower.value = [...searchedFollower];
        }
        return {follower, search};
    },
    data(){
        return{

        }
    }
}
</script>

<style>

</style>